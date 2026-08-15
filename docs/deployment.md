# ACT Deployment

This document describes the deployment procedure of the trained ACT policy on the real Piper robot.

The deployment uses the LeRobot rollout framework to load the trained ACT checkpoint and execute predicted action sequences on the physical robot.


## 1. Deployment Overview

The complete deployment pipeline:

ACT Checkpoint

    ↓
    
LeRobot Rollout

    ↓
    
Camera Observation

    ↓
    
ACT Policy

    ↓
    
Predicted Action Chunk

    ↓
    
Piper Robot Execution


The deployment system consists of:

- Trained ACT policy checkpoint
- Piper robotic arm
- RGB cameras
- CAN communication interface
- LeRobot rollout framework


---

# 2. Hardware Preparation

Before deployment, verify the robot system.


## Piper Robot

Check:

- Piper power supply is enabled
- CAN cable connection is normal
- Robot communication is available


CAN interface:

```bash
sudo ip link set can0 type can bitrate 1000000 && sudo ip link set can0 up
```
The robot uses:
CAN interface: can0
Bitrate: 1000000

## Camera Setup
The deployment camera configuration should match the training data collection setup.

Requirements:

Same camera viewpoints

Same camera indexes

Similar lighting conditions

Similar object initial positions

Example camera configuration:

front camera:
 /dev/video0

wrist camera:
 /dev/video2

# 3. Model Preparation
 Before deployment:

Download the trained ACT checkpoint
Verify the checkpoint path
Ensure pretrained_model exists
Example:
```bash
models/

└── act_piper_test_20000/

    └── pretrained_model/
```
The deployment command loads the policy using:
```bash
--policy.path
```

# 4. Run ACT Inference
Run rollout:
```bash
lerobot-rollout \
  --strategy.type=base \
  --policy.path=./models/act_piper_test_20000/pretrained_model \
  --robot.type=piper \
  --robot.can_interface=can0 \
  --robot.bitrate=1000000 \
  --robot.include_gripper=true \
  --robot.use_degrees=false \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 30}, wrist: {type: opencv, index_or_path: 2, width: 640, height: 480, fps: 30}}" \
  --task="Put the banana on the plate" \
  --duration=20
```
# 5. Deployment Parameters
| Parameter               | Description                      |
| ----------------------- | -------------------------------- |
| `policy.path`           | Trained ACT checkpoint           |
| `robot.type`            | Piper robot                      |
| `robot.can_interface`   | CAN communication interface      |
| `robot.bitrate`         | CAN bitrate                      |
| `robot.include_gripper` | Enable gripper control           |
| `robot.cameras`         | Camera observation configuration |
| `task`                  | Manipulation task description    |
| `duration`              | Maximum rollout duration         |

# 6. Safety Notes
For the first deployment:

 - Set a short rollout duration (10-20 seconds)
 - Keep hands away from robot workspace
 - Prepare emergency stop procedure
 - Stop execution using Ctrl+C if unexpected motion occurs
# 7. Evaluation
During deployment, evaluate:

Task completion success
Motion smoothness
Trajectory consistency
Robustness against environment variations

Successful and failed examples are stored in:
```bash
results/
```
#  Notes
ACT is a behavior cloning policy.

Unlike Vision-Language-Action (VLA) models, ACT does not directly condition on language instructions.
Therefore, deployment performance depends strongly on:

 - Demonstration quality
 - Camera calibration
 - Robot setup consistency
 - Similarity between training and deployment environments
