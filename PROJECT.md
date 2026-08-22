# AI Data Analyst V1

## 1. Project Summary

AI Data Analyst V1은 사용자가 데이터 파일과 자연어 문제를 입력하면 다음 전체 분석 흐름을 지원하는 개인용 데이터 분석 Agent이다.

> 문제 정의 → 데이터 진단 → 분석 설계 → EDA → 가설 생성 → 전처리 설계 → 모델 후보 추천 → 실제 학습/비교 → 평가 → 해석 → 추가 분석/해결방안 → 보고서

이 프로젝트의 목적은 단순 AutoML이 아니다.

모델을 자동으로 학습하는 기능보다 먼저,
"왜 이 분석을 하는가", "어떤 문제 유형인가", "이 데이터에서 무엇을 조심해야 하는가", "어떤 방식으로 검증해야 하는가"를 구조화하는 것을 핵심 가치로 둔다.

## 2. V1 Goal

사용자는 아래와 같이 사용할 수 있어야 한다.

```text
입력:
- factory.csv
- "최근 제품 불량이 늘어난 원인을 분석하고 개선방안을 찾아줘."

출력:
1. 문제 정의
2. Target/분석 단위 후보
3. 데이터 품질 진단
4. EDA 요약
5. 분석 가설
6. 추천 전처리
7. 추천 모델 및 이유
8. 실제 모델 비교 결과
9. 최종 평가
10. 주요 변수/오류 분석
11. 가능한 원인과 반증 조건
12. 추가 검증 항목
13. 해결방안 후보
14. 최종 분석 보고서
```

## 3. V1 Supported Problems

V1에서 지원하는 문제 유형은 다음 네 개로 제한한다.

- Classification
- Regression
- Clustering
- Anomaly Detection

다음은 V1 범위 밖이다.

- Time-series forecasting
- Survival/RUL
- Image/Vision
- NLP 모델 학습
- Spark/Kafka 실시간 처리
- Auto deployment
- Production DB write
- PLC/MES 제어
- 완전 자율 의사결정

## 4. Core Principle

### 4.1 LLM은 계산 엔진이 아니다

LLM:
- 문제 구조화
- 가설 생성
- 분석 계획
- 추천 이유 설명
- 결과 해석
- 추가 분석 제안
- 해결방안 후보 생성
- 보고서 작성

Python/분석 엔진:
- 통계
- 결측률
- 분포
- 상관/연관
- 데이터 분할
- 전처리
- 모델 학습
- Cross Validation
- Metrics
- Feature Importance
- Error Analysis

### 4.2 추천은 실험으로 검증한다

LLM이 특정 모델을 추천했다는 이유만으로 그 모델을 최종 선택하지 않는다.

최종 모델 선택은 실제 평가 결과와 문제 목적에 의해 결정한다.

### 4.3 문제 목적에 맞는 Metric을 사용한다

Accuracy를 기본 정답으로 사용하지 않는다.

예:
- 불균형 분류 → PR-AUC, Recall, Precision, F1
- 일반 분류 → ROC-AUC, F1, Accuracy 등
- 회귀 → MAE, RMSE, R²
- 군집 → Silhouette + 해석 가능성
- 이상탐지 → 라벨 존재 여부에 따라 Precision/Recall 또는 정성/통계 검증

### 4.4 상관관계와 인과관계를 구분한다

Agent는 분석 결과를 원인으로 단정하면 안 된다.

결과는 다음 수준으로 표현한다.

- 관찰
- 연관성
- 가설
- 추가 검증
- 가능한 해결방안

## 5. Initial Technology Stack

- Python 3.11+
- pandas
- numpy
- scipy
- scikit-learn
- matplotlib
- plotly
- pydantic
- pyarrow
- openpyxl
- joblib
- pytest
- FastAPI (V1 후반)
- Jinja2 (보고서)
- optional: LightGBM
- optional: XGBoost
- optional: CatBoost
- optional: Optuna
- optional: AutoGluon benchmark
- LLM Adapter: provider-independent interface

## 6. Data Formats

Input:
- CSV
- XLSX
- Parquet

Internal canonical representation:
- pandas DataFrame

Persisted analysis dataset:
- Parquet

V1에서는 Delta Lake/Spark를 구현하지 않는다.

## 7. Non-Goals

V1에서는 다음을 하지 않는다.

- 모든 문제를 자동으로 완벽히 해결
- 인과 추론을 자동 확정
- 모든 모델/라이브러리 지원
- 사용자의 업무 의사결정을 자동 실행
- 데이터 삭제/DB 쓰기
- 외부 시스템 제어
- LLM의 자기 보고만으로 분석 완료 판정

## 8. Definition of Done

V1은 최소 다음 E2E 시나리오가 통과하면 완료다.

1. 분류 데이터셋 입력
2. 문제 설명 입력
3. Target 선택 또는 후보 제시
4. 데이터 프로파일 생성
5. 데이터 위험요소 탐지
6. 전처리 계획 생성
7. 최소 3개 모델 비교
8. 평가 결과 생성
9. Feature importance 또는 coefficient 기반 해석
10. 가설/추가 검증/해결방안 생성
11. Markdown 또는 HTML 보고서 출력
12. 전체 테스트 통과
