# Rotary Positional Embeddings for Length Generalization in Decision Transformers

> **⚠️ PREPRINT NOTICE**: This is a non-peer-reviewed preprint version of research conducted as part of the Implementing Transformers course (Winter Semester 2025/26). The findings and methodologies presented here have not undergone formal peer review and should be considered preliminary results from a course project.

## Project Overview

This repository contains the complete implementation, experimental results, and analysis for the research project **"Rotary Positional Embeddings for Length Generalization in Decision Transformers"** conducted as part of the Implementing Transformers course (Winter Semester 2025/26).

### Abstract

This project investigates the impact of positional encoding strategies on the length generalization capabilities of Decision Transformers. We compare three approaches:

- Learned absolute positional embeddings (Baseline)
- Fixed sinusoidal positional embeddings (Sinus)
- Rotary Positional Embeddings (RoPE)

Through a Key-Door maze navigation task trained exclusively on 8×8 grids, we evaluate performance on grids up to 20×20. Results demonstrate that RoPE significantly outperforms baselines, achieving 86% success on 10×10 grids compared to 64% (learned) and 40% (sinusoidal). On 12×12 grids, RoPE maintains 43.3% success versus 18.3% and 1.7% for alternatives.

## Repository Structure

```
Impl_TF_Project/
├── README.md                          # This file
├── Code/                              # Main experimental code
│   ├── README.md                      # Code documentation
│   ├── DCTF_final.ipynb              # Main experiment notebook
│   ├── exports/                       # Experimental results (JSON)
│   └── paper_figures/                # Generated figures and tables
├── Additional/                        # Supplementary analyses
│   ├── README.md                      # Additional analysis documentation
│   ├── Plot_visualizations.ipynb     # Visualization code
│   └── wavelength.ipynb              # RoPE wavelength analysis
└── Reports/                           # Final publications
    ├── README.md                      # Report documentation
    ├── Impl_TF_Report_professional_von_Danwitz.pdf
    ├── Impl_TF_Report_von_Danwitz.pdf
    └── Impl_TF_Presentation_von_Danwitz.pdf
```

## Key Results

### Length Generalization Performance

| Grid Size | Baseline | Sinus  | RoPE      | RoPE Advantage |
| --------- | -------- | ------ | --------- | -------------- |
| 8×8       | 100.0%   | 100.0% | 100.0%    | -              |
| 10×10     | 64.0%    | 40.0%  | **86.0%** | +22.0%         |
| 12×12     | 18.3%    | 1.7%   | **43.3%** | +25.0%         |
| 15×15     | 1.3%     | 0.0%   | **13.7%** | +12.4%         |
| 20×20     | 2.3%     | 0.0%   | **5.3%**  | +3.0%          |

### Key Findings

1. **Superior Generalization**: RoPE demonstrates robust length generalization through relative positional encodings
2. **Translation Invariance**: Empirically verified with <3% error across position shifts
3. **Stability**: Lower variance (±12.1%) compared to learned embeddings (±25.2%)
4. **Position Extrapolation**: RoPE's advantage stems from handling novel position indices, not context capacity

## Getting Started

### Prerequisites

```bash
python >= 3.8
torch >= 1.10.0
numpy >= 1.21.0
matplotlib >= 3.4.0
jupyter >= 1.0.0
```

### Installation

1. Clone the repository:

```bash
git clone <repository-url>
cd Impl_TF_Project
```

2. Install dependencies:

```bash
pip install torch numpy matplotlib jupyter pandas scipy tqdm
```

3. Install additional packages for visualization:

```bash
pip install seaborn plotly
```

### Quick Start

1. **Run Main Experiments**:

   ```bash
   cd Code
   jupyter notebook DCTF_final.ipynb
   ```

   This notebook contains all three model variants (Baseline, Sinus, RoPE) with complete training and evaluation pipelines.

2. **Generate Visualizations**:

   ```bash
   cd Additional
   jupyter notebook Plot_visualizations.ipynb
   ```

3. **Analyze RoPE Properties**:
   ```bash
   cd Additional
   jupyter notebook wavelength.ipynb
   ```

## Methodology

### Environment: Key-Door Maze

- **Task**: Sequential navigation requiring agent to collect key → open door → reach goal
- **Training Data**: 5,000 trajectories on 8×8 grids (70% optimal, 30% suboptimal)
- **Evaluation**: Grid sizes from 8×8 to 20×20 (zero-shot generalization)

### Model Architecture

- **Type**: Decision Transformer (modified for grid-world)
- **Embedding Dimension**: 320
- **Attention Heads**: 10 (head dim = 32)
- **Layers**: 8 transformer blocks
- **Context Length**: 30 timesteps
- **FFN Hidden Dim**: 1280 (4× expansion)
- **Dropout**: 0.1

### Positional Encoding Variants

1. **Baseline**: Learned absolute embeddings for context positions
2. **Sinus**: Fixed sinusoidal embeddings (Vaswani et al., 2017)
3. **RoPE**: Rotary positional embeddings (Su et al., 2021)

_Note_: All variants use sinusoidal encoding for global episode timesteps.

## Reproducibility

### Training Protocol

- **Optimizer**: AdamW (lr=1e-4, weight_decay=0.01)
- **Batch Size**: 64
- **Epochs**: 100
- **Seeds**: 3 random seeds for statistical validation
- **Evaluation**: 100 episodes per grid size per seed

### Reproducing Results

The main notebook `Code/DCTF_final.ipynb` contains:

- Complete data generation pipeline
- All three model implementations
- Training loops with checkpointing
- Comprehensive evaluation suite
- Statistical analysis

To reproduce the exact results:

1. Set random seeds as specified in the notebook
2. Run all cells sequentially
3. Results will be saved to `Code/exports/`
4. Figures will be generated in `Code/paper_figures/`

## Experimental Results

### Result Files

All experimental data is stored in `Code/exports/`:

- `baseline_stats_all.json`: Complete statistics for learned embeddings
- `sinus_stats_all.json`: Complete statistics for sinusoidal embeddings
- `rope_stats_all.json`: Complete statistics for RoPE
- `*_aggregated.json`: Aggregated results across seeds
- `sinus_rope_scaling_results.json`: Context length scaling experiments
- `optimal_path_lengths.json`: Ground truth path lengths

### Figures

Publication-quality figures in `Code/paper_figures/`:

- Multiple formats: PDF, PNG; (LaTeX)
- `fig1_length_generalization.pdf`: Main generalization comparison
- `fig2_mathematical_properties.pdf`: Attention pattern analysis
- `results_table.tex`: LaTeX table of results

## Statistical Analysis

### Effect Sizes (Cohen's d)

| Grid Size | Effect Size | p-value   | Significant |
| --------- | ----------- | --------- | ----------- |
| 10×10     | 1.23        | 0.208     | No          |
| 12×12     | **2.56**    | **0.035** | **Yes**     |
| 15×15     | 1.21        | 0.213     | No          |
| 20×20     | 1.18        | 0.223     | No          |

All effect sizes exceed d=0.8 (large effect), with 12×12 achieving statistical significance despite small sample size (n=3 seeds).

## Key References

1. Vaswani, A., et al. (2017). Attention is all you need. _NeurIPS_.
2. Su, J., et al. (2021). RoFormer: Enhanced transformer with rotary position embedding. _arXiv:2104.09864_.
3. Chen, L., et al. (2021). Decision transformer: Reinforcement learning via sequence modeling. _NeurIPS_.
4. Press, O., et al. (2021). Train short, test long: Attention with linear biases enables input length extrapolation. _arXiv:2108.12409_.

## Project Timeline

- **Course**: Implementing Transformers
- **Institution**: Winter Semester 2025/26
- **Start Date**: 12 January 2026
- **Completion Date**: 7 February 2026

## Acknowledgments

- Course instructors for guidance on transformer implementations
- Decision Transformer paper authors (Chen et al., 2021) for the foundational architecture
- RoFormer authors (Su et al., 2021) for the RoPE formulation

## Contact

**Author**: Lasse von Danwitz  
**Course**: Implementing Transformers  
**Semester**: Winter 2025/26

For questions or collaboration inquiries, please open an issue in this repository.

## AI Assistance Disclosure

Throughout the course of this project, Large Language Models (LLMs) have been used for:

- Code development and debugging
- Documentation generation
- Text refinement and editing

All conceptual work, experimental design, implementation decisions, and scientific conclusions are the original work of the author.
