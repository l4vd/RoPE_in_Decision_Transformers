# Development Quickstart Guide

> **⚠️ PREPRINT NOTICE**: This is a non-peer-reviewed preprint version of research from the Implementing Transformers course (Winter Semester 2025/26). Results have not undergone formal peer review.

This guide helps you get the project running quickly for development or reproduction.

## Quick Setup (5 minutes)

### 1. Install Python Dependencies

```bash
# Create virtual environment
python -m venv venv

# Activate (Unix/Mac)
source venv/bin/activate

# Activate (Windows PowerShell)
venv\Scripts\activate

# Install requirements
pip install torch numpy matplotlib jupyter tqdm pandas scipy
```

### 2. Open Main Notebook

```bash
cd Code
jupyter notebook DCTF_final.ipynb
```

### 3. Run Experiments

**Option A: Quick Test (10 minutes)**

- Modify in notebook: `epochs = 5` (instead of 100)
- Modify: `num_episodes = 500` (instead of 5000)
- Run all cells
- Note: Results will differ from paper

**Option B: Full Reproduction (18-24 hours on GPU)**

- Keep default settings
- Run all cells
- Results saved to `exports/`
- Should match published results

## Project Structure

```
Impl_TF_Project/
├── Code/
│   └── DCTF_final.ipynb          # Start here
├── Additional/
│   ├── Plot_visualizations.ipynb # Architecture diagrams
│   └── wavelength.ipynb          # RoPE analysis
├── Reports/
│   └── *.pdf                     # Final papers (preprint)
└── README.md                     # Full documentation
```

## Common Tasks

### Generate Figures Only

```python
# In DCTF_final.ipynb, after training
# Jump to Section 12 (Figure Generation)
# Run those cells to regenerate all figures
```

### Run Single Model

```python
# In DCTF_final.ipynb
models_to_train = ['rope']  # Instead of ['baseline', 'sinus', 'rope']
# Speeds up testing
```

### Change Grid Sizes

```python
# In evaluation section
test_grid_sizes = [8, 10, 12]  # Instead of [8, 10, 12, 15, 20]
# Faster evaluation
```

### Generate Architecture Diagrams

```bash
cd Additional
jupyter notebook Plot_visualizations.ipynb
# Run cells to generate architecture_comparison.pdf and key_door_env.pdf
```

## Troubleshooting

### "CUDA out of memory"

```python
batch_size = 32  # Reduce from 64
# Or reduce context_length = 20  # from 30
```

### "Module not found"

```bash
pip install <missing-module>
# Common: pip install tqdm pandas scipy
```

### "Notebook kernel died"

- Reduce batch size or context length
- Check available RAM/VRAM with `nvidia-smi` (GPU) or Task Manager
- Restart kernel and try again

### "Results don't match paper"

- Ensure using same random seeds (42, 123, 456)
- Verify 100 epochs trained
- Check evaluation on 100 episodes per grid size
- Minor variations normal due to hardware differences

## Next Steps

1. Read main [README.md](README.md) for full documentation
2. Check [Code/README.md](Code/README.md) for notebook details
3. See [Additional/README.md](Additional/README.md) for visualizations

## Key Results to Expect

After running full experiments (preprint):

| Grid  | Baseline | Sinus | RoPE |
| ----- | -------- | ----- | ---- |
| 8×8   | 100%     | 100%  | 100% |
| 10×10 | 64%      | 40%   | 86%  |
| 12×12 | 18%      | 2%    | 43%  |

If your results differ significantly, check:

- ✅ Random seeds set correctly (42, 123, 456)
- ✅ Full 100 epochs trained
- ✅ Evaluation on 100 episodes
- ✅ Using same PyTorch version (~1.13)

## Minimal vs Full Installation

**Minimal** (just to run code):

```bash
pip install torch numpy matplotlib jupyter
```

**Full** (for all features):

```bash
pip install -r requirements.txt
```

## Directory Navigation

```bash
# Main experiments
cd Code
jupyter notebook DCTF_final.ipynb

# Visualizations
cd Additional
jupyter notebook Plot_visualizations.ipynb

# View results
cd Code/exports
cat length_gen_dict.json | python -m json.tool

# View figures
cd Code/paper_figures
# Open PDFs in your PDF reader
```

## Data Files

Results automatically saved to:

- `Code/exports/*.json` - All experimental data
- `Code/paper_figures/*.pdf` - Publication figures
- `Code/paper_figures/data/*.json` - Statistical summaries

## Reproducibility Notes

### Hardware Variations

- Different GPUs may produce slightly different results
- CPU vs GPU results may differ slightly
- Numerical differences are normal (<1%)

## Quick Validation

After installation, verify setup:

```python
import torch
import numpy as np
import matplotlib.pyplot as plt

print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
print(f"NumPy version: {np.__version__}")
print("✓ All dependencies working!")
```

## Important Reminders

⚠️ **Preprint Status**: This is preliminary research not yet peer-reviewed

📊 **Results**: Findings are from course project, not validated externally

🔬 **Methods**: Implementation follows established papers but may have variations

---

**Need help?**

1. Check [README.md](README.md) for detailed docs
2. Review [Code/README.md](Code/README.md) for technical details
3. Open an issue with your question

---

**Status**: Non-peer-reviewed preprint  
**Last Updated**: February 11, 2026  
**Maintainer**: Lasse von Danwitz
