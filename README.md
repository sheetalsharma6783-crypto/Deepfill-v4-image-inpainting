---

## Model Architecture

The model follows a two-stage coarse-to-fine architecture inspired by DeepFill.

### Coarse Generator
Generates an initial reconstruction of the occluded region using gated convolutions.

### Refinement Generator
Further refines the coarse reconstruction to improve structural consistency and visual quality.

### PatchGAN Discriminator
A PatchGAN discriminator is used to encourage realistic textures and image details through adversarial training.

The implementation also includes a contextual attention module as part of the refinement architecture.

---

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
