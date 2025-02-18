---
layout:: dongjun
title: "Boostcamp - OCR"
description: "Data-centric approach to tackle an OCR task related to receipts."
categories: Boostcamp OCR
---

Optical Character Recognition (OCR) is a crucial technology for extracting textual data from images, particularly for structured documents like **receipts**. This project follows a **data-centric approach** to OCR, emphasizing **data preprocessing, augmentation, and enhancement** to improve model robustness. The goal is to **build an effective OCR model** that can accurately extract text from **noisy, occluded, and multilingual receipt images**.

<div class="btn-row">
  <a href="https://principled-nation-e2a.notion.site/Project-03-Data-Centric-567de4b0201e417e99d2c772001d5bb8?pvs=4" target="_blank" class="btn">
    <img src="https://upload.wikimedia.org/wikipedia/commons/e/e9/Notion-logo.svg" alt="Notion" class="btn-icon"> Notion
  </a>
  <a href="https://github.com/boostcampaitech7/level2-cv-datacentric-cv-01" target="_blank" class="btn">
    <img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" alt="GitHub" class="btn-icon"> GitHub
  </a>
  <a href="https://github.com/user-attachments/files/18843783/OCR.pdf" target="_blank" class="btn">
    <img src="https://cdn-icons-png.flaticon.com/256/80/80942.png" alt="Report" class="btn-icon"> Report
  </a>
</div>

## Abstract
This project focuses on **OCR (Optical Character Recognition) for receipt images**, utilizing a **data-centric methodology**.  
Instead of purely optimizing model architectures, the emphasis is on **data preprocessing, augmentation, and quality enhancement** to:
- **Improve text detection & recognition in noisy conditions.**
- **Handle occluded, blurred, and distorted text areas.**
- **Increase model robustness across multiple languages.**

The dataset consists of **400 receipt images** with labeled text annotations stored in **UFO (Universal Format for OCR) JSON files**.  
Throughout the project, we explored **various preprocessing techniques** and **experimented with multiple deep learning models** for OCR.

---

## Features

### 1. Exploratory Data Analysis (EDA)
Before preprocessing, we analyzed key dataset characteristics:

- **Image Size & Aspect Ratio**  
  - Most images were **vertically long**, aligning with typical receipt structures.  
  - **Insight:** Resize transformations were adjusted to maintain aspect ratio.

- **Word Count per Image**  
  - **Receipts contained fewer words per image** compared to general documents.  
  - **Insight:** Different augmentation strategies were needed for small text regions.

- **Text Orientation Analysis**  
  - **Majority of the text was horizontally aligned**, with very few vertically rotated words.  
  - **Insight:** The model was not well-trained to recognize vertical text.

- **Occlusion Analysis**  
  - **Many words were partially obscured** by shadows, objects, or blurring effects.  
  - **Insight:** Data augmentation was necessary to mimic real-world occlusions.

- **Multilingual Content**  
  - Although primarily in **Korean**, a substantial amount of **English and numerical** text was present.  
  - **Insight:** External datasets could be leveraged to improve multilingual OCR.

---

### 2. Data Preprocessing & Augmentation
To improve text recognition accuracy, we experimented with **several augmentation techniques**:

- **Salt & Pepper Noise**: Simulated ink smudging and paper texture imperfections.  
  - ✅ **+0.0332 F1-score improvement**

- **Binarization**: Applied **thresholding (128)** to enhance text contrast.  
  - ✅ **+0.0545 F1-score improvement**

- **Adaptive Threshold Binarization**: Dynamically applied local thresholding.  
  - ❌ **Caused performance degradation due to incorrect normalization.**

- **Super Resolution (x4 using ESRGAN)**: Improved text clarity.  
  - ✅ **+8–9% F1-score boost over baseline.**

- **External Dataset Augmentation**: Integrated additional English OCR datasets.  
  - ❌ **Lowered recall due to annotation mismatches.**

- **Remove Dash ("—") Annotations**: Fixed incorrect label bounding boxes.  
  - ✅ **+13% public LB improvement.**

- **K-Fold Training (3-Fold, 9:1 Split)**: Enhanced generalization on limited data.  
  - ✅ **+2.6% public LB, +1.6% private LB improvement.**

---

### 3. Model Selection & Training
We based our OCR pipeline on a **VGG16-based EAST model**:

#### **Baseline Model**
- **Architecture**: VGG16 + EAST (Efficient and Accurate Scene Text Detector)
- **Dataset**: UFO JSON-formatted receipt images
- **Loss Function**: SmoothL1Loss
- **Optimizer**: AdamW
- **Scheduler**: Cosine Annealing

#### **Model Enhancements**
- **Super Resolution Augmentation**
- **Binarization + Normalization**
- **Ensemble Learning (Weighted Box Fusion)**

The final **ensemble model** significantly outperformed the baseline.

---

## System Overview
### Hardware
- **GPU: V100 32G × 4**
- **Development Frameworks**: PyTorch, PyTorch Lightning
- **Deep Learning Libraries**: OpenCV, MMDetection, Ultralytics

### Software
- **Ubuntu 20.04**
- **Python 3.10**
- **CUDA 12.1**
- **PyTorch 2.1.0**

---

## Experiments & Findings

### 1. **Effect of Super Resolution**
- Applying **4× Super Resolution (ESRGAN)** significantly improved F1-score:
  - **Baseline F1-score**: **0.7629**
  - **Super Resolution (x4)**: **0.8439**  
  ✅ **+8.1% improvement**

### 2. **Impact of Binarization**
- **Binarization with threshold = 128** led to:
  - **Baseline F1-score**: **0.7787**
  - **Binarization-enhanced F1-score**: **0.8332**  
  ✅ **+5.4% improvement**

### 3. **Effect of Dash Removal**
- Removing incorrectly labeled dashes resulted in:
  - ✅ **+13% improvement on Public LB.**

### 4. **K-Fold Cross-Validation**
- Reducing **validation split from 8:2 to 9:1** improved generalization:
  - **+2.6% Public LB**
  - **+1.6% Private LB**

---

## Additional Commentary
Throughout this project, we encountered several challenges:

1. **Normalization Issues**
   - Adaptive thresholding initially **caused performance drops** due to incompatible normalization values.

2. **Annotation Errors**
   - External datasets introduced **bounding box mismatches**, reducing recall.

3. **Lessons Learned**
   - **Data preprocessing has a larger impact than minor model changes.**
   - **Super Resolution significantly benefits OCR tasks.**
   - **Mislabeling corrections (e.g., dash removal) yield large gains.**