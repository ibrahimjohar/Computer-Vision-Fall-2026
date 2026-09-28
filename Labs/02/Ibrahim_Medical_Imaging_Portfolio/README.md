# Medical Imaging Portfolio - CV Lab 02

**23K-0074 - Ibrahim**

Classical OpenCV enhancement techniques (histogram equalization, false-colour mapping,
colour balance, thresholding, log and power-law transforms, weighted fusion) applied to
three medical imaging problems.

| task | folder | notebook | data |
|---|---|---|---|
| 1. chest x-ray enhancement | `Task_1_Chest_XRay/` | `xray_enhancement.ipynb` | COVID19+PNEUMONIA+NORMAL Chest X-Ray (Kaggle) |
| 2. CT + MRI cardiac fusion | `Task_2_Cardiac_Fusion/` | `modal_fusion.ipynb` | Heart CT & MRI Dataset (Kaggle) |
| 3. real-time echo video | `Task_3_Echo_Analysis/` | `realtime_echo.ipynb` | Stanford EchoNet-Dynamic (Kaggle) |

Only the few sample files each notebook uses are included in its `data/` folder
(the full datasets are 1GB+). Results are written to each task's `output/` folder.

## Setup

```bash
python -m venv venv
venv\Scripts\activate          # windows  (source venv/bin/activate on linux/mac)
pip install -r requirements.txt
```

## Running

Open a notebook **from inside its task folder** (all paths are relative, e.g.
`data/xray_1.png`) and run all cells:

```bash
cd Task_1_Chest_XRay
jupyter notebook xray_enhancement.ipynb
```

Task 3 uses `cv2.imshow`, so it must be run locally (not in Colab). Press `q` to close the video window.

## Dataset sources

- Task 1: https://www.kaggle.com/datasets/sachinkumar413/covid-pneumonia-normal-chest-xray-images
- Task 2: https://www.kaggle.com/datasets/ziya07/heart-ct-and-mri-dataset
- Task 3: https://www.kaggle.com/datasets/manojkumarcs28/echonet-dynamic-by-stanford-university
