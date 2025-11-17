# DRL-Autonomous-Path-Following

[![DOI](https://img.shields.io/badge/DOI-10.3390%2Fs24020561-blue)](https://doi.org/10.3390/s24020561)

This repository contains the source code for the paper **“Path Following for Autonomous Mobile Robots with Deep Reinforcement Learning”**.

## 🚀 Highlights

- **Pure Pursuit + SAC**: Pure Pursuit handles steering, while a SAC-based policy adaptively controls the linear velocity.  
- **Path-relative speed control**: The policy uses the robot’s state relative to the path (including the `nearest` and `lookahead` points) to adjust speed and reduce cross-track error.  
- **Generalization to unseen paths**: The same controller works on randomly generated paths without retraining, showing strong path-level generalization.  

## 🎮 Pure Pursuit Steering + SAC Velocity Control

These animations show our **Pure Pursuit + SAC controller** in action. The trajectory is color-coded by **linear velocity**, highlighting how the learned policy adjusts speed based on the robot’s state relative to the path (including the `nearest` and `lookahead` points).

<p align="center">
  <img src="assets/ani_eight.gif" alt="Eight-shaped Path Following" width="70%">
</p>

- **Eight-shaped path:** Pure Pursuit steers the robot, while SAC adapts the speed using path-relative information from the `nearest` and `lookahead` points, slowing in demanding segments and accelerating where tracking is easier.  

<p align="center">
  <img src="assets/ani_change.gif" alt="Lane-change Path Following" width="70%">
</p>

- **Lane-change path:** Using both the `nearest` and `lookahead` points, SAC shapes the velocity profile so that Pure Pursuit can anticipate the lateral shift and execute the lane change smoothly with small tracking error.

## 🌐 Generalization to Random Paths

To test generalization, we evaluate the **Pure Pursuit + SAC controller** on randomly generated smooth paths that are not seen during training.  

<p align="center">
  <img src="assets/rand_pf.jpg" alt="Random Path Following" width="100%">
</p>

The starting point is randomly generated in the vicinity of (0, 0).

## 📦 Installation

Clone this repository and install the required Python packages:

```bash
# Clone the repository
git clone https://github.com/cao-yu/DRL-Autonomous-Path-Following.git
cd DRL-Autonomous-Path-Following

# Install dependencies
pip install -r requirements.txt
```

## 🏃‍♂️ Usage

### 1. Train the SAC policy

```bash
# From the repository root
cd pure_pursuit_plus_sac

# Train a SAC policy on a randomly generated path with a fixed seed
python main.py \
  --path random \
  --seed 0 \
  --save_model

# Run a batch of experiments over multiple seeds
# (see run_experiments.py for the list of seeds and settings)
python run_experiments.py
```

### 2. Evaluate the trained policy

```bash
# Evaluate the trained policy on an eight-shaped path and display the animation
python eval_policy.py \
  --policy SAC \
  --path eight \
  --eval_episodes 1 \
  --seed 0 \
  --load_model default \
  --disp_ani
```

```bash
# Evaluate the trained policy on randomly generated paths over multiple episodes
python eval_policy.py \
  --policy SAC \
  --path random \
  --eval_episodes 10 \
  --seed 0 \
  --load_model default
```
