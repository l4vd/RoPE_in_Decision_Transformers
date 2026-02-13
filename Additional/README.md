# Additional Analyses and Visualizations

> **⚠️ PREPRINT NOTICE**: This is a non-peer-reviewed preprint version of the research project submitted for the Implementing Transformers course (Winter Semester 2025/26). The findings and methodologies have not undergone formal peer review and should be considered preliminary.

This directory contains supplementary notebooks for visualization and theoretical analysis beyond the main experimental pipeline.

## Overview

While `../Code/DCTF_final.ipynb` contains the core experimental implementation, this directory provides additional visualizations and analyses to support the publication and deepen understanding of the architecture and RoPE properties.

## Contents

```
Additional/
├── README.md                    # This file
├── Plot_visualizations.ipynb   # Architecture and environment visualizations
└── wavelength.ipynb            # RoPE wavelength calculations
```

## Notebooks

### `Plot_visualizations.ipynb`

**Purpose**: Generate publication-quality visualizations of the Decision Transformer architecture and Key-Door maze environment.

#### Contents

1. **Decision Transformer Architecture Diagram**
   - Side-by-side comparison of Baseline vs. RoPE architectures
   - Complete visual representation including:
     - Input trajectory segment (context length K=30)
     - Three modality embeddings (State, Return, Action)
     - Positional encoding layer (Learned vs. RoPE)
     - Transformer block with 8 layers
     - Multi-head attention (10 heads, d_head=32)
     - Feed-forward network (d_ff=1280)
     - Residual connections (clearly marked)
     - Layer normalization positions
     - Output prediction head
   - Configuration parameters displayed: d_model=320, L=8, H=10
   - Color-coded components with legend
   - Residual connections shown as curved dashed arrows

2. **Key-Door Environment Visualization**
   - Professional grid-world rendering for publication
   - High DPI (300) for print quality
   - Color-blind friendly palette:
     - White: Floor
     - Dark Slate Gray: Walls
     - Royal Blue: Start (S)
     - Goldenrod: Key (K)
     - Firebrick Red: Door (D)
     - Forest Green: Goal (G)
   - Text overlay for black-and-white printing
   - Clean legend placement
   - Serif fonts (Times New Roman) for academic style
   - Export to PDF for LaTeX integration

#### Code Features

**Function: `create_decision_transformer_architecture_with_residuals()`**

- Creates 18×12 inch figure with two subplots
- Draws all architectural components with proper scaling
- Implements curved residual connection arrows
- Adds explanatory annotations
- Returns matplotlib figure object

**Function: `plot_publication_grid()`**

- Takes grid layout as list of strings
- Customizable title and overlay options
- Saves directly to PDF format
- Professional typography and spacing
- Suitable for IEEE/ACM style papers

#### Usage

```python
# Generate architecture diagram
fig = create_decision_transformer_architecture_with_residuals()
plt.show()
# Or save: fig.savefig('architecture.pdf', bbox_inches='tight')

# Generate environment visualization
level_layout = [
    "#########",
    "#S......#",
    "#......K#",
    "###D#####",
    "#.......#",
    "#......G#",
    "#########"
]
plot_publication_grid(level_layout,
                     title="Key-Door Environment Layout",
                     save_path="key_door_env.pdf")
```

#### Outputs

- `architecture_comparison.pdf`: Side-by-side architecture diagrams
- `key_door_env.pdf`: Environment grid visualization
- Both optimized for two-column paper format

---

### `wavelength.ipynb`

**Purpose**: Calculate and analyze RoPE's theoretical wavelength properties to understand extrapolation capabilities.

#### Contents

1. **Basic Wavelength Calculation**

   ```python
   base = 10000
   theta_min = 1.0 / base
   max_wavelen = 2 * math.pi / theta_min  # ≈ 62,832 positions
   ```

   Shows the absolute maximum wavelength from the highest frequency component.

2. **Dimension-Specific Analysis**
   - Configuration: base=10000, d=32 (head dimension)
   - Computes lowest frequency: `theta_min = base^(-30/32)`
   - Calculates maximum wavelength per dimension pair
   - Result: ~35,000 tokens per full rotation for longest wavelength

#### Key Calculations

**RoPE Frequency Setup**:

```
For dimension pair i (where i = 0, 1, ..., d/2-1):
  theta_i = base^(-2i/d)
  wavelength_i = 2π / theta_i
```

**Project Configuration**:

- Base: 10,000
- Embedding dimension: 320
- Head dimension: 32
- Maximum dimension index: 15

**Results**:

- Minimum wavelength: 2π ≈ 6.28 positions (highest frequency)
- Maximum wavelength: ~35,000 positions (lowest frequency)
- This covers 4,375× the training sequence length (8 steps on 8×8 grid)

#### Theoretical Implications

**Extrapolation Capacity**:

- RoPE theoretically supports positions up to ~35,000
- Training length: ~8 positions (8×8 grid optimal path)
- Evaluation length: up to 20 positions (20×20 grid)
- Wavelength coverage is sufficient for the task

**Observed Limitations**:

- Performance degrades at ~50 positions (2.5× training)
- Well within RoPE's theoretical capacity
- Bottleneck is likely the state encoder, not positional encoding
- State encoder compression ratio increases with grid size

#### Usage

```bash
cd Additional
jupyter notebook wavelength.ipynb
# Run cells to see wavelength calculations
```

**Runtime**: < 1 minute (pure computation, no training)

---

## Installation

### Dependencies

Beyond the main project requirements:

```bash
# Visualization
pip install matplotlib numpy

# Already included if main project installed
pip install -r ../requirements.txt
```

### Verification

```python
import matplotlib.pyplot as plt
import matplotlib.patches as mpatches
import numpy as np
import math
print("All dependencies installed successfully")
```

## Output Files

### From `Plot_visualizations.ipynb`

- **Architecture diagram**: High-resolution PDF showing:
  - Baseline architecture (learned embeddings)
  - RoPE architecture (rotary embeddings)
  - All architectural details labeled
- **Environment grid**: Publication-ready PDF of Key-Door maze

### From `wavelength.ipynb`

- Console output with wavelength calculations
- No file outputs (analytical notebook)

## Integration with Main Project

These notebooks are **supplementary** for visualization and analysis:

1. **Architecture Diagram**: Used in paper introduction/methodology
2. **Environment Visualization**: Used in paper experimental setup section
3. **Wavelength Analysis**: Supports discussion of RoPE properties

Main experimental results are generated in `../Code/DCTF_final.ipynb`.

## Usage Workflow

### For Paper Writing

1. Run `Plot_visualizations.ipynb` to generate:
   - Figure 1: Environment diagram
   - Supplementary Figure: Architecture comparison
2. Run `wavelength.ipynb` for theoretical calculations
3. Include PDFs directly in LaTeX document

### For Presentations

1. Use architecture diagram for methodology slides
2. Use environment visualization for problem setup
3. Wavelength calculations for Q&A backup slides

## Computational Requirements

| Notebook            | GPU Required | RAM  | Time   |
| ------------------- | ------------ | ---- | ------ |
| Plot_visualizations | No           | 2 GB | 1 min  |
| wavelength          | No           | 1 GB | <1 min |

Both are lightweight visualization/calculation notebooks.

## File Formats

### Outputs

- **PDF**: Vector graphics for publication (recommended)
- Can modify to output PNG (high-res) or SVG if needed

### Input Data

- `Plot_visualizations.ipynb`: Hardcoded layouts (no external data)
- `wavelength.ipynb`: Configuration parameters only

## Extending the Visualizations

### Adding New Grid Layouts

```python
# In Plot_visualizations.ipynb
custom_layout = [
    "##########",
    "#S.......#",
    "#...K....#",
    "####D#####",
    "#.......G#",
    "##########"
]
plot_publication_grid(custom_layout,
                     title="Custom Environment",
                     save_path="custom_env.pdf")
```

### Modifying Architecture Diagram

Edit the `draw_architecture()` function in `Plot_visualizations.ipynb`:

- Change colors via `colors` dictionary
- Adjust box positions via `y_positions`
- Modify labels and annotations
- Add new components following existing patterns

### Wavelength Analysis Extensions

```python
# Compare different base values
for base in [1000, 10000, 100000]:
    theta_min = base ** (-30/32)
    wavelength = 2 * math.pi / theta_min
    print(f"Base {base}: max wavelength = {wavelength:.0f}")
```

## Reproducibility

### Output Consistency

Figures are deterministic:

- No random elements
- Fixed layouts and parameters
- Consistent across runs

### Version Information

- Matplotlib: 3.5+
- NumPy: 1.21+
- Python: 3.8+

## Known Limitations

1. **Static Visualizations**: Notebooks generate static images, not interactive plots
2. **Fixed Parameters**: Architecture diagram uses specific configuration (d_model=320, etc.)
3. **No Animation**: Grid visualization is single frame, not trajectory animation

## Troubleshooting

### Issue: "Module 'matplotlib' not found"

**Solution**: `pip install matplotlib`

### Issue: "Figure too large for display"

**Solution**: Figures are designed for saving, not display. Use:

```python
fig.savefig('output.pdf', bbox_inches='tight')
```

### Issue: "Font not found warnings"

**Solution**: Times New Roman fallback to DejaVu Serif (acceptable for publication)

## Related Documentation

- Main project: `../README.md`
- Core experiments: `../Code/README.md`
- Results data: `../Code/exports/README.md`
- Final paper: `../Reports/README.md`

## Version History

- **v1.0** (Feb 2026): Initial release
  - Architecture visualization with residual connections
  - Environment grid visualization
  - Wavelength theoretical analysis

## Contact

For questions about visualizations:

1. Review notebook markdown cells and comments
2. Check this README
3. Consult main project documentation
4. Open an issue with specific questions

## License

Same as main project (MIT License). See `../LICENSE`.

---

**Status**: ✅ Publication-ready visualizations  
**Maintainer**: Lasse von Danwitz  
**Last Updated**: February 11, 2026  
**Acknowledgment**: Non-peer-reviewed preprint version
