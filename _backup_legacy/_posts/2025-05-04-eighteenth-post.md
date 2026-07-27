---
title: "강화학습 심화 알고리즘 - SAC"
layout: post
date: 2025-05-04 21:06:00 +0900
categories: study
---

# 강화학습 심화 알고리즘 中 SAC에 대하여

SAC는 Soft Actor-Critic으로, 지금까지 DDPG -> TD3의 발전과정처럼 SAC 역시 TD3에서 개선된 알고리즘이다.
SAC의 기본 아이디어는 TD3의 안정성에 exploration 강화를 위한 엔트로피 보너스를 더한 구조이다.

## SAC의 핵심 구성 요소
1. Stochastic Policy (확률적 정책)
* 지금까지 DDPG, TD3는 Determinstic policy였고, SAC는 Stochastic Policy이다.
* 여기서 중요한 점은 Reparameterization Trick을 사용해야한다. 그럼으로써 backpropagation을 사용할 수 있다. (이에 관한 자세한 설명은 후에 있음)
2. Entropy Regularization (엔트로피 보너스)
- 정책이 다양한 선택을 시도하게 유도한다.
3. Critic을 2개 사용한다.
- 이것은 TD3와 동일

## Reperameterization Trick이란?
### 왜 Reperameterization Trick이 필요한가?
- SAC 같은 Stochastic Poilicy는 확률 분포에서 액션을 샘플링한다.
- 하지만 이 방법에서 문제는 이러한 연산은 Non-Determinstic해서 gradient를 수행할 수 없고, 이 말은 즉슨 policy gradient로 학습이 불가능하다는 것을 의미한다.

### 핵심 아이디어는 다음과 같다.
- 정규 확률 분포에서 직접 샘플링을 하지 않고, 정규분포에서 epsilon 값만 노이즈 샘플링을 하고 나머지는 계산은 전부 deterministic하게 수행한다.
- 그렇게 하면, 정책 gradient를 backpropagation으로 계산할 수 있게 된다.
- (수식을 함께 써놓으면 좋겠지만, LaTex 문법이 익숙치 않아 글로만 적는다.)