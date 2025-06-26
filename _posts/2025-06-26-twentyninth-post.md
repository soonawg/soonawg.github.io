---
title: "MaxEnt IRL & AIRL 공부 - 모방학습: 여름방학 공부기"
layout: post
date: 2025-06-26 14:00:00 +0900
categories: study
---

# MaxEnt IRL

## MaxEnt IRL이 필요한 이유
기존 IRL 방식은 **다양한 보상 함수**가 **동일한 전문가 정책**을 설명할 수 있어서 multi-solution함
그래서 MaxEnt IRL은 이를 해결하기 위해 **확률적 행동 모델**과 **최대 엔트로피 원칙**을 도입

### Multi-Solution 문제란?
간단히 말하면, 다음과 같다.
IRL은 multi-solution하다는 말은 동일한 전문가 정책을 설명할 수 잇는 보상함수(R)가 여러 개 존재한다는 뜻이다.
즉,
- 동일한 전문가 행동(정책)을 설명할 수 있는 보상 함수가 여러 개 존재함
- 이 때문에 학습된 보상 함수가 애초의 의도와 다를 수 있음
- MaxEnt IRL은 이 중에서 가장 일반적이고 덜 확신에 찬 해법을 선택

## MaxEnt IRL의 핵심 아이디어
1. 전문가의 행동은 목표를 달성하면서도, 가능한 한 다양한 경로를 선택한다는 가정을 한다.
2. 따라서 MaxEnt IRL은 단순히 "보상을 최대화 하는 경로"만 찾는 것이 아니라, 그 보상을 따르면서도 가능한 다양한 정책 중 "엔트로피가 높은" 정책을 찾는다.
3. 이렇게 하면,
- 과도하게 특정 행동에 집착하지 않게 되고
- 데이터가 부족해도 일반화가 잘 되며
- **다양한 정책이 하나의 보상으로 설명되는 'multi-solution 문제'**도 어느 정도 해결된다.

### MaxEnt IRL의 학습 흐름 요약:
- 전문가 데이터를 바탕으로 feature expectation을 추출
- 현재 보상 함수가 만들어내는 정책으로 샘플 trajectories 생성
- 두 feature expectation 사이의 차이를 줄이도록 보상 함수의 파라미터를 업데이트
- 이 과정에서, 정책은 확률적으로 동작하고(soft policy), 엔트로피가 높도록 유도된다.

## 샘플 코드
### [Code](https://github.com/soonawg/maxent_irl_sample/blob/main/maxent_irl.py)

---

# AIRL

## 개요 및 배경
AIRL은 IRL과 GAIL을 결합한 형태.
보상 함수를 추정하면서도, 전문가의 행동을 모방하는 정책을 학습하는 알고리즘이다.
- GAIL은 전문가 정책을 흉내내지만, 보상 함수는 명시적으로 모델링하지 않음
- 반면, AIRL은 보상 함수를 추정할 수 있고, 환경을 바꿔도 **일반화가 가능**

## 목적 및 핵심 아이디어
AIRL의 목표는:
- 전문가의 행동을 설명하는 보상함수를 추정하고
- 그 보상에 따라 학습된 정책이 전문가와 유사한 행동을 하게끔 하는 것이다.
여기서 중요한 것은:
- AIRL은 보상함수를 학습하고 추출할 수 있다는 점이 GAIL과 다르다.

## 학습 구조
AIRL은 GAN처럼 두 가지 모델을 번갈아 학습한다.
- Discriminator: 전문가 vs 정책 데이터를 구분
- Geneartor (policy): 판별기를 속이도록 행동을 생성
- Reward Model: 판별기의 내부 구성 요소로 존재함

학습은 다음과 같이 진행된다:
1. 전문가와 에이전트의 데이터를 수집
2. **Discriminator를 학습**해 두 데이터를 구분
3. **Discriminator의 출력으로부터 reward를 추정 (GAIL과의 차이점)**
4. **PPO 같은 RL 알고리즘으로 policy를 학습**
5. 위 과정을 반복

## AIRL의 장점과 특징
- 보상 함수 학습 가능: 단순히 흉내 내는 GAIL과 달리 환경이 바뀌어도 재사용이 가능
- MaxENnt IRL 기반: MaxEnt IRL의 목적함수를 GAN 방식으로 근사
- 강화학습 알고리즘 통합 가능: PPO, TRPO, SAC 등과 결합 가능

## 샘플 코드
### [Code](https://github.com/soonawg/maxent_irl_sample/blob/main/airl.py)