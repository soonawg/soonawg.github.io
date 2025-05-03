---
title: "강화학습 심화 알고리즘 - DDPG"
layout: post
date: 2025-05-03 22:45:00 +0900
categories: study
---

# 강화학습 심화 알고리즘 中 DDPG에 대하여
이제 DQN, PPO, Q-LEARNING 같은 알고리즘을 넘어서 좀 더 심화된 알고리즘을 공부하고, 복잡한 환경의 문제를 해결해볼 계획이다.
DDPG에 대해 간단히 요약해보자


## DDPG란?
- **연속적인 액션 공간**에서 작동하는 강화학습 알고리즘.
- **Actor-Critic 구조**를 사용하며, Actor는 행동을 선택하고 Critic은 그 행동의 가치를 평가합니다.
- 이름에서 알 수 있듯이, Policy Gradient 계열이며, deterministic한(확률이 아닌 결정적인) policy를 사용합니다.
- DQN(Deep Q-Network)의 아이디어를 일부 가져왔으며, replay buffer와 target network도 사용합니다.


## DDPG의 장점과 단점
### 장점

- 연속적인 액션 공간에서 사용 가능
- 안정성을 위해 target network와 replay buffer를 함께 사용
- deterministic policy라서 학습이 안정적인 편

### 단점

- 탐험(exploration)에 어려움: deterministic polify라서 노이즈를 직접 추가해줘야 함 (예: OU 노이즈 | 추후에 설명)
- Critic이 불안정하면 Actor도 같이 흔들리는 문제 발생


## 노이즈를 추가해 탐험 유도
DDPG에서 탐험(EXPLORATION)이 어렵다는 문제를 해결하기 위해 외부에서 인위적으로 노이즈를 추가해서 탐험을 유도하게 한다.

### 왜 노이즈가 필요한가?
- DDPG는 결정론적 정책을 사용하는데, 탐험을 하지 않으면 초기에 잘못된 정책이 고정될 위험이 크다. 그래서 노이즈를 추가해주면 탐험을 유도할 수 있다.
- 대표적으로 OU 노이즈와 가우시안 노이즈가 있다.
- OU 노이즈는 시간 상관성이 있고, 행동 변화의 연속성이 높다.
- 가우시안 노이즈는 시간 상관성이 없고, 행동 변화의 연속성이 낮다.