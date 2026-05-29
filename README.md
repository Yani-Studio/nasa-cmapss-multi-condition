# 🚀 NASA Aero-Engine Intelligence: Advanced Multi-Regime Ensemble Prognostics

> **Hybrid Deep Learning Pipeline for C-MAPSS FD002 & FD004 Data Streams**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Hardware: Apple M5](https://img.shields.io/badge/Hardware-Apple_M5_24GB-black.svg)]()
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

---

## 🖥️ Computational Environment
All deep learning model training and time-series ensemble operations for this pipeline have been optimized for the **Apple MacBook Pro (M5 Base Chipset, 24GB Unified Memory)** environment. By leveraging the M5's high-efficiency computing architecture, we process complex aerospace engine sensor data at real-time speeds.

---

## 📊 1. Performance Benchmark: NASA Penalty Score
> **Metric Explanation:** This is the most critical metric for the NASA C-MAPSS dataset. Beyond simply reducing Mean Squared Error (MSE), our ensemble model dramatically improves penalty scores by mitigating 'Late Predictions' (identifying a failure as normal), which are heavily penalized. Our model physically suppresses these heavy-tail errors.

| FD002 - Penalty Score (78.4% ↓) | FD004 - Penalty Score (89.2% ↓) |
| :---: | :---: |
| ![FD002](./FD002/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) | ![FD004](./FD004/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) |

---

## 🛠️ 2. Core Workflow & Logic (Notebook Pipeline)

### Phase 1: Signal Pre-processing & Normalization
* **Logic:** Kalman Filter Smoothing + Regime-Clustered Min-Max Scaling
* **Meaning:** By aligning the baseline for each of the 6 operating regimes, we prevent signal distortion that occurs with standard global normalization.

### Phase 2: Heterogeneous Architecture Feature Extraction
* **Logic:** `CNN-BiLSTM-Attention` + `Dual Transformer Encoder`
* **Meaning:** We simultaneously extract local patterns (CNN-BiLSTM) and global correlations (Transformer) to precisely parse minute inflection points occurring during engine degradation.

### Phase 3: Geometric Blending Ensemble
* **Logic:** Geometric Averaging (FN Risk Suppression)
* **Meaning:** By applying geometric averaging instead of simple arithmetic mean, we force the model to ensure that outliers from specific sub-models do not contaminate the overall prediction.

### Phase 4: Operational Boundary Fine-Tuning
* **Logic:** Business-driven Thresholding (Failure < 30, Alert < 45)
* **Meaning:** We performed post-processing to align technical predictions with real-world maintenance decision margins (defining failure as < 30 cycles).

---

## 📈 3. Experimental Results & Diagnostics

### 3.1. Model Convergence (Training Loss & Confusion Matrix)
> **Chart Explanation:** The Training Loss curve demonstrates the stability of the model's pattern learning. The confusion matrix on the right verifies that our final test set captures 'Failures' without error (FN nearing 0) and triggers 'Alerts' precisely.

| Model | FD002 Convergence | FD004 Convergence |
| :--- | :--- | :--- |
| **CNN-BiLSTM-Attention** | ![FD002 BiLSTM](./FD002/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 BiLSTM](./FD004/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Dual Transformer** | ![FD002 Transformer](./FD002/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Transformer](./FD004/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Geometric Ensemble** | ![FD002 Ensemble](./FD002/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Ensemble](./FD004/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test)%20.png) |

### 3.2. Diagnostic Reliability (Residuals & Regression)
> **Chart Explanation:** Residual distribution forming a tight Gaussian band around 0 indicates high prediction accuracy. The closer the data points in the scatter plot are to the $y=x$ line, the stronger the regression performance (RUL precision) of the model.

| Diagnostic | FD002 Plot | FD004 Plot |
| :--- | :--- | :--- |
| **Residuals Distribution** | ![FD002 Res](./FD002/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) | ![FD004 Res](./FD004/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) |
| **Prediction Trend** | ![FD002 Trend](./FD002/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) | ![FD004 Trend](./FD004/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) |
| **Scatter Analysis** | ![FD002 Scatter](./FD002/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) | ![FD004 Scatter](./FD004/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) |

---

## 🤖 4. Integration: Voiceflow Report Builder

> **Concept:** This system connects the prediction engine (MacBook M5) to the field interface (Voiceflow chatbot) via API, transforming the complex inferences of the deep learning model into an immediate **Maintenance Decision Report**.

### 4.1. Data-to-Insight Workflow
This pipeline enables technicians to take immediate action without separate data interpretation by following this automated workflow:

1.  **Inference Execution:** The `CombinedModel` receives engine sensor data and infers RUL (Remaining Useful Life) and failure probability in real-time.
2.  **Automated Visualization:** The visualization engine automatically renders the inference results (regression results and residual distribution charts) and saves them as image files with unique IDs.
3.  **Webhook Trigger:** A JSON payload containing the image path and Maintenance Action Plan is sent to the Voiceflow API.
4.  **Field Delivery:** Field technicians instantly check the charts on their mobile chatbot interface and decide whether to perform immediate maintenance.

### 4.2. Delivery Payload Structure
For efficient inter-system communication, we use the following JSON payload structure:

# Predictive Maintenance & RUL Estimation System

This repository provides a framework for Remaining Useful Life (RUL) estimation and State of Health (SOH) monitoring, leveraging deep learning models on the NASA C-MAPSS dataset.

## 4.3. Strategic Impact

* **Latency Reduction:** Reduces the time from data analysis to maintenance recommendation to a matter of minutes.
* **Interpretation Support:** Simplifies complex model indicators into a 'Maintenance Guide,' enabling intuitive decision-making for non-experts in the field.
* **Continuous Feedback:** Uses field maintenance result data as a feedback loop to the model, laying the foundation for continuous tuning of False Positive rates.

## 📚 References
* **Dataset:** [NASA C-MAPSS Data Repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)
* **CNN-BiLSTM-Attention:** [Attention-based CNN-BiLSTM for SOH and RUL estimation](https://doi.org/10.1177/17483026221130598)
* **Transformer-based:** [A Dual-Scale Transformer-Based Remaining Useful Life Prediction Model](https://irep.ntu.ac.uk/id/eprint/51147/1/1877636_Mumtaz.pdf)

## Repository Structure

```text
.
├── FD002/Visualization/ # FD002 Detailed analysis results
├── FD004/Visualization/ # FD004 Detailed analysis results
├── src/                 # Pipeline core logic
└── README.md            # Project documentation

