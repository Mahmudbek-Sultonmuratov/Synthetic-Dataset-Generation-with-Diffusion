# Synthetic-Dataset-Generation-with-Diffusion
computer vision project 220222 220211

# 🧬 Synthetic Dataset Generation with Diffusion

This project explores the use of text-to-image diffusion models to generate synthetic medical images for rare disease classification. We compare real-only training with synthetic-augmented pipelines using Stable Diffusion XL (SDXL).

## Datasets Used

This project uses publicly available medical imaging datasets licensed for academic research:

- **ChestX-ray14**  
  A dataset of 112,000 frontal-view chest X-ray images labeled across 14 disease categories, including rare conditions such as pneumothorax and fibrosis.  
  🔗 [Access ChestX-ray14](https://nihcc.app.box.com/v/ChestXray-NIHCC)

- **HAM10000**  
  A dermatoscopic image dataset of 10,015 skin lesions spanning 7 diagnostic classes, including rare melanoma subtypes.  
  🔗 [Access HAM10000 on Kaggle](https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000)

- **MIMIC-CXR**  
  A large-scale dataset of chest radiographs paired with clinical reports, sourced from Beth Israel Deaconess Medical Center. Requires credentialed access and completion of a data use agreement.  
  🔗 [Access MIMIC-CXR via PhysioNet](https://physionet.org/content/mimic-cxr/2.0.0/)


## Environment Setup

We recommend using Python 3.10+ and setting up a virtual environment:

```bash
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt

pip install torch transformers diffusers datasets scikit-learn matplotlib tensorflow

# train.py

import torch
from torchvision import transforms
from torch.utils.data import DataLoader
from model import CNNClassifier  # define your model separately

def train():
    # TODO: Load dataset
    # TODO: Apply transforms
    # TODO: Initialize model, optimizer, loss
    # TODO: Train loop
    pass

if __name__ == "__main__":
    train()

# evaluate.py

def evaluate(model, test_loader):
    # TODO: Compute AUROC, F1-score, Accuracy
    pass


