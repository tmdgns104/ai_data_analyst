# TASK-007 - Model Registry and Experiment Runner

Status: BLOCKED BY TASK-006

## Goal
문제 유형별 baseline 및 후보 모델을 일관된 방식으로 실행한다.

## Core Models
Classification:
- DummyClassifier
- LogisticRegression
- RandomForestClassifier
- HistGradientBoostingClassifier

Regression:
- DummyRegressor
- Ridge
- RandomForestRegressor
- HistGradientBoostingRegressor

Clustering:
- KMeans
- DBSCAN

Anomaly:
- IsolationForest
- LocalOutlierFactor

## Experiment Evidence
- params
- metrics placeholder
- runtime
- feature metadata
- seed
- warnings

## Acceptance
각 문제 유형 fixture에서 최소 1개 experiment 실행 PASS.
