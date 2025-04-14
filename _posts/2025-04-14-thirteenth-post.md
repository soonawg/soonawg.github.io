---
title: "PPO 알고리즘을 이용하여 Hopper-v5 환경을 해결해보자"
layout: post
date: 2025-04-14 23:35:00 +0900
categories: study
---

# 강화학습 PPO 알고리즘을 이용하여 Mujoco의 Hopper-v5 환경을 해결해보자.
지금까지 배웠던 강화학습 이론들을 이용하여 조금 더 복잡한 실습을 진행하고자한다.
Mujoco에서 기본으로 제공되는 환경중 하나인 `Hopper-v5` 환경을 PPO 알고리즘을 통하여 해결하는 실습을 진행할 것이다.
단, 완전 기본 Mujoco는 아니고, `gymnasium[mujoco]`이다.

## PPO 알고리즘의 주요 특징들
우선, PPO 알고리즘의 주요 특징들을 살펴보자.
1. Clipping
PPO 알고리즘의 핵심 아이디어는 각 학습 에포크에서 policy가 너무 크게 변화하지 않도록 안정성을 높이는 것이다.
그를 위해, 클리핑이라는 아이디어를 이용한다.
2. GAE
GAE를 요약하자면, 편향-분산 트레이드오프를 조정하여 advantage와 value function을 추정할 수 있다.   

즉, 주요 특징은 학습에 있어서의 안정성을 높이는 것이다.

## 첫번째 버전의 코드
[First_PPO](https://github.com/soonawg/ppo_hopper_v5/blob/562e237cec4ffa0eaa961390bf14a44cde5be4b2/first_version_ppo.py)   
위 링크를 통해 확인 가능하다.
![Result_image_first](/assets/images/2025-04-14/ppo_hopper.png)
결과 사진을 보면 Reward가 거의 0에 수렴한다.

## 두번째 버전의 코드
[Second PPO](https://github.com/soonawg/ppo_hopper_v5/blob/562e237cec4ffa0eaa961390bf14a44cde5be4b2/second_version_ppo.py)
위 링크를 통해 확인 가능하다.
![Result_image_second](/assets/images/2025-04-14/ppo_hopper1.png)
결과 사진을 보면 Reward가 평균이 약 60정도로 많이 개선된 것을 볼 수 있다.

## 무엇을 바꿔서 이렇게 개선됬을까?
첫번째 버전의 코드와 두번째 버전의 코드는 전체적인 틀은 같으나 바꾼점이 몇가지 있다.

### Actor 네트워크의 활성화 함수
Actor 네트웨크에서 활성화 함수를 Tanh에서 ReLU로 바꿨다.
그렇다면 왜 활성화 함수를 Tanh에서 ReLU로 바꾼게 성능이 좋아지게 된걸까?
우선 Tanh에서 ReLU로 변경했을 때의 장점들을 생각할 수 있다.
* Tanh 함수는 출력 범위가 [-1, 1]로 제한되어 있어, 네트워크의 깊이가 깊어질수록 기울기 소실 문제가 발생할 수 있다. 반면 ReLU는 기울기 소실 문제가 발생할 문제를 줄여준다.
* ReLU는 비선형성을 제공하면서도 `exp()` 함수를 상요하지 않으므로 학습 속도가 빠르다.
* Tanh는 출력 범위가 [-1, 1]로 제한되어 있지만, ReLU는  [0, 무한대)로 더 넓은 범위를 가진다. 이는 네트워크가 다양한 표현을 학습할 수 있게 된다.   


## 결과적으로
* 약 250 에피소드에서 최고 성능이 도달됬음
* Actor의 ReLU와 Critic의 Tanh 조합이 안정적인 학습을 제공함
* GAE를 통한 효과적인 advantage 추정
* PPO 클리핑으로 안정적인 Policy 업데이트가 가능해짐