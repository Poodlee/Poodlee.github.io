---
layout: dongjun
title: "Baedari: Round Trip RC Car"
date:   2023-12-20 21:03:36 +0800
description: "Graduation Project with IR & 2D LiDAR Navigation"
categories: ROS1 UnderGraduate
---

Baedari is a **low-cost RC car project** designed for round-trip autonomous driving, inspired by the **path integration** mechanism of desert ants. Developed from July to December 2023 as a graduation project, this system leverages **IR sensors** and a **2D LiDAR** to detect obstacles, plan routes, and return to its starting point.

<div class="btn-row">
  <a href="https://principled-nation-e2a.notion.site/EEE4610_Obstacle_Avoidance-O-A-305fb86405554a93bf36bfe0f830b4d1?pvs=4" target="_blank" class="btn">
    <img src="https://upload.wikimedia.org/wikipedia/commons/e/e9/Notion-logo.svg" alt="Notion" class="btn-icon"> Notion
  </a>
  <a href="https://github.com/Poodlee/EEE4610_finals" target="_blank" class="btn">
    <img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" alt="GitHub" class="btn-icon"> GitHub
  </a>
  <a href="https://github.com/user-attachments/files/17011959/7.pdf" target="_blank" class="btn">
    <img src="https://cdn-icons-png.flaticon.com/256/80/80942.png" alt="Report" class="btn-icon"> Report
  </a>
</div>

## Abstract
Baedari aims to develop a bio-inspired navigation system, modeled after desert ants, to enable an RC car to perform autonomous driving and obstacle detection at a low cost. The key challenge was designing a system that could navigate to a target coordinate and accurately return to its origin, similar to how desert ants rely on visual cues and path integration.

To achieve this, we integrated infrared (IR) sensors of different frequencies for close-range safety and a 2D LiDAR for object avoidance and path planning. A stepwise experimental approach was employed, iteratively refining hardware components, object detection algorithms, and path planning strategies to enhance the car’s navigation performance.

However, the odometry system, which relied on motor signals, introduced cumulative errors over long distances and during tight turns, reducing positional accuracy. Additionally, indoor testing revealed that when the PWM signal exceeded 0.075, the motor would unexpectedly accelerate, making it difficult to maintain a precise speed-to-PWM mapping. This further impacted odometry accuracy, especially in environments with limited traction. In future iterations, a more reliable approach would involve establishing a precise speed-to-PWM mapping by determining stable speed values for different PWM inputs. This could be done by defining speed benchmarks, such as maintaining a consistent velocity after the car has traveled 5 meters, and ensuring the wheels do not slip, allowing for more accurate odometry and motion control.


## **Result**  

The following video demonstrates the RC car's performance, while the accompanying localization result shows the difference between **ground truth**, **LiDAR-based localization**, and **our path integration method**.

<div style="display: flex; align-items: center;">
  <div class="video-container">
    <iframe width="420" height="315" 
      src="https://www.youtube.com/embed/BxYMxmuMnUI" 
      frameborder="0" allowfullscreen>
    </iframe>
  </div>
  <div>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/3e68fe49-5883-490d-83ee-d04b9fbeba66" 
           alt="Localization Result" width="300"/>
    </p>
  </div>
</div>

### **Localization Accuracy & Errors**  
Our goal was to implement an **autonomous navigation system at a low cost**, which meant relying on **motor signal-based path integration** rather than expensive, high-precision localization methods.  

The image above compares **Ground Truth (Yellow)**, **LiDAR Localization (Green)**, and **our Path Integration (Blue)**:  
- While the **blue path deviates** from the actual trajectory due to accumulated errors,  
- The **overall path shape** remains similar to the real route, showing that the system maintains a rough sense of direction.  

### **Why Does This Discrepancy Occur?**  
1. **Motor-Based Path Integration** → Errors accumulate over distance and turns, as odometry relies solely on **motor signals**.  
2. **LiDAR Localization Latency** → While LiDAR-based localization is more accurate, its **low update rate** limits real-time corrections.  

Although the localization is **not perfectly accurate**, the results suggest that even with a **low-cost implementation**, the system can approximate the intended path, maintaining **general trajectory consistency**.  


## Features
### 1. Incremental Implementation & Testing
Following our supervisor’s guidance, we adopted a stage-by-stage development approach to systematically build and refine the navigation system:
1. **Basic Hardware Assembly & IR Sensor Testing** – Initial tests were conducted at low speeds to ensure safety and reliable obstacle detection.  
2. **Translation to 2D LiDAR** – After IR sensor testing, we integrated a 2D LiDAR to improve obstacle detection. By using a LiDAR with a longer range and lower frequency, we were able to test early obstacle avoidance strategies for smoother navigation.
3. **Map Creation & Path Planning** – Explored mapping solutions for refined navigation, but found that open-source options (e.g., Cartographer) had high computational costs, making them impractical for real-time use
4. **Desert Ant–inspired path integration** – Implemented a bio-inspired approach for real-time location updates and efficient route planning.

### **2. Path Integration–Based Position Estimation** 
Inspired by desert ant navigation mechanisms, we developed an algorithm that updates the car’s position using motor signals (PWM).

<p align="center"> <img src="https://github.com/user-attachments/assets/278697bb-5272-4273-9954-a66483683fab" alt="Desert Ant Navigation" width="500"/> </p>
Straight-line movement is estimated through distance integration.
Turning follows an Ackermann steering model to account for angular changes.
To reduce accumulated error, sharp turns are minimized, and the system prioritizes straight paths whenever possible.
(More details can be found in our report.)

### **3. Obstacle Avoidance**  
The RC car employs **infrared (IR) sensors** and **2D LiDAR** to detect and avoid obstacles. The system dynamically adjusts its path based on detected obstacles, prioritizing an **"open path"** that aligns best with the intended goal direction.  

<p align="center">
  <img src="https://github.com/user-attachments/assets/2aabe56d-2e78-46d3-8b8c-8ed8eded335c" alt="Obstacle Avoidance" width="400"/>
</p>  

### **Sensor Considerations**  
The performance of obstacle avoidance is highly dependent on sensor characteristics:  

| Sensor Type | Max Distance (m) | Frequency (Hz) | Field of View |
|-------------|-----------------|----------------|--------------|
| **Infrared Sensors** | ~1.5 | 24–25 | Nearly straight line |
| **LiDAR** | ~12 | 5–7 | 360° |

- **IR sensors** provide fast response times, enabling **high-speed reactive maneuvers**.  
- **LiDAR** allows for **long-range detection and local path planning** but has **lower update frequency**, introducing latency.  

### **Obstacle Avoidance Algorithms**  
Initially, we tested **IR-based obstacle avoidance algorithms (WVAD & SBAD)** before transitioning to **LiDAR-based avoidance (OFSD)**.  

| Algorithm | Sensor Type | Goal-Oriented | Smoothness | Avg. Speed (m/s) | Computational Load |
|-----------|-------------|--------------|------------|-------------------|-------------------|
| **WVAD** | IR Sensors | No | High | 2.8 | High |
| **SBAD** | IR Sensors | No | High | 2.4 | Low |
| **OFSD** | LiDAR | Yes | Medium | 1.1–1.4 | Medium |

### **WVAD (Wall Detection Autonomous Driving) – IR-Based**  
- Detects obstacles using IR sensors and **constructs a virtual wall** at detected positions.  
- The car **navigates towards the center of the largest open space**.  

✔ **High-speed capable**, smooth driving.  
✖ **Not goal-oriented**, **high computational cost** for real-time mapping.  

### **SBAD (State-Based Autonomous Driving) – IR-Based**  
- Uses a **state machine** to adjust movement based on **past sensor readings**, reducing noise sensitivity.  
- Instead of **immediately constructing a virtual wall (like WVAD)**, it considers **previous states** before making adjustments.  

✔ **More robust against sensor noise**, **lower computational load**.  
✖ **Heavily dependent on thresholds**, **not goal-oriented**.  

### **Challenges in Transitioning to LiDAR-Based Avoidance**  
Since **WVAD and SBAD were developed for IR sensors**, applying them to **LiDAR-based navigation** had significant challenges:  
- **Selecting which LiDAR points to use**—processing **all 1000+ data points** per scan was computationally infeasible.  
- **Real-time performance**—existing algorithms required too much processing time, causing delays.  

### **Final Selection: OFSD (Open Field Selective Driving) – LiDAR-Based**  
To address these challenges, we developed **OFSD**, which efficiently leverages **LiDAR’s long-range detection**.  

#### **How OFSD Works:**  
1. **Scanning the Environment**:  
   - LiDAR scans obstacles within a **6-meter radius** (`detect_distance`).  
   - Obstacles are filtered out from potential driving paths.  

2. **Identifying Open Spaces**:  
   - Analyzes **angles and lengths of gaps** between obstacles.  
   - Ignores openings smaller than **`min_avoid_space_length = 0.5m`**.  

3. **Selecting the Best Path**:  
   - If the **goal direction is open**, the car **moves directly toward it**.  
   - If blocked, the car **chooses the closest available open angle**.  

#### **Configurable Parameters:**  
| Parameter | Default Value | Description |
|-----------|--------------|-------------|
| **`detect_distance`** | 6m | LiDAR scanning radius. |
| **`min_avoid_space_length`** | 0.5m | Minimum width required for an open space. |
| **`oa_threshold`** | 3m | Distance threshold for activating avoidance behavior. |
| **`emergency_stop_threshold`** | 0.4m | Stops the RC car if an obstacle is too close. |

#### **Limitations**
LiDAR's low update frequency (~167ms interval) introduces latency, causing the RC car to potentially misjudge obstacle positions at high speeds, leading to delayed reactions and possible collisions

## System Overview
### **Hardware**  

| Item | Quantity | Price (KRW / USD) | Purchase Link |
|------|----------|-------------------|---------------|
| RC Car (WLtoys 144001) | 1EA | ₩120,000 / $86 | [Link](https://www.example.com) |
| Raspberry Pi 4B (4GB) | 1EA | ₩110,000 / $79 | [Link](https://www.example.com) |
| RPLiDAR A1 | 1EA | ₩400,000 / $286 | [Link](https://www.example.com) |
| IR Sensor (E18-D80NK) | 2EA | ₩15,000 / $11 | [Link](https://www.example.com) |
| Motor Driver (BTS7960) | 1EA | ₩25,000 / $18 | [Link](https://www.example.com) |
| DC-DC Converter (12V to 5V) | 1EA | ₩10,000 / $7 | [Link](https://www.example.com) |
| LiPo Battery (2S 7.4V 2200mAh) | 1EA | ₩25,000 / $18 | [Link](https://www.example.com) |

---

### **Total Cost**  
- **Total (KRW):** ₩705,000  
- **Total (USD, approx.):** $504  

---

### Software
- **Ubuntu 20.04**  
- **ROS1 (Noetic)**  
- **C++** (navigation node)  
- **Gazebo** (simulation)  

## **Simulation Result**  

The video below shows the **Gazebo simulation of the RC car navigating a virtual environment**.  
It demonstrates how the **simulated model processes sensor data, executes motor commands, and follows the planned path** based on the implemented navigation logic.  

<video width="420" height="315" controls>
  <source src="https://github.com/user-attachments/assets/e07aa5f4-e6fa-45b6-ae40-c25e4a5ed3c7" type="video/mp4" />
  Your browser does not support the video tag.
</video>

## **Additional Commentary**  

This project aimed to develop a **low-cost, round-trip autonomous RC car**, inspired by **biological navigation** in desert ants. By integrating **IR sensors and 2D LiDAR**, the system successfully performs **obstacle avoidance** and **goal-directed navigation**.  

However, relying solely on **motor signals** for localization introduced **clear limitations**. Errors accumulated over long distances and sharp turns, making precise tracking difficult. Despite these challenges, we maintained a focus on **cost efficiency**, avoiding expensive sensors or high-performance computing units.  

For future improvements while keeping costs low, we plan to:  
- **Use wheels with better traction** to minimize slippage.  
- **Integrate a low-cost IMU** to enhance localization without significantly increasing expenses.  
- **Upgrade the processing unit slightly** to improve real-time performance while staying within budget.  

This study highlights the trade-offs in designing a **budget-friendly autonomous system** and suggests practical enhancements to **bridge the gap between affordability and accuracy**.  
