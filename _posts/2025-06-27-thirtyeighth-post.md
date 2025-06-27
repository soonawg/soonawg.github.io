---
title: "AWAC 공부 - Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-27 13:43:00 +0900
categories: study
---

# 배경 및 개념
- AWAC는 **offline RL** 및 **offline-to-online RL** 상황에서 행동 복제(Behavior Cloning, BC)와 RL을 결합한 **hybrid 알고리즘**이다.
- 기존의 behavior cloning은 expert 데이터를 잘 모방하지만, **환경과 상호작용** 없이 수집된 데이터만으로는 보상을 최적화하기 어렵다.
- 반면 오프라인 RL은 최적화를 시도하지만 데이터 분포 바깥(out-of-distribution)의 행동을 선택할 때 catastrophic한 성능 저하가 발생한다.
- AWAC는 **advantage를 기반으로 한 가중치(weighted behavior cloning)** 를 적용하여 **advantage가 높은 행동을 더 많이 따라가고** advantage가 낮거나 부정적인 행동은 덜 따라가는 방식으로 policy를 업데이트한다.
        
→ 즉 **BC + advantage 기반의 soft policy improvement** 라고 볼 수 있다.

# 핵심 아이디어
- 전문가 데이터를 그대로 복제(BC)하되, **advantage가 높은 행동**일수록 더 강하게 모방하도록 확률적으로 유도한다.
- 이렇게 하면:
    - 오프라인 데이터 내의 **좋은 행동(good actions)** 은 강화하고
    - 나쁜 행동(bad actions)은 억제할 수 있음
- 결국 **offline 데이터의 한계를 보완**하면서 안정적으로 policy를 fine-tune할 수 있다.

# 학습 파이프라인
- Q-함수는 일반적인 DDPG, TD3, SAC 기반의 Q-learning으로 학습
- actor는 위의 **advantage-weighted likelihood** 방식으로 학습
- replay buffer에 저장된 transition 데이터를 사용
- actor-critic 형태를 그대로 따르면서, actor는 advantage 기반 behavior cloning으로 업데이트

# 장단점
✅ 장점

- offline 데이터에서도 안정적으로 policy 학습 가능
- BC 기반이라 수렴 안정성이 높음
- advantage weighting을 통해 sub-optimal한 행동은 억제 가능

❌ 단점

- advantage 추정이 잘못되면 잘못된 행동에 weight를 실어줄 수 있음
- lambda 하이퍼파라미터 튜닝 필요