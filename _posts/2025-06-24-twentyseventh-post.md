---
title: "DAgger 공부 - 모방학습: 여름방학 공부기"
layout: post
date: 2025-06-24 13:37:00 +0900
categories: study
---

# 지난 시간에 한것
지난 시간에는 CartPole 환경에서 모방학습(Behavior Cloning, BC)를 실습해보아싿.
처음엔 전문가 궤적만으로 학습했지만, 성능이 불안정했고 평균 보상이 50~150 사이로 크게 흔들렸다.
이유는 바로 distribution shift 때문이었고, 이를 해결하기 위해 DAgger를 공부해보았고, 적용해보았다.

# Behavior Cloning의 한계
기존 BC 포스트에서도 언급했지만, BC는 전문가의 행동을 그대로 사용하는 supervised learning 방식이다.
단, 이 방식의 한계는 훈련된 상태 분포에서 나와 모르는 상태 분포로 가게되면 잘못된 예측을 하게 된다는 것이다.
이를 해결하기 위해 DAgger 아이디어가 제시되었다.

# DAgger란?
DAgger란 **Dataset Aggregation**의 줄임말로, 모방 학습에서 발생하는 Distribution Shift 문제를 해결하기 위한 Iterative Learning 알고리즘이다.

# DAgger의 동작 방식
1. 학습자가 rollout을 수행하고
2. 그 과정에서 만난 상태들에 대해 전문가에게 정답 행동을 다시 묻고
3. 데이터를 계속 누적하면서 재학습
즉, 결과적으로 iteration이 반복될수록 학습자는 전문가를 점점 더 잘 따라갔고, reward는 200까지 안정적으로 도달했다.

# 배운점
- BC는 간단하지만 상태 분포가 다르면 쉽게 무너진다.
- 전문가 피드백을 반복적으로 받을 수 있다면, DAgger는 훌륭한 대안이다.

# 다음 공부할 것
다음에는 GAIL과 Inverse RL도 공부할 예정이다.

# 실습 코드
## [Code](https://github.com/soonawg/dagger_cartpole/blob/main/dagger_cartpole.py)