Here is the clean README in pure Markdown so you can copy-paste directly into README.md on GitHub.

# WGAN-based Data Reconstruction Attack (GLASS)

This repository contains the implementation and experiments for **WGAN-based Data Reconstruction Attack (GLASS)**, a GAN-driven method for reconstructing private data from **Split Inference (SI)** systems.

The project explores how intermediate representations (IR) shared between edge devices and cloud servers can leak sensitive information, and demonstrates how **Wasserstein GAN with Gradient Penalty (WGAN-GP)** can reconstruct private inputs from these representations.

This work was conducted at **Georgia State University** by **Anoop Lashyal and Vimeth Jayawardana**.

---

# Project Overview

Split Inference enables collaborative deep learning between **edge devices and cloud servers**, where part of the neural network runs locally and the rest runs remotely.

Although this improves efficiency and privacy, the intermediate features transmitted to the cloud can still leak sensitive information.

This project rebuilds the **GLASS (GAN-based Latent Space Search)** attack and improves it by replacing **StyleGAN with WGAN-GP**, enabling more stable training and high-quality reconstructions.

The attack reconstructs private images by searching the **latent space of a pre-trained generator** to find the image whose intermediate representation best matches the intercepted feature representation.

---

# Key Contributions

- Reimplemented the **GLASS attack framework** for data reconstruction.
- Replaced **StyleGAN with WGAN-GP** for improved training stability.
- Integrated **DISCO defense evaluation** to test reconstruction robustness.
- Evaluated reconstruction quality using **PSNR and LPIPS metrics**.
- Demonstrated that GAN-based attacks can recover private inputs even when defenses are applied.

---

# Attack Pipeline

The attack follows this workflow:

1. **Split Inference Setup**
   - A neural network is divided into a **client model** (edge) and **server model** (cloud).

2. **Intermediate Representation Leakage**
   - The client model produces feature representations `z_q` which are transmitted to the server.

3. **Latent Space Search**
   - The attacker searches the **latent space of a WGAN-GP generator** to find a latent vector that reproduces similar intermediate representations.

4. **Image Reconstruction**
   - The generator produces an image corresponding to the optimized latent vector.

5. **Evaluation**
   - Reconstruction quality is measured using:
     - PSNR
     - LPIPS
     - SSIM
     - MSE

---

# Dataset

The experiments were conducted using the **CIFAR-10 dataset**.

Dataset characteristics:

- 60,000 images
- 10 classes
- 32 × 32 RGB images
- 6,000 images per class

---

# Model Architecture

## Split Inference Classifier

A CNN classifier was trained on CIFAR-10 and split into:

### Client Model
- Two convolution layers
- ReLU activation
- Outputs intermediate representation `z_q`

### Server Model
- Fully connected layer
- Produces final classification label

The classifier was trained for **200 epochs**.

---

# Loss Functions

The attack optimizes two losses.

### Reconstruction Loss

Measures similarity between the original image and the reconstructed image.

L_recon = ||x − x_hat||^2

### WGAN Loss

Encourages the generator to produce realistic images.

L_WGAN = E[D(x)] − E[D(x_hat)]

### Total Loss

L_total = λ1 * L_recon + λ2 * L_WGAN

Balancing these losses allows the model to generate **realistic images while matching the intermediate representation**.

---

# Defense Evaluation

The attack was evaluated against **DISCO (Dynamic and Invariant Sensitive Channel Obfuscation)**.

DISCO attempts to prevent reconstruction by **obfuscating sensitive channels in the intermediate representations**.

The experiments show that the GAN-based GLASS attack can still achieve strong reconstruction quality under this defense.

---

# Results

### Quantitative Improvements

- **PSNR increases by ~6.4 dB**
- **LPIPS decreases by ~0.49**

This indicates that reconstructed images become **closer to the original and perceptually sharper**.

### Observations

- GLASS performs particularly well at **deep split points**, where other reconstruction attacks struggle.
- GAN-based latent search improves **stability and reconstruction quality** compared to traditional optimization-based methods.

---

# Installation

Clone the repository:

~~~sh
git clone https://github.com/yourusername/GLASS-WGAN.git
cd GLASS-WGAN

Install dependencies:

pip install -r requirements.txt
~~~

⸻

Running the Project

Train the WGAN generator:

python train_wgan.py

Run the reconstruction attack:

python run_attack.py

Evaluate results:

python evaluate_attack.py


⸻

Future Work

Possible research directions include:
	•	Extending attacks to larger datasets (ImageNet, medical datasets)
	•	Evaluating reconstruction under stronger privacy defenses
	•	Integrating VQ-GAN or diffusion models for improved reconstruction quality
	•	Testing attacks in real-world edge-cloud deployment scenarios

⸻

Authors

Anoop Lashyal
Georgia State University

Vimeth Jayawardana
Georgia State University

⸻

References
	1.	Gupta, O., & Raskar, R. — Distributed Learning of Deep Neural Networks Over Multiple Agents
	2.	Li, Z. et al. — GAN You See Me? Enhanced Data Reconstruction Attacks Against Split Inference
	3.	Singh, A. et al. — DISCO: Dynamic and Invariant Sensitive Channel Obfuscation
