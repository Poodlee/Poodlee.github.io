---
layout: dongjun
title: "Baedari: Round Trip RC Car"
date: 2023-12-20 21:03:36 +0800
description: "Graduation Project with IR & 2D LiDAR Navigation"
categories: ROS1 UnderGraduate
---

# Baedari: Autonomous RC Car Navigation
Baedari is a low-cost RC car designed for round-trip autonomous driving, inspired by desert ants' path integration mechanisms. Developed between July and December 2023 as a graduation project, it utilizes IR sensors and a 2D LiDAR for obstacle detection, route planning, and returning to its starting position.

## 📌 Project Resources
- **[Notion Documentation](https://principled-nation-e2a.notion.site/EEE4610_Obstacle_Avoidance-O-A-305fb86405554a93bf36bfe0f830b4d1?pvs=4)**
- **[GitHub Repository](https://github.com/Poodlee/EEE4610_finals)**
- **[Project Report (PDF)](https://github.com/user-attachments/files/17011959/7.pdf)**

## 📝 Abstract
This project focuses on developing a **bio-inspired navigation system** for an RC car at a low cost. The goal was to navigate to a target coordinate and return to the starting point, mimicking desert ants' navigation strategies. 

### Key Features
- **Infrared (IR) sensors** ensure close-range safety.
- **2D LiDAR** enhances obstacle detection and path planning.
- **Motor signal-based odometry** allows cost-effective localization.

🚧 **Challenges:**
- Odometry errors accumulate over long distances.
- High-speed movements cause instability due to PWM fluctuations.
- Limited traction impacts accuracy.

🎯 **Future Improvements:**
- Optimize speed-to-PWM mapping.
- Introduce low-cost IMU for better localization.
- Enhance wheel traction for more stable navigation.

---

## 🎥 Results: Performance Demonstration
### Video Demonstration & Localization Analysis
The video below shows the RC car’s navigation performance and a comparison of localization methods:

- **🟡 Ground Truth** (Yellow)
- **🟢 LiDAR-Based Localization** (Green)
- **🔵 Path Integration-Based Estimation** (Blue)

![Localization Result](https://github.com/user-attachments/assets/3e68fe49-5883-490d-83ee-d04b9fbeba66)

⏳ **Key Observations:**
- The path integration method itself works as expected, maintaining a general trajectory close to ground truth.
- The deviation in localization results primarily stems from the absence of map-based position corrections.
- Incorporating additional reference points (e.g., landmarks or feature-based localization) could improve accuracy.

### Localization Challenges
1. No Map-Based Position Correction → The system cannot compare its location to a predefined map.

2. Motor Signal-Based Path Integration → Small odometry errors accumulate over time but do not significantly affect overall navigation.

3. LiDAR Localization Lag → The update rate is too low for real-time corrections in fast movement scenarios.

---

## 🛠 Key Features & Development Process
### 🔹 Incremental Development Approach
1. **Basic Hardware Setup & IR Sensor Testing** → Ensured safety and obstacle detection.
2. **2D LiDAR Integration** → Improved long-range obstacle detection.
3. **Path Planning Implementation** → Evaluated open-source mapping solutions.
4. **Bio-Inspired Path Integration** → Developed a real-time navigation algorithm based on desert ant behavior.

### 🔹 Path Integration-Based Position Estimation
- **Straight-line movement** estimated through distance integration.
- **Turning** follows an Ackermann steering model.
- **Error reduction** through optimized turn minimization.

![Desert Ant Navigation](https://github.com/user-attachments/assets/278697bb-5272-4273-9954-a66483683fab)

### 🔹 Obstacle Avoidance Strategy
| Sensor Type  | Max Distance (m) | Frequency (Hz) | Field of View |
|-------------|-----------------|----------------|--------------|
| **Infrared Sensors** | ~1.5 | 24–25 | Narrow |
| **LiDAR** | ~12 | 5–7 | 360° |

**Obstacle Avoidance Algorithms:**
| Algorithm | Sensor | Goal-Oriented | Smoothness | Speed (m/s) | Computation |
|-----------|--------|--------------|------------|-------------|--------------|
| **WVAD** | IR Sensors | No | High | 2.8 | High |
| **SBAD** | IR Sensors | No | High | 2.4 | Low |
| **OFSD** | LiDAR | Yes | Medium | 1.1–1.4 | Medium |

#### ✅ OFSD (Open Field Selective Driving) – LiDAR-Based Avoidance
**How it Works:**
1. **Scan Environment** → Detects obstacles within a 6m radius.
2. **Analyze Open Spaces** → Identifies passable gaps.
3. **Select Optimal Path** → Moves towards the goal or finds an alternative route.

**Key Parameters:**
| Parameter | Value | Description |
|-----------|--------|-------------|
| `detect_distance` | 6m | LiDAR scanning radius |
| `min_avoid_space_length` | 0.5m | Minimum required open space |
| `oa_threshold` | 3m | Distance threshold for avoidance activation |
| `emergency_stop_threshold` | 0.4m | Stops the car if an obstacle is too close |

📌 **Limitations:**
- LiDAR’s update frequency (~167ms) causes minor delays.
- High-speed movements introduce miscalculations due to sensor lag.

---

## 🏗 System Overview
### 🔸 Hardware Components
| Component | Quantity | Price (KRW/USD) |
|-----------|----------|-----------------|
| RC Car (WLtoys 144001) | 1 | ₩120,000 / $86 |
| Raspberry Pi 4B (4GB) | 1 | ₩110,000 / $79 |
| RPLiDAR A1 | 1 | ₩400,000 / $286 |
| IR Sensor (E18-D80NK) | 2 | ₩15,000 / $11 |
| Motor Driver (BTS7960) | 1 | ₩25,000 / $18 |
| DC-DC Converter (12V to 5V) | 1 | ₩10,000 / $7 |
| LiPo Battery (2S 7.4V 2200mAh) | 1 | ₩25,000 / $18 |

💰 **Total Cost:** ₩705,000 (~$504 USD)

### 🔸 Software Stack
- **Ubuntu 20.04**
- **ROS1 (Noetic)**
- **C++ (Navigation Node)**
- **Gazebo (Simulation)**

---

## 🏁 Conclusion
Baedari successfully demonstrates a **budget-friendly, bio-inspired autonomous RC car** using IR sensors and LiDAR. While localization accuracy needs improvement, the project highlights:
- ✅ **Cost-effective navigation solutions**
- ✅ **Trade-offs between affordability and accuracy**
- ✅ **Future potential for low-cost robotics applications**

📌 **Next Steps:**
- Improve localization accuracy with **low-cost IMU integration**.
- Refine **real-time processing** to enhance obstacle avoidance.
- Upgrade **hardware (wheels, traction control)** for stability.

This project serves as a **foundation for affordable autonomous vehicle research** and presents a practical balance between cost and performance.

