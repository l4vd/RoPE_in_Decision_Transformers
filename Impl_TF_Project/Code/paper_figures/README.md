# Publication Figures

This directory contains all publication-quality figures and tables generated for the research paper on Rotary Positional Embeddings in Decision Transformers.

## Overview

All figures are provided in multiple formats to support various publication workflows:

- **PDF**: Vector format for high-quality print and LaTeX inclusion
- **PNG**: Raster format for presentations and web

## Directory Structure

```
paper_figures/
├── README.md                           # This file
├── fig1_length_generalization.pdf     # Main results figure
├── fig2_mathematical_properties.pdf   # Attention analysis
├── results_table.tex                  # LaTeX results table
├── summary_report.txt                 # Human-readable summary
├── data/                              # Underlying figure data
│   ├── report_context_scaling_report.json
│   ├── statistical_summary_baseline_vs_rope.json
│   ├── statistical_summary_baseline_vs_sinus.json
│   └── statistical_summary_sinus_vs_rope.json
├── pdf/                               # All PDF figures
├── png/                               # All PNG figures (300 DPI)
```

## Main Figures

### Figure 1: Length Generalization Comparison (`fig1_length_generalization.pdf`)

**Description**: Primary result showing success rates across grid sizes for all three positional encoding strategies.

**Panels**:

1. **Main plot**: Line plot showing success rate vs. grid size
   - Blue line: Baseline (learned embeddings)
   - Orange line: Sinus (sinusoidal embeddings)
   - Green line: RoPE (rotary embeddings)
   - Error bars: ±1 standard deviation across 3 seeds
   - Shaded region: Training distribution (8×8)

2. **Inset**: Zoomed view of 10×12 grid region

**Key Features**:

- Colorblind-friendly palette
- Error bars for uncertainty quantification
- Grid lines for readability
- LaTeX-formatted axis labels
- Legend with effect sizes

**Dimensions**:

- Full width: 7 inches (two-column layout)
- Height: 4 inches
- DPI (PNG): 300

**Usage in LaTeX**:

```latex
\begin{figure}[t]
    \centering
    \includegraphics[width=\linewidth]{fig1_length_generalization.pdf}
    \caption{Length generalization performance across grid sizes.}
    \label{fig:length_gen}
\end{figure}
```

### Figure 2: Mathematical Properties (`fig2_mathematical_properties.pdf`)

**Description**: Analysis of attention patterns and positional encoding properties.

**Panels** (2×2 grid):

1. **Top-left: Attention Distance Decay**
   - X-axis: Relative distance between tokens
   - Y-axis: Average attention weight
   - Comparison of attention falloff patterns
   - Shows RoPE's consistent distance-based attention

2. **Top-right: Translation Invariance Verification**
   - Heatmap showing attention pattern correlation
   - Comparing patterns at different absolute positions
   - Color scale: 0 (no correlation) to 1 (identical)
   - Empirical validation: mean error < 2%

3. **Bottom-left: Attention Heatmaps**
   - Side-by-side attention matrices
   - Left: Baseline (position-specific patterns)
   - Right: RoPE (banded structure)
   - Token positions on both axes

4. **Bottom-right: Context Scaling Results**
   - Bar plot: Success rate vs. context length
   - Groups: 30, 45, 60, 90 tokens
   - Shows stable RoPE advantage (~21%)

**Dimensions**:

- Width: 7 inches
- Height: 6 inches
- DPI (PNG): 300

**Usage in LaTeX**:

```latex
\begin{figure*}[t]
    \centering
    \includegraphics[width=\textwidth]{fig2_mathematical_properties.pdf}
    \caption{Analysis of positional encoding properties.}
    \label{fig:analysis}
\end{figure*}
```

## Supplementary Figures

Located in format-specific subdirectories (`pdf/`, `png/`):

### Training Dynamics (`training_dynamics.pdf`)

- Training and validation loss curves
- Success rate during training
- Overfitting indicators
- Comparison across all models

### Statistical Analysis (`statistical_analysis.pdf`)

- Effect sizes (Cohen's d) visualization
- Confidence intervals
- Significance markers
- Grid size on x-axis

### Efficiency Analysis (`path_efficiency.pdf`)

- Steps to goal vs. optimal path length
- Efficiency ratio (actual/optimal)
- Performance degradation patterns

### Attention Patterns Individual (`attention_patterns_*.pdf`)

- Detailed attention heatmaps for each model
- Multiple layers shown
- Individual seed visualizations

## Tables

### `results_table.tex`

**Description**: LaTeX-formatted table with complete numerical results.

**Structure**:

```latex
\begin{table*}[t]
\centering
\begin{tabular}{lccccc}
\toprule
\textbf{Grid} & \textbf{Baseline} & \textbf{Sinus} & \textbf{RoPE} &
\textbf{Δ vs Base} & \textbf{Δ vs Sinus} \\
\midrule
8×8   & 100.0 ± 0.0   & 100.0 ± 0.0   & 100.0 ± 0.0   & --- & --- \\
10×10 & 64.0 ± 25.2   & 40.0 ± 40.6   & 86.0 ± 12.1   & +22.0 & +46.0 \\
12×12 & 18.3 ± 19.7   & 1.7 ± 1.2     & 43.3 ± 18.1   & +25.0 & +41.6 \\
15×15 & 1.3 ± 0.9     & 0.0 ± 0.0     & 13.7 ± 12.9   & +12.4 & +13.7 \\
20×20 & 2.3 ± 3.3     & 0.0 ± 0.0     & 5.3 ± 5.6     & +3.0 & +5.3 \\
\bottomrule
\end{tabular}
\caption{Length generalization success rates.}
\label{tab:length_gen}
\end{table*}
```

**Features**:

- Mean ± standard deviation format
- Percentage values (0-100 range)
- Absolute differences in final columns
- Uses booktabs package for professional appearance

**Usage**: Include directly in LaTeX document:

```latex
\input{results_table.tex}
```

## Data Files

### `data/statistical_summary_*.json`

**Files**:

- `statistical_summary_baseline_vs_rope.json`
- `statistical_summary_baseline_vs_sinus.json`
- `statistical_summary_sinus_vs_rope.json`

**Structure**:

```json
{
  "comparison": "rope_vs_sinus",
  "grid_sizes": [8, 10, 12, 15, 20],
  "statistics": {
    "10x10": {
      "rope_mean": 86.0,
      "sinus_mean": 40.0,
      "difference": 46.0,
      "cohens_d": 1.23,
      "p_value": 0.208,
      "significant": false,
      "effect_interpretation": "large"
    }
    // ... other grid sizes
  }
}
```

**Purpose**: Underlying data for statistical visualizations and verification.

### `data/report_context_scaling_report.json`

**Description**: Context length scaling experiment data.

**Structure**:

```json
{
  "context_lengths": [30, 45, 60, 90],
  "models": ["sinus", "rope"],
  "results": {
    "sinus": {
      "30": 28.7,
      "45": 30.0,
      "60": 29.6,
      "90": 29.7
    },
    "rope": {
      "30": 50.1,
      "45": 50.2,
      "60": 50.1,
      "90": 50.1
    }
  },
  "rope_advantage": [21.3, 21.2, 20.5, 20.4]
}
```

## Format Specifications

### PDF Figures

- **Vector format**: Scalable without quality loss
- **Fonts**: Embedded Type 1 fonts
- **Color space**: RGB
- **Compression**: Lossless
- **Size**: Typically 50-200 KB

**Best for**: Print publication, LaTeX documents, archival

### PNG Figures

- **Resolution**: 300 DPI
- **Color depth**: 24-bit RGB
- **Compression**: PNG (lossless)
- **Transparency**: Yes (alpha channel)
- **Size**: Typically 500-1500 KB

**Best for**: PowerPoint presentations, Word documents, web (resized)

## Figure Generation

### Source Code

Figures generated by `../DCTF_final.ipynb` in section 12 (Figure Generation).

### Regeneration

To regenerate all figures:

```python
# In DCTF_final.ipynb
import matplotlib.pyplot as plt
import json

# Load results
with open('exports/length_gen_dict.json', 'r') as f:
    data = json.load(f)

# Generate figures
generate_all_figures(data, output_dir='paper_figures/')

# Exports to all formats automatically
```

### Customization

Modify figure parameters in notebook:

```python
# Figure aesthetics
plt.rcParams.update({
    'font.size': 10,
    'axes.labelsize': 12,
    'axes.titlesize': 14,
    'legend.fontsize': 10,
    'figure.dpi': 300,
    'savefig.dpi': 300,
    'savefig.bbox': 'tight',
    'savefig.pad_inches': 0.1
})

# Color scheme (colorblind-friendly)
colors = {
    'baseline': '#0173B2',  # Blue
    'sinus': '#DE8F05',     # Orange
    'rope': '#029E73'       # Green
}
```

## Quality Assurance

All figures validated for:

✅ **Resolution**: Minimum 300 DPI for raster formats  
✅ **Fonts**: Readable at publication size (8-10pt)  
✅ **Colors**: Distinguishable in grayscale and for colorblind readers  
✅ **Labels**: All axes labeled with units  
✅ **Legends**: Clear and unambiguous  
✅ **Error bars**: Present where appropriate  
✅ **Aspect ratio**: Standard publication ratios  
✅ **File size**: Optimized for web and print

## Usage in Publications

### IEEE Format

```latex
\usepackage{graphicx}
\begin{figure}[t]
    \centering
    \includegraphics[width=\columnwidth]{fig1_length_generalization.pdf}
    \caption{Length generalization comparison.}
    \label{fig:length}
\end{figure}
```

### Two-Column Paper

```latex
% Single column
\begin{figure}[h]
    \includegraphics[width=\columnwidth]{fig1_length_generalization.pdf}
\end{figure}

% Full width
\begin{figure*}[t]
    \includegraphics[width=\textwidth]{fig2_mathematical_properties.pdf}
\end{figure*}
```

### Beamer Presentations

```latex
\begin{frame}{Results}
    \begin{figure}
        \includegraphics[width=0.8\textwidth]{fig1_length_generalization.pdf}
    \end{figure}
\end{frame}
```

## File Sizes Summary

| Format | Total Size | Typical per Figure |
| ------ | ---------- | ------------------ |
| PDF    | ~800 KB    | 100-200 KB         |
| PNG    | ~6 MB      | 500-1500 KB        |

**Total directory size**: ~10.5 MB

## Related Documentation

- Main notebook: `../DCTF_final.ipynb`
- Results data: `../exports/README.md`
- Project overview: `../../README.md`

## Support

For questions about figures:

1. Check generation code in `DCTF_final.ipynb` section 12
2. Review this README for format specifications
3. Consult matplotlib documentation for customization

## Errata

No known issues with current figures. Any corrections will be documented here with version updates.
