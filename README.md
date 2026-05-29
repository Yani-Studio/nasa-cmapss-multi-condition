# 🏭 Multi-Model Manufacturing Defect Detection & Evaluation

An advanced deep learning framework designed for high-precision manufacturing process optimization and defect classification.

---

## 📈 Performance Benchmarking (Real Test)

| Model Architecture | Class | Accuracy | Precision | Recall | F1-Score |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **1. CNN-BiLSTM-ATT** | Normal / Failure | 89.36% | 93.28% / 79.63% | 91.91% / 82.69% | 92.59% / 81.13% |
| **2. DUAL TRANSFORMER** | Normal / Failure | 94.15% | 98.45% / 84.75% | 93.38% / 96.15% | 95.85% / 90.09% |
| **3. SOFTMIN ENSEMBLE** | Normal / Failure | 92.02% | 95.49% / 83.64% | 93.38% / 88.46% | 94.42% / 85.98% |

---

## 📊 Experimental Visualizations

### 1️⃣ CNN-BiLSTM-Attention Network
![CNN-BiLSTM-ATT](assets/CNN-BILSTM-ATT.png)

### 2️⃣ Dual Transformer Architecture
![DUAL TRANSFORMER](assets/DUAL-TRANSFORMER.png)

### 3️⃣ Softmin Ensemble Fine-Tuning
![SOFTMIN ENSEMBLE](assets/SOFTMIN-ENSEMBLE.png)

---

## 🛠️ Pipeline Execution
Run the following commands to reproduce the benchmark results:

```bash
# Data Preprocessing & Training
python train_models.py --architectures cnn_bilstm transformer softmin

# Evaluation & Matrix Generation
python evaluate.py --test_mode real
