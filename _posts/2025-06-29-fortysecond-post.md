---
title: "실습 - BC & DAgger: 여름방학 공부기"
layout: post
date: 2025-06-29 21:35:00 +0900
categories: study
---

# 실습 공부
이제 모방학습과 Offline RL에 대한 어느 정도의 공부를 마쳤고, 실습을 진행하고자 한다.
나의 실습 계획은 다음과 같다.

## 1. BC + DAgger 통합 실습
## 2. GAIL VS BC VS IRL 성능 비교
## 3. Offline RL 파이프라인
## 4. IQ-Learn DAC 실험

이와 같은 실헝으로 알고리즘을 실제로 구현해보고, 다음 공부로 넘어가기 위한 준비를 하고자한다.
이번 포스트에서는 **BC + DAgger 통합 실습**을 진행하고서 리뷰를 위한 포스트를 진행하고자 한다.

-----

# BC + DAgger 통합 실습
이 실습의 전제는 다음과 같다.
* 환경: CartPole-v1, LunarLander-v2 같은 gymnasium 기반 가벼운 환경
* 데이터: 직접 수집한 expert rollout
* 진행:
    * 수집한 데이터를 BC로 학습
    * 이후 DAgger 반복 
    
참고로, 나는 CartPole-v1에서 실습을 진행하였다.

## 실행 흐름
실행 흐름은 다음과 같다.
전문가 데이터 수집 -> BC 초기 학습 -> DAgger 반복 -> 성능 평가

## 코드 전체 구조
전체 구조:
├── 1. 라이브러리 임포트 및 설정
├── 2. 정책 네트워크 클래스들
│   ├── PolicyNetwork (학습 대상) : 학습할 신경망 정책
│   └── ExpertPolicy (전문가 정책) : 규칙 기반 전문가 정책
├── 3. 데이터 관리 클래스
│   ├── DataCollector (데이터 수집) : 데이터 수집 관리
│   └── Dataset (데이터 저장/로드)
├── 4. 학습 알고리즘 클래스들
│   ├── BC (Behavioral Cloning) : Behavior Cloning 학습
│   └── DAgger (Dataset Aggregation) : DAgger 반복
├── 5. 평가 및 유틸리티 클래스
│   ├── Evaluator (성능 평가)
│   └── Logger (로그 기록)
└── 6. 메인 실행 함수
    └── main() (전체 파이프라인)

## 코드 성능
Final performance는 216.20 평균적인 성능이 나왔다.
다음 사진을 보면, DAgger의 반복을 통해 performance 및 데이터셋 증가가 어떻게 이루어졌는지 알 수 있다.
![DAgger_prac](/assets/images/2025-06-29/DAgger_prac.png)

### 전체 코드는 다음과 같다.
**[Code](https://github.com/soonawg/offline_rl_sample/blob/main/%EC%8B%A4%EC%8A%B5/bc_dagger.py)**