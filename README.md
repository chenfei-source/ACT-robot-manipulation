# ACT-robot-manipulation

A robot imitation learning project based on Action Chunking with Transformers (ACT).

This project explores learning visuomotor manipulation policies from real robot demonstrations collected through a leader-follower teleoperation system.

The pipeline includes robot demonstration collection, dataset processing, ACT policy training and real robot deployment.


## 🎥 Demo

Coming soon.


## Overview

Action Chunking with Transformers (ACT) is a transformer-based imitation learning method that predicts future action sequences from robot observations.

This project investigates ACT-based manipulation learning on a real robotic platform.


Pipeline:

Human Demonstration  
↓  
SO101 Leader Arm  
↓  
LeRobot-record Data Collection  
↓  
Dataset Processing  
↓  
ACT Policy Training  
↓  
Piper Robot Deployment


## Hardware

Leader:

- SO101 robotic arm

Follower:

- Piper 6-DOF robotic arm

Sensors:

- RGB cameras
- Robot joint states


## Dataset

Real robot demonstrations collected through LeRobot-record.

Statistics:

- 55 episodes
- 13,311 frames
- 15 FPS
- ~30 seconds per episode


Modalities:

- RGB images
- Robot states
- End-effector actions
- Demonstration trajectories


## Training Pipeline

Coming soon.


## Deployment

Coming soon.


## Results

Coming soon.


## Repository Structure
