---
title: "심화 Exploration 기법에 대해 알아보자"
layout: post
date: 2025-05-07 22:30:00 +0900
categories: study
---

# 심화 Exploration 기법에 대해서 알아보자.
기존에 배웠던 강화학습 알고리즘들에서는 Exploration 기법으로 epsilon-greedy, Gaussian noise, entropy 보상 등을 사용하였지만 이에 더해 더 강력한 Exploration 기법이 있다.

## RND (Random Network Distillation)
### 핵심 아이디어
- 처음 보는 상태일수록 예측이 어려우니 보상을 많이 주기
### 구조
- Target Network : 랜덤하게 초기화되어 고정됨
- Predictor Network : 입력을 받아 Target Network의 출력을 모방하도록 학습됨
- 즉, `Intrinsic reward = prediction error between predictor and target`

### 특징
- 에이전트가 예측하기 어려운 상태를 더 자주 방문하게 됨
- Sparse reward 문제에서 매우 효과적임


## Go-Explore
### 핵심 아이디어
- 지금까지 가본 상태를 명시적으로 기억하고, 그 상태로부터 탐험 계속

### 세 단계 전략
1. Explore : 가능한 다양한 상태(또는 셀)를 방문하고 기록
2. Return : 과거에 갔던 흥미로운 상태로 복귀
3. Explore from there : 그 상태로부터 다시 탐험을 시작


이정도가 간단히 RND와 Go-Explore의 개념을 정리한 것이다.
그렇다면, 정마로 심화 Exploration 기법이 epsilon-greedy나 Entropy Bonus보다 더 효율적일까?

## 탐험 기법 비교
RND 중점적으로 비교

### RND가 더 효율적인 경우
- 보상이 희박한 환경
- 복잡한 상태 공간
- 전통적인 탐험 기법이 빠르게 수렴하는 환경에서 새로운 상태를 유도하고 싶을 때

### 그렇다고 RND가 항상 완벽하지는 않음
- 모든 상태를 고르게 탐험하는게 아니라 오히려 노이즈가 많아질 수 있음