---
title: "DAC 공부 - 모방학습"
layout: post
date: 2025-06-26 14:01:00 +0900
categories: study
---

# 배경과 동기
GAIL은 다음과 같은 한계를 가진다:
- 샘플 효율성 부족
- PPO의 불안정성
- replay buffer를 사용하지 않음
=> 이를 해결하기 위해 DAC은 off-policy 방식으로 GAIL 구조를 재설계함

# 학습 과정
1. 초기화: policy, Q-network, Discriminator, replay buffer
2. 환경 상호작용: 에이전트 환경에서 (s, a, r, s^') 수집 -> replay buffer에 저장
3. Discriminator 학습:
- Expert의 (s, a)와 Agent의 (s, a)를 사용해 Discriminator를 binary classification으로 학습
- Discriminator(s, a)는 전문가일 확률
4. 보상 생성 (reward shaping):
- `r̂(s,a) = -log(1 - D(s,a))` 또는 `r̂(s,a) = log(D(s,a))`
- Discriminator가 잘 속이면 낮은 보상, 못 속이면 높은 보상
5. Actor-Critic 학습:
- shaped reward를 사용하여 Q-learning 진행 (off-policy)
- TD3 또는 DDPG 또는 SAC 기반으로 안정적 업데이트
6. 반복

# 특징 및 장점
- 샘플 효율성: replay buffer를 사용하여 학습 안정성 높음
- PPO 불필요: TD3/DDPG 등으로 더 안정적이고 가벼운 학습 가능
- 오프라인 가능성: 수집한 경험만으로도 imitation 가능 (offline RL과 호환)
- 성능: GAIL보다 더 빠르게 학습하고 더 높은 reward 도달

# 샘플 코드
## [Code](https://github.com/soonawg/dac_sample/blob/main/dac_sample.py)