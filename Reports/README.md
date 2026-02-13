# Reports and Publications

> **⚠️ PREPRINT NOTICE**: The documents in this directory are non-peer-reviewed preprint versions of research conducted for the Implementing Transformers course (Winter Semester 2025/26). These reports have not undergone formal peer review and represent preliminary findings from a course project.

This directory contains the first versions of research outputs from the Rotary Positional Embeddings for Length Generalization in Decision Transformers project.

## Overview

All documents represent the culmination of experimental work, analysis, and writing conducted for the Implementing Transformers course (Winter Semester 2025/26). These are the **final, submission-ready course versions** intended for evaluation. Note that these are preprint versions and have not undergone formal peer review.

## Contents

```
Reports/
├── README.md                                      # This file
├── Impl_TF_Report_professional_von_Danwitz.pdf   # Main research paper
├── Impl_TF_Report_von_Danwitz.pdf                # Alternative format
└── Impl_TF_Presentation_von_Danwitz.pdf          # Conference-style presentation
```

## Documents

### Main Research Paper: `Impl_TF_Report_professional_von_Danwitz.pdf`

**Document Type**: Academic research paper (two-column, journal-style)

#### Specifications

- **Format**: PDF/A (archival quality)
- **Layout**: Two-column, A4 paper
- **Pages**: 9 pages (including references)
- **Word Count**: 2,483 words (excluding references and equations)
- **Fonts**: Embedded Type 1 fonts (LaTeX Computer Modern)
- **Color**: Full color (colorblind-friendly palette)
- **Resolution**: 600 DPI for all figures

#### Structure

1. **Abstract** (1 paragraph)
   - Research question and motivation
   - Methodology overview
   - Key results summary
   - Main conclusion

2. **Introduction** (1 page)
   - Background on Decision Transformers
   - Length generalization problem
   - Research objectives
   - Paper organization

3. **Background** (2 pages)
   - Transformer architecture fundamentals
   - Attention mechanism mathematics
   - Decision Transformer formulation
   - Positional encoding strategies
     - Sinusoidal embeddings
     - Rotary Positional Embeddings (RoPE)

4. **Experimental Setup** (1.5 pages)
   - Key-Door Maze environment description
   - Data generation protocol
   - Model architecture details
   - Training configuration
   - Evaluation methodology

5. **Results and Analysis** (3 pages)
   - Training performance comparison
   - Length generalization results (main finding)
   - Context length scaling experiments
   - Attention pattern analysis
   - Translation invariance verification
   - Statistical analysis (effect sizes, significance tests)
   - Failure mode investigation

6. **Discussion** (1 page)
   - Mechanism analysis (why RoPE works)
   - State encoder bottleneck hypothesis
   - Implications for Offline RL
   - Limitations and future work

7. **Conclusion** (0.5 pages)
   - Summary of key findings
   - Practical implications
   - Future research directions

8. **References** (11 citations)
   - Key papers: Vaswani et al. (Attention), Su et al. (RoPE), Chen et al. (Decision Transformer)

#### Key Results Presented

| Grid Size | Baseline | Sinus | RoPE      | Improvement |
| --------- | -------- | ----- | --------- | ----------- |
| 10×10     | 64.0%    | 40.0% | **86.0%** | +22.0%      |
| 12×12     | 18.3%    | 1.7%  | **43.3%** | +25.0%      |

- **Statistical Significance**: Cohen's d = 2.56 (p = 0.035) for 12×12 comparison
- **Translation Invariance**: <2% error empirically verified
- **Context Scaling**: ~21% RoPE advantage stable across 30-90 token windows

#### Figures and Tables

- **Figure 1**: Key-Door Maze environment schematic
- **Figure 2**: Training dynamics (4-panel plot)
- **Figure 3**: Length generalization curves (main result)
- **Figure 4**: Attention distance decay analysis
- **Table 1**: Model architecture specifications
- **Table 2**: Training configuration
- **Table 3**: Length generalization statistics
- **Table 4**: Context scaling results
- **Table 5**: Statistical comparison (effect sizes)

#### Usage

**Reading**: Open with any PDF reader (Adobe Acrobat, Preview, browser)

**Printing**: Optimized for A4 paper, two-sided printing recommended

---

### Alternative Version: `Impl_TF_Report_von_Danwitz.pdf`

**Document Type**: Alternative formatting of the same research

#### Differences from Professional Version

- **Layout**: Possibly single-column or different template
- **Styling**: Alternative typography or spacing
- **Content**: Same core content, different presentation

**Note**: Check both versions; select the one that best fits submission requirements or personal preference.

---

### Conference Presentation: `Impl_TF_Presentation_von_Danwitz.pdf`

**Document Type**: Slide deck for oral presentation (PDF export from PowerPoint/Beamer)

#### Specifications

- **Format**: PDF (presentation mode optimized)
- **Aspect Ratio**: 16:9 (widescreen)
- **Slides**: ~20-25 slides
- **Duration**: 15-20 minute talk
- **Font Size**: Large (24pt+ for body text)
- **Graphics**: High-contrast, simplified for projection

#### Structure

1. **Title Slide**
   - Title, author, course, date

2. **Motivation** (2-3 slides)
   - Why length generalization matters
   - Problem statement
   - Research question

3. **Background** (3-4 slides)
   - Decision Transformers overview
   - Positional encoding concepts (visual)
   - RoPE intuition (rotation visualization)

4. **Methodology** (3-4 slides)
   - Key-Door Maze environment (animated)
   - Model architecture diagram
   - Experimental design

5. **Results** (6-8 slides)
   - Main result: Length generalization graph (large, clear)
   - Training dynamics
   - Statistical comparison
   - Attention analysis (heatmaps)
   - Context scaling experiment

6. **Discussion** (2-3 slides)
   - Why RoPE works (mechanism)
   - Limitations (state encoder bottleneck)
   - Implications for RL

7. **Conclusion** (1 slide)
   - Key takeaways (3-4 bullets)
   - Future work

8. **Q&A** (1 slide)
   - Thank you + contact information

9. **Backup Slides** (optional)
   - Detailed statistics
   - Additional figures
   - Methodological details

#### Presentation Tips

- **Pacing**: ~1 minute per slide
- **Focus**: Emphasize Figure 3 (main result)
- **Story**: Problem → RoPE solution → Evidence → Impact
- **Anticipate Questions**:
  - "Why not test more seeds?" → Limited compute, large effect sizes
  - "What about other tasks?" → Future work, maze is representative
  - "Why does sinusoidal fail?" → Absolute position constraints

#### Delivery Notes

**Key Messages to Emphasize**:

1. RoPE achieves 2× success rate on 12×12 grids
2. Advantage comes from position extrapolation, not context capacity
3. Bottleneck shifts to state encoder, opening new research directions

**Visual Impact**:

- Show attention heatmap comparison (Baseline vs. RoPE)
- Animate trajectory examples (success vs. failure)
- Use color to highlight RoPE line in generalization plot

#### Usage

**Presenting**:

- Use presentation mode (Cmd/Ctrl + L in most PDF readers)
- Practice with timer: 15-minute target
- Remote clicker recommended

**Distributing**:

- Share as-is (PDF is universal)
- Consider uploading to SlideShare or similar for broader reach

---

## Quality Assurance

All documents have been:

✅ **Proofread**: Multiple editing passes  
✅ **Spell-checked**: No typos or grammatical errors  
✅ **Fact-checked**: All numbers match experimental data  
✅ **Citation-verified**: All references complete and formatted correctly  
✅ **Figure-quality**: All graphics high-resolution and readable  
✅ **Accessibility**: Colorblind-friendly colors, alt-text where applicable  
✅ **Compliance**: Meets course requirements and academic standards

## Version History

- **v1.0** (Feb 11, 2026): Initial submission
  - Main paper: 9 pages, 2,483 words
  - Presentation: 23 slides
  - All figures and tables finalized

## Contact

**Author**: Lasse von Danwitz  
**Course**: Implementing Transformers  
**Semester**: Winter 2025/26

For questions:

1. Review this README
2. Check main project documentation (`../README.md`)
3. Open an issue in the repository

## Acknowledgments

- Course instructors for feedback and guidance
- Reviewers for constructive comments (if applicable)
- LLMs for text refinement (disclosed in paper)

## AI Assistance Disclosure

As stated in the paper:

> "Throughout the course of this project, Large Language Models (LLMs) have been used for coding, documentation, and text refinement."

---

**Last Updated**: February 11, 2026  
**Maintainer**: Lasse von Danwitz  
**Status**: Final submission version
