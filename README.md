# 🚀 NASA Aero-Engine Intelligence: Advanced Multi-Regime Ensemble Prognostics

> **Hybrid Deep Learning Pipeline for C-MAPSS FD002 & FD004 Data Streams with Real-Time Voiceflow PHM Field Integration**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Hardware: Apple M5](https://img.shields.io/badge/Hardware-Apple_M5_24GB-black.svg)]()
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

---

## ⚙️ System Target: Gas Turbine Aero-Engine Architecture
This project tracks the internal thermodynamic degradation patterns of gas turbine engines operating under highly volatile multi-regime conditions. The target system components (Fan, LPC, HPC, Combustor, HPT, LPT, Nozzle) and their corresponding 3D cross-sectional profiles utilized for prognostic validation are structured below:

| 2D Component Guidelines & Sensor Locations (Schematic) | 3D Structural Cross-Sectional Framework |
| :---: | :---: |
| ![Gas Turbine 2D](./snake_case/gas_turbine_2d_schematic.png) | ![Gas Turbine 3D](./snake_case/gas_turbine_3d_cross_section.jpeg) |

---

## 🖥️ Computational Environment
All deep learning model training and time-series ensemble operations for this pipeline have been optimized for the **Apple MacBook Pro (M5 Base Chipset, 24GB Unified Memory)** environment. By leveraging the M5's high-efficiency computing architecture, we process complex aerospace engine sensor data at real-time speeds.

---

## 📊 1. Performance Benchmark: NASA Penalty Score

> **Metric Explanation:** This is the most critical metric for the NASA C-MAPSS dataset. Beyond simply reducing Mean Squared Error (MSE), our ensemble model dramatically improves penalty scores by mitigating 'Late Predictions' (identifying a failure as normal), which are heavily penalized. Our model physically suppresses these heavy-tail errors.

| Dataset Classification | Operational Complexity | Performance Improvement (NASA Penalty Score) |
| :---: | :--- | :---: |
| **FD002** | 6 Operating Regimes, 1 Fault Mode | **42.3% ↓** Penalty Reduction (vs. CNN-BiLSTM-Att) |
| **FD004** | 6 Operating Regimes, 2 Fault Modes | **90.4% ↓** Penalty Reduction (vs. CNN-BiLSTM-Att) |

### Penalty Score Visualization
* **FD002 Evaluation:** ![FD002](./FD002/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png)
* **FD004 Evaluation:** ![FD004](./FD004/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png)

---

## 🛠️ 2. Core Workflow & Logic (Notebook Pipeline)

### Phase 1: Signal Pre-processing & Normalization
* **Logic:** Kalman Filter Smoothing + Regime-Clustered Min-Max Scaling
* **Meaning:** By aligning the baseline for each of the 6 operating regimes, we prevent signal distortion that occurs with standard global normalization.

### Phase 2: Heterogeneous Architecture Feature Extraction
* **Logic:** `CNN-BiLSTM-Attention` + `Dual Transformer Encoder`
* **Meaning:** We simultaneously extract local patterns (CNN-BiLSTM) and global correlations (Transformer) to precisely parse minute inflection points occurring during engine degradation.

### Phase 3: Heterogeneous Ensemble Strategies
* **Logic:**
    * **FD002 (Geometric Ensemble):** Geometric Averaging for FN Risk Suppression.
    * **FD004 (Soft-Min Ensemble):** Soft-Min Weighting for Risk-Aware Scaling.
* **Meaning:** * **FD002:** By applying geometric averaging, we ensure that outliers from specific sub-models do not contaminate the overall prediction, effectively suppressing False Negatives.
    * **FD004:** We utilize the Soft-Min function to assign higher weights to lower predicted RUL values (i.e., identifying earlier failure warnings), effectively prioritizing critical risk detection in complex, noisy environments.

### Phase 4: Operational Boundary Fine-Tuning
* **Logic:** Business-driven Thresholding (Failure < 30, Alert < 35)
* **Meaning:** We performed post-processing to align technical predictions with real-world maintenance decision margins (defining failure as < 30 cycles).

---

## 📈 3. Experimental Results & Diagnostics

### 3.1. Model Convergence (Training Loss & Confusion Matrix)
> **Chart Explanation:** The Training Loss curve demonstrates the stability of the model's pattern learning. The confusion matrix on the right verifies that our final test set captures 'Failures' without error (FN nearing 0) and triggers 'Alerts' precisely.

| Model Architecture | FD002 Convergence & Confusion Matrix | FD004 Convergence & Confusion Matrix |
| :--- | :---: | :---: |
| **CNN-BiLSTM-Attention** | ![FD002 BiLSTM](./FD002/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 BiLSTM](./FD004/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Dual Transformer** | ![FD002 Transformer](./FD002/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Transformer](./FD004/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Ensemble (Geo/Soft-Min)** | ![FD002 Ensemble](./FD002/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Ensemble](./FD004/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test)%20.png) |

### 3.2. Diagnostic Reliability (Residuals & Regression)
> **Chart Explanation:** Residual distribution forming a tight Gaussian band around 0 indicates high prediction accuracy. The closer the data points in the scatter plot are to the $y=x$ line, the stronger the regression performance (RUL precision) of the model.

| Diagnostic Metric | FD002 Plot Analysis | FD004 Plot Analysis |
| :--- | :---: | :---: |
| **Residuals Distribution** | ![FD002 Res](./FD002/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) | ![FD004 Res](./FD004/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) |
| **Prediction Trend (Sorted)** | ![FD002 Trend](./FD002/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) | ![FD004 Trend](./FD004/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) |
| **Scatter Analysis (True vs Pred)** | ![FD002 Scatter](./FD002/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) | ![FD004 Scatter](./FD004/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) |

---

## 🤖 4. Integration: Voiceflow PHM Report Builder

Advanced RUL inference outputs generated from our M5-backed pipeline are integrated directly via API with the **Voiceflow Intelligent PHM Agent Interface**. This empowers field technicians to query and intercept real-time fleet health statuses instantly.

### 4.1. Knowledge Base & Vector Core Structure
To manage massive engine lifecycle metrics and physics-of-failure documents for real-time RAG (Retrieval-Augmented Generation), we decoupled the agent's knowledge base into two primary data streams:

* `chatbot_analysis_report_2.txt`: Structured text report containing operational history, variables, and model inference outputs for the fleet.
* `Damage Propagation Modeling.pdf`: Detailed failure physics manual detailing crack propagation modeling and thermodynamic lifespan degradation formulas.

![Voiceflow Knowledge Base](./snake_case/voiceflow_knowledge_base.png)

### 4.2. Agent Prompt Architecture & Gatekeeper Logic
To prevent hallucinations and conversational drift, we enforce a strict, mutually exclusive 3-stage state machine (`Mutually Exclusive Mode Selection`) along with tight output constraints inside the global instructions layer:

![Voiceflow Agent Instructions](./snake_case/voiceflow_agent_instructions.png)

* **MODE 1: Initial Greeting / Session Startup**
    * Restricts the initial output rigidly to the designated system message: `"Engine diagnostic bot here. What can I diagnose for you?"` Absolute prohibition of raw JSON or arbitrary greetings.
* **MODE 2: Casual Small Talk & Acknowledgments**
    * Intercepts out-of-domain requests (e.g., weather inquiries) and routes them back politely but firmly to turbine health diagnostics.
* **MODE 3: Explicit Data-Driven Engine Diagnosis**
    * Triggered exclusively when a valid `Engine_ID` is parsed. It fetches matching vector embeddings and structural constraints to synthesize a single, clean JSON block (`Single JSON Synthesis Constraint`).
* **Domain Criteria Decision Rules:**
    * **Failure Condition:** If `Actual_RUL` < 30 cycles, map to `Actual_Status: Failure`.
    * **Alert Condition:** If `Predicted_RUL` < 35 cycles, map to `Predicted_Status: Alert`.

### 4.3. Live Chat Execution Log & Diagnostics Result
Below is the execution stream captured during live multi-turn field diagnostic testing, demonstrating successful state transitions and RAG evaluations:

![Voiceflow Execution](./snake_case/voiceflow_chat_execution.png)

#### 📝 Live Test Scenario & Diagnostic Result Summary Table

| Evaluation Target | User Input Context | System Inference Status & Action Plan (JSON Parse Result) |
| :--- | :--- | :--- |
| **Domain Out Check** | "What is the temperature tonight?" | Mode 2 Triggered. Rejects weather scope gracefully and re-pivots to turbine health support. |
| **Engine 203** | "Please analyze the current health status and operational margin of Engine 203." | `Current_Cycle: 192`, `Actual_RUL: 112`, `Predicted_RUL: 106.7`<br>Both metrics sit safely above decision boundaries. System maps status as **Normal** and recommends continuous monitoring. |
| **Engine 32** | "Please provide a professional diagnostic evaluation based on the current cycle of Engine 32." | `Current_Cycle: 170`, `Actual_RUL: 130`, `Predicted_RUL: 125.6`<br>The delta between actual and predicted metrics is a tight 4.4 cycles, verifying excellent model fidelity. Status remains **Normal**. |
| **Engine 58** | "Please evaluate the degradation risk and Health Index of Engine 58 at the 245-cycle mark." | `Current_Cycle: 247`, `Actual_RUL: 26`, `Predicted_RUL: 22.3`<br>**Critical Risk Detected:** Metrics drop below the 30-cycle failure threshold. The system registers a True Positive (TP) fault detection and flags an immediate **"Failure"** status, ordering an immediate maintenance intervention. |

---

## 📚 References
* **Dataset:** [NASA C-MAPSS Data Repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)
* **CNN-BiLSTM-Attention:** [Attention-based CNN-BiLSTM for SOH and RUL estimation](https://doi.org/10.1177/17483026221130598)
* **Transformer-based:** [A Dual-Scale Transformer-Based Remaining Useful Life Prediction Model](https://irep.ntu.ac.uk/id/eprint/51147/1/1877636_Mumtaz.pdf)

## Repository Structure
```text
.
├── FD002/Visualization/ # FD002 Detailed analysis results
├── FD004/Visualization/ # FD004 Detailed analysis results
├── snake_case/          # Voiceflow Interface & Gas Turbine Components Layout Figures
├── src/                 # Pipeline core logic
└── README.md            # Project documentation
