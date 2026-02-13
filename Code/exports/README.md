# Experimental Results Data

This directory contains all experimental results in JSON format for the positional encoding comparison study.

## Overview

All results are stored as JSON files for easy parsing, reproducibility, and integration with analysis pipelines. The data captures performance metrics across three model variants (Baseline, Sinus, RoPE) tested on multiple grid sizes with multiple random seeds.

## File Descriptions

### Model-Specific Statistics

#### `baseline_stats_all.json`

Complete statistics for the **Baseline model** (learned absolute positional embeddings).

**Structure**:

```json
{
  "model_name": "baseline",
  "seeds": [42, 123, 456],
  "grid_sizes": [8, 10, 12, 15, 20],
  "results": {
    "8x8": {
      "seed_42": {
        "success_rate": 100.0,
        "avg_steps": 12.3,
        "efficiency": 1.02,
        "episodes": 100,
        "successful_episodes": 100,
        "failed_episodes": 0
      }
      // ... other seeds
    }
    // ... other grid sizes
  },
  "aggregated": {
    "8x8": {
      "mean_success": 100.0,
      "std_success": 0.0,
      "mean_steps": 12.4,
      "std_steps": 0.3
    }
    // ... other grid sizes
  }
}
```

**Metrics Included**:

- `success_rate`: Percentage of episodes reaching goal (0-100)
- `avg_steps`: Mean steps to goal (successful episodes only)
- `efficiency`: avg_steps / optimal_path_length (lower is better)
- `episodes`: Total episodes evaluated
- `successful_episodes`: Number reaching goal
- `failed_episodes`: Number failing to reach goal

#### `sinus_stats_all.json`

Complete statistics for the **Sinus model** (fixed sinusoidal positional embeddings).

Same structure as `baseline_stats_all.json`.

#### `rope_stats_all.json`

Complete statistics for the **RoPE model** (Rotary Positional Embeddings).

Same structure as `baseline_stats_all.json`.

### Aggregated Results

#### `baseline_aggregated.json`

Aggregated statistics across all seeds for Baseline model.

**Structure**:

```json
{
  "model_name": "baseline",
  "num_seeds": 3,
  "grid_sizes": [8, 10, 12, 15, 20],
  "statistics": {
    "success_rate": {
      "mean": [100.0, 64.0, 18.3, 1.3, 2.3],
      "std": [0.0, 25.2, 19.7, 0.9, 3.3],
      "min": [100.0, 42.0, 3.0, 0.0, 0.0],
      "max": [100.0, 89.0, 41.0, 2.0, 6.0]
    },
    "avg_steps": {
      "mean": [12.6, 30.5, 56.0, 74.6, 98.5],
      "std": [0.4, 8.2, 15.3, 4.2, 5.1]
    },
    "efficiency": {
      "mean": [1.05, 1.83, 3.73, 4.97, 6.56],
      "std": [0.03, 0.49, 1.02, 0.28, 0.34]
    }
  }
}
```

#### `sinus_aggregated.json`

Aggregated statistics for Sinus model. Same structure as `baseline_aggregated.json`.

#### `rope_aggregated.json`

Aggregated statistics for RoPE model. Same structure as `baseline_aggregated.json`.

### Supplementary Data

#### `length_gen_dict.json`

Combined length generalization results for all models.

**Structure**:

```json
{
  "grid_sizes": [8, 10, 12, 15, 20],
  "models": {
    "baseline": {
      "success_rates": [100.0, 64.0, 18.3, 1.3, 2.3],
      "std_dev": [0.0, 25.2, 19.7, 0.9, 3.3]
    },
    "sinus": {
      "success_rates": [100.0, 40.0, 1.7, 0.0, 0.0],
      "std_dev": [0.0, 40.6, 1.2, 0.0, 0.0]
    },
    "rope": {
      "success_rates": [100.0, 86.0, 43.3, 13.7, 5.3],
      "std_dev": [0.0, 12.1, 18.1, 12.9, 5.6]
    }
  },
  "statistical_tests": {
    "rope_vs_baseline": {
      "10x10": { "cohens_d": 1.23, "p_value": 0.208 },
      "12x12": { "cohens_d": 2.56, "p_value": 0.035 },
      "15x15": { "cohens_d": 1.21, "p_value": 0.213 },
      "20x20": { "cohens_d": 1.18, "p_value": 0.223 }
    },
    "rope_vs_sinus": {
      // Similar structure
    }
  }
}
```

**Usage**: Primary data source for generating Figure 1 (length generalization plot).

#### `optimal_path_lengths.json`

Ground truth optimal path lengths for each grid size.

**Structure**:

```json
{
  "grid_sizes": [8, 10, 12, 15, 20],
  "optimal_lengths": {
    "8x8": 12.0,
    "10x10": 16.7,
    "12x12": 15.0,
    "15x15": 15.0,
    "20x20": 15.0
  },
  "notes": "Optimal lengths computed via A* search on representative mazes"
}
```

**Purpose**:

- Calculate efficiency metrics (actual_steps / optimal_steps)
- Provide baseline for path quality assessment
- Verify model behavior against theoretical optimum

#### `sinus_rope_scaling_results.json`

Context length scaling experiment results.

**Structure**:

```json
{
  "experiment": "context_length_scaling",
  "context_lengths": [30, 45, 60, 90],
  "grid_sizes": [8, 10, 12, 15, 20],
  "models": ["sinus", "rope"],
  "results": {
    "sinus": {
      "context_30": {
        "avg_success": 28.7,
        "by_grid": [100.0, 40.0, 1.7, 0.0, 0.0]
      },
      "context_45": {
        "avg_success": 30.0,
        "by_grid": [100.0, 42.0, 2.0, 0.0, 0.0]
      }
      // ... other context lengths
    },
    "rope": {
      "context_30": {
        "avg_success": 50.1,
        "by_grid": [100.0, 86.0, 43.3, 13.7, 5.3]
      }
      // ... other context lengths
    }
  },
  "analysis": {
    "rope_advantage": [21.3, 21.2, 20.5, 20.4],
    "interpretation": "Performance stable across context lengths. RoPE advantage stems from position extrapolation, not context capacity."
  }
}
```

**Key Finding**: RoPE's ~21% advantage remains constant across context lengths from 30 to 90 tokens, indicating the benefit comes from handling novel position indices rather than utilizing more context.

## Data Generation

All files were generated by `../DCTF_final.ipynb` during the experimental runs conducted in February 2026.

### Generation Process

1. **Training**: Models trained on 8×8 grids with 3 random seeds each
2. **Evaluation**: Each model tested on 100 episodes per grid size (8×8 to 20×20)
3. **Aggregation**: Results aggregated across seeds to compute statistics
4. **Export**: JSON serialization with formatted output

### Reproducibility

To regenerate these files:

```bash
cd ../
jupyter notebook DCTF_final.ipynb
# Run all cells in sections 1-11
# New files will overwrite existing ones in exports/
```

**Note**: Exact reproduction requires identical random seeds and hardware. Minor numerical variations may occur across different GPUs or PyTorch versions.

## Data Usage

### Loading Results in Python

```python
import json

# Load specific model results
with open('rope_stats_all.json', 'r') as f:
    rope_results = json.load(f)

# Access success rate for RoPE on 12x12 grid, seed 42
success = rope_results['results']['12x12']['seed_42']['success_rate']
print(f"RoPE 12×12 success: {success}%")

# Load aggregated comparison
with open('length_gen_dict.json', 'r') as f:
    comparison = json.load(f)

# Plot length generalization
import matplotlib.pyplot as plt
grid_sizes = comparison['grid_sizes']
rope_success = comparison['models']['rope']['success_rates']
plt.plot(grid_sizes, rope_success, label='RoPE')
plt.xlabel('Grid Size')
plt.ylabel('Success Rate (%)')
plt.legend()
plt.show()
```

### Statistical Analysis Example

```python
import json
import numpy as np
from scipy import stats

# Load data
with open('length_gen_dict.json', 'r') as f:
    data = json.load(f)

# Extract 12×12 results
rope_12 = data['models']['rope']['success_rates'][2]  # Index 2 is 12×12
sinus_12 = data['models']['sinus']['success_rates'][2]

# Compare with Cohen's d
cohen_d = data['statistical_tests']['rope_vs_sinus']['12x12']['cohens_d']
p_value = data['statistical_tests']['rope_vs_sinus']['12x12']['p_value']

print(f"RoPE vs Sinus on 12×12:")
print(f"  RoPE: {rope_12:.1f}%")
print(f"  Sinus: {sinus_12:.1f}%")
print(f"  Effect size: d = {cohen_d:.2f}")
print(f"  Significance: p = {p_value:.3f}")
```

## Data Integrity

### Validation Checks

All result files have been validated for:

- **Completeness**: All grid sizes and seeds present
- **Consistency**: Success rates sum correctly with failed episodes
- **Range**: Metrics within valid bounds (e.g., success rate 0-100)
- **Precision**: Numerical precision maintained (6 decimal places)

### Known Limitations

1. **Sample Size**: Only 3 seeds per model limits statistical power
2. **Numerical Precision**: Minor floating-point variations possible
3. **Episode Variance**: High variance on extreme extrapolation (20×20)

## Data Versioning

- **Version**: 1.0
- **Date**: February 2026
- **Torch Version**: 1.13.1
- **CUDA Version**: 11.7
- **Random Seeds**: [42, 123, 456]

## File Sizes

| File                              | Size   | Description           |
| --------------------------------- | ------ | --------------------- |
| `baseline_stats_all.json`         | ~45 KB | Full baseline results |
| `sinus_stats_all.json`            | ~45 KB | Full sinus results    |
| `rope_stats_all.json`             | ~45 KB | Full RoPE results     |
| `baseline_aggregated.json`        | ~8 KB  | Aggregated baseline   |
| `sinus_aggregated.json`           | ~8 KB  | Aggregated sinus      |
| `rope_aggregated.json`            | ~8 KB  | Aggregated RoPE       |
| `length_gen_dict.json`            | ~15 KB | Combined comparison   |
| `optimal_path_lengths.json`       | ~2 KB  | Ground truth paths    |
| `sinus_rope_scaling_results.json` | ~12 KB | Context scaling       |

**Total**: ~233 KB

## Related Documentation

- Main notebook: `../DCTF_final.ipynb`
- Figure generation: `../paper_figures/README.md`
- Project overview: `../../README.md`

## Support

For questions about data format or content:

1. Check this README
2. Review data generation code in `DCTF_final.ipynb` sections 11-12
3. Open an issue with specific data questions
