---
title: "DDPG vs TD3 vs SAC"
layout: post
date: 2025-05-04 21:05:00 +0900
categories: study
---

# DDPG, TD3, SAC 비교분석

DDPG, TD3, SAC는 약간의 특징들만 발전하고 유사한 알고리즘으로 비교 분석하여 환경에 따라 어떤 특징을 살리며 어떤 알고리즘을 사용해야할까를 생각하는게 좋을까를 분석하는게 생각하는 것이 바람직하다고 생각하여 이 글을 작성한다.
차이를 중점으로 작성하였으며, 그 이상의 세부적인 것은 배제하였다.

## DDPG, TD3, SAC 정책 타입
- DDPG : Deterministic
- TD3 : Deterministic
- SAC : Stochastic

## DDPG, TD3, SAC Exploration 방법
- DDPG : 외부 노이즈 필요 (e.g. OU 노이즈)
- TD3 : 외부 노이즈 필요 + Target Policy Smoothing
- SAC : 내부적으로 엔트로피 보너스를 통해 Exploration 유도

## Critic 구조
- DDPG : Q-Network 1개
- TD3 : Q-Network 2개 (min 사용)
- SAC : Q-Network 2개 (min 사용 + 엔트로피 보정)

## Actor 업데이트
- DDPG & SAC : 매 step마다
- TD3 : Delayed update

## 주관적인 평가
- DDPG : 쉬운 알고리즘 구현 난이도와 간단한 복잡도, 빠른 학습 속도 등 장점이 있으나, 낮은 성능 안정성 등으로 실전 활용도는 낮은 편이다. 그 결과 사용 자체는 힘든 편이고, 학습의 기초 개념 파악용으로 쓰는게 좋을 것 같았다.
- TD3 : SAC가 아무리 개선한 알고리즘이더라도 SAC에 비해서 더 빠른 학습 속도와 Deterministic의 장점이 있을 거라 생각한다(아직은 모르긴함).
- SAC : 가장 셋 중에 개선된 알고리즘이고, 안정적이므로 실전에서 많이 사용되고, 복잡한 Continuous space에서 널리 사용될 것이다.