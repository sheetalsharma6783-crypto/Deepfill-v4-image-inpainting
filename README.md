# DeepFill-v4 Image Inpainting

A DeepFill-inspired deep learning approach for reconstructing occluded regions in drone imagery using a two-stage coarse-to-fine image inpainting framework.

The project focuses on restoring missing or occluded regions in aerial images while preserving the visual structure, texture, and content of the surrounding scene.

---

## Overview

Image inpainting is the process of reconstructing missing or damaged regions of an image using information from the surrounding context.

In aerial and drone imagery, objects or regions can become occluded or unavailable due to obstacles, image corruption, or artificially introduced missing regions. This project explores a deep learning-based approach to reconstruct these regions automatically.

The implemented model follows a **two-stage coarse-to-fine architecture inspired by DeepFill**, where:

1. A **Coarse Generator** produces an initial reconstruction.
2. A **Refinement Generator** improves the coarse reconstruction.
3. A **PatchGAN Discriminator** encourages the generated image to appear realistic.
4. Multiple losses are combined to improve reconstruction quality.

---

## Dataset

This project uses the **VisDrone2019-DET** dataset for training and evaluating the image inpainting model.

VisDrone is a large-scale drone-based aerial imagery dataset containing images captured from various scenes along with corresponding object annotations.

The object bounding-box annotations are used to generate artificial occlusion masks for the image inpainting task.

### Dataset Processing

The preprocessing pipeline includes:

- Loading aerial images from the VisDrone dataset
- Reading object bounding-box annotations
- Generating binary occlusion masks
- Applying masks to the corresponding image regions
- Resizing images to **256 × 256**
- Converting images into tensors
- Normalizing image values for model training

The processed dataset contains approximately **6,253 training images with corresponding annotations**.

> The complete VisDrone dataset is not included in this repository because of its size. Please download the dataset separately and configure the dataset paths in the notebook.

---

## Problem Statement

Given a drone image with artificially occluded regions, the objective is to reconstruct the missing content while maintaining:

- Structural consistency
- Visual similarity to the original image
- Realistic textures
- Smooth transitions between reconstructed and non-occluded regions

The model learns to generate a reconstructed image from the masked input and compares the generated output with the original ground-truth image.

---

## Methodology

The overall image reconstruction pipeline is:

```text
Original Drone Image
        │
        ▼
Bounding Box Annotations
        │
        ▼
Generate Occlusion Mask
        │
        ▼
Masked Image
        │
        ▼
┌────────────────────────┐
│    Coarse Generator    │
│    Gated Convolutions  │
└────────────────────────┘
        │
        ▼
Coarse Reconstruction
        │
        ▼
┌────────────────────────┐
│  Refinement Generator  │
│    Gated Convolutions  │
└────────────────────────┘
        │
        ▼
Refined Reconstruction
        │
        ├──────────────► PatchGAN Discriminator
        │
        ▼
Final Inpainted Image

## Loss Functions

The model uses a combination of multiple losses to improve reconstruction quality:

- **L1 Reconstruction Loss** – encourages pixel-level similarity with the ground-truth image.
- **Adversarial Loss** – encourages realistic image generation using the PatchGAN discriminator.
- **Perceptual Loss** – improves perceptual similarity between reconstructed and ground-truth images.
- **Total Variation Loss** – promotes smoothness and reduces artifacts.

---

## Evaluation Metrics

The reconstructed images are evaluated using:

| Metric | Purpose | Better Value |
|--------|---------|--------------|
| **PSNR** | Measures pixel-level reconstruction quality | Higher |
| **SSIM** | Measures structural similarity | Higher |
| **LPIPS** | Measures perceptual similarity | Lower |

### Example Result

| Metric | Value |
|--------|------:|
| PSNR | **32.52 dB** |
| SSIM | **0.9497** |
| LPIPS | **0.0458** |

> These values represent an example reconstruction result and are not test-set averages unless calculated across the complete evaluation dataset.

---

## Reconstruction Results

Example outputs of the image inpainting model are provided in the `results/` directory.

The results compare the **masked input, reconstructed output, and ground-truth image** to demonstrate the effectiveness of the proposed approach.

---
