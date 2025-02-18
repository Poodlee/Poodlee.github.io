---
layout: dongjun
title: "Boostcamp - Object Detection"
date:   2024-10-26 21:03:36 +0800
description: "Trash Image Object Detection"
categories: Boostcamp ObjectDetection
---

With the rapid increase in mass production and consumption, waste management has become a pressing global issue. Our project **aims to build an object detection model for classifying and detecting waste items**, contributing to more efficient recycling and waste sorting. This model can potentially be deployed in recycling centers for automated sorting or be used in educational applications for raising awareness about proper disposal methods.

<div class="btn-row">
  <a href="https://principled-nation-e2a.notion.site/Project-02-Object-Detection-110921078eca80e3acd1e064c39538eb?pvs=4" target="_blank" class="btn">
    <img src="https://upload.wikimedia.org/wikipedia/commons/e/e9/Notion-logo.svg" alt="Notion" class="btn-icon"> Notion
  </a>
  <a href="https://github.com/boostcampaitech7/level2-objectdetection-cv-01" target="_blank" class="btn">
    <img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" alt="GitHub" class="btn-icon"> GitHub
  </a>
  <a href="https://github.com/user-attachments/files/18843605/Object.Detection.pdf" target="_blank" class="btn">
    <img src="https://cdn-icons-png.flaticon.com/256/80/80942.png" alt="Report" class="btn-icon"> Report
  </a>
</div>

## Abstract
The **main goal of this project** is to develop an object detection model capable of accurately recognizing and classifying various types of waste in images. This project followed a **step-by-step approach**, involving:
- **Exploratory Data Analysis (EDA)** to understand image characteristics.
- **Preprocessing & Augmentation** to enhance dataset diversity.
- **Model Selection & Training** using **both one-stage and two-stage object detectors**.
- **Ensemble Learning** to maximize model robustness and accuracy.

## Features

### 1. Exploratory Data Analysis (EDA)
Understanding dataset characteristics was crucial before training models:
- **Brightness Distribution**: Analyzed how lighting conditions affect model inference.
- **Bounding Box (IoU) Distribution**: Assessed overlap between objects to optimize anchor sizes.
- **Color Distribution Bias**: Investigated whether specific colors dominated certain classes.

### 2. Data Preprocessing & Augmentation
To improve generalization, we experimented with:
- **Contrast Enhancement (CLAHE)**
- **Shift and Rotation Augmentations**
- **Geometric Transformations (Crop, Grid Mask)**
- **Texture-Based Transformations (Elastic Deformation)**

### 3. Model Selection & Training

#### **One-Stage Models**
- **RT-DETR-L / RT-DETR-X**:  
  - Backbone: **ResNet** (Pretrained)  
  - Optimizer: **AdamW / SGD**  
  - Loss: **VarifocalLoss**  
  - Scheduler: **CosineLR**

- **DINO**:  
  - Backbone: **Swin Transformer**  
  - Optimizer: **AdamW**  
  - Loss: **FocalLoss / GIoU Loss**  
  - Scheduler: **MultiStepLR**

#### **Two-Stage Models**
- **Faster R-CNN**:  
  - Backbone: **Swin Transformer Tiny / Small / Base, ResNet101**  
  - Optimizer: **SGD**  
  - Loss: **SmoothL1Loss, CrossEntropyLoss**  

- **ViTDet**:  
  - Backbone: **ViT (Pretrained)**  
  - Loss: **Contrastive Learning, SmoothL1**  

### 4. Ensemble Learning
By combining different models, we achieved significant performance boosts:
- **Weighted Box Fusion (WBF)** was used for optimal bounding box predictions.
- **RT-DETR, Faster R-CNN, ViTDet, and DINO were ensembled** for final predictions.

## System Overview
### Hardware
- **GPU: V100 32G × 4**
- **Development Frameworks: PyTorch, PyTorch Lightning**
- **Deep Learning Libraries: MMDetection, Detectron2, Ultralytics**

### Software
- **Ubuntu 20.04**
- **Python 3.10**
- **CUDA 11.6**
- **PyTorch 1.12**

## Experiments & Findings

### 1. **Class Decomposition**
- **Motivation**: The **General Trash class had high intra-class variance**, making detection difficult.
- **Solution**: We **split General Trash into three subclasses using K-Means clustering**.
- **Result**: **Leaderboard (LB) score improved**, but precision and recall decreased due to class imbalance.

### 2. **Contrastive Learning for Intra-Class Variation**
- **Problem**: Objects within the same class varied significantly in appearance.
- **Solution**: **Contrastive loss was applied** to group similar embeddings.
- **Result**: **Improved detection accuracy**, but required additional computation.

### 3. **Backbone Experiments**
- **Replacing Faster R-CNN's ResNet Backbone with Swin Transformer** resulted in **better small object detection**.

### 4. **Resolution Scaling**
- **Training on different image resolutions (1024×2048, 2048×1024) improved small object detection**, but gains were marginal.

## Additional Commentary
During this project, I faced several challenges and learned valuable lessons:
1. **Model Exploration & Library Adaptation**
   - Gained hands-on experience with **Detectron2 and MMDetection**.
   - Focused on modifying **loss functions, optimizers, and augmentation strategies**.

2. **Challenges & Limitations**
   - **Detectron2's complex model registration** made customization difficult.
   - **Limited training data** led to overfitting in some models.

3. **Future Improvements**
   - **Structured Experimentation**: Instead of understanding every detail in a library, focus on **modular improvements**.
   - **Better Collaboration**: Using Notion for systematic experiment tracking proved to be very effective.
   - **Deeper Model Analysis**: Investigate **why certain backbones outperform others** rather than just comparing scores.