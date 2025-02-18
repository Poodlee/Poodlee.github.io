---
layout:: post
title: "Boostcamp - Classification"
description: "Sketch Image Classification"
categories: ["Boostcamp", "Classification"]
---

Boostcamp AI Tech provided an opportunity to tackle a **sketch image classification** problem.  
Leveraging various **CNN and Transformer-based models**, we explored **Ensemble methods** and multiple **data augmentation** strategies to improve performance. 

<div class="btn-row">
  <a href="https://principled-nation-e2a.notion.site/Project-01-Sketch-Classification-4d0e8852a2c14601b638a091e2d28e39?pvs=4" target="_blank" class="btn">
    <img src="https://upload.wikimedia.org/wikipedia/commons/e/e9/Notion-logo.svg" alt="Notion" class="btn-icon"> Notion
  </a>
  <a href="https://github.com/boostcampaitech7/level1-imageclassification-cv-01" target="_blank" class="btn">
    <img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" alt="GitHub" class="btn-icon"> GitHub
  </a>
  <a href="https://github.com/user-attachments/files/18843546/Sketch.classification.pdf" target="_blank" class="btn">
    <img src="https://cdn-icons-png.flaticon.com/256/80/80942.png" alt="Report" class="btn-icon"> Report
  </a>
</div>

## Abstract
This project focuses on **sketch image classification** using PyTorch Lightning.  
Two primary goals were pursued:
1. Complete our first deep learning project with a solid **module-based structure**.  
2. Enhance performance via **ensembling multiple models**.

Our final solution integrates advanced CNN and Transformer architectures, including **ResNet**, **ResNeXt**, and **Swin Transformer**, as well as a combined **ViT_CLIP + Swin + ConvNeXtV2** ensemble.

---

## Features

1. **Model Variety**  
   - **ResNet, ResNeXt, Swin Transformer** (individual models)  
   - **Ensemble** combining **ViT_CLIP, Swin Transformer, ConvNeXtV2**  

2. **Extensive Augmentation**  
   - Canny, Morphology, Gaussian Blur, Motion Blur, RandAugment, AutoAugment  
   - Focused on boosting data representation for classes with very few samples

3. **Observations & Takeaways**  
   - Learned how to interpret open-source models, restructure modules, and implement ensemble methods.  
   - Identified significant overfitting issues due to limited data, even after augmentation.

---

## System Overview
The entire classification pipeline is managed using **PyTorch Lightning** for consistency and ease of experimentation.

- **Environment**  
  - Ubuntu / Python  
  - PyTorch Lightning framework  
  - CUDA-compatible GPU (e.g., Tesla V100 / RTX 3090)

- **Data Setup**  
  - Original sketch dataset: ~30 images per class  
  - Augmented dataset applying Canny, Morphology, Blur, etc. to expand coverage

- **Model Implementations**  
  1. **ResNet / ResNeXt**  
  2. **Swin Transformer**  
  3. **ViT_CLIP**  
  4. **ConvNeXtV2**  

- **Training Process**  
  - Train single models individually (ResNet, ResNeXt, Swin, etc.)  
  - Evaluate each model’s validation/test performance  
  - Combine selected models into **Ensemble** (average or weighted predictions)

---

## Results & Visualizations

### Single Model Results
<p align="center">
  <img src="https://github.com/user-attachments/assets/b258bb23-6b84-41c3-aa15-5f1edb3fdffb" alt="Single Model Performance" width="700"/>
</p>

### Ensemble (Swin Transformer + ResNeXt)
<p align="center">
  <img src="https://github.com/user-attachments/assets/e81a87ce-9a3d-4c48-a352-74c7eee7c838" alt="Ensemble 1" width="700"/>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/b2fd77bc-1d8c-4bb0-a004-468e3fadc51a" alt="Ensemble 2" width="700"/>
</p>

### Ensemble (ViT_CLIP + Swin Transformer + ConvNeXtV2)
<p align="center">
  <img src="https://github.com/user-attachments/assets/e81f2a92-6e42-4daa-b10d-a97e4a6ad3ce" alt="Ensemble 3" width="700"/>
</p>

#### Analysis
- Transitioning **from a single model to ensembles** yielded higher accuracy.  
- **Overfitting** remained an issue; validation accuracy was ~10% higher than test accuracy.  
- Stratified K-Fold CV and various augmentation methods did not fully resolve the discrepancy.

<p align="center">
  <img src="https://github.com/user-attachments/assets/2e6d6e2c-bf65-4ce9-8f58-bddc43ed75da" alt="Overfitting 1" width="500"/>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/3e458438-e66f-4811-b68a-72b57455374c" alt="Overfitting 2" width="500"/>
</p>

---

## Additional Commentary

### Procedure & Data Considerations
- **Offline Augmentation** drastically increased total images, potentially intensifying data overlap between train/validation sets and causing severe overfitting.  
- After removing near-duplicate images (via SSIM, Cosine Similarity), the dataset shrank from ~15,000 to 11,000 images.  
- K-Fold CV attempted to mitigate overfitting, but with limited success due to inherent data noise and label inaccuracies.

### Ensemble Model Highlights
- **ViT_CLIP**: Pre-trained on large datasets, offering robust performance.  
- **Swin Transformer**: Captures both local and global features with shifted windows.  
- **ConvNeXtV2**: A CNN architecture that incorporates Transformer-like concepts (Depthwise Convolution, LayerNorm), achieving strong performance in modern benchmarks.

### Future Plans
- Investigate more advanced **overfitting countermeasures** (e.g., better data splitting, refined augmentation).  
- For subsequent projects (e.g., **Object Detection**), plan to **manually tweak and implement** core layers rather than purely relying on SOTA models.  
- Aim to achieve real-time detection (~30fps) by balancing network complexity and performance.

