---
title: "PPO 알고리즘을 이용하여 Reacher-v5 환경을 해결해보자"
layout: post
date: 2025-05-03 17:05:00 +0900
categories: study
---

# PPO 알고리즘을 이용해서 Reacher-v5 환경을 해결해보자.
4월 중순부터는 중간고사 시험공부에만 몰두하느라 개인 개발 공부는 신경쓰지 못하였다. 이번주 월요일에 중간고사가 끝났고 오늘부터 다시 시작하려고한다.

지난번에 Hopper-v5 환경을 PPO 알고리즘으로 해결한 적이 있다.
환경 하나 더 해결을 해본 뒤 SAC, DDPG 같은 심화 알고리즘을 공부해볼 생각이다.

## Reacher-v5
Reacher-v5는 Hopper-v5보다 간단한 환경이다.
실은, Walker2d 환경을 PPO 알고리즘으로 시도했으나 알고리즘의 한계인지 코드를 잘못 짠것인지는 모르겠으나
성능이 좋아지지 않아서 Reacher-v5는 심화 알고리즘을 공부한 후 시도하려고 한다.

## Hopper-v5 코드의 변형
[Reacher](https://github.com/soonawg/reacherv5/blob/main/reacher.py)
처음에 생각한 접근법은 Hopper-v5에서 사용한 코드를 아예 그대로 사용하면 아무리 그대로 환경이 다르니 무리가 있을 수 있다고 생각해
변형을 주었다.
주요한 것들만 설명하면 다음과 같다.
1. 네트워크 구조 최적화
* Hidden 차원 축소 : 256에서 128로 줄였다. Reacher 환경은 Hopper보다 단순한 문제라고 생각해서 줄였다.
* 활성화 함수 변경 : Actor 네트워크에서 ReLU 대신 Tanh를 사용했다. Reacher는 위치 제어 문제이므로 Tanh가 더 효과적일 수 있다 생각했다.
* 더 안정적인 초기화 : log_std 초기값을 -0.5로 설정하여 초기 탐색을 줄였다.
2. 보상 스케일링을 추가하였다.
* Reacher의 보상은 주로 음수이므로 스케일링을 통해 학습 신호를 강화하였다.
3. 하이퍼파라미터 조정
* Critic 학습률 증가 : 1e-3로 증가시켜 빠른 학습 유도
* 배치 크기 감소 : 256에서 64로 줄여 더 작은 배치로 더 자주 업데이트한다.
* 엔트로피 계수 증가 : 0.01에서 0.02로 증가시켜 더 많은 탐색을 장려한다.
* 그래디언트 클리핑 추가 : 학습 안정성을 높이기 위해 그래디언트 클리핑을 적용했다.

## Hopper-v5에서 사용했던 코드를 그대로 사용해보기도 하였다.
[Reacher-Hop](https://github.com/soonawg/reacherv5/blob/main/reacher_hop.py)

하지만 실제로는, 오히려 코드를 그대로 사용한 것이 성능이 좋았다.
왜 그랬을까?
추측은 다음과 같다.
1. 환경에 맞추어 코드를 변경하는 것보다 어느 때는 일반화하는 것이 효율적이다.
2. Hopper-v5 코드에서 사용했던, 더 큰 네트워크, ReLU 활성화 함수의 장점 같은 일반적인 장점들이 좋게 작용했다.

하지만, 환경에 달리 코드를 변경하거나 하이퍼파라미터를 조정하는 것은 좋은 경험이라고 생각한다.