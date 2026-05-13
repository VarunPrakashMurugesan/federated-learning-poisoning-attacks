# Adaptive and Coordinated Targeted Label Flipping Attacks in Federated Learning with Krum-Based Defense

**Author:** Varun Prakash Murugesan  
**Course:** CYSE 686 — Introduction to Federated Learning  
**Institution:** George Mason University  

---

## Project Overview

This project investigates security vulnerabilities in Federated Learning (FL) through the implementation and progressive strengthening of targeted label flipping attacks, and evaluates the Krum Byzantine-robust aggregation defense against them.

The experiments are organized into three steps:
1. **Baseline Attack** — Standard targeted label flipping under FedAvg
2. **Improved Attack** — Adaptive PGD attack with dynamic stealth/boost scheduling, stealthy two-phase attack, and coordinated multi-client poisoning
3. **Defense (Bonus)** — Krum Byzantine-robust aggregation evaluated against the improved attack

---

## Dataset

This project uses the **MNIST handwritten digit dataset**, which is automatically downloaded via `torchvision` when you run the notebooks. You do **not** need to download it manually.

- **Official link:** http://yann.lecun.com/exdb/mnist/
- **Size:** 60,000 training samples, 10,000 test samples
- **Format:** Grayscale 28×28 images, 10 classes (digits 0–9)
.

---

## Requirements

Install the required Python packages before running the notebooks:

```bash
pip install torch torchvision numpy matplotlib jupyter
```

**Recommended environment:**
- Python 3.8+
- PyTorch 1.12+
- Jupyter Notebook or JupyterLab
- GPU optional (CPU works fine for MNIST scale)

---

## File Structure

```
fed/
├── fedavg_label_flipping_baseline.ipynb   # Step 1: Baseline attack
├── Adaptive_attack_final.ipynb            # Step 2: Adaptive PGD attack
├── stealthy-attack_final.ipynb            # Step 2: Stealthy two-phase attack
├── multi.ipynb                            # Step 2: Coordinated multi-client attack
├── krum.ipynb                             # Step 3: Krum defense evaluation
└── README.md                              # This file
```

---

## How to Reproduce Results

Run the notebooks **in order** using Jupyter Notebook or JupyterLab:

### Step 1 — Baseline Attack
```
fedavg_label_flipping_baseline.ipynb
```
- 10 clients, 20 rounds, 2 malicious clients, attack scale 5.0
- Source class: digit 1 → Target class: digit 7
- **Expected output:** Final ASR ≈ 0.9683, Final MTA ≈ 0.8715

### Step 2a — Adaptive PGD Attack
```
Adaptive_attack_final.ipynb
```
- 3 malicious clients, stealth weight 2.0→0.5, boost factor 2.0→3.2
- **Expected output:** Final ASR ≈ 0.7163, Final MTA ≈ 0.895

### Step 2b — Stealthy Two-Phase Attack
```
stealthy-attack_final.ipynb
```
- 4 malicious clients, attack scale 1.6, two-phase clean+poison training
- **Expected output:** Final ASR ≈ 0.5374, Final MTA ≈ 0.9224

### Step 2c — Coordinated Multi-Client Attack
```
multi.ipynb
```
- Tests 1, 3, and 5 malicious clients under FedAvg
- **Expected output:**

| Malicious Clients | Final MTA | Final ASR |
|---|---|---|
| 1 | 0.9792 | 0.0062 |
| 3 | 0.8894 | 0.7665 |
| 5 | 0.8773 | 0.5753 |

### Step 3 — Krum Defense (Bonus)
```
krum.ipynb
```
- Same adaptive multi-client attack with Krum replacing FedAvg
- **Expected output:**

| Malicious Clients | Final MTA | Final ASR |
|---|---|---|
| 1 | 0.9749 | 0.0009 |
| 3 | 0.9723 | 0.0009 |
| 5 | 0.9448 | 0.0009 |

---

## Key Parameters

| Parameter | Value |
|---|---|
| Dataset | MNIST |
| Total Clients | 10 |
| Communication Rounds | 20 |
| Local Epochs | 3 |
| Batch Size | 32 |
| Learning Rate | 0.01 |
| Source Class | 1 (digit 1) |
| Target Class | 7 (digit 7) |
| Random Seed | 42 |

---

## Reproducibility

All notebooks use a fixed random seed (`SEED = 42`) for reproducibility:
```python
random.seed(42)
np.random.seed(42)
torch.manual_seed(42)
```

Results may vary slightly depending on hardware (CPU vs GPU) and PyTorch version.
