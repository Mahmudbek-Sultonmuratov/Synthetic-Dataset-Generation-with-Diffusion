# Synthetic-Dataset-Generation-with-Diffusion  
**Computer Vision Project — 220222, 220211**

**Synthetic Dataset Generation with Diffusion**

This project investigates **synthetic medical image generation** using a **latent-space diffusion model** trained on chest X-ray data.  
Instead of generating images directly in pixel space, we employ a **two-stage pipeline** consisting of:

- a **Variational Autoencoder (VAE)**  
- followed by a **conditional Denoising Diffusion Probabilistic Model (DDPM)** operating in latent space.

The primary objective is to generate **disease-conditioned synthetic chest X-ray images** that preserve anatomical structure and pathological patterns, enabling **safer data sharing** and **dataset augmentation** for medical imaging research.

---

## Dataset Used

This project uses a publicly available medical imaging dataset licensed for academic research:

### **ChestX-ray14 (NIH)**

- A large-scale dataset containing **over 112,000 frontal-view chest X-ray images**
- Annotated with **15 thoracic disease labels** in a **multi-label setting**
- Diseases include:
  - Pneumonia
  - Effusion
  - Cardiomegaly
  - Edema
  - Pneumothorax
  - and others

🔗 https://nihcc.app.box.com/v/ChestXray-NIHCC

### Image Properties
- Grayscale images  
- Resolution: **256 × 256**  
- Multi-label annotations per image  

---

## Method Overview

We implement a **two-stage generative pipeline**:

### Variational Autoencoder (VAE)

- Compresses **256 × 256** chest X-rays into a **256-dimensional latent representation**
- Trained with:
  - Mean Squared Error (MSE) reconstruction loss
  - **LPIPS perceptual loss**
  - **β-weighted KL divergence**
- Uses **ResNet-style residual blocks** for:
  - Training stability
  - Improved structure preservation

---

### Conditional Diffusion Model (DDPM)

- Trained **entirely in latent space**
- Conditioned on disease labels (**one-hot vectors**)
- Learns to denoise latent vectors using the **DDPM framework**
- Enables **controlled generation**, e.g.:
  - *“Generate Pneumonia”*

---

### Decoding

- Sampled latent vectors are rescaled to match the VAE latent distribution
- The **VAE decoder** reconstructs realistic synthetic X-ray images

---

## Pipeline Diagram (Conceptual)

Real Chest X-ray
↓
VAE Encoder (ResNet-style)
↓
256-D Latent Space
↓
Conditional DDPM
↓
Sampled Latent
↓
VAE Decoder
↓
Synthetic Chest X-ray
---

##  Evaluation Strategy

Since traditional accuracy metrics are not meaningful for generative models, we rely on **qualitative and proxy evaluation metrics**, including:

- Pixel intensity statistics (min / max / mean / std)
- Edge density comparison (synthetic vs real)
- Structural Similarity Index (**SSIM**)
- Histogram **Wasserstein distance**
- Visual side-by-side comparison with real images

These measures help assess:
- Contrast realism
- Structural coherence
- Anatomical plausibility

---

## Key Challenges Encountered

Initial diffusion experiments produced **blurry or nearly blank images**, revealing that the **latent space quality** was the main bottleneck.

This was resolved by:

- Retraining the VAE with:
  - Residual connections
  - LPIPS perceptual loss
  - Improved KL regularization
- Re-extracting all latent representations
- Retraining the diffusion model on the improved latent space

This significantly improved **image sharpness, structure, and contrast**.

---

## Environment Setup

We recommend **Python 3.10+** and a virtual environment:

```bash
python -m venv venv
source venv/bin/activate   # Linux / macOS
# or
venv\Scripts\activate      # Windows

Install the required dependencies:

```bash
pip install torch torchvision numpy matplotlib pandas tqdm
pip install lpips scikit-image scipy

## Repository Structure
├── vae_training.ipynb
├── latent_extraction.ipynb
├── diffusion_training.ipynb
├── sampling_and_evaluation.ipynb
├── checkpoints/
│   ├── best_model.pt
│   └── ddpm_cond.pt
├── latents/
│   ├── latents_train.pt
│   └── latents_val.pt
├── results/
│   ├── synthetic_images/
│   └── evaluation_figures/
└── README.md

## Future Work

Higher-resolution latent diffusion

Text-based conditioning (clinical report → image)

Classifier-free guidance

Quantitative evaluation using downstream classifiers

Expert clinical validation

## Disclaimer

This project is intended for educational and research purposes only.
Generated images must not be used for medical diagnosis or clinical decision-making.



