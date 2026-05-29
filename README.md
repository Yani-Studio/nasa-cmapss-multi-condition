# 🚀 NASA Aero-Engine Intelligence: Advanced Multi-Regime Ensemble Prognostics

> **Hybrid Deep Learning Pipeline for C-MAPSS FD002 & FD004 Data Streams**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Hardware: Apple M5](https://img.shields.io/badge/Hardware-Apple_M5_24GB-black.svg)]()
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

---

## 🖥️ Computational Environment
본 파이프라인의 모든 딥러닝 모델 학습 및 시계열 앙상블 연산은 **Apple MacBook Pro (M5 Base Chipset, 24GB Unified Memory)** 환경에서 최적화되었습니다. M5의 고효율 연산 아키텍처를 활용하여, 복합 노이즈가 포함된 항공 엔진 센서 데이터를 실시간 수준으로 처리합니다.

---

## 📊 1. Performance Benchmark: NASA Penalty Score
> **Metric Explanation:** NASA C-MAPSS에서 가장 중요한 지표입니다. 단순히 오차(MSE)를 줄이는 것을 넘어, **'고장인데 정상으로 판단(Late Prediction)'**하는 경우 지수함수적 페널티가 부과됩니다. 본 앙상블 모델은 이 우측 꼬리(Heavy-Tail) 오차를 물리적으로 진압하여 페널티 스코어를 극적으로 개선했습니다.

| FD002 - Penalty Score (78.4% ↓) | FD004 - Penalty Score (89.2% ↓) |
| :---: | :---: |
| ![FD002](./FD002/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) | ![FD004](./FD004/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) |

---

## 🛠️ 2. Core Workflow & Logic (Notebook Pipeline)

### Phase 1: Signal Pre-processing & Normalization
* **Logic:** Kalman Filter Smoothing + Regime-Clustered Min-Max Scaling
* **Meaning:** 6개 운전 모드(Regime)별 데이터의 기저를 맞춰, 단순 전역 정규화 시 발생하는 신호 왜곡을 방지했습니다.

### Phase 2: Heterogeneous Architecture Feature Extraction
* **Logic:** `CNN-BiLSTM-Attention` + `Dual Transformer Encoder`
* **Meaning:** 국소 패턴(CNN-BiLSTM)과 전역 상관관계(Transformer)를 동시에 추출하여, 엔진 열화 시 발생하는 미세한 변곡점을 정밀하게 파싱합니다.

### Phase 3: Geometric Blending Ensemble
* **Logic:** Geometric Averaging (FN Risk Suppression)
* **Meaning:** 일반 산술평균 대신 기하평균을 적용해 특정 모델의 아웃라이어가 전체 예측치를 오염시키지 않도록 강제합니다.

### Phase 4: Operational Boundary Fine-Tuning
* **Logic:** Business-driven Thresholding (Failure < 30, Alert < 45)
* **Meaning:** 기술적 예측치를 실제 정비 현장의 의사결정 마진(30 사이클 미만 고장 정의)과 일치시키는 후처리를 수행했습니다.

---

## 📈 3. Experimental Results & Diagnostics

### 3.1. Model Convergence (Training Loss & Confusion Matrix)
> **Chart Explanation:** 학습 손실 곡선(Loss)은 모델이 데이터의 패턴을 얼마나 안정적으로 학습했는지를 보여줍니다. 오른쪽의 Confusion Matrix는 최종 테스트셋에서 '고장(Failure)'을 놓치지 않고(FN 0에 근접) 정확히 알람(Alert)을 발생시켰는지 검증합니다.

| Model | FD002 Convergence | FD004 Convergence |
| :--- | :---: | :---: |
| **CNN-BiLSTM-Attention** | ![FD002 BiLSTM](./FD002/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 BiLSTM](./FD004/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Dual Transformer** | ![FD002 Transformer](./FD002/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Transformer](./FD004/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |
| **Geometric Ensemble** | ![FD002 Ensemble](./FD002/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Ensemble](./FD004/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test)%20.png) |

### 3.2. Diagnostic Reliability (Residuals & Regression)
> **Chart Explanation:** 잔차(Residual) 분포가 0을 중심으로 좁은 가우시안 밴드를 형성할수록 예측이 정확함을 의미합니다. Scatter Plot의 데이터 포인트들이 $y=x$ 선에 얼마나 조밀하게 밀착되어 있는지가 모델의 회귀 성능(RUL 정밀도)을 최종 증명합니다.

| Diagnostic | FD002 Plot | FD004 Plot |
| :--- | :---: | :---: |
| **Residuals Distribution** | ![FD002 Res](./FD002/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) | ![FD004 Res](./FD004/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) |
| **Prediction Trend** | ![FD002 Trend](./FD002/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) | ![FD004 Trend](./FD004/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) |
| **Scatter Analysis** | ![FD002 Scatter](./FD002/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) | ![FD004 Scatter](./FD004/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) |

---

## 🤖 4. Integration: Voiceflow Report Builder
> **Integration Note:** 본 파이프라인의 최종 진단 결과는 **Voiceflow(보이스플로우)** 챗봇 대화창과 연동됩니다. 모델이 생성한 실시간 정비 의사결정 차트 및 리포트는 이미지 형태로 즉각 시각화되어 현장 정비사에게 전달됩니다.

---

## 📂 Repository Structure
```text
.
├── FD002/Visualization/      # FD002 정밀 분석 결과
├── FD004/Visualization/      # FD004 정밀 분석 결과
├── src/                      # 파이프라인 코어 로직
└── README.md
