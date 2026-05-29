# 🚀 NASA Aero-Engine Intelligence: Advanced Multi-Regime Ensemble Prognostics

> **Hybrid Deep Learning Pipeline for C-MAPSS FD002 & FD004 Data Streams**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

---

## 📊 1. Performance Benchmark: NASA Penalty Score
> **Engineering Insight:** 파이프라인의 성과를 먼저 제시합니다. NASA C-MAPSS 데이터셋에서 단일 모델 대비 앙상블 기법이 얼마나 비대칭적 손실(Penalty)을 물리적으로 억제했는지 시각적으로 확증합니다.

| FD002 - Penalty Score (78.4% ↓) | FD004 - Penalty Score (89.2% ↓) |
| :---: | :---: |
| ![FD002](./FD002/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) | ![FD004](./FD004/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) |

---

## 🛠️ 2. Core Workflow (Notebook Pipeline)

### Phase 1: Signal Pre-processing & Normalization
> **Engineering Insight:** 주피터 노트북의 첫 번째 섹션입니다. 고도와 운전 모드가 혼재된 원시 센서 데이터는 전역 정규화 시 기저가 왜곡됩니다. 칼만 필터로 노이즈를 제거한 뒤, 6개 운전 조건별로 인스턴스를 군집화하여 독립적으로 스케일링하는 과정을 수행했습니다.

* **Key Logic:** Kalman Filter Smoothing + Regime-Clustered Min-Max Scaling

### Phase 2: Heterogeneous Architecture Feature Extraction
> **Engineering Insight:** 노트북의 모델링 파트입니다. 서로 다른 수학적 계통인 CNN-BiLSTM(국소 패턴 집중)과 Dual Transformer(전역 상관관계 파악)를 병렬 배치하여, 시계열 데이터의 변곡점을 다각도에서 포착하도록 설계했습니다.

* **Key Logic:** `CNN-BiLSTM-Attention` + `Dual Transformer Encoder`

### Phase 3: Geometric Blending Ensemble
> **Engineering Insight:** 앙상블 섹션입니다. 단순 산술평균은 특정 모델의 오차 폭발(아웃라이어)에 취약합니다. 이를 기하평균 기반 블렌딩으로 처리하여 오차 분포의 우측 꼬리(Heavy-Tail)를 강제로 억제했습니다.

* **Key Logic:** Geometric Averaging to suppress False Negative (FN) Risks

### Phase 4: Operational Boundary Fine-Tuning
> **Engineering Insight:** 마지막 단계는 비즈니스 로직 최적화입니다. 단순히 수치를 예측하는 것에 그치지 않고, 30 사이클(Failure)과 45 사이클(Alert) 경계면을 정밀하게 튜닝하여 실제 현장 정비 윈도우를 확보했습니다.

* **Key Logic:** Business-driven Thresholding (Failure < 30, Alert < 45)

---

## 📈 3. Experimental Results & Diagnostics

### 3.1. Convergence Analysis
| Model | FD002 Convergence | FD004 Convergence |
| :--- | :---: | :---: |
| **CNN-BiLSTM-Attention** | ![FD002 BiLSTM](./FD002/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 BiLSTM](./FD004/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Dual Transformer** | ![FD002 Transformer](./FD002/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Transformer](./FD004/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Geometric Ensemble** | ![FD002 Ensemble](./FD002/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Ensemble](./FD004/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test)%20.png) |

### 3.2. Diagnostic Reliability
> **Engineering Insight:** 잔차(Residuals) 분포와 Scatter Plot 분석을 통해 모델이 0 근방에서 얼마나 강건하게 유지되는지 검증한 결과입니다. 정답선(y=x)에 대한 밀집도는 모델의 최종 신뢰도를 의미합니다.

| Diagnostic | FD002 Plot | FD004 Plot |
| :--- | :---: | :---: |
| **Residuals Distribution** | ![FD002 Res](./FD002/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) | ![FD004 Res](./FD004/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) |
| **Prediction Trend** | ![FD002 Trend](./FD002/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) | ![FD004 Trend](./FD004/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) |
| **Scatter Analysis** | ![FD002 Scatter](./FD002/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) | ![FD004 Scatter](./FD004/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) |

---

## 🤖 4. Integration: Voiceflow Report Builder
> **Concept Note:** 모델의 최종 진단 결과는 **Voiceflow(보이스플로우)** 인터페이스와 통합되어 운용됩니다. 실시간 정비 의사결정이 필요한 경우, 시스템이 생성한 진단 차트 이미지가 챗봇 대화창을 통해 즉각적으로 제공됩니다.

---

## 📂 Repository Structure
```text
.
├── FD002/Visualization/      # FD002 분석 결과 시각화 자산
├── FD004/Visualization/      # FD004 분석 결과 시각화 자산
├── src/                      # 주피터 노트북 로직 기반 코어 파이프라인
└── README.md
