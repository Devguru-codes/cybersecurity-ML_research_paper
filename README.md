# Revisiting ML Models for Intrusion Detection  
### Do Tree-Based Methods Win on Both Known and Zero-Day Attacks?

> A research-focused implementation comparing tree-based machine learning models against deep learning approaches for intrusion detection on the CICIDS2017 dataset.

---

# 📌 Overview

This repository contains the implementation and experimentation pipeline for our research paper:

**“Revisiting ML Models for Intrusion Detection: Do Tree-Based Methods Win on Both Known and Zero-Day Attacks?”**

The project evaluates:

## Supervised Tree Models
- Random Forest
- XGBoost
- LightGBM
- CatBoost

## Unsupervised Tree Models
- Isolation Forest
- Extended Isolation Forest (EIF)

against deep learning baselines such as:
- MLP
- CNN
- OCSVM
- LOF

using the CICIDS2017 dataset for:
- Known attack detection
- Zero-day attack detection

---

# 🚀 Key Results

| Task | Best Model | F1 Score |
|---|---|---|
| Known Attack Detection | CatBoost | **0.9643** |
| Zero-Day Detection | Extended Isolation Forest | **0.7821** |

## Major Findings
- CatBoost outperformed MLP by 2.0% F1
- EIF outperformed OCSVM by 2.5% F1
- Tree-based methods dominated deep learning on structured network traffic data
- SHAP analysis revealed:
  - Supervised models learn signature-like patterns
  - Unsupervised models learn behavioral anomalies

---

# 🧠 Research Motivation

Recent IDS benchmarks heavily favored deep learning architectures while ignoring gradient-boosted tree ensembles.

This work revisits that assumption and asks:

> “Can tree-based models outperform deep learning on both known and unseen attacks?”

The answer, based on extensive experimentation and explainability analysis, is YES.

---

# 📂 Project Structure

```bash
.
├── data/
│   ├── raw/
│   ├── processed/
│   └── splits/
│
├── notebooks/
│   ├── preprocessing.ipynb
│   ├── supervised_training.ipynb
│   ├── unsupervised_training.ipynb
│   ├── shap_analysis.ipynb
│   └── evaluation.ipynb
│
├── models/
│   ├── catboost/
│   ├── xgboost/
│   ├── lightgbm/
│   ├── random_forest/
│   ├── isolation_forest/
│   └── extended_if/
│
├── src/
│   ├── preprocessing/
│   ├── training/
│   ├── evaluation/
│   ├── explainability/
│   └── utils/
│
├── outputs/
│   ├── confusion_matrices/
│   ├── shap_plots/
│   ├── metrics/
│   └── logs/
│
├── paper/
│   └── cybe_paper_final.pdf
│
├── requirements.txt
├── README.md
└── LICENSE
```

---

# 📊 Dataset

## CICIDS2017

We use the CICIDS2017 intrusion detection benchmark dataset containing:

- 2.83 million network flow records
- 80+ engineered network features
- 14 attack categories
- Realistic benign + malicious traffic patterns

## Zero-Day Holdout Protocol

The following attacks were completely removed from training:

- DoS slowloris
- DoS Slowhttptest
- Bot

These attacks were used exclusively for evaluating true zero-day generalization.

---

# ⚙️ Methodology

## Data Processing Pipeline

### Preprocessing
- Missing value handling
- Infinite value replacement
- StandardScaler normalization
- Train-test split with `random_state=42`

### Training Strategy

#### Supervised Models
Trained on:
- Benign traffic
- Known attacks

#### Unsupervised Models
Trained only on:
- Benign traffic

This setup enables realistic anomaly detection evaluation.

---

# 🌲 Models Used

## Supervised Models

### 1. Random Forest
- Bootstrap aggregation
- Ensemble voting
- Baseline tree ensemble

### 2. XGBoost
- Gradient boosting
- Regularized optimization
- Histogram tree method

### 3. LightGBM
- GOSS sampling
- Leaf-wise growth
- Exclusive Feature Bundling

### 4. CatBoost
- Ordered boosting
- Oblivious trees
- Leakage-resistant training

---

## Unsupervised Models

### 1. Isolation Forest
- Random recursive partitioning
- Anomaly scoring via path length

### 2. Extended Isolation Forest
- Oblique hyperplane splits
- Removes axis-aligned geometric bias
- Better multivariate anomaly detection

---

# 🔬 Hyperparameter Optimization

All models were optimized using:

## Optuna
- TPE sampler
- Validation F1 optimization
- 50 trials per supervised model

## Best Configurations

| Model | Important Parameters |
|---|---|
| CatBoost | depth=8, iterations=125 |
| XGBoost | max_depth=5, lr=0.1964 |
| LightGBM | num_leaves=144 |
| Random Forest | max_depth=15 |
| EIF | contamination=0.05 |

---

# 📈 Results

# 1️⃣ Known Attack Detection

| Model | F1 Score |
|---|---|
| MLP | 0.9446 |
| CNN | 0.9160 |
| Random Forest | 0.9465 |
| LightGBM | 0.9610 |
| XGBoost | 0.9635 |
| CatBoost | **0.9643** |

## Observations
- All tree models outperformed deep learning
- CatBoost achieved SOTA performance
- Gradient boosting significantly improved recall

---

# 2️⃣ Zero-Day Detection

| Model | F1 Score |
|---|---|
| MLP | 0.2973 |
| CNN | 0.3218 |
| LOF | 0.6814 |
| OCSVM | 0.7575 |
| Isolation Forest | 0.7762 |
| Extended IF | **0.7821** |

## Observations
- Deep learning collapsed on unseen attacks
- EIF achieved the best balance between:
  - precision
  - recall
  - operational usability

---

# 🔍 Explainability with SHAP

We performed full SHAP analysis on all tree models.

## Supervised Models Learned:
- Destination Port
- TCP Window Sizes

➡ Signature-like behavior

## Unsupervised Models Learned:
- Packet timing anomalies
- Flow inter-arrival patterns
- Packet length irregularities

➡ Behavioral anomaly detection

This explains why unsupervised trees generalize better to zero-day attacks.

---

# 🛠️ Tech Stack

## Languages
- Python 3.11

## ML Libraries
- Scikit-learn
- XGBoost
- LightGBM
- CatBoost
- PyOD
- SHAP
- Optuna

## Data Processing
- NumPy
- Pandas

## Visualization
- Matplotlib
- SHAP

---

# 💻 Hardware Used

| Component | Specification |
|---|---|
| CPU | AMD Ryzen 7 8845HS |
| GPU | RTX 4060 Laptop GPU |
| RAM | 16GB |

---

# 📦 Installation

```bash
git clone https://github.com/your-username/tree-based-ids.git

cd tree-based-ids
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# ▶️ Usage

## Run Supervised Training

```bash
python train_supervised.py
```

## Run Unsupervised Training

```bash
python train_unsupervised.py
```

## Generate SHAP Explanations

```bash
python shap_analysis.py
```

## Evaluate Models

```bash
python evaluate.py
```

---

# 📊 Generated Outputs

The project automatically generates:

- Confusion matrices
- Precision/Recall metrics
- F1-score comparisons
- SHAP summary plots
- Feature importance rankings

---

# 🧪 Experimental Insights

## Why Tree Models Win

### Deep Learning Fails Because:
- Network traffic is tabular
- Features are heterogeneous
- No compositional structure exists
- Neural embeddings are ineffective

### Trees Succeed Because:
- They naturally model irregular decision boundaries
- No embedding assumptions required
- Better handling of feature heterogeneity

---

# 🏗️ Recommended Real-World IDS Architecture

## Hybrid Two-Stage IDS

### Stage 1 — CatBoost
- Fast classification
- Known attack filtering

### Stage 2 — Extended Isolation Forest
- Zero-day anomaly detection
- Behavioral analysis

This architecture provides:
- High precision
- Strong zero-day generalization
- Explainable security alerts

---

# 📚 Research Paper

The full research paper is available in:

```bash
paper/cybe_paper_final.pdf
```

---

# 📖 Citation

```bibtex
@article{treeids2026,
  title={Revisiting ML Models for Intrusion Detection:
  Do Tree-Based Methods Win on Both Known and Zero-Day Attacks?},
  author={Dhruv Singh and Raghav Singh and Devguru Tiwari and Nischal Yeremseti},
  journal={Research Paper},
  year={2026}
}
```

---

# 🤝 Contributors

- Dhruv Singh
- Raghav Singh
- Devguru Tiwari
- Nischal Yeremseti

---

# ⭐ Future Work

- CICIDS2018 evaluation
- Real-time IDS deployment
- Streaming inference pipeline
- Transformer-based comparisons
- Federated intrusion detection
- Adversarial robustness testing

---

# 📜 License

This project is released under the MIT License.

---

# 🌟 Final Takeaway

> Tree-based methods are not only competitive with deep learning for intrusion detection — they are superior for both known attack classification and zero-day anomaly detection on structured network traffic data.
