---
title: "TD3+BC 공부 - Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-27 12:50:00 +0900
categories: study
---

# 배경
- Offline RL에서 표준 TD3는 충분한 데이터 다양성이 없을 때 overestimation 및 OOD action 문제에 취약함
- BC는 데이터셋의 행동을 그대로 모방하므로 안정적이지만, 최적 정책은 찾지만 못한다.
- TD3+BC는 TD3의 가치추정 능력과 BC의 안정성을 결합하여, offline dataset 기반에서도 성능을 높이는 목적의 알고리즘이다.

# 핵심 아이디어
- TD3를 기반으로 한 actor-critic 구조를 유지하면서, actor의 업데이트 시 기존 offline 데이터셋의 행동을 모방하는 BC loss를 가중해서 섞어준다.
- 즉, policy가 dataset 내의 행동 분포에서 크게 벗어나지 않도록 유도하여 offline setting에서 더 안정적이고 합리적인 정책을 학습하도록 한다.

# 학습 절차
1. Critic 업데이트 (TD3)
- target policy로 다음 상태의 액션을 샘플
- double Q-learning을 적용
- target smoothing으로 노이즈를 추가
- MSE loss로 Q를 업데이트
2. Actor 업데이트 (TD3+BC)
- policy의 출력을 dataset action에 가깝게 만드는 BC loss
- Q값을 높이는 RL loss
- 두 가지 alpha 비율로 혼합
3. Target network soft update
- TD3와 동일하게 target network를 타우로 부드럽게 업데이트
4. 위의 과정을 반복

# 장점
- Offline dataset에서 out-of-distribution action이 발생하지 않도록 안전하게 학습
- BC로 안정성을 보장하면서 Q-learning 기반 TD3의 성능도 유지
- 구현이 간단하면서도 매우 

# 단점
- alpha 조절에 따라 성능이 민감할 수 있다
- 여전히 Q-function의 overestimation 문제가 남아 있을 수 있다

# 샘플 코드
## [Code](https://github.com/soonawg/offline_rl_sample/blob/main/td3_bc_sample.py)