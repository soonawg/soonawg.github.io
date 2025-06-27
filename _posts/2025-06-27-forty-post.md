---
title: "BEAR 공부 - Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-27 18:05:00 +0900
categories: study
---

# BEAR의 개념
- BEAR는 Offline RL에서 잘못된 OOD 행동을 방지하기 위해 제안된 알고리즘이다.
- 기존 actor-critic은 actor가 학습 데이터 분포 밖의 행동을 선택해 잘못된 Q 추정값을 유도할 수 있다는 문제가 있었다.
- BEAR는 Behavior Regularization(즉, 데이터셋의 행동 분포와 가까운 행동을 선호하도록 유도)으로 이를 해결한다.
- 핵심적으로는:
    - 행동 정책이 데이터셋의 행동 분포와 너무 멀어지지 않도록
    - Maximum Mean Discrepancy (MMD)를 이용해 policy를 제약한다

# 왜 BEAR가 필요한가?
- Offline RL에서는 Q-value가 **데이터셋에 없는 상태-행동 쌍**에 대해서는 부정확하다.
    - 이런 OOD 행동으로 actor가 policy를 업데이트하면 policy가 망가짐
- 따라서 actor가 **데이터셋 내 행동과 유사**한 행동만 탐색하게 제한해야 한다.
- BEAR는 이를 **MMD**로 수식적으로 강제한다.

# BEAR의 핵심 아이디어
1. **Actor-critic** 프레임워크를 사용한다.
2. Actor가 생성한 행동이 **데이터셋의 행동 분포**에서 크게 벗어나지 않도록 **MMD**를 이용해 regularization을 걸어준다.
3. MMD는 쉽게 말해 두 분포의 거리를 측정하는 non-parametric distance metric으로, Gaussian kernel 등을 쓴다.
4. Policy가 업데이트될 때,
    - Q를 최대화하려고 노력하면서
    - MMD 거리가 너무 커지면 패널티를 부여해서 dataset 행동 분포 근처로 유지하도록 만든다.

# BEAR의 학습 과정
1. **Q-function** 업데이트:
    - 표준 Bellman backup으로 업데이트 (TD3와 유사)
    - 클리핑이나 target smoothing 적용
2. **Actor** 업데이트:
    - Q값이 높으면서
    - MMD 거리가 작도록 최적화
3. **Behavior policy estimation**:
    - 데이터셋의 행동 분포 mu(a|s)를 추정하기 위해
    - 일반적으로 **VAE**를 사용해 행동 모델을 학습
    - 이 VAE 샘플을 기준으로 MMD를 계산
4. **Policy constraint**:
    - VAE에서 샘플한 행동과 actor의 행동 분포가 비슷한지를 MMD로 체크
    - threshold를 넘으면 penalty를 크게 적용
5. **Action selection**:
    - actor가 최종적으로 선택한 행동을 perturbation 없이 그대로 사용
    - OOD를 
    
# 샘플 코드
## [Code](https://github.com/soonawg/offline_rl_sample/blob/main/bear_sample.py)