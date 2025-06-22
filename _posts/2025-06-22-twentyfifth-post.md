---
title: "Behavior Cloning 공부 - 모방학습"
layout: post
date: 2025-06-22 21:55:00 +0900
categories: study
---

# BC의 정의
**Behavior Cloning**은 말 그대로 행동을 복제하는 것이다.

전문가(Expert)의 행동 데이터를 보고 **그와 똑같은 행동을 할 수 있도록 정책을 학습하는 방식**이다.

- 지도학습(supervised learning)의 형식을 그대로 사용
- 입력은 상태(state), 출력은 행동(action)
- 정답은 전문가가 실제 했던 행동

# Behavior Cloning의 장점과 한계

## 장점
- 매우 간단하고 구현이 쉬움
- 빠르게 정책을 학습할 수 있음
- 전문가가 충분히 잘 수행하면 높은 성능 가능
## 단점
- Distribution Shift 문제 발생
- 전문가 정책이 완벽해야 함
- 학습 데이터가 부족하면 취약함
- Long-horizon task에서는 오류가 점점 누적됨

# Distribution Shift 문제란?
Behavior Cloning은 **전문가의 데이터 분포에만 의존해서 학습**한다. 그런데, 학습한 정책이 환경에서 직접 행동을 수행하게 되면 **훈련 때와는 다른 상태들**에 자주 노출됨.

## 해결 방법으로는?
1. Noise Injection / Data Augmentation
2. 정규화
3. Hybrid Approaches
4. 앞으로 공부할 DAgger

# 실습
[Code](https://github.com/soonawg/bc_cartpole/blob/main/bc_cartpole.py)