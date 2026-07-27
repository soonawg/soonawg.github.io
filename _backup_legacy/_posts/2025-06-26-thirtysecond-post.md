---
title: "Offline RL 공부 - Offline RL"
layout: post
date: 2025-06-26 21:06:00 +0900
categories: study
---

# 정의
Offline RL은 에이전트가 환경과 상호작용하지 않고 오직 **미리 수집된 고정된 데이터셋**을 이용해서 학습하는 강화학습 방법론이다.

# 왜 필요할까?
기존 Online RL의 한계:
- 실제 환경에서 상호작용은 위험, 비용, 시간이 많이 든다.
- 충분한 탐험을 하지 못하면 이미 수집된 로그 데이터만으로 학습해야 하는 경우가 많다.

# Offline RL의 핵심 문제
1. Distribution Shift
2. Extraplation Error
3. Overestimation & Exploitation


# 핵심 아이디어
Offline RL의 핵심 아이디어는 Out-of-Distribution 행동을 억제하고 데이터 내 행동을 더 신뢰하게 하는 제약 또는 정책 보정을 쓰는 것이다.

# 대표 알고리즘 분류
## 1. Conversative Value Estimation (Q함수 보수화)
- CQL
- EDAC

## 2. Implicit Value Learning
- IQL

## 3. Behavior Cloning 보강
- TD3 + BC
- AWAC

## 4. IRL 기반 Offline RL
- IQ-Learn
- BCQ