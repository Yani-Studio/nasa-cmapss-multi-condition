# 🚀 Dual-Engine Hybrid Prognostics: Multi-Regime Ensemble Pipeline

> **NASA C-MAPSS FD002 & FD004 Multi-Regime Prognostics Framework**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

---

## 📌 Project Overview
본 프로젝트는 **NASA C-MAPSS (Commercial Modular Aero-Propulsion System Simulation)** 데이터셋의 다중 운전 조건(Multi-Regime) 환경에서, 앙상블 블렌딩 기법을 통해 **NASA Penalty Score**를 혁신적으로 최적화한 고성능 예지보전(PdM) 파이프라인입니다.

* **FD002:** 6개 운전 모드(Regimes) 노이즈 복합 환경 타깃
* **FD004:** 복합 열화(HPC/Fan Degradation) 중첩 시나리오 타깃

---

## 📈 Performance Benchmarking
Baseline 단일 모델 대비 본 프레임워크가 달성한 페널티 감소율입니다.

| Dataset | Metric (Penalty Score) | Improvement |
| :--- | :--- | :---: |
| **FD002** | 78.4% Reduction | ⚡ High |
| **FD004** | 89.2% Reduction | 🔥 Extreme |

---

## 🛠️ Pipeline Architecture
1. **Phase 1: Operational Regime Clustered Normalization**
   * 칼만 필터(Kalman Filter) 기반 신호 평활화 및 Regime-based Min-Max 정규화.
2. **Phase 2: Heterogeneous Feature Extraction**
   * `CNN-BiLSTM-Attention` 및 `Dual Transformer Encoder` 병렬 매핑.
3. **Phase 3: Geometric Blending Ensemble**
   * Late Prediction 및 Heavy-Tail 리스크 물리적 진압.
4. **Phase 4: Operational Boundary Fine-Tuning**
   * 정비 운영 마진 최적화 (Failure < 30, Alert < 45).

---

## 📊 Result Showcase

### Geometric Ensemble Stability (Real Test)
| Ensemble Model | FD002 Convergence | FD004 Convergence |
| :--- | :---: | :---: |
| **Geometric Blending** | ![FD002](./FD002/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004](./FD004/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test)%20.png) |

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install -r requirements.txt
