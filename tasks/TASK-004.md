# TASK-004 - Risk Detector and Leakage Guard

Status: BLOCKED BY TASK-003

## Goal
분석 전에 주요 데이터 위험요소를 경고한다.

## Detect
- severe missing
- imbalance
- high cardinality
- suspicious ID
- constant/near constant
- outlier warning
- skew warning
- duplicate warning
- leakage candidate heuristic

## Leakage Policy
확정 삭제하지 않는다.
warning + human review만 생성한다.

## Acceptance
위험 fixture별 expected warning test PASS.
