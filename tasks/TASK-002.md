# TASK-002 - Input and Domain Contracts

Status: BLOCKED BY TASK-001

## Goal
입력 로더와 핵심 domain model 계약을 구현한다.

## Implement
- CSV loader
- XLSX loader
- Parquet loader
- ProblemDefinition
- ColumnProfile
- DataProfile
- DataWarning
- AnalysisPlan
- ExperimentResult

## Acceptance
- 세 파일 형식 fixture 읽기
- unsupported extension SAFE FAIL
- empty dataset SAFE FAIL
- model serialization test PASS

## Forbidden
- profiling 계산 로직
- LLM
- model training
