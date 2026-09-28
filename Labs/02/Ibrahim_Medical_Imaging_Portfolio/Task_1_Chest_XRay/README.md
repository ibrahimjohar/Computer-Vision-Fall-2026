# Task 1 - Diagnostic Enhancement of Chest X-Rays

**Goal:** make underexposed chest x-rays readable before they are handed to doctors.

**Data:** `data/xray_1.png` - a dark/underexposed image from the
[COVID19+PNEUMONIA+NORMAL Chest X-Ray dataset](https://www.kaggle.com/datasets/sachinkumar413/covid-pneumonia-normal-chest-xray-images).

## Pipeline

1. **Load** the x-ray as a single-channel grayscale matrix.
2. **Histogram equalization** (`cv2.equalizeHist`) - redistributes intensities over 0-255; compared against the raw image and histograms.
3. **False colour** (`COLORMAP_JET`) - small gray differences become large hue differences, which makes fluid boundaries easier to see.
4. **Colour balance** - gray-world correction: each B/G/R channel is scaled so its mean equals the overall mean, removing the colour cast.
5. **Thresholding** - `cv2.threshold(eq, 200, 255, THRESH_BINARY)` keeps only the densest tissue (bone / dense fluid).
6. **Log transform** on the raw image - `s = c·log(1+r)` expands the dark regions and reveals the outer ribcage.
7. **Gamma (γ < 1)** on the raw image - `s = 255·(r/255)^γ` lifts midtones and softens bone contrast so lung tissue is clearer.

All intermediate and final images are saved to `output/`.

## Run

```bash
jupyter notebook xray_enhancement.ipynb
```
(run from inside this folder)
