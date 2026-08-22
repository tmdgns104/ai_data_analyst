# AGENTS

## Purpose

Codex 또는 다른 코딩 Agent가 이 Repository를 구현할 때 따라야 하는 작업 규칙이다.

## 1. Source of Truth

우선순위:

1. REQUIREMENTS.md
2. ARCHITECTURE.md
3. DECISIONS.md
4. 현재 tasks/TASK-XXX.md
5. PROJECT.md

충돌 시 위 순서를 따른다.

## 2. One Task at a Time

한 번에 현재 Task 하나만 구현한다.

다음 Task를 선행 구현하지 않는다.

## 3. Architecture Preservation

Task 해결을 이유로 Architecture를 임의 변경하지 않는다.

구조 변경이 필요하면:
- 구현 중단
- 이유 보고
- Decision 필요 여부 제시

## 4. Evidence

"완료했다"는 자기 보고를 신뢰하지 않는다.

완료 판정은:
- test
- lint/type check if configured
- git diff
- required artifact
로 확인한다.

## 5. Safety

금지:
- 원본 사용자 데이터 수정
- production DB write
- 외부 시스템 제어
- secret 출력
- target/leakage 컬럼 임의 삭제

## 6. Minimal Change

현재 Task에 필요한 최소 범위만 수정한다.

## 7. Tests First Where Practical

버그 수정 또는 명시적 계약 구현 시 관련 테스트를 먼저 또는 동시에 추가한다.

## 8. Deterministic Core

가능한 판단은 deterministic code로 구현한다.

LLM으로 대체하지 않는다.

예:
- missing rate
- dtype
- imbalance ratio
- metric calculation
- model score

## 9. LLM Boundary

LLM 출력은 구조화하고 validation한다.

LLM failure가 core profiling/experiment 기능 전체를 깨뜨리지 않게 한다.

## 10. Done Report

Task 종료 시 다음만 보고한다.

```text
Task:
Changed:
Tests:
Result:
Git status:
Remaining risks:
```
