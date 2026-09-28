# Task 2 - Multi-Modal Cardiac Image Fusion

**Goal:** combine a CT slice (sharp structural boundaries) and the matching MRI slice
(soft-tissue detail) into one diagnostic image.

**Data:** from the [Heart CT & MRI dataset](https://www.kaggle.com/datasets/ziya07/heart-ct-and-mri-dataset):
- `data/ct.png`, `data/mri.png`: a matched pair (`synthetic_slices/Patient_001`, same slice number).
  This dataset is **simulated**: every slice is a flat two-tone circle.
- `data/ct_real.png`: real chest CT slice `0049` from the dataset's `DICOM TO PNG/` folder, used in the
  bonus section. That folder has no MRI, so an MRI-style image is **simulated** from the same slice
  (soft tissue bright, bone dark, blurred, with a brightness drift and noise). It is **not a real MRI**.

## Pipeline

1. Load both slices as grayscale; resize the MRI to the CT size.
2. Histogram-equalize each modality independently.
3. Colour map: CT → `COLORMAP_BONE`, MRI → `COLORMAP_JET`.
4. Weighted fusion: `cv2.addWeighted(ct_color, 0.7, mri_color, 0.3, 0)`.
5. Log transform, then gamma (γ = 1.3) on the fused image to avoid crushed blacks / blown whites.
6. Side-by-side CT | MRI | fused, with entropy, std dev and edge-strength metrics.

## Fusion weighting logic

- **CT gets α = 0.7** - CT carries the sharp anatomical edges (heart wall, vessels, bone). Giving it
  the heavier weight keeps those edges crisp in the fused result.
- **MRI gets β = 0.3** - enough for its soft-tissue variations to show up as colour, but not so much
  that the saturated JET colours wash out the CT structure.
- An α sweep (0.5 → 0.8) is shown in the notebook: 0.5 lets the MRI colours dominate, 0.8 almost hides
  the MRI; 0.7 was the best balance.

## Analysis

| | CT | MRI | fused |
|---|---|---|---|
| **synthetic pair:** entropy (bits) | 0.84 | 0.56 | **1.11** |
| **synthetic pair:** edge strength | 15.0 | 10.4 | 13.4 |
| **real CT bonus:** entropy (bits) | 2.15 | 2.60 (simulated) | 2.55 |
| **real CT bonus:** std dev (contrast) | 66.2 | 66.2 | **80.3** |
| **real CT bonus:** edge strength | 40.3 | 29.2 | 29.2 |

- **Synthetic pair:** the fused image carries more information than either input (higher entropy) and keeps
  about 89% of the CT's edge strength.
- **Real CT bonus:** the fused image has the highest contrast and nearly the MRI-style image's information.
  Bone boundaries remain the sharpest structures, but measured edge strength drops, because the blurred MRI
  layer softens the gradients.
- **Log + gamma:** no new blow-out. Values at 255 stay at 0.44% on the real CT. On the synthetic pair the
  brightest flat colour maps to 255, but no colours merge (still 3 distinct colours). Values at 0 are
  empty background (0 in both inputs), not crushed data. The trade-off: on the real CT, log merges some
  close bright shades (9,037 → 5,892 distinct colours).

Charts: `output/06_comparison.png`, `output/06_metrics_chart.png`, `output/bonus_04_comparison.png`.
