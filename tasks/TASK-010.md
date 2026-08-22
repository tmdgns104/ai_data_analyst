# TASK-010 - End-to-End Pipeline and Report

Status: BLOCKED BY TASK-009

## Goal
입력부터 최종 report까지 연결한다.

## Pipeline
load
→ frame
→ profile
→ risk
→ plan
→ EDA
→ preprocess
→ experiment
→ evaluate
→ insight
→ report

## Output
artifacts/<run_id>/
- config.json
- profile.json
- analysis_plan.json
- experiments.json
- evaluation.json
- insights.json
- plots/
- report.md
- optional report.html

## E2E Acceptance
최소:
- binary classification fixture
- regression fixture
- clustering fixture
- anomaly fixture

각 fixture에서 전체 pipeline 완료.

## Final Gate
- full pytest PASS
- no original input modification
- artifacts reproducible
