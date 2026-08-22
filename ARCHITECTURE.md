# ARCHITECTURE

## 1. High-Level Architecture

```text
User
 │
 ├── Problem statement
 └── CSV/XLSX/Parquet
          │
          ▼
┌────────────────────┐
│ Input Layer         │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Problem Framer      │◄──── LLM Adapter
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Data Profiler       │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Risk Detector       │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Analysis Planner    │◄──── LLM Adapter
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ EDA Engine          │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Hypothesis Engine   │◄──── LLM Adapter
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Preprocess Planner  │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Model Recommender   │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Experiment Runner   │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Evaluator           │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Insight Engine      │◄──── LLM Adapter
└─────────┬──────────┘
          ▼
┌────────────────────┐
│ Report Generator    │
└────────────────────┘
```

## 2. Design Rule

LLM은 계산 결과를 만들지 않는다.

예:

잘못된 구조:

```text
LLM → "결측률은 약 12%입니다."
```

권장 구조:

```text
Python profiler → missing_rate = 0.128
                         ↓
LLM → "압력 컬럼은 결측률이 12.8%로 높아..."
```

## 3. Package Layout

```text
ai-data-analyst/
├── src/
│   └── ai_data_analyst/
│       ├── io/
│       │   ├── loader.py
│       │   └── schema.py
│       ├── problem/
│       │   ├── models.py
│       │   ├── framer.py
│       │   └── router.py
│       ├── profiling/
│       │   ├── profiler.py
│       │   ├── quality.py
│       │   └── risk.py
│       ├── eda/
│       │   ├── numeric.py
│       │   ├── categorical.py
│       │   ├── target.py
│       │   └── plots.py
│       ├── hypothesis/
│       │   └── engine.py
│       ├── preprocessing/
│       │   ├── planner.py
│       │   ├── builders.py
│       │   └── leakage.py
│       ├── models/
│       │   ├── registry.py
│       │   ├── classification.py
│       │   ├── regression.py
│       │   ├── clustering.py
│       │   └── anomaly.py
│       ├── experiment/
│       │   ├── runner.py
│       │   └── store.py
│       ├── evaluation/
│       │   ├── classification.py
│       │   ├── regression.py
│       │   ├── clustering.py
│       │   ├── anomaly.py
│       │   └── errors.py
│       ├── insight/
│       │   ├── interpreter.py
│       │   └── solution.py
│       ├── llm/
│       │   ├── protocol.py
│       │   ├── prompts.py
│       │   └── adapters/
│       ├── report/
│       │   ├── markdown.py
│       │   └── html.py
│       ├── pipeline/
│       │   └── runner.py
│       └── common/
│           ├── config.py
│           ├── logging.py
│           └── exceptions.py
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│   └── fixtures/
├── reports/
├── artifacts/
├── configs/
├── tasks/
├── pyproject.toml
└── README.md
```

## 4. Domain Models

Pydantic/dataclass 기반의 명시적 계약을 사용한다.

### ProblemDefinition

```python
class ProblemDefinition:
    business_goal: str
    analytical_goal: str
    analysis_unit: str | None
    target: str | None
    problem_type: str
    prediction_or_explanation: str
    primary_metric: str | None
    error_cost_notes: list[str]
    missing_information: list[str]
```

### DataProfile

```python
class DataProfile:
    rows: int
    columns: int
    column_profiles: list[ColumnProfile]
    duplicate_rows: int
    target_profile: dict | None
    warnings: list[DataWarning]
```

### AnalysisPlan

```python
class AnalysisPlan:
    problem: ProblemDefinition
    split_strategy: str | None
    preprocessing_steps: list[PlanStep]
    model_candidates: list[ModelCandidate]
    metrics: list[str]
    hypotheses: list[Hypothesis]
    human_gates: list[HumanGate]
```

### ExperimentResult

```python
class ExperimentResult:
    run_id: str
    model_name: str
    metrics: dict[str, float]
    train_time: float
    params: dict
    warnings: list[str]
```

## 5. Rule Engine + LLM

판단 구조:

```text
Deterministic Rules
        +
Statistical Evidence
        +
LLM Reasoning
        +
Actual Experiment Evidence
```

우선순위:

```text
실제 계산/실험 결과
>
명시적 규칙
>
LLM 추천
```

충돌 시 LLM 추천보다 실험 결과를 우선한다.

## 6. Model Registry

모델을 코드에 흩뿌리지 않고 Registry로 관리한다.

각 모델 정의:

```python
{
    "name": "logistic_regression",
    "problem_types": ["classification"],
    "requires_scaling": True,
    "supports_sparse": True,
    "supports_missing": False,
    "interpretability": "high",
    "cost": "low"
}
```

이를 이용해 Preprocessing Planner와 Model Recommender가 공통 정보를 사용한다.

## 7. Preprocessing Architecture

ColumnTransformer + Pipeline 기반으로 만든다.

중요:

- train에 fit
- validation/test에는 transform만
- split 전에 target-aware preprocessing 금지
- preprocessing 자체도 experiment에 저장

## 8. LLM Adapter

provider 독립 계약:

```python
class LLMClient(Protocol):
    def complete(self, messages: list[dict], *, schema=None) -> dict:
        ...
```

V1 핵심 로직은 특정 OpenAI/Qwen/Ollama SDK에 종속되지 않는다.

Adapter 예:
- OpenAIAdapter
- OllamaAdapter
- MockLLMAdapter

Mock adapter는 테스트에서 사용한다.

## 9. Report Pipeline

모든 보고서 문장은 가능한 한 구조화된 evidence object에서 생성한다.

```text
DataProfile
AnalysisPlan
EDAResult
ExperimentResult[]
EvaluationResult
InsightResult
        ↓
ReportContext
        ↓
Markdown/HTML
```

## 10. Artifact Policy

분석 실행마다:

```text
artifacts/<run_id>/
├── config.json
├── profile.json
├── analysis_plan.json
├── experiments.json
├── evaluation.json
├── insights.json
├── plots/
└── report.md
```

원본 데이터는 artifact 폴더로 복사하지 않는 것을 기본값으로 한다.

## 11. Failure Policy

다음 상황은 SAFE FAIL:

- 파일 파싱 실패
- target 없음 + supervised 분석 강제 요청
- 데이터 0행
- target 값이 1종류뿐임
- 메모리 초과 위험
- split 후 학습 불가능한 클래스 수
- unsupported problem type

일부 분석만 가능한 경우 PARTIAL로 보고한다.

## 12. Future Extension Boundary

V2 이후:
- Time Series
- Optuna
- AutoGluon benchmark
- Dask
- SHAP
- SQL/DB
- API service

V3 이후:
- Spark
- Kafka
- Vision
- NLP
- Agent orchestration
- RAG
- MES/PLC integrations
