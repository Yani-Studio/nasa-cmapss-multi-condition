# Dual-Engine Hybrid Prognostics: Multi-Regime Ensemble Pipeline for NASA C-MAPSS FD002 & FD004

---

## 📊 1. Benchmark Evaluation & Ensemble Validation

본 프레임워크는 6개 다중 운전 조건(Multi-Regime) 노이즈가 복합적으로 작용하는 **FD002** 시나리오와 복합 부품 열화(HPC/Fan Degradation) 고장 모드까지 중첩된 최하위 한계 검증 시나리오인 **FD004**를 타깃으로 설계된 고성능 예지보전(PdM) 파이프라인입니다.

단일 모델 사용 시 발생하는 지수함수적 오차인 **NASA Penalty Score** 폭발 문제를 완벽하게 진압하여, 제안하는 기하학적 블렌딩(Geometric Blending) 및 운전 임계치 파인튜닝 파이프라인의 산업적 타당성을 정량적으로 확증합니다.

### 📈 NASA Penalty Score Pairwise Comparison
> **앙상블 검증 타당성 (Validation Justification):** 실측 평가 결과 Baseline 단일 모델 대비 **FD002 데이터셋에서 약 78.4%의 오차 스코어 절감**, 복합 열화 조건인 **FD004 데이터셋에서 약 89.2%의 폭발적인 패널티 감소**를 달성했습니다. 이는 비대칭적 손실 페널티 환경에서 본 하이브리드 블렌딩 프레임워크가 자산 파손 리스크를 최소화하는 데 필수적임을 증명합니다.

| NASA C-MAPSS FD002 - Penalty Score (78.4% ↓) | NASA C-MAPSS FD004 - Penalty Score (89.2% ↓) |
| :---: | :---: |
| ![FD002 NASA Penalty Score](/Users/gyuminkang/Desktop/nasa/FD002/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) | ![FD004 NASA Penalty Score](/Users/gyuminkang/Desktop/nasa/FD004/Visualization/NASA%20Penalty%20Score%20Comparison%20(Lower%20is%20Better).png) |

---

## 🛠️ 2. End-to-End Fine-Tuning Pipeline Description

원시 센서 신호 스트림 전처리부터 실시간 정비 의사결정 리포트 빌더 연동까지의 전 과정은 다음과 같은 유기적 4단계 말단 파인튜닝 파이프라인을 거쳐 수행됩니다.

* **Phase 1: Operational Regime Clustered Normalization (운전 조건별 군집 정규화)**
  고도, 마하 수, 스로틀 리졸버 설정에 따라 총 6가지 운전 조건(Regimes)이 교차하는 환경 특성상 전역 스케일링은 신호 기저를 왜곡합니다. 이를 방어하기 위해 미세 가우스 노이즈를 필터링하는 칼만 필터(Kalman Filter) 평활화를 거친 후, 6개 Regime별로 인스턴스를 군집화하여 독립적인 Min-Max 정규화를 수행함으로써 다채널 신호 원점을 일치시켰습니다.

* **Phase 2: Heterogeneous Architecture Feature Extraction (이종 특징 추출)**
  시계열의 국소 및 전역적 특징 변곡점을 완벽히 파싱하기 위해 독립된 수학적 계통의 백본 네트워크 2종을 병렬 매핑합니다.
  1. **CNN-BiLSTM-Attention**: 1D CNN 계층을 통해 센서 간의 국소 공간 패턴을 압축하고, 양방향 BiLSTM 구조를 통해 장기 시계열 맥락 정보를 추출한 뒤, 어텐션 계층을 통해 급격한 마모 임계 구간에 고전력 가중치를 부여합니다.
  2. **Dual Transformer Encoder**: 멀티 헤드 셀프 어텐션(Multi-Head Self-Attention) 메커니즘을 기반으로 전체 생애 주기 시퀀스의 전역적 상관관계를 추적하여 급격한 열화 가속 시점을 정밀 파악합니다.

* **Phase 3: Geometric Blending Ensemble Platform (기하학적 블렌딩 앙상블)**
  일반적인 산술평균 기반 앙상블은 단일 모델이 과대평가 아웃라이어(Late Prediction)를 도출할 경우 패널티가 동반 폭발하는 치명적인 구조적 한계가 존재합니다. 본 파이프라인은 두 이종 모델의 추정치를 기하평균 기반으로 블렌딩하여 오차 분포의 우측 Heavy-Tail(늦은 알람 리스크) 구역을 물리적으로 진압하고 잔차 분산을 0 근방으로 밀집시킵니다.

* **Phase 4: Operational Boundary Fine-Tuning (운전 경계면 최적화)**
  최종 추정된 RUL 값은 예지보전 비즈니스 정비 운영 마진 룰에 직결됩니다. 실제 장비의 붕괴 전조 마진을 30 사이클 미만(`Failure`)으로 정의하고, 모델의 사전 알람 마진을 45 사이클 미만(`Alert`)으로 정밀 미세조정(Fine-Tuning)하여 과잉 정비 비용 최소화와 자산 보호 간의 최적 정비 윈도우를 확보합니다.

---

## 📈 3. Pairwise Experimental Result Showcase

하이브리드 파이프라인의 견고성과 도메인 범용성을 즉각적으로 상호 대조할 수 있도록 모든 검증 시각화 자산은 **FD002(좌)와 FD004(우)**를 1:1로 매칭하여 제시합니다.

### 📊 3.1. Base Model vs Ensemble 수렴 안정성 및 Real Test 오차행렬

#### [1] CNN-BiLSTM-Attention Evaluation
시공간 특징 결합을 통해 Train/Validation 손실 곡선이 안정적으로 수렴하나, 복합 고장 모드(FD004)로 갈수록 오차 행렬 내 경계선 부근에서 미탐(FN) 위험성이 일부 존재하여 단일 모델의 한계를 보입니다.

| FD002 - CNN-BiLSTM-Attention (Real Test) | FD004 - CNN-BiLSTM-Attention (Real Test) |
| :---: | :---: |
| ![FD002 BiLSTM](/Users/gyuminkang/Desktop/nasa/FD002/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 BiLSTM](/Users/gyuminkang/Desktop/nasa/FD004/Visualization/CNN-BILSTM-ATT%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |

#### [2] Dual Transformer Encoder Evaluation
전역 셀프 어텐션을 바탕으로 정상 장비에 대한 안정적인 분별력(TN)을 보여주나, 다중 운전 조건 교란으로 인해 학습 초기 손실 진동 및 Plateau 구간이 관측됩니다.

| FD002 - Dual Transformer Encoder (Real Test) | FD004 - Dual Transformer Encoder (Real Test) |
| :---: | :---: |
| ![FD002 Transformer](/Users/gyuminkang/Desktop/nasa/FD002/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Transformer](/Users/gyuminkang/Desktop/nasa/FD004/Visualization/DUAL%20TRANSFORMER%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) |

#### [3] Geometric Ensemble Fine-Tuning Evaluation
이종 모델의 장점을 기하학적으로 블렌딩한 결과, 손실 진동 변동성이 완벽하게 스무딩되어 이상적인 수렴 상태에 도달합니다. 오차 행렬 상에서 플랜트 폭발 및 자산 파손으로 직행하는 **False Negative(FN, 고장인데 알람 미작동) 리스크를 완전히 봉쇄**하여 예지 정비 신뢰성을 극대화했습니다.

| FD002 - Geometric Ensemble (Real Test) | FD004 - Geometric Ensemble (Real Test) |
| :---: | :---: |
| ![FD002 Ensemble](/Users/gyuminkang/Desktop/nasa/FD002/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test).png) | ![FD004 Ensemble](/Users/gyuminkang/Desktop/nasa/FD004/Visualization/GEOMETRIC%20ENSEMBLE%20FINE-TUNING%20-%20Training%20Loss%20,%20Confusion%20Matrix%20(Real%20Test)%20.png) |

---

### 📊 3.2. Global Performance Diagnostics (전역 성능 진단 지표)

#### [1] Residuals (Error) Distribution Comparison
잔차 오차 확률밀도함수(PDF) 분석 결과, 본 앙상블 시스템은 0을 중심으로 완벽하게 대칭적이고 좁은 가우시안 밴드를 형성합니다. NASA 페널티 스코어 폭발의 주원인인 우측 Heavy-Tail(늦은 알람 영역)이 칼날처럼 제거된 것을 실측 데이터로 증명합니다.

| FD002 - Residuals Distribution Comparison | FD004 - Residuals Distribution Comparison |
| :---: | :---: |
| ![FD002 Residuals](/Users/gyuminkang/Desktop/nasa/FD002/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) | ![FD004 Residuals](/Users/gyuminkang/Desktop/nasa/FD004/Visualization/Residuals%20(Error)%20Distribution%20Comparison.png) |

#### [2] RUL Prediction Trend (Sorted by True RUL)
실제 잔여수명 정렬선(`True RUL`)을 기준으로 모델의 추세 예측치를 오버레이한 지표입니다. 엔진 노후화가 가속되는 **고위험 전조 구간(True RUL < 40)**에 진입할수록 예측 신뢰도가 정답선에 완벽히 밀착 수렴하는 강력한 강건성을 보입니다.

| FD002 - RUL Prediction Trend (Sorted) | FD004 - RUL Prediction Trend (Sorted) |
| :---: | :---: |
| ![FD002 Trend](/Users/gyuminkang/Desktop/nasa/FD002/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) | ![FD004 Trend](/Users/gyuminkang/Desktop/nasa/FD004/Visualization/RUL%20Prediction%20Trend%20(Sorted%20by%20True%20RUL).png) |

#### [3] True RUL vs Predicted RUL Scatter Plot
이상적인 기준선($y = x$) 주위로 아웃라이어 이탈 없이 조밀하고 일관된 선형 분산 밴드를 유지하고 있습니다. 복합 다중 고장 교란 속에서도 모델의 회귀 일반화 성능이 완전히 안착했음을 최종 확증합니다.

| FD002 - True RUL vs Predicted RUL Scatter | FD004 - True RUL vs Predicted RUL Scatter |
| :---: | :---: |
| ![FD002 Scatter](/Users/gyuminkang/Desktop/nasa/FD002/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) | ![FD004 Scatter](/Users/gyuminkang/Desktop/nasa/FD004/Visualization/True%20RUL%20vs%20Predicted%20RUL%20Scatter%20Plot.png) |

---

## 🤖 4. Downstream Integration: Decision Boundary & Automated Report Builder

하이브리드 파이프라인 최하단에 통합되어 진짜 테스트셋(Real Test) 데이터를 실시간 진단하고 정형 의사결정 보고서를 추출하는 배포용 모듈 및 비즈니스 매트릭스입니다.

### 📊 예지보전 최적화 의사결정 매트릭스
* **Target Failure Threshold**: `30` (실제 엔진 가동 마진 30 사이클 미만 시 고장상태 `Failure` 정의)
* **Optimal Alert Threshold**: `45` (예측 잔여수명 45 사이클 미만 감지 시 예지 정비 알람 `Alert` 발령)

| 결과 클래스 (Result Class) | 실제 장비 및 모델 제어 상태 | 도메인 관점의 비즈니스 비용/리스크 해석 |
| :--- | :--- | :--- |
| **True Negative (TN)** | Actual $\ge 30$ (Normal) & Predicted $\ge 45$ (Normal) | 장비 정상 및 모델 정상 판단. 과잉 정비가 배제된 완벽한 정상 가동 상태 유지. |
| **False Positive (FP)** | Actual $\ge 30$ (Normal) & Predicted < 45 (Alert) | 장비는 정상이나 보수적 정비 알람 작동. **일부 조기 정비 비용**이 발생하나 안전함. |
| **False Negative (FN)** | Actual < 30 (Failure) & Predicted $\ge 45$ (Normal) | **최악의 치명적 시나리오.** 고장 상태이나 알람 미작동. **대형 자산 파손 및 페널티 폭발.** |
| **True Positive (TP)** | Actual < 30 (Failure) & Predicted < 45 (Alert) | **최적의 예지 정비 성공.** 고장 전 최소 15 사이클 이상의 안전 정비 윈도우 확보 완료. |

### 💻 Automated Chatbot Report Generation Script
```python
# 드롭 컬럼 반영 후, 진짜 테스트셋 기반 챗봇 리포트 생성 자동화 소스 코드
target_failure_threshold = 30
optimal_threshold = 45

report_path = "outputs/chatbot_report.txt"
with open(report_path, "w", encoding="utf-8") as f:
    f.write("Engine_ID,Actual_Cycles,Actual_RUL,Predicted_RUL,Actual_Status,Predicted_Status,Result_Class\n")
    
    for idx in range(len(test_engine_ids)):
        eng_id = test_engine_ids[idx]
        act_cyc = test_act_cycles[idx]
        act_rul = y_test_actual[idx]
        pred_rul = y_pred_ens1[idx]
        
        act_status = "Failure" if act_rul < target_failure_threshold else "Normal"
        pred_status = "Alert" if pred_rul < optimal_threshold else "Normal"
        
        if act_status == "Normal" and pred_status == "Normal":
            res_class = "True_Negative(TN)"
        elif act_status == "Normal" and pred_status == "Alert":
            res_class = "False_Positive(FP)"
        elif act_status == "Failure" and pred_status == "Normal":
            res_class = "False_Negative(FN)"
        else:
            res_class = "True_Positive(TP)"
            
        f.write(f"Engine_{eng_id},{act_cyc},{act_rul},{pred_rul:.1f},{act_status},{pred_status},{res_class}\n")
        
    f.write("=== END OF REPORT ===")

print(f"\n🎉 완료: 드롭 컬럼 수정 후, 진짜 테스트셋 기반 챗봇 리포트('{report_path}') 생성이 완료되었습니다.")
