# Task 3 - Real-Time Echocardiogram Video Analysis

**Goal:** enhance a noisy, low-contrast ultrasound video frame-by-frame and show it live next to the raw feed.

**Data:** `data/echo.mp4` - one short clip from
[Stanford EchoNet-Dynamic](https://www.kaggle.com/datasets/manojkumarcs28/echonet-dynamic-by-stanford-university).

## Per-frame pipeline

1. Grayscale → histogram equalization (fix murky contrast)
2. `COLORMAP_JET` (highlight intensity / blood-flow regions)
3. Gray-world colour balance
4. Log transform (reveal the dark heart chambers)
5. Power-law, γ = 1.5 (suppress bright backscatter noise)

The raw and enhanced frames are joined with `np.hstack` and shown with `cv2.imshow`.

## How to run

1. Must be run **locally** - `cv2.imshow` opens a desktop window, which does not work in Google Colab.
2. From this folder:
   ```bash
   jupyter notebook realtime_echo.ipynb
   ```
3. Run all cells. A window titled *"echocardiogram - raw | enhanced"* opens and the clip loops.
4. Click the video window and press **q** to stop.

Snapshots of the side-by-side view (`frame_*.png`), a pipeline-stages figure and the full
side-by-side video (`side_by_side.mp4`) are saved to `output/`.
