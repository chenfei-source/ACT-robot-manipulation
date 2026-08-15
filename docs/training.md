# ACT Training

This document describes the training procedure of the ACT policy using the LeRobot framework.

The cloud server is used only for model training. Robot hardware (SO101 leader arm and Piper follower arm) is not required during this stage.


## 1. Training Overview

The ACT training pipeline:

Robot Demonstrations

    ↓
    
LeRobot Dataset

    ↓
    
ACT Policy Training

    ↓
    
Checkpoint Saving

    ↓
    
Robot Deployment

The training uses:

- Policy: Action Chunking with Transformers (ACT)
- Framework: LeRobot
- Dataset: Piper robot manipulation demonstrations
- Device: CUDA GPU


---

# 2. Environment Setup

If the LeRobot training environment has already been prepared, this step can be skipped.

For a new training server:

## Create Conda Environment

```bash
conda init bash
source ~/.bashrc

conda create -n lerobot python=3.12 -y

conda activate lerobot

sudo apt update

sudo apt install -y ffmpeg

pip install --upgrade pip

pip install 'lerobot[training]'

python - <<'EOF'
import torch

print('cuda available:', torch.cuda.is_available())

if torch.cuda.is_available():
    print(torch.cuda.get_device_name(0))
EOF
```

# 3. Start Training Session

Since ACT training may take a long time, screen is recommended to prevent SSH disconnection from interrupting the training process.

```bash
screen -S act_piper_train
conda activate lerobot
```
# 4. ACT Training Command
The ACT policy is trained using the LeRobot training framework.

Training command:
```bash
lerobot-train \
  --dataset.root=/root/piper_record_test \
  --dataset.repo_id=local/piper_record_test \
  --policy.type=act \
  --output_dir=outputs/train/act_piper_test \
  --policy.device=cuda \
  --wandb.enable=false \
  --policy.push_to_hub=false
```
Training Configuration
| Parameter            | Description                |
| -------------------- | -------------------------- |
| `dataset.root`       | Local LeRobot dataset path |
| `dataset.repo_id`    | Dataset identifier         |
| `policy.type`        | ACT policy                 |
| `output_dir`         | Training output directory  |
| `policy.device`      | CUDA training device       |
| `wandb.enable`       | Disable online logging     |
| `policy.push_to_hub` | Disable HuggingFace upload |
# 5. Check Training Checkpoints
After training, locate saved checkpoints:
```bash
find /root/outputs/train/act_piper_test \
-type d \
-name "pretrained_model"
```
Example output:
```bash
/root/outputs/train/act_piper_test/checkpoints/020000/pretrained_model
```
The checkpoint number depends on the training progress and saving frequency.

The actual path should always be obtained using the find command.

# 6. Notes
ACT training is a supervised imitation learning process.

The final policy performance depends strongly on:

demonstration quality
dataset diversity
task consistency
observation quality

The trained checkpoint is evaluated on the real Piper robot during deployment.
