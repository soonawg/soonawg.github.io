---
title: "객체 추적: vision - 여름방학 공부기"
layout: post
date: 2025-07-16 17:29:00 +0900
categories: study
---

# 객체 추적이란?
객체 추적은 비디오 프레임 시퀀스 속에서 동일한 객체를 시간에 따라 추적하는 기술이다.

# 객체 추적이 필요한 이유
- 로봇 - 사람이나 물체의 이동을 추적하여 인터렉션
- CCTV - 사람/차량의 이동 경로 추적
- 자율주행 - 다른 차량의 속도, 위치 예측
- AR/VR - 현실 속 객체의 지속적인 트래킹

# 주요 객체 추적 기법 정리
- Kalman Filter
- SORT
- DeepSORT
- ByteTrack
등

# Kalman Filter란?
Kalman Filter는 연속적인 상태 추정 알고리즘이다.
예를 들어,
- 물체의 위치(x, y), 속도(vx, vy)를 추적한다고 할 때
- 지금 물체가 어디에 있을지 예측
- 감지 결과와 결합하여 보정한다

Kalman Filter의 특징으로는
- 빠르지만 단순하고, apperance는 고려 불가하여 혼동되기 쉽다.

# DeepSORT
SORT에 딥러닝 기반 외형 특징 추출기를 추가한 고급 트래커이다.

## 구성 요소
- Kalman Filter -> 위치 예측
- CNN feature extractor -> 객체의 외형 임베딩
- Cosine Distance + IOU -> ID Matching

## 장점
- 외형 기반 ID 유지 가능
- 여러 유사한 객체가 있어도 잘 구분
- 비교적 정확함

## 단점
- CNN feature 추출로 인해 속도가 느려짐
- 실제 임베딩 네트워크 훈련 필요

