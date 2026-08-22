# REQUIREMENTS

## R-001 Input

시스템은 CSV, XLSX, Parquet 파일을 입력받아야 한다.

## R-002 Natural-Language Problem

사용자는 데이터와 함께 자연어 문제를 입력할 수 있어야 한다.

예:

```text
"제품 불량이 증가한 이유를 알고 싶다."
"어떤 고객이 이탈할 가능성이 높은지 알고 싶다."
"센서 데이터에서 비정상 상태를 찾고 싶다."
```

## R-003 Problem Framing

시스템은 다음을 구조화해야 한다.

- business_goal
- analytical_goal
- analysis_unit
- target_candidate
- problem_type
- prediction_or_explanation
- decision_context
- error_cost
- recommended_metrics
- missing_information

## R-004 Data Profiling

다음을 자동 계산해야 한다.

- row/column count
- dtype
- missing count/rate
- unique count/rate
- duplicated rows
- numeric summary
- categorical summary
- class distribution
- potential ID columns
- constant/near-constant columns
- datetime candidates

## R-005 Data Risk Detection

최소 다음 위험요소를 진단한다.

- missing values
- severe imbalance
- high cardinality
- outliers
- skew
- duplicate data
- target leakage candidates
- train/test leakage risk
- identifier-like columns
- suspicious post-outcome variables

자동 판정이 불가능하면 "검토 필요"로 남긴다.

## R-006 EDA

V1은 최소 다음 EDA를 지원한다.

- target distribution
- numeric distributions
- category counts
- numeric correlation
- target-vs-feature summary
- group comparison
- missingness summary

모든 그래프를 무조건 생성하지 않는다.
문제 유형과 데이터 특성에 따라 필요한 분석만 선택한다.

## R-007 Hypothesis

Agent는 EDA 결과에서 분석 가설을 생성할 수 있다.

각 가설은 다음 구조를 가진다.

```json
{
  "hypothesis": "...",
  "evidence": ["..."],
  "counter_explanation": ["..."],
  "validation_plan": ["..."],
  "status": "unverified"
}
```

가설은 검증 전 사실로 표현하지 않는다.

## R-008 Preprocessing Recommendation

전처리는 데이터와 모델 후보를 함께 고려해야 한다.

지원 항목:

- missing value handling
- scaling
- categorical encoding
- outlier handling
- log/power transform
- datetime feature extraction
- ID exclusion
- class weighting
- sampling recommendation

## R-009 Leakage Guard

Target 또는 미래 정보를 포함할 가능성이 있는 변수는 모델 학습 전 경고해야 한다.

자동으로 확신할 수 없는 경우 삭제하지 않고 사람 검토 대상으로 표시한다.

## R-010 Split Strategy

Classification:
- stratified split 기본 검토

Regression:
- random split 기본
- 데이터 순서 의미가 있으면 경고

Time-related data:
- V1 모델 범위는 아니어도 random split leakage 가능성을 경고

## R-011 Model Recommendation

문제 유형별 최소 후보:

### Classification
- LogisticRegression
- RandomForestClassifier
- HistGradientBoostingClassifier
- optional: LightGBM / XGBoost / CatBoost

### Regression
- Linear/Ridge
- RandomForestRegressor
- HistGradientBoostingRegressor
- optional: LightGBM / XGBoost / CatBoost

### Clustering
- KMeans
- DBSCAN

### Anomaly Detection
- IsolationForest
- LocalOutlierFactor

모델 추천에는 반드시 이유가 포함되어야 한다.

## R-012 Baseline First

복잡한 모델 전에 baseline을 만든다.

Classification:
- DummyClassifier
- LogisticRegression 후보

Regression:
- DummyRegressor
- Linear/Ridge 후보

## R-013 Experiment Runner

각 실험은 다음 정보를 저장해야 한다.

- model_name
- preprocessing
- hyperparameters
- split seed
- metrics
- training_time
- inference_time if practical
- feature list
- warnings
- run timestamp

## R-014 Cross Validation

가능한 경우 단일 holdout 결과와 CV 결과를 구분한다.

## R-015 Evaluation

### Classification
지원 지표:
- Accuracy
- Precision
- Recall
- F1
- ROC-AUC
- PR-AUC
- Confusion Matrix

### Regression
- MAE
- RMSE
- R²

### Clustering
- Silhouette
- cluster size distribution

### Anomaly
라벨 존재 시:
- Precision
- Recall
- F1

라벨 미존재 시:
- anomaly rate
- score distribution
- top anomaly inspection

## R-016 Model Selection

가장 높은 단일 숫자만으로 모델을 선택하면 안 된다.

선택 근거에 다음을 포함한다.

- primary metric
- secondary metric
- stability
- complexity
- interpretability
- runtime
- business error cost

## R-017 Explainability

최소 다음 중 가능한 것을 사용한다.

- linear coefficient
- tree feature importance
- permutation importance

SHAP은 optional dependency로 둔다.

## R-018 Error Analysis

분류에서는 최소:
- false positive sample summary
- false negative sample summary

회귀에서는 최소:
- largest residual cases
- residual distribution

## R-019 Solution Advisor

해결방안은 다음 구조로 생성한다.

- finding
- possible_causes
- evidence
- uncertainty
- additional_checks
- action_candidates

"원인 확정"과 "가능성"을 구분한다.

## R-020 Report

최종 보고서는 최소 다음 섹션을 가진다.

1. Problem Definition
2. Dataset Summary
3. Data Quality
4. EDA Findings
5. Hypotheses
6. Preprocessing
7. Model Experiments
8. Evaluation
9. Interpretation
10. Error Analysis
11. Additional Validation
12. Solution Candidates
13. Limitations

## R-021 Reproducibility

동일 데이터와 동일 설정에서 가능한 범위 내 동일한 결과가 재현되어야 한다.

- random_state 고정
- config 저장
- experiment 기록

## R-022 Human Gate

다음 사항은 자동 삭제/확정하지 않는다.

- suspected leakage column removal
- high missing column removal
- outlier removal
- business target 결정
- high-impact action
- causal conclusion

## R-023 Safety

원본 파일은 수정하지 않는다.

V1 분석은 Read/Analyze 중심으로 제한한다.

## R-024 Performance

V1 기준:
- 일반 노트북/PC에서 수십 MB~수백 MB tabular 데이터 우선
- 데이터가 메모리에 들어오지 않으면 SAFE FAIL 또는 sampling 안내
- Spark/Dask 자동 전환은 V1 범위 밖

## R-025 Tests

필수:
- unit test
- integration test
- E2E test
- deterministic fixture datasets
