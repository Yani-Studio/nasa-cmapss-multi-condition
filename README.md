# 🚀 Dual-Engine Hybrid Prognostics: Multi-Regime Ensemble Pipeline

> **NASA C-MAPSS FD002 & FD004 Multi-Regime Prognostics Framework**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

---

## 📊 1. NASA Penalty Score: Pairwise Comparison
Baseline 단일 모델 대비 앙상블 프레임워크의 성능을 즉각적으로 확인합니다. 

| FD002 - Penalty Score (78.4% ↓) | FD004 - Penalty Score (89.2% ↓) |
| :---: | :---: |
| ![FD002](./FD002/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) | ![FD004](./FD004/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) |

---

## 📌 Project Overview
본 프로젝트는 **NASA C-MAPSS** 데이터셋의 다중 운전 조건 환경에서, **기하학적 블렌딩(Geometric Blending)** 앙상블 기법을 통해 **NASA Penalty Score** 폭발 문제를 물리적으로 억제한 고성능 예지보전(PdM) 파이프라인입니다.

* **FD002:** 6개 운전 모드(Regimes) 노이즈 복합 환경 타깃
* **FD004:** 복합 열화(HPC/Fan Degradation) 중첩 시나리오 타깃

---

## 🛠️ Pipeline Architecture
1. **Phase 1 (Normalizer):** 칼만 필터(Kalman Filter) 평활화 및 Regime-based Min-Max 정규화.
2. **Phase 2 (Feature Extraction):** `CNN-BiLSTM-Attention` & `Dual Transformer Encoder` 병렬 매핑.
3. **Phase 3 (Ensemble):** 기하학적 블렌딩을 통한 Heavy-Tail 리스크 및 잔차 분산 최소화.
4. **Phase 4 (Boundary Tuning):** 정비 운영 마진 최적화 (Failure < 30, Alert < 45).

---

## 📈 Result Showcase

### 3.1. Model Evaluation & Convergence
| Model | FD002 Evaluation | FD004 Evaluation |
| :--- | :---: | :---: |
| **CNN-BiLSTM-Attention** | ![FD002 BiLSTM](./FD002/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 BiLSTM](./FD004/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Dual Transformer** | ![FD002 Transformer](./FD002/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Transformer](./FD004/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Geometric Ensemble** | ![FD002 Ensemble](./FD002/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Ensemble](./FD004/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test)%20.png) |

### 3.2. Global Performance Diagnostics
| Diagnostic | FD002 Plot | FD004 Plot |
| :--- | :---: | :---: |
| **Residuals Distribution** | ![FD002 Res](./FD002/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) | ![FD004 Res](./FD004/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) |
| **RUL Prediction Trend** | ![FD002 Trend](./FD002/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) | ![FD004 Trend](./FD004/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) |
| **Scatter Plot (y=x)** | ![FD002 Scatter](./FD002/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) | ![FD004 Scatter](./FD004/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) |

---

## 🤖 Downstream: Automated Report Builder
```python
# 진짜 테스트셋 기반 예지보전 챗봇 리포트 생성
target_failure_threshold = 30
optimal_threshold = 45

# ... (Decision Logic: TP, TN, FP, FN classification)
# Result: High Precision & Recall in Failure Prediction
