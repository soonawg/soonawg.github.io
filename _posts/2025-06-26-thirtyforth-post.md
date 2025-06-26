---
title: "IQL 공부 - Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-26 22:16:00 +0900
categories: study
---

# 배경 및 문제 의식
## Offline RL의 주요 도전과제
- Distribution Shift
- Out-of-Distribution Actions
- 기본 Q-learning은 데이터 외 행동에 과한 Q값을 추정하며 unstable함

## IQL의 핵심 아이디어
- Q-learning을 그대로 쓰되, **정책을 명시적으로 업데이트하지 않음**
- 대신, Q 함수와 V 함수, Advantage를 추정하고, 이를 기반으로 **implicit하게 정책을 유도**
- **Advantage-weighted regression** 방식으로 좋은 행동만 imitation

# 전체 구조 및 학습 절차
IQL은 세 가지 주요 컴포넌트를 학습함:
1. **Q-function:**
    일반적인 Bellman backup을 사용해 TD 방식으로 학습
2. **Value fnction:**
    Q-function에서 나온 Q(s, a)와 행동 a로부터 V(s)를 추정
3. **Policy (π)**:
    Advantage-weighted regression으로 정책을 업데이트
    즉, advantage가 클수록 imitation을 더 많이 하도록 함

# IQL의 장점
- 정책을 **explicit하게 업데이트하지 않기 때문에** OOD 행동을 방지
- Advantage 기반 imitation으로 **"좋은 행동만 따라함"**
- TD 방식이기 때문에 sample-efficient
- Simpler + Stable → Benchmark (D4RL 등)에서 매우 좋은 성능

# 샘플 코드
## [Code](https://github.com/soonawg/offline_rl_sample/blob/main/iql_sample.py)