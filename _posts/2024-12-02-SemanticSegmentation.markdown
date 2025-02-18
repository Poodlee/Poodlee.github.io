---
layout: post
title: "Boostcamp - Semantic Segmentation"
description: "Developing a Deep Learning-Based Bone Segmentation Model for Hand X-ray Images"
categories: Boostcamp Semantic Segmentation
---

Bone segmentation is an essential task in medical imaging, playing a critical role in diagnostics and treatment planning. Despite advancements in deep learning, **accurate bone segmentation remains a challenging problem** due to issues such as **low contrast, overlapping structures, and variations in hand positioning**. This project aims to develop a **semantic segmentation model** capable of **segmenting 29 distinct bone classes** from hand X-ray images. 

<div class="btn-row">
  <a href="https://github.com/boostcampaitech7/level2-cv-semanticsegmentation-cv-21-lv3" target="_blank" class="btn">
    <img src="https://uxwing.com/wp-content/themes/uxwing/download/brands-and-social-media/github-icon.png" alt="GitHub" class="btn-icon"> GitHub
  </a>
  <a href="https://github.com/user-attachments/files/18844130/Semantic.segmentation.pdf" target="_blank" class="btn">
    <img src="https://uxwing.com/wp-content/themes/uxwing/download/file-and-folder-type/powerpoint-icon.png" alt="PPT" class="btn-icon"> PPT
  </a>
  <a href="https://github.com/user-attachments/files/18844135/Semantic.Segmentation.Wrap.up.report.pdf" target="_blank" class="btn">
    <img src="https://cdn-icons-png.flaticon.com/256/80/80942.png" alt="Report" class="btn-icon"> Report
  </a>
</div>


## Abstract
This project develops a **deep learning-based segmentation model** to accurately **classify and separate hand bones** in X-ray images. Given the complexity of hand bone segmentation, the model must handle **multi-label classification**, **overlapping structures**, and **low-contrast images**. The key objectives are:
- **Developing an effective segmentation pipeline** using **UNet++ and FPN models**.
- **Enhancing small object segmentation** by **resizing images** and **applying targeted augmentations**.
- **Improving edge detection** for overlapping bones through **contrast enhancement** techniques.
- **Optimizing model generalization** using **advanced learning rate scheduling and data augmentation**.

---

## Features

### 1. Exploratory Data Analysis (EDA)
Analyzing the dataset revealed key challenges in segmentation:
- **Size Disparities Among Bone Classes**: Some bone classes, such as **Finger1, Finger4, and Trapezoid**, occupy less than **0.06% of the total image area**, making them harder to detect.
- **Variability in Wrist Angle**: Hand orientation varies significantly, requiring **rotation augmentation** for robustness.
- **Low Contrast Between Bones and Surrounding Tissue**: The **brightness difference between bones and soft tissues is minimal**, making edge detection difficult.
- **Overlapping Bones**: The **Pisiform, Triquetrum, Trapezoid, and Trapezium** bones often overlap, causing **multi-label classification issues**.

---

## Model Selection & Training

| Model       | Library | Encoder      | Epochs | Public Dice Score |
|------------|---------|-------------|--------|--------------------|
| **UNet++** | SMP     | ResNet-152   | 150    | **0.953**         |
| **FPN**    | SMP     | ResNet-152   | 150    | 0.9491            |
| **DeepLabV3+** | SMP | ResNet-152  | 150    | 0.945             |
| **Attention UNet** | - | -          | 40     | 0.9393            |

- **UNet++ was chosen as the final model** due to its **dense skip connections**, which improve feature extraction for **small and complex structures**.
- **FPN was used as a secondary model** for its **pyramid structure**, which enhances multi-scale feature extraction.

---

## Hyperparameter Tuning

### 1. Loss Function Selection

| Loss Function | Public Dice Score |
|--------------|-------------------|
| BCE Loss     | 0.9197            |
| **Dice Loss** | **0.941**         |
| Combo Loss (Dice: 0.6, BCE: 0.4) | 0.9402 |

- **Dice Loss was selected**, as it improved segmentation performance by **better handling class imbalance**.

### 2. Learning Rate Scheduling

| Model   | Scheduler                     | Public Dice Score |
|--------|--------------------------------|-------------------|
| FPN    | None (Flat LR 0.0001)         | 0.9491            |
| FPN    | Warm-up + Cosine Annealing    | **0.9497**        |
| UNet++ | None (Flat LR 0.0001)         | 0.953             |
| UNet++ | Warm-up + Cosine Annealing    | **0.9533**        |

- **Cosine Annealing + Warm-up** helped the model **escape saddle points and stabilize early training**.

### 3. Optimizer Selection

| Optimizer | Public Dice Score |
|-----------|-------------------|
| SGD (Momentum) | -             |
| Adam         | 0.9515          |
| **AdamW**    | **0.9538**      |
| Lion         | 0.9513          |

- **AdamW was chosen**, as it improved **generalization performance** while reducing **overfitting**.

---

## Additional Commentary
Throughout this project, I encountered and overcame several challenges:
1. **Understanding Model Behaviors**:
   - **Studied deep learning models** beyond just training.
   - **Explored encoder/decoder architectures** in detail.

2. **Challenges & Key Takeaways**:
   - **Lack of GPU memory limited large-scale model experimentation**.
   - **TTA did not improve results due to training/testing distribution similarities**.
   - **Data-Centric approaches, such as annotation refinement, yielded mixed results**.

3. **Future Directions**:
   - **Investigate meta-learning for segmentation tasks**.
   - **Implement dynamic loss weighting strategies for better class balance**.
   - **Explore transformer-based segmentation models for enhanced performance**.