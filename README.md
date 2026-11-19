# FireSense

**Multispectral wildfire detection with channel attention and probabilistic calibration.**

A segmentation model for wildfire detection in Landsat-8 imagery. The point of the project was not
only to find fire, but to produce confidence scores an operational team could actually act on — a
model that is 90% confident should be right about 90% of the time.

Full write-up: **[basanthyajman.com/publication/firesense-fire-detection](https://basanthyajman.com/publication/firesense-fire-detection)**

## Results

| Metric | Value |
|---|---|
| Mean IoU | 69.6% |
| Improvement over classical baselines | +45.6% |
| Expected Calibration Error reduction | 86.1% |

The calibration number is the one I care about most. The baseline model was confident and wrong;
temperature scaling cut the gap between stated and actual confidence by 86.1%.

## Approach

- **Architecture** — ResNet34-UNet with Convolutional Block Attention Modules (CBAM) applied across
  all 10 Landsat-8 spectral bands, so the model learns which bands matter rather than being told.
- **Soft labels** — fire boundaries are genuinely ambiguous, and annotators disagree. Rather than
  forcing a hard edge, labels come from multi-annotator consensus, which preserves that uncertainty.
- **Calibration** — temperature scaling on a held-out split, evaluated with Expected Calibration
  Error and reliability diagrams, not accuracy alone.
- **Explainability** — Grad-CAM over the attention maps to check the model fires on burn scars
  rather than on clouds or water.

## Repository layout

The work runs in five phases, each a self-contained notebook:

| Notebook | What it covers |
|---|---|
| `Phase1_Dataset_and_EDA.ipynb` | Landsat-8 data, band statistics, class imbalance |
| `Phase2_Baseline_and_Modules.ipynb` | ResNet34-UNet baseline and module design |
| `Phase3_Soft_Labels_and_Attention.ipynb` | CBAM, multi-annotator soft labels, hyperparameter sweep |
| `Phase4_Calibration_and_Evaluation.ipynb` | Temperature scaling, ECE, ablations, channel importance |
| `Phase5_Final_Integration_and_XAI.ipynb` | Integration and Grad-CAM explainability |

Figures in the repository root (`phase4_calibration.png`, `phase4_ablation_final.png`,
`phase4_channel_importance.png`, `gradcam_*.png`) are the outputs referenced in the report.
`Group4_Project_Report.pdf` is the full write-up.

## Context

Graduate research for DATA 255 at San Jose State University, 2025. It fed directly into my later
wildfire work as a Graduate Research Assistant at SJSU Research Foundation and as a research intern
on the NASA Ames FireSage program.
