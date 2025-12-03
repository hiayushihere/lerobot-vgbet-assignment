# VQ-BeT Evaluation – LeRobot Assignment

This repository contains the evaluation outputs for the **VQ-BeT (Vector Quantized Behavior Transformer)** model using the **LeRobot** framework.  
The assignment required setting up LeRobot, migrating pretrained VQ-BeT weights, running evaluation, and submitting the videos + metrics.

---

## 📁 Folder Structure

assignment/
│
├── videos/ # Evaluation episode videos (10 episodes)
│
└── results/
└── eval_summary.txt # Evaluation metrics summary

---

## 🔧 Setup Instructions (Steps Used)

### **1. Clone LeRobot Repository**
```bash
git clone https://github.com/huggingface/lerobot.git
cd lerobot
2. Create & Activate Virtual Environment
python3 -m venv .venv
source .venv/bin/activate
3. Install LeRobot
pip install -e ".[aloha,pusht]"
4. Download Pretrained VQ-BeT Model
python - << 'EOF'
from huggingface_hub import snapshot_download
snapshot_download("lerobot/vqbet_pusht", local_dir="lerobot/vqbet_pusht")
EOF
5. Migrate Pretrained Model (Required for Latest Version)
python src/lerobot/processor/migrate_policy_normalization.py \
    --pretrained-path lerobot/vqbet_pusht
A new migrated model directory will appear:
lerobot/vqbet_pusht_migrated/
▶️ Running Evaluation
python -m lerobot.scripts.lerobot_eval \
    --env.type=pusht \
    --policy.type=vqbet \
    --policy.pretrained_path=lerobot/vqbet_pusht_migrated \
    --eval.n_episodes=10 \
    --eval.batch_size=5 \
    --policy.device=cpu \
    --policy.use_amp=false
This generates:
10 evaluation videos
Evaluation metrics
Logs and rollout results
📊 Evaluation Summary
Average Sum Reward: 57.43
Average Max Reward: 0.53
Success Rate: 40%
Episodes: 10
Full summary is stored at:
assignment/results/eval_summary.txt
🎥 Videos
All evaluation episode videos are available at:
assignment/videos/
