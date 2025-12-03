## VQ-BeT Evaluation – LeRobot Assignment

This repository contains the evaluation outputs for the **VQ-BeT (Vector Quantized Behavior Transformer)** model using the **LeRobot** framework on the **pusht** environment.

The task involved setting up the LeRobot environment, migrating the pretrained VQ-BeT weights, running the evaluation, and collecting the results.

---

### 📁 Folder Structure

assignment/ │ ├── videos/ # Evaluation episode videos (10 episodes) │ └── results/ └── eval_summary.txt # Evaluation metrics summary


### 🔧 Setup and Execution Details

The following exact steps were used to set up the environment and run the evaluation:

#### 1. Clone LeRobot Repository

git clone https://github.com/huggingface/lerobot.git cd lerobot


#### 2. Create & Activate Virtual Environment

python3 -m venv .venv source .venv/bin/activate


#### 3. Install LeRobot

pip install -e ".[aloha,pusht]"


#### 4. Download Pretrained VQ-BeT Model

The pretrained VQ-BeT model weights for the `pusht` environment were downloaded from the Hugging Face Hub:

from huggingface_hub import snapshot_download snapshot_download("lerobot/vqbet_pusht", local_dir="lerobot/vqbet_pusht")


#### 5. Migrate Model Weights

The normalization configuration of the pretrained weights was migrated for compatibility with the latest LeRobot:

python src/lerobot/processor/migrate_policy_normalization.py

--pretrained-path lerobot/vqbet_pusht

This created the working directory: `lerobot/vqbet_pusht_migrated/`

#### 6. Running Evaluation

The evaluation was run for 10 episodes on the `pusht` environment using the migrated VQ-BeT model on the CPU:

python -m lerobot.scripts.lerobot_eval

--env.type=pusht

--policy.type=vqbet

--policy.pretrained_path=lerobot/vqbet_pusht_migrated

--eval.n_episodes=10

--eval.batch_size=5

--policy.device=cpu

--policy.use_amp=false


---

### 📊 Evaluation Summary

The evaluation was conducted over 10 episodes. The key metrics recorded are:

| Metric | Value |
| :--- | :--- |
| **Episodes** | 10 |
| **Success Rate** | 40% |
| **Average Sum Reward** | 57.43 |
| **Average Max Reward** | 0.53 |

The full evaluation log and metrics are saved in `assignment/results/eval_summary.txt`.

---

### 🎥 Evaluation Videos

The 10 rollout videos generated during the evaluation process can be found in the `assignment/videos/` directory.
