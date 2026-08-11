# Data

This folder is a placeholder — the dataset itself isn't committed to the repo (it's ~150MB of MRI images).

## Get it

**Kaggle notebook:** Add Input → search "Brain Tumor MRI Dataset" (by masoudnickparvar). It'll be mounted at `/kaggle/input/brain-tumor-mri-dataset`.

**Locally:**
```bash
pip install kagglehub
python -c "import kagglehub; print(kagglehub.dataset_download('masoudnickparvar/brain-tumor-mri-dataset'))"
```
This prints the local path — point `DATA_DIR` in the notebook / `--data_dir` in `src/train.py` at it, or symlink it into `data/`.

## Expected layout

```
data/
├── Training/
│   ├── glioma/
│   ├── meningioma/
│   ├── notumor/
│   └── pituitary/
└── Testing/
    ├── glioma/
    ├── meningioma/
    ├── notumor/
    └── pituitary/
```
