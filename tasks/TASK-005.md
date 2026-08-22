# TASK-005 - Problem Router and Analysis Planner

Status: BLOCKED BY TASK-004

## Goal
ProblemDefinition과 DataProfile을 기반으로 문제 유형과 분석 계획을 만든다.

## Supported
- classification
- regression
- clustering
- anomaly_detection

## Planner Output
- problem_type
- target
- primary_metric
- secondary_metrics
- split_strategy
- preprocessing recommendations
- model candidate classes
- human gates

## Rule
가능한 부분은 deterministic rule 우선.

## Acceptance
대표 fixture 4종에서 expected route PASS.
