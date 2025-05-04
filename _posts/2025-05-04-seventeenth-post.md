---
title: "강화학습 심화 알고리즘 - TD3"
layout: post
date: 2025-05-04 21:05:00 +0900
categories: study
---

# 강화학습 심화 알고리즘 中 TD3에 대하여

TD3의 기본 아이디어는 DDPG에 3가지 핵심 개선점을 더한 알고리즘이다.
DDPG의 불안정성과 과도한 Qvalue 추정을 줄이고, 학습을 더 안정적이고 신뢰성 있게 만든 알고리즘이다.

## TD3가 도입한 3가지 기술
1. Twin Q-networks
* Q-funciton을 2개를 사용하는 것이다.
* 그 중 더 작은 값을 선택하여 overestimation을 방지한다.
2. Target Policy Smoothing
- 액터가 타겟 네트워크로부터 행동을 생성할 때 작은 노이즈 추가
- 그럼으로써 과적합을 억제한다.
3. Delayed Policy Updates
- Critic은 자주 업데이트하고, Actor는 느리게 업데이트한다.

## 왜 필요한가? (DDPG의 문제점)
### 문제 1: Q-value 과추정
- DDPG는 Q-Network 하나만 사용하여 Q값을 추정함
- Q값이 점점 과하게 높아지고, Actor는 나쁜 정책을 강화하게 될 수 있음
### 문제 2: 정책에 과도하게 적응
- Actor가 Q-network에 맞춰 과하게 조정되며, 학습이 불안해짐
### 문제 3: Actor 업데이트가 너무 자주 발생
- Q값이 아직 안정적이지 않은데도 Actor 업데이트가 너무 자주 발생하여 발산 가능성이 존재함.

이와 같은 문제를 해결하는 것이 TD3이다.