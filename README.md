# DELY-X — Autonomous Smart Delivery Robot

DELY-X is an autonomous smart delivery robot graduation project that combines **Computer Vision, Object Tracking, Path Planning, Reinforcement Learning, and real-world hardware integration**.

The system provides a complete navigation pipeline for a traffic-aware delivery robot: **detecting and tracking obstacles, planning routes using A*, making high-level driving decisions using PPO, and preparing the trained policy for deployment on embedded hardware.**

## 🎓 Project Overview

- **Project Type:** Graduation Project
- **Project:** DELY-X — Autonomous Smart Delivery Robot
- **Primary Focus:** AI-based autonomous navigation and safe delivery in dynamic environments
- **Environment:** Samaad City, Talha, Dakahlia, Egypt
- **Map:** 50 × 50 grid using OpenStreetMap with cached-map fallback

## 🧠 System Pipeline

Camera / Vision
↓
YOLOv8 Object Detection
↓
Kalman + IoU Tracking
↓
Robot State + Vision + LiDAR
↓
Attention-Based Feature Extraction
↓
PPO Reinforcement Learning
↓
MOVE / STOP / WAIT / AVOID
↓
A* Path Planning

## 👁️ Computer Vision

The vision system uses **YOLOv8** to detect traffic objects such as:

- Cars
- Trucks
- Motorcycles
- Buses
- Pedestrians

Detected objects are tracked using:

- Kalman Filter
- IoU-based matching
- Multi-object tracking

The resulting object features are provided to the reinforcement-learning agent.

## 🗺️ A* Path Planning

The A* planner operates on a **2D binary grid** and:

- Finds routes between start and goal points
- Supports 8-directional movement
- Uses Euclidean distance as the heuristic
- Treats obstacles as blocked cells
- Generates waypoints for navigation

## 🌍 Reinforcement Learning Environment

The custom **Gymnasium `DeliveryRobotEnv`** represents Samaad City using a 50 × 50 grid.

The environment supports:

- OpenStreetMap / OSMnx
- Overpass API fallback
- Cached street-template fallback
- Dynamic vehicles
- Collision detection
- Goal reaching
- Timeout handling
- Traffic difficulty levels
- Reward shaping
- A* waypoint replanning

### Observation Space

The agent receives **63 features**:

- 7 robot-state features
- 48 vision/object features
- 8 LiDAR-style features

### Action Space

| Action | Meaning |
|---|---|
| 0 | MOVE |
| 1 | STOP |
| 2 | WAIT |
| 3 | AVOID |

## 🧩 Custom Neural Network

A custom Stable-Baselines3 feature extractor processes the 63-dimensional observation:

Robot State (7) → MLP → 32 features

Vision (48) → Shared MLP → Attention Pooling → 16 features

LiDAR (8) → MLP → 16 features

↓

**128-D Feature Representation**

## 🚀 PPO Training

The project uses **Proximal Policy Optimization (PPO)** from Stable-Baselines3.

### Training Configuration

| Parameter | Value |
|---|---:|
| Learning Rate | 3e-4 |
| n_steps | 1024 |
| Batch Size | 64 |
| Epochs | 10 |
| Gamma | 0.99 |
| GAE Lambda | 0.95 |
| Clip Range | 0.2 |
| Entropy Coefficient | 0.01 |
| Value Coefficient | 0.5 |
| Parallel Environments | 4 |
| Total Timesteps | 200,000 |
| Device | CUDA / GPU when available |

The trained models are saved under `./models/`.

## 📊 Evaluation Results

The trained policy was evaluated across different traffic difficulties:

| Difficulty | Mean Reward |
|---|---:|
| 🟢 Easy | 119.5 ± 126.9 |
| 🟡 Medium | 79.6 ± 136.9 |
| 🔴 Hard | 44.0 ± 144.2 |

### Final PPO Evaluation

- **Mean Reward:** 77.42 ± 148.98
- **Mean Episode Length:** 210.00 ± 236.83

## 🔄 Curriculum Learning

The training environment progressively increases difficulty:

**Easy → Medium → Hard**

This allows the agent to learn basic navigation before handling more complex traffic scenarios.

## 📹 Hardware Integration

A `RealRobotInterface` is included to support future physical deployment using:

- Raspberry Pi 4 / Jetson Nano
- USB or CSI Camera
- RPLidar A1
- GPS
- IMU
- Arduino / ESP32 for motor control

The interface connects the trained PPO policy and perception system with the robot-control layer.

## 🛠️ Technology Stack

### AI / Machine Learning
- Python
- PyTorch
- Stable-Baselines3
- PPO
- Attention-based Feature Extraction

### Computer Vision
- YOLOv8
- OpenCV
- Kalman Filter
- IoU-based Tracking

### Robotics & Navigation
- A* Path Planning
- Gymnasium
- LiDAR-style Sensing
- Dynamic Obstacle Simulation

### Mapping
- OpenStreetMap
- OSMnx
- Overpass API
- NetworkX
- Shapely

### Visualization
- Matplotlib

## 📦 Installation

🏆 Project Outcome

DELY-X demonstrates a complete AI-driven autonomous delivery pipeline:

Perception
    ↓
Object Detection
    ↓
Object Tracking
    ↓
Sensor Feature Fusion
    ↓
A* Global Planning
    ↓
PPO Decision Making
    ↓
Safe Navigation
    ↓
Hardware Deployment Interface

The project demonstrates the integration of Artificial Intelligence, Computer Vision, Reinforcement Learning, and Robotics into an autonomous delivery system.

👨‍💻 Project

DELY-X — Autonomous Smart Delivery Robot

Graduation Project — Communications & Electronics Engineering


```bash
pip install gymnasium stable-baselines3 ultralytics opencv-python-headless \
torch torchvision numpy matplotlib scipy filterpy pybullet

pip install osmnx networkx shapely requests
