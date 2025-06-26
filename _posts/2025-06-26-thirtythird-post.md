---
title: "CQL 공부 - Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-26 21:07:00 +0900
categories: study
---

# 핵심 아이디어
기존의 오프라인 강화학습은 행동 정책의 바이어스로 인해 Q함수 추정이 부정확해지는 문제가 발생했다.
CQL은 **"보수적인 Q함수 추정"**을 통해, 행동하지 않은 행동에 대해서는 Q값을 낮추도록 유도함으로써 이 문제를 해결한다.

# 기존 Q-Learning의 한계
- Q-Learning에서는 다음과 같은 Bellman backup을 수행함.
- `Q(s, a) ← r + γ * max_{a′} Q(s^′, a^′)`
- offline setting에서는 행동하지 않은 (s, a′)에 대한 Q값이 부정확하게 높아져, 학습이 overestimation에 빠지게 됨
- 따라서 agent는 실제로는 나쁜 행동을 선택할 수 있게 됨

# CQL의 해결 방법
## Conservative Q-Learning Objective
간단하게 설명하면 데이터셋 밖의 unseen action에 대해서는 Q값을 낮게 추정하고자 하는 것이다.

# CQL 학습 프로세스 (TD3/SAC 기반)
1. 오프라인 데이터셋 준비: (s, a, r, s') 수집
2. Q-function 업데이트:
- 일반적인 TD target 계산
- conservative penalty 계산 및 합산
3. Policy 업데이트 (actor step)
- actor는 Q에 기반하여 정책을 업데이트함
4. 타겟 네트워크 업데이트
5. 반복

# 샘플 코드
## [Code](https://github.com/soonawg/offline_rl_sample/blob/main/cql_sample.py)