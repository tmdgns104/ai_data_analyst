# TASK-001 - Repository Foundation

Status: READY

## Goal
V1 구현을 위한 Python package와 테스트 골격을 만든다.

## Allowed
- pyproject.toml
- src/
- tests/
- configs/
- README.md
- .gitignore

## Requirements
- src layout
- package import 성공
- pytest 실행 가능
- Python 3.11+
- optional dependencies 그룹 분리 준비

## Acceptance
- `python -m pytest` 실행 가능
- 최소 smoke test PASS
- repository 구조가 ARCHITECTURE.md와 일치

## Forbidden
- 분석 기능 구현
- LLM 실제 연결
- 모델 학습 기능
