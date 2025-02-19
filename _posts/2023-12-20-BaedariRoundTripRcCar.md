---
layout: dongjun
title: "Baedari: Round Trip RC Car"
date: 2023-12-20 21:03:36 +0800
description: "Graduation Project with IR & 2D LiDAR Navigation"
categories: ROS1 UnderGraduate
---

# Baedari: Round Trip RC Car

Baedari is a **low-cost RC car project** designed for autonomous round-trip navigation, inspired by **desert ants' path integration**. Developed as a graduation project from July to December 2023, it leverages **IR sensors** and **2D LiDAR** for obstacle detection, route planning, and returning to its starting point.

## 📌 Project Links
- [**Notion**](https://principled-nation-e2a.notion.site/EEE4610_Obstacle_Avoidance-O-A-305fb86405554a93bf36bfe0f830b4d1?pvs=4)
- [**GitHub**](https://github.com/Poodlee/EEE4610_finals)
- [**Final Report**](https://github.com/user-attachments/files/17011959/7.pdf)

## 🎥 Demonstration & Results

<div style="display: flex; align-items: center;">
  <div style="flex: 1;">
    <h3>🏁 Video Demonstration</h3>
    <iframe width="420" height="315" src="https://www.youtube.com/embed/BxYMxmuMnUI" frameborder="0" allowfullscreen></iframe>
  </div>
  <div style="flex: 1;">
    <h3>📊 Localization Accuracy</h3>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/3e68fe49-5883-490d-83ee-d04b9fbeba66" alt="Localization Result" width="300"/>
    </p>
  </div>
</div>

| Path Type | Color | Observations |
|-----------|-------|-------------|
| **Ground Truth** | Yellow | Actual route taken |
| **LiDAR Localization** | Green | More accurate but slow updates |
| **Path Integration** | Blue | Low-cost, but drift accumulates |

✅ **Path shape is maintained**, but **errors accumulate over time**. 

## 🔍 Why Does This Discrepancy Occur?
The **path integration method** works as expected, maintaining a general trajectory close to ground truth. However, deviations in localization primarily stem from the **absence of map-based position corrections**. To improve accuracy, incorporating additional **reference points** (e.g., landmarks or feature-based localization) could be beneficial.

### **Localization Challenges**
- **No Map-Based Position Correction** → The system cannot compare its location to a predefined map, leading to cumulative errors.
- **Motor Signal-Based Path Integration** → Small odometry errors accumulate over time but do not significantly affect overall navigation.
- **LiDAR Localization Lag** → The update rate is too low for real-time corrections in fast movement scenarios.

## 🔧 System Features

### **Sensor Comparison**
| Sensor Type | Maximum Distance | Frequency | Field of View |
|-------------|-----------------|-----------|--------------|
| **Infrared Sensors** | ~1.5m | 24–25Hz | Narrow |
| **LiDAR** | ~12m | 5–7Hz | 360° |

- **Infrared sensors** provide quick response times for close-range obstacle detection.
- **LiDAR sensors** offer broader field coverage and long-range detection, enhancing global navigation awareness.

### **Obstacle Avoidance Algorithms**
<p align="center"> <img src="https://github.com/user-attachments/assets/2aabe56d-2e78-46d3-8b8c-8ed8eded335c" alt="Obstacle Avoidance" width="400"/> </p>

| Algorithm | Sensor | Goal-Oriented? | Smoothness | Avg. Speed | Computational Load |
|-----------|--------|--------------|------------|------------|------------------|
| **WVAD** | IR | No | High | 2.8 m/s | High |
| **SBAD** | IR | No | High | 2.4 m/s | Low |
| **OFSD** ✅ | LiDAR | Yes | Medium | 1.1–1.4 m/s | Medium |

### **Detailed Algorithm Descriptions**
- **WVAD (Wall Detection Autonomous Driving)**: Uses IR sensors to create a virtual wall for obstacle avoidance, enabling smooth navigation but lacks goal direction.
- **SBAD (State-Based Autonomous Driving)**: Implements a state machine approach to reduce noise sensitivity and improve stability.
- **OFSD (Open Field Selective Driving)** ✅: LiDAR-based avoidance algorithm that selects the best path based on detected open spaces.

## 🛠 Hardware Components

| Item | Quantity | Price (KRW / USD) |
|------|----------|-------------------|
| RC Car (WLtoys 144001) | 1 | ₩120,000 / $86 |
| Raspberry Pi 4B (4GB) | 1 | ₩110,000 / $79 |
| RPLiDAR A1 | 1 | ₩400,000 / $286 |
| IR Sensor (E18-D80NK) | 2 | ₩15,000 / $11 |
| Motor Driver (BTS7960) | 1 | ₩25,000 / $18 |
| DC-DC Converter (12V to 5V) | 1 | ₩10,000 / $7 |
| LiPo Battery (2S 7.4V 2200mAh) | 1 | ₩25,000 / $18 |

💰 **Total Cost:** ₩705,000 (~$504 USD)

## 🎮 Simulation Results

<video width="420" height="315" controls>
  <source src="https://github.com/user-attachments/assets/e07aa5f4-e6fa-45b6-ae40-c25e4a5ed3c7" type="video/mp4" />
  Your browser does not support the video tag.
</video>

## 📢 Final Thoughts
Baedari successfully achieves **goal-oriented navigation, obstacle avoidance, and low-cost implementation**. Future improvements include integrating an IMU and refining odometry accuracy while maintaining affordability. 🚗💨