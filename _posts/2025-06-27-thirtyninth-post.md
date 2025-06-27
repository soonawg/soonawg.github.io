---
title: "BCQ 공부 - Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-27 18:00:00 +0900
categories: study
---

# 배경
- Offline RL 환경에서는 데이터셋(=behavior policy로 수집한 데이터) 밖의 행동을 탐험(extrapolation)하면 Q-function이 잘못된 값을 예측해 **overestimation** 이 발생할 수 있다.
- 특히, Q-learning 기반 방식(DDPG, TD3 등)은 행동공간 어디든 행동을 탐색하므로, **dataset에 존재하지 않는 행동에 대해 Q-value가 부정확해지는 문제**가 크다.
- 이를 막기 위해 **Behavior Cloning을 통한 행동 제약 + Q-learning**을 결합한 것이 BCQ이다.

# 핵심 아이디어
- **Q-러닝을 그대로 사용하되**, 정책이 행동을 샘플링할 때 dataset(behavior policy)에서 멀리 벗어나지 못하게 **제약**을 둔다.
- 이를 위해:
    - VAE(Variational Autoencoder)를 사용해 데이터셋의 행동 분포를 근사 (Behavior policy 모델링)
    - Q-value가 가장 높은 행동을 선택할 때, VAE에서 샘플링한 행동 중에서만 선택
    - 즉, “**dataset 행동 안에서의 argmax Q**” 를 하도록 강제하는 구조

# 구성요소
## VAE-based Behavior Policy
- 행동 분포를 복원하기 위해 (state, action) 데이터를 학습
- 주어진 상태에서 plausible(데이터셋에 존재할 법한)한 행동을 샘플링
- latent 공간을 활용해 **다양한 행동을 생성**할 수 있도록 한다

## Perturbation model
- VAE가 샘플링한 행동에 소규모 perturbation(미세한 변화)을 추가해 fine-tuning
- VAE만으로는 완전히 최적 행동을 찾기 어렵기에, “조금의 변화”를 허용해 Q-value를 높일 수 있게 한다

## Q-function

# 학습 과정 요약
1. **VAE** 학습: 데이터셋의 행동 분포를 잘 복원하도록 학습
2. **Q-function** 학습: DDPG/TD3 방식으로 Bellman backup
3. **Perturbation model** 학습:
    - VAE 행동에 작은 perturbation을 주어 Q-value가 더 높은 행동을 찾도록 훈련
4. 정책(action selection):
    - VAE에서 여러 행동 샘플 → perturb → 그 중 Q가 가장 높은 행동 선택
    - 즉, offline 데이터의 분포에서 크게 벗어나지 않는 safe한 행동

# BCQ의 장점
✅ Out-of-distribution action을 강하게 제약해 **offline RL의 extrapolation error** 를 크게 줄인다
✅ 데이터셋 안에서 잘 generalize할 수 있다
✅ VAE+Perturbation 조합이기 때문에, dataset 내의 다양성을 유지하면서도 Q-learning의 최적화 장점을 쓸 수 있다

# 샘플 코드
## [Code](https://github.com/soonawg/offline_rl_sample/blob/main/bcq_sample.py)