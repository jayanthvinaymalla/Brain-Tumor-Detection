# Brain Tumor MRI Classification

Fine-tunes a pretrained ResNet50 (ImageNet weights) on brain MRI scans to classify **glioma, meningioma, pituitary tumor, or no tumor**. Same shape as a typical fine-tuning project — load an existing model, adapt it with staged fine-tuning, evaluate, save, predict — applied to CNN transfer learning instead of an LLM.

## Repository Structure

```
.
├── notebooks/
│   └── Fine-Tune-Brain-Tumor-Detection.ipynb   # end-to-end pipeline
├── src/
│   ├── dataset.py     # ImageFolder loaders, transforms, class weights
│   ├── model.py        # ResNet50 / EfficientNet-B0 builder + staged unfreezing
│   ├── train.py         # two-phase fine-tuning loop, CLI entrypoint
│   ├── predict.py       # inference on a single image or a folder
│   └── utils.py          # seeding, metrics, plots, Grad-CAM
├── data/
│   └── README.md          # where to get the dataset, expected folder layout
├── requirements.txt
├── LICENSE
└── .gitignore
```

## Dataset

[Brain Tumor MRI Dataset](https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset) (Kaggle) — 4 classes, pre-split `Training/` / `Testing/`. See `data/README.md` for how to fetch it.

## Setup

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
```

## Train

Notebook (recommended — includes EDA, Grad-CAM, plots):
```bash
jupyter notebook notebooks/Fine-Tune-Brain-Tumor-Detection.ipynb
```

Or the standalone script:
```bash
python src/train.py --data_dir /path/to/brain-tumor-mri-dataset --architecture resnet50
```

## Predict

```bash
# single image
python src/predict.py --checkpoint outputs/brain_tumor_resnet50.pt --image scan.jpg

# folder -> CSV
python src/predict.py --checkpoint outputs/brain_tumor_resnet50.pt --folder scans/ --out preds.csv
```

## Approach

- **Backbone:** ResNet50 pretrained on ImageNet (swap to EfficientNet-B0 via `--architecture`).
- **Fine-tuning:** two phases — freeze the backbone and train only the new head first, then unfreeze the last residual block (`layer4`) and continue at a lower LR. Avoids wrecking pretrained features with large early gradients.
- **Class imbalance:** inverse-frequency class weights fed into the loss if the 4 classes aren't evenly represented.
- **Interpretability:** Grad-CAM overlays so you can sanity-check the model is attending to the tumor region, not scanner artifacts or the skull border.

## Notes

- This is a research/education pipeline, not a validated diagnostic tool. Real clinical deployment needs a much larger, clinically-sourced dataset, expert-verified labels, and regulatory review.

## License

See [LICENSE](LICENSE).
