---
layout: dongjun
title: "Boostcamp - Classification"
date: 2024-10-02 21:03:36 +0800
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


## Key Challenges & Learnings
During this project, a significant amount of time was spent understanding the structure of deep learning projects. As a result, we primarily used fundamental architectures such as **ResNet** and slightly advanced models, without exploring the latest state-of-the-art architectures. Towards the end, incorporating models like **ViT CLIP** demonstrated a substantial performance boost, highlighting the importance of leveraging modern architectures from the beginning.

Additionally, during augmentation, **offline augmentation** was applied naïvely, leading to nearly identical images being present in both the training and validation sets. This resulted in validation accuracy failing to serve as an accurate performance metric. A key realization was that when performing **offline augmentation**, it is crucial to ensure that augmented images remain exclusive to either the training or validation set. Alternatively, adopting **online augmentation** would be a more appropriate approach to prevent information leakage.

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
  - Ubuntu 20.04 / Python 3.10  
  - PyTorch Lightning framework  
  - CUDA-compatible GPU: **4 × Tesla V100 (32GB)**

- **Data Setup**  
  - Original sketch dataset: ~30 images per class (ImageNet)
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

## Future Plans
- Investigate more advanced **overfitting countermeasures** (e.g., better data splitting, refined augmentation).  
- For subsequent projects (e.g., **Object Detection**), plan to **manually tweak and implement** core layers rather than purely relying on SOTA models.  
- Aim to achieve real-time detection (~30fps) by balancing network complexity and performance.
