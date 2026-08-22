# DECISIONS

## ADR-001 - V1은 AutoML이 아니라 Analysis Decision System이다

Status: Accepted

### Decision
V1의 중심은 모델 자동 선택이 아니라 문제 정의와 분석 의사결정이다.

### Reason
모델만 자동 실행하면 잘못된 target, leakage, metric, split, 전처리를 자동화할 위험이 있다.

---

## ADR-002 - V1 문제 유형은 4개만 지원한다

Status: Accepted

### Included
- Classification
- Regression
- Clustering
- Anomaly Detection

### Excluded
- Time Series
- Vision
- NLP
- Survival/RUL

### Reason
V1에서 분석 전체 흐름의 품질을 먼저 검증한다.

---

## ADR-003 - LLM과 계산 엔진을 분리한다

Status: Accepted

### Decision
통계/모델 결과는 Python이 계산한다.
LLM은 문제 구조화, 가설, 설명, 추가 분석 제안을 담당한다.

### Rule
LLM이 생성한 숫자를 분석 근거로 사용하지 않는다.

---

## ADR-004 - 실제 실험 결과가 추천보다 우선한다

Status: Accepted

### Priority
1. measured experiment evidence
2. deterministic rule
3. LLM recommendation

---

## ADR-005 - scikit-learn Pipeline/ColumnTransformer를 기본 전처리 구조로 사용한다

Status: Accepted

### Reason
데이터 누수 방지와 재현성 확보.

---

## ADR-006 - Parquet을 내부 저장 포맷으로 사용한다

Status: Accepted

CSV/XLSX는 입력 형식이다.
반복 분석/중간 산출물에는 Parquet을 우선한다.

---

## ADR-007 - Pandas-first로 시작한다

Status: Accepted

### Decision
V1은 in-memory tabular 분석에 집중한다.

Dask/Spark 자동 전환은 구현하지 않는다.

### Failure
메모리 한계를 넘는 데이터는 sampling 또는 다음 버전 기술을 안내한다.

---

## ADR-008 - 모델 Registry를 둔다

Status: Accepted

### Reason
모델별 scaling/missing/categorical/interpretability 특성을 한곳에서 관리한다.

---

## ADR-009 - Human Gate를 유지한다

Status: Accepted

자동 확정 금지:
- target 확정
- leakage 의심 컬럼 삭제
- 많은 결측 컬럼 삭제
- outlier 제거
- causal conclusion
- high-impact action

---

## ADR-010 - 해결방안은 가설 수준으로 출력한다

Status: Accepted

결과 표현:
- Observation
- Evidence
- Hypothesis
- Counter Explanation
- Validation
- Action Candidate

"원인이다" 대신 "가능성이 있다"를 사용한다.

---

## ADR-011 - Optional ML libraries는 핵심 의존성에서 분리한다

Status: Accepted

Core:
- sklearn

Optional:
- lightgbm
- xgboost
- catboost
- optuna
- autogluon
- shap

Core test는 optional package 없이 통과해야 한다.

---

## ADR-012 - LLM Adapter는 provider independent다

Status: Accepted

V1 core architecture는 특정 API 또는 로컬 모델에 종속되지 않는다.

테스트는 MockLLMAdapter로 수행 가능해야 한다.

---

## ADR-013 - 분석 실행 결과를 구조화된 Artifact로 남긴다

Status: Accepted

각 run은 JSON + plot + report로 재현 가능한 증거를 남긴다.
