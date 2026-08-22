# TASK-006 - Preprocessing Builder

Status: BLOCKED BY TASK-005

## Goal
AnalysisPlan을 sklearn Pipeline/ColumnTransformer로 변환한다.

## Implement
- numeric missing imputer
- optional scaler
- categorical imputer
- OneHotEncoder
- ID exclusion
- target separation
- pipeline-safe fit/transform

## Leakage Constraint
split 이후 train 데이터에서 fit해야 한다.

## Acceptance
- train/test leakage 방지 test
- unseen category test
- missing test
- scaled/non-scaled model path test
