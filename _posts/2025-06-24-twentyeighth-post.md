---
title: "GAIL 공부 - 모방학습"
layout: post
date: 2025-06-24 22:00:00 +0900
categories: study
---

# GAIL이란?
GAIL은 강화학습 환경에서 전문가의 행동을 모방하되, 보상을 따로 주지 않아도 되는 Adversarial Imitation Learning 방식이다. GAN과 유사한 구조로, 정책(Generator)이 전문가처럼 보이게 행동하도록 학습되고, 판별기(Discriminator)는 전문가와 에이전트를 구분한다.

# GAIL의 기본 아이디어
GAIL은 GAN의 구조를 모방학습에 응용한 방식이다.
- Generator -> 에이전트의 정책 pi
- Discriminator -> 전문가와 에이전트를 구분하는 판별기
전문가 데이터와 에이전트 데이터를 구분하는 판별기 D를 학습하고
에이전트는 이 판별기를 속이도록 강화학습을 통해 정책을 학습한다.

# GAIL의 학습 흐름 요약
1. 전문가의 학습 데이터 (s, a)를 수집
2. 에이전트는 현재 정책으로 데이터를 수집
3. 판별기 D는 (s, a)가 전문가인지 에이전트인지 구분하도록 학습
4. 에이전트는 `-log(D(s, a))`를 보상으로 삼아 강화학습
5. 반복

# GAIL의 장점과 한계
## 장점
- 보상이 주어지지 않는 환경에서도 전문가의 행동을 모방가능
- 강화학습 기반이라 일반화 성능이 뛰어날 수 있음
- 최소한의 전문가 데이터로도 강력한 성능 가능
## 단점
- GAN처럼 학습이 불안정할 수 있음
- 강화학습 자체의 샘플 효율 문제 존재

# 샘플 코드
## [Code](https://github.com/soonawg/gail_example/blob/main/gail_example.py)