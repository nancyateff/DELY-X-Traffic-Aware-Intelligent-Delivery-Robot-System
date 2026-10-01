# 🤖 DELY-X — Traffic-Aware Intelligent Delivery Robot System

**DELY-X** is an autonomous smart delivery robot that combines **Computer Vision, Object Detection, Object Tracking, Reinforcement Learning, and Embedded Hardware** to navigate through a defined environment, detect obstacles, make intelligent decisions, and move autonomously.

Unlike a software-only simulation, DELY-X is implemented as a **real-world robotic system**, integrating AI models with physical hardware using a **Raspberry Pi 5**, cameras, sensors, and motor-control components.

---

## 📚 Table of Contents

- [🎯 Project Overview](#-project-overview)
- [💡 Problem Statement](#-problem-statement)
- [🚀 Proposed Solution](#-proposed-solution)
- [🧠 System Architecture](#-system-architecture)
- [🔍 Computer Vision](#-computer-vision)
- [🎮 Reinforcement Learning](#-reinforcement-learning)
- [🤖 Hardware Implementation](#-hardware-implementation)
- [⚙️ How It Works](#%EF%B8%8F-how-it-works)
- [📊 Performance](#-performance)
- [🛠️ Technologies Used](#%EF%B8%8F-technologies-used)
- [📁 Project Structure](#-project-structure)
- [▶️ Getting Started](#%EF%B8%8F-getting-started)
- [📽️ Demo](#%EF%B8%8F-demo)
- [👥 Team](#-team)
- [🎯 Project Goals](#-project-goals)

---

## 🎯 Project Overview

Traditional delivery robots require reliable navigation systems to operate safely in environments containing **pedestrians, vehicles, and unexpected obstacles**.

**DELY-X** addresses this challenge by combining AI-based perception with Reinforcement Learning to create a robot capable of:

- 👁️ Understanding its surrounding environment.
- 🚧 Detecting obstacles, pedestrians, and vehicles.
- 🎯 Tracking detected objects over time.
- 🧠 Representing the current environment as a state.
- 🤖 Selecting appropriate movement actions.
- 📡 Sending movement commands to the physical robot.
- 🛞 Navigating autonomously through a predefined environment.

The complete pipeline is:

```text
Camera
   ↓
YOLOv8 Object Detection
   ↓
Kalman Filter Tracking
   ↓
State Representation
   ↓
PPO Reinforcement Learning Agent
   ↓
Action Selection
   ↓
UART Communication
   ↓
Robot Movement

```

---

## 💡 Problem Statement

Delivery robots operating in real-world environments must continuously perceive their surroundings and make decisions while dealing with dynamic objects.

A conventional rule-based navigation system may struggle when the environment changes or when multiple obstacles appear simultaneously.

Therefore, DELY-X aims to develop an intelligent robotic system capable of combining:

- **Real-time visual perception**
- **Object tracking**
- **Intelligent decision-making**
- **Autonomous movement**
- **Real-world hardware integration**

---

## 🚀 Proposed Solution

DELY-X uses a layered AI and robotics architecture.

### 👁️ Perception Layer

The robot uses cameras to capture its environment.

YOLOv8 processes the camera frames and detects relevant objects such as:

- 🚶 Pedestrians
- 🚗 Vehicles
- 🚧 Obstacles

Each detection provides:

- Bounding box
- Object class
- Confidence score

### 🎯 Tracking Layer

A **Kalman Filter** is used to track detected objects across consecutive frames.

This helps the system maintain information about object movement and provides a more stable representation of the environment.

### 🧠 Decision-Making Layer

The detected and tracked information is transformed into a **state representation**.

This state is provided to a **Proximal Policy Optimization (PPO)** reinforcement learning agent.

The agent selects the most appropriate action according to the current environment.

### 🤖 Control Layer

The selected action is converted into a movement command and transmitted to the physical robot through **UART communication**.

---

## 🧠 System Architecture

```text
                  ┌──────────────────────┐
                  │      Environment     │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │   CSI Camera System  │
                  │     2 × Cameras      │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │       YOLOv8         │
                  │   Object Detection   │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │    Kalman Filter     │
                  │   Object Tracking    │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │  State Representation│
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │    PPO RL Agent      │
                  │  Decision Making     │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │   Action Selection   │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │   UART Communication │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │    Robot Hardware    │
                  │ Motors + Sensors     │
                  └──────────────────────┘

```

---

## 🔍 Computer Vision

### YOLOv8 Object Detection

DELY-X uses **YOLOv8** for real-time object detection.

The model was trained using a dataset prepared through **Roboflow**.

The detection module identifies objects in the robot's environment and provides their locations and confidence values.

### Detection Metrics

| Metric    | Result  |
| --------- | ------- |
| mAP\@0.5  | **74%** |
| Precision | **78%** |
| Recall    | **71%** |
| F1-Score  | **74%** |

---

## 🎯 Object Tracking

To improve temporal consistency between consecutive frames, DELY-X uses a **Kalman Filter**.

The tracker helps estimate object movement and maintain object information across frames.

This allows the system to obtain a more stable representation of dynamic objects before passing the information to the decision-making module.

---

## 🎮 Reinforcement Learning

DELY-X uses **Proximal Policy Optimization (PPO)** as the main reinforcement learning algorithm.

The RL agent receives a representation of the robot's current environment and learns to select actions that improve navigation performance.

### RL Pipeline

```text
Environment State
       ↓
State Representation
       ↓
PPO Agent
       ↓
Action Selection
       ↓
Robot Movement
       ↓
New Environment State
       ↓
Reward
       ↓
PPO Update

```

### RL Performance

| Metric                 | Simulation    |
| ---------------------- | ------------- |
| Average Reward         | **+82**       |
| Success Rate           | **82%**       |
| Collision Rate         | **12%**       |
| Average Episode Length | **135 steps** |
| Convergence            | **Stable**    |

The trained policy was also evaluated on the physical robot to examine the transfer from simulation to real-world hardware.

---

## 🤖 Hardware Implementation

DELY-X is not limited to simulation.

The system is integrated with physical robotic hardware using:

### Main Components

- 🧠 **Raspberry Pi 5**
- 📷 **3 × CSI Cameras**
- 📡 Sensors
- ⚙️ Motor control system
- 🔌 UART communication
- 🛞 Mobile robot platform

The Raspberry Pi acts as the main computing platform responsible for running the perception and decision-making pipeline and communicating the selected actions to the robot's hardware.

---

## ⚙️ How It Works

### Step 1 — Environment Perception

The cameras continuously capture the robot's surroundings.

### Step 2 — Object Detection

YOLOv8 processes the captured frames and identifies relevant objects.

### Step 3 — Object Tracking

The Kalman Filter tracks detected objects between consecutive frames.

### Step 4 — State Representation

Detection and tracking information are transformed into a representation of the robot's current environment.

### Step 5 — Decision Making

The PPO agent receives the state and selects an appropriate action.

### Step 6 — Hardware Communication

The selected action is converted into a command and transmitted through UART.

### Step 7 — Robot Movement

The physical robot executes the command and moves accordingly.

### Step 8 — Continuous Feedback

The robot observes the updated environment and repeats the process continuously.

---

## 📊 Simulation vs Real-World Performance

The trained system was evaluated in both simulation and real-world environments.

| Metric         | Simulation | Real Robot |
| -------------- | ---------- | ---------- |
| Success Rate   | **82%**    | **73%**    |
| Collision Rate | **12%**    | **18%**    |

The difference between simulation and real-world performance is influenced by practical hardware constraints, processing limitations, sensor behavior, and real-world environmental conditions.

---

## 🛠️ Technologies Used

| Domain                 | Technologies                           |
| ---------------------- | -------------------------------------- |
| Programming            | Python                                 |
| Computer Vision        | YOLOv8                                 |
| Dataset & Training     | Roboflow                               |
| Object Tracking        | Kalman Filter                          |
| Reinforcement Learning | PPO                                    |
| AI / ML                | Deep Learning + Reinforcement Learning |
| Embedded Computing     | Raspberry Pi 5                         |
| Cameras                | CSI Cameras                            |
| Communication          | UART                                   |
| Hardware               | Sensors + Motors                       |
| Version Control        | Git & GitHub                           |

---

## 📁 Project Structure

```text
DELY-X/
│
├── 📁 computer_vision/
│   ├── 📁 dataset/
│   ├── 📁 detection/
│   ├── 📁 tracking/
│   └── yolo_model/
│
├── 📁 reinforcement_learning/
│   ├── environment/
│   ├── agent/
│   ├── training/
│   └── evaluation/
│
├── 📁 hardware/
│   ├── raspberry_pi/
│   ├── sensors/
│   ├── motors/
│   └── uart/
│
├── 📁 simulation/
│   └── simulation_environment/
│
├── 📁 documentation/
│   ├── reports/
│   └── diagrams/
│
├── 📁 demo/
│   └── videos/
│
├── requirements.txt
├── README.md
└── LICENSE

```

> **Note:** The structure above can be adjusted to match the exact folders and files included in the repository.

---

## ▶️ Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/DELY-X.git
cd DELY-X

```

### 2. Install Dependencies

```bash
pip install -r requirements.txt

```

### 3. Prepare the AI Models

Place the trained YOLOv8 and reinforcement learning model files in their corresponding directories.

### 4. Connect the Hardware

Connect the Raspberry Pi, cameras, sensors, and motor-control components according to the hardware configuration.

### 5. Run the System

Start the perception and decision-making pipeline according to the provided project scripts.

---

## 📽️ Demo






https://github.com/user-attachments/assets/1ece8337-da1b-4a08-88c6-f97df9e4c769


### 🎬 Real DELY-X


https://github.com/user-attachments/assets/b5f2a512-f2a3-4702-ac1c-2b712b71ae51

## 📈 Results

DELY-X demonstrates the feasibility of integrating:

```text
Computer Vision
       +
Object Tracking
       +
Reinforcement Learning
       +
Embedded Systems
       +
Physical Robotics

```

The project achieved:

- **74% mAP\@0.5** for object detection.
- **78% Precision**.
- **71% Recall**.
- **82% simulation success rate**.
- **73% real-world success rate**.
- Successful integration between the AI pipeline and physical robot hardware.

---

## 🎯 Project Goals

The main goals of DELY-X are to:

- 🤖 Develop an autonomous delivery robot.
- 👁️ Enable real-time environmental perception.
- 🚧 Detect and track dynamic obstacles.
- 🧠 Apply Reinforcement Learning for autonomous decision-making.
- 🔌 Connect AI decisions to physical hardware.
- 🛞 Validate the system in a real-world environment.
- 🔬 Study the difference between simulated and real-world robotic performance.

---

## 👥 Team
- Nancy Atef Mahmoud
- Salma Yasser
- Mariem Elsayed
- Mariem Abd-Elhamid
- Engi Alaa
- Walaa Osama
- Abdelrahman Nagi
- Ayman Atta
- Ahmed Abdo
- Ahmed Sami
- Karim Elsayed
- Mohamed Tamer
- Mohamed Gamal
  

### DELY-X Graduation Project

**Communications & Electronics Engineering**

The project combines expertise in:

- Artificial Intelligence
- Computer Vision
- Reinforcement Learning
- Embedded Systems
- Robotics
- Hardware–Software Integration

---

## 🏆 Graduation Project

**DELY-X — Traffic-Aware Intelligent Delivery Robot System**

> An intelligent robotic system that **sees, understands, decides, and moves**.

---

⭐ **If you find this project interesting, consider giving the repository a star!**
