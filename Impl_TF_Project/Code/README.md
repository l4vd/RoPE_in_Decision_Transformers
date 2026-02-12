# Code Documentation

This directory contains the primary experimental implementation for evaluating positional encoding strategies in Decision Transformers.

## Files and Structure

```
Code/
├── README.md                    # This file
├── DCTF_final.ipynb            # Main experiment notebook
├── exports/                     # Experimental results (JSON format)
│   ├── README.md
│   ├── baseline_stats_all.json
│   ├── baseline_aggregated.json
│   ├── sinus_stats_all.json
│   ├── sinus_aggregated.json
│   ├── rope_stats_all.json
│   ├── rope_aggregated.json
│   ├── length_gen_dict.json
│   ├── optimal_path_lengths.json
│   └── sinus_rope_scaling_results.json
└── paper_figures/               # Generated figures for publication
    ├── README.md
    ├── fig1_length_generalization.pdf
    ├── fig2_mathematical_properties.pdf
    ├── results_table.tex
    ├── summary_report.txt
    └── [pdf/png/svg/pgf subdirectories]
```

## Main Notebook: DCTF_final.ipynb

### Overview

This Jupyter notebook contains the complete experimental pipeline for comparing three positional encoding strategies in Decision Transformers:

1. **Baseline**: Learned absolute positional embeddings
2. **Sinus**: Fixed sinusoidal positional embeddings
3. **RoPE**: Rotary Positional Embeddings

### Notebook Structure

The notebook is organized into the following sections:

#### 1. Environment Setup

- Package imports and dependency verification
- Random seed configuration for reproducibility
- Device selection (CUDA/CPU)

#### 2. Environment Implementation

**Key-Door Maze Environment**

- Grid-based navigation task
- Discrete action space: {Up, Down, Left, Right}
- Observation space: Grid with agent, walls, key, door, goal
- Reward structure:
  - +2.0 for reaching goal
  - +0.5 for picking up key or opening door
  - -0.01 per step (efficiency incentive)
  - -0.05 for invalid moves

**Environment Features**:

```python
class KeyDoorMazeEnv:
    - __init__(size=8): Initialize grid of specified size
    - reset(): Reset environment to initial state
    - step(action): Execute action and return (obs, reward, done, info)
    - render(): Visualize current state
    - get_optimal_path_length(): Ground truth path length
```

#### 3. Data Generation

**Dataset Creation**

- 5,000 episodes on 8×8 grids
- Mixed optimality: 70% optimal, 30% suboptimal
- Trajectory format: (state, action, reward, return-to-go)

**Data Splits**:

- Training: 70% (3,500 episodes)
- Validation: 15% (750 episodes)
- Test: 15% (750 episodes)

#### 4. Model Architecture

**Decision Transformer Components**

1. **State Encoder**:

   ```python
   - Item embeddings for discrete observations
   - 2-layer CNN (64 filters, 3×3 kernels)
   - AdaptiveAvgPool2d for variable grid sizes
   - Linear projection to d_model=320
   ```

2. **Modality Embeddings**:
   - Return-to-go: Linear projection
   - State: CNN encoder
   - Action: Learned lookup table

3. **Positional Encodings**:
   - **Global timestep (t)**: Sinusoidal (all variants)
   - **Context position (k)**: Variant-specific
     - Baseline: Learned embeddings
     - Sinus: Fixed sinusoidal
     - RoPE: Rotary embeddings

4. **Transformer Backbone**:
   ```
   Architecture:
   - Layers: 8
   - Attention heads: 10
   - Head dimension: 32
   - FFN hidden: 1280 (4× expansion)
   - Dropout: 0.1
   - Pre-LayerNorm configuration
   ```

#### 5. RoPE Implementation

**Rotary Positional Embeddings**

```python
def rope_rotation(x, position_ids, base=10000):
    """
    Apply rotary positional embeddings to queries/keys.

    Args:
        x: Input tensor [batch, seq_len, heads, head_dim]
        position_ids: Position indices [batch, seq_len]
        base: Frequency base (default: 10000)

    Returns:
        Rotated tensor with same shape
    """
    # Compute frequency bands
    dim = x.shape[-1]
    inv_freq = 1.0 / (base ** (torch.arange(0, dim, 2) / dim))

    # Generate position-dependent angles
    freqs = position_ids.unsqueeze(-1) * inv_freq

    # Apply rotation
    cos = freqs.cos()
    sin = freqs.sin()

    x_rot = x * cos + rotate_half(x) * sin
    return x_rot
```

**Key Properties**:

- Relative position encoding: Attention depends on (m-n) distance
- Translation invariance: Shifting entire sequence preserves patterns
- Theoretical extrapolation: Supports arbitrary sequence lengths

#### 6. Training Pipeline

**Training Configuration**:

```python
optimizer = AdamW(
    model.parameters(),
    lr=1e-4,
    weight_decay=0.01,
    betas=(0.9, 0.999)
)

training_params = {
    'batch_size': 64,
    'epochs': 100,
    'context_length': 30,
    'max_grad_norm': 1.0,
}
```

**Training Loop**:

1. Sample trajectories with context window
2. Forward pass with causal masking
3. Compute action prediction loss (CrossEntropy)
4. Backpropagation with gradient clipping
5. Validation every 5 epochs
6. Model checkpointing based on validation loss

**Loss Function**:

```python
loss = F.cross_entropy(
    action_predictions,  # [batch, seq_len, action_dim]
    target_actions,      # [batch, seq_len]
    reduction='mean'
)
```

#### 7. Evaluation Protocol

**Length Generalization Testing**

Test grid sizes: 8×8, 10×10, 12×12, 15×15, 20×20

For each model and grid size:

1. Generate 100 test episodes
2. Run model in autoregressive mode
3. Record success rate, path efficiency, steps to goal

**Metrics Computed**:

- **Success Rate**: Percentage of episodes reaching goal
- **Path Efficiency**: steps_taken / optimal_path_length
- **Average Steps**: Mean steps for successful episodes
- **Variance**: Standard deviation across seeds

**Evaluation Code**:

```python
def evaluate_model(model, grid_size, num_episodes=100):
    """
    Evaluate model on specified grid size.

    Returns:
        {
            'success_rate': float,
            'avg_steps': float,
            'efficiency': float,
            'failures': int
        }
    """
```

#### 8. Context Length Scaling

**Experiment**: Test if RoPE's advantage comes from context capacity or position extrapolation

Context lengths tested: 30, 45, 60, 90 tokens

Results show performance stability, confirming RoPE advantages stem from position extrapolation.

#### 9. Attention Analysis

**Visualization of Attention Patterns**:

- Extract attention weights from middle layer
- Average across heads and episodes
- Compare distance decay patterns
- Verify translation invariance for RoPE

#### 10. Statistical Analysis

**Hypothesis Testing**:

- Welch's t-test (unequal variances)
- Cohen's d effect size
- Significance threshold: α = 0.05

**Reported Statistics**:

- Mean ± standard deviation
- Effect sizes with interpretation
- p-values with significance markers

#### 11. Results Export

All results saved to `exports/` in JSON format:

```python
results = {
    'model_name': 'rope',
    'grid_sizes': [8, 10, 12, 15, 20],
    'seeds': [42, 123, 456],
    'statistics': {
        'mean': [...],
        'std': [...],
        'min': [...],
        'max': [...]
    },
    'raw_data': [...]
}
```

#### 12. Figure Generation

Publication-quality figures saved to `paper_figures/`:

- Multiple formats: PDF (vector), PNG (raster), SVG (web), PGF (LaTeX)
- Color scheme: colorblind-friendly palette
- Font sizes optimized for two-column papers
- LaTeX-compatible labels

### Running the Notebook

#### Prerequisites

```bash
pip install torch numpy matplotlib jupyter pandas scipy tqdm seaborn
```

#### Execution

1. **Full Reproduction** (complete training, ~6-8 hours on GPU):

   ```bash
   jupyter notebook DCTF_final.ipynb
   # Run all cells
   ```

2. **Results Only** (skip training, load checkpoints):
   - Run cells in sections 1, 7-12
   - Loads pre-trained models and generates visualizations

3. **Single Model Test**:
   - Modify `models_to_train` variable
   - Run only relevant sections

#### Expected Outputs

After running the complete notebook:

- 3 trained models (one per variant, one per seed = 9 total)
- Statistical comparison tables
- Attention visualization plots
- Length generalization curves
- JSON files with all metrics

## Code Quality Standards

### Best Practices Implemented

1. **Reproducibility**:
   - Fixed random seeds
   - Deterministic operations where possible
   - Version logging for dependencies

2. **Documentation**:
   - Docstrings for all functions
   - Inline comments for complex logic
   - Markdown cells explaining methodology

3. **Modularity**:
   - Clear separation of concerns
   - Reusable functions
   - Minimal code duplication

4. **Error Handling**:
   - Input validation
   - Graceful fallbacks
   - Informative error messages

### Code Style

- **Format**: PEP 8 compliant
- **Naming**: Descriptive variable names
- **Comments**: Explain "why", not "what"
- **Organization**: Logical section ordering

## Troubleshooting

### Common Issues

1. **CUDA Out of Memory**:

   ```python
   # Reduce batch size
   batch_size = 32  # instead of 64

   # Or reduce context length
   context_length = 20  # instead of 30
   ```

2. **Slow Training**:
   - Ensure CUDA is available: `torch.cuda.is_available()`
   - Check GPU utilization: `nvidia-smi`
   - Consider reducing model size

3. **Reproducibility Issues**:

   ```python
   # Ensure all seeds are set
   torch.manual_seed(seed)
   np.random.seed(seed)
   torch.backends.cudnn.deterministic = True
   ```

4. **Import Errors**:
   ```bash
   # Install missing packages
   pip install -r requirements.txt
   ```

## Extending the Code

### Adding New Positional Encodings

1. Implement encoding function:

   ```python
   def my_positional_encoding(positions, d_model):
       # Your implementation
       return embeddings
   ```

2. Integrate into model:

   ```python
   if pos_encoding_type == 'my_encoding':
       pos_emb = my_positional_encoding(positions, d_model)
   ```

3. Add to comparison loop and evaluation

### Testing on New Environments

1. Implement environment following Gym API
2. Modify state encoder for new observation space
3. Adjust action space dimensions
4. Update reward processing if needed

### Parameter Sweeps

```python
# Example: Test different context lengths
context_lengths = [15, 30, 45, 60]
results = {}

for ctx_len in context_lengths:
    model = train_model(context_length=ctx_len)
    results[ctx_len] = evaluate_model(model)
```

## Data Format Specifications

### Trajectory Format

```python
trajectory = {
    'states': np.array,      # [T, H, W] for grid observations
    'actions': np.array,     # [T] discrete actions
    'rewards': np.array,     # [T] scalar rewards
    'returns': np.array,     # [T] return-to-go
    'timesteps': np.array,   # [T] timestep indices
    'success': bool,         # Episode outcome
    'path_length': int       # Total steps
}
```

### Model Checkpoint Format

```python
checkpoint = {
    'model_state_dict': OrderedDict,
    'optimizer_state_dict': dict,
    'epoch': int,
    'train_loss': float,
    'val_loss': float,
    'config': dict,
    'seed': int
}
```

## Version History

- **v1.0** (Feb 2026): Initial release with all three positional encoding variants
- Includes training, evaluation, and visualization pipelines
- Publication-ready results and figures

## Related Files

- Main project README: `../README.md`
- Visualization code: `../Additional/`
- Final reports: `../Reports/`
- Exported results: `./exports/README.md`
- Figure documentation: `./paper_figures/README.md`

## Support

For questions about the code implementation:

1. Check the inline documentation in the notebook
2. Review this README
3. Consult the main project README
4. Open an issue with detailed error description
