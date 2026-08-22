# TASK-009 - LLM Adapter, Hypothesis and Insight Engine

Status: BLOCKED BY TASK-008

## Goal
계산된 evidence를 LLM에 전달해 문제 정의 보완, 가설, 해석, 해결방안 후보를 구조화한다.

## Implement
- LLMClient protocol
- MockLLMAdapter
- prompt templates
- structured response validation
- Hypothesis model
- InsightResult
- SolutionCandidate

## Critical Rule
LLM은 새로운 metric 숫자를 생성해 근거로 사용할 수 없다.

## Failure
LLM unavailable:
- core analysis remains usable
- report marks narrative section unavailable

## Acceptance
Mock adapter 기반 deterministic tests PASS.
