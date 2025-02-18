---
layout: dongjun
title: "Baedari RC Car"
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
Baedari aims to develop a **desert ant–inspired navigation system** for an RC car that can operate at low cost while performing obstacle detection and autonomous driving. By integrating **infrared (IR) sensors** of different frequencies for close-range safety and a **2D LiDAR** for mapping, the car is able to **drive to a target coordinate** and **return** to its origin. Throughout the project, a **stepwise experimental approach** was employed to tackle challenges in hardware, sensor fusion, and real-time path planning.

## Features

### 0. Incremental Implementation & Testing
Following the advice of our supervisor, we adopted a **stage-by-stage approach**:
1. **Basic hardware assembly + IR sensor tests** at low speed (safety & obstacle avoidance).  
2. **2D LiDAR integration** for obstacle detection.  
3. **Map creation & path planning** for more refined navigation.  
4. **Desert Ant–inspired path integration** for real-time location updates and efficient route planning (reducing computational overhead).


### **1. Incremental Implementation & Testing**
Following the advice of our supervisor, we adopted a **stage-by-stage approach**:
1. **Basic hardware assembly + IR sensor tests** at low speed (safety & obstacle avoidance).  
2. **2D LiDAR integration** for obstacle detection.  
3. **Map creation & path planning** for more refined navigation.  
4. **Desert Ant–inspired path integration** for real-time location updates and efficient route planning (reducing computational overhead).

### **2. Path Integration–Based Position Estimation** 
Drawing on the **mechanisms of desert ants**, we designed an algorithm that updates the car’s current position based on motor signals (PWM).  
- **Straight-line movement** relies on a simple distance integration.  
- **Turning** leverages an Ackermann steering model to account for angular changes.  
- Accumulated error is mitigated by minimizing sharp turns and favoring straight paths whenever possible.

<details>
  <summary><b>Desert Ant Navigation Table</b></summary>
  <p align="center">
    <img src="https://github.com/user-attachments/assets/278697bb-5272-4273-9954-a66483683fab" alt="Desert Ant Navigation" width="600"/>
  </p>
  More details can be found in our final report:  
  <a href="https://github.com/user-attachments/files/17011959/7.pdf">[종합설계보고서].pdf</a>
</details>

### **3. Obstacle Avoidance** 
Using **IR sensors** and a **2D LiDAR**, the car detects obstacles in its path. If an obstacle is too close or blocks the route, the system chooses an “open path” that best aligns with the original goal direction. The car’s width is also accounted for to ensure safe passage.

<p align="center">
  <img src="https://github.com/user-attachments/assets/2aabe56d-2e78-46d3-8b8c-8ed8eded335c" alt="Obstacle Avoidance" width="400"/>
</p>

### **4. Position Update (Localization)** 
For **low-speed** operation, the car can fuse LiDAR measurements with a pre-mapped environment, improving position accuracy. At **higher speeds**, simpler dead reckoning (Path Integration) is used for speed efficiency.

<p align="center">
  <img src="https://github.com/user-attachments/assets/3e68fe49-5883-490d-83ee-d04b9fbeba66" alt="Localization Result" width="300"/>
</p>

## System Overview
#### RC Car & Batteries
- **RC Car (WLtoys 144010 V2 RC)**  
  - Price: ~180,000 KRW (approx. **\$125 USD**) 
  - Included Battery, Battery Storage Bag  
  - [Store Link](https://m.smartstore.naver.com/uzustar/products/7934978349)

- **Battery (Wltyos 7.4V, 3000mAh)**  
  - Already included with the RC Car (additional spares may be purchased)

- **LiPo Battery (extra)** ×3 (7.4V, 1800mAh etc.)  
  - ~69,000 KRW total (approx. **\$49 USD**)  
  - [Link](https://buyrc.co.kr/product/product_detail.asp?product_number=100035)

#### Sensors
- **IR Sensors**  
  1. **GP2Y0A02** × 10 (Range: 0.2m–1.5m, 25Hz)  
     - ~79,000 KRW (approx. **\55.5 USD**)
     - [Store Link](https://www.devicemart.co.kr/goods/view?no=1328552)  
  2. **GP2Y0A710K0F** × 3 (Range: 1m–5.5m, 45Hz)  
     - ~69,000 KRW (approx. **\48.6 USD**)
     - [Store Link](https://www.devicemart.co.kr/goods/view?no=36586)

- **IR Sensor Cables** ×10  
  - ~20,000 KRW (approx. **\13.89**)
  - [Link](https://www.icbanq.com/P013154848)

#### Controllers & Modules
- **Raspberry Pi 8GB** ×1  
  - ~115,000 KRW (approx. **\$80 USD**)  
  - [Link](https://www.devicemart.co.kr/goods/view?no=12553062)

- **Raspberry Pi Case** ×1  
  - ~6,000 KRW  
  - [Link](https://www.devicemart.co.kr/goods/view?no=12234823)

- **Arduino Mega** ×1  
  - ~54,000 KRW  
  - [Link](https://www.devicemart.co.kr/goods/view?no=34405)

- **Jumper Wires** ×2  
  - ~2,000 KRW  
  - [Link](https://smartstore.naver.com/gongzipsa/products/7446652949)

### Software
- **Ubuntu 20.04**  
- **ROS1 (Noetic)**  
- **C++** (navigation node)  
- **Gazebo** (simulation)  

## Making Video
The **left window** shows the Gazebo environment where the RC car model navigates a virtual map, while the **right window** displays **actual motor command outputs and sensor readings** in real time, illustrating how closely the real system follows the simulated plan.


<video width="420" height="315" controls>
  <source src="https://github.com/user-attachments/assets/e07aa5f4-e6fa-45b6-ae40-c25e4a5ed3c7" type="video/mp4" />
  Your browser does not support the video tag.
</video>

For more details on the path integration and obstacle avoidance logic, see the code in  
[desert_ant_navigation_node.cpp](https://github.com/Poodlee/EEE4610_finals/blob/main/catkin_ws/src/desert_ant_navigation_node.cpp).

## Additional Commentary
This project demonstrates how **biological navigation** (desert ants) can be adapted into a robotic system for **affordable, rapid development**. By combining IR and 2D LiDAR data, the car can **avoid obstacles** and **reach specified coordinates** autonomously. While the **low-speed localization** achieves higher accuracy, the **high-speed mode** relies on simpler path integration to reduce computational overhead. Future improvements might include:
- More advanced SLAM algorithms for robust mapping  
- Integration with a higher-performance MCU or Jetson board  
- Enhanced obstacle detection using higher-resolution LiDARs

---

### 🔗 References
- [Final Report (PDF)](https://github.com/user-attachments/files/17011959/7.pdf)


