---
title: "IQ-Learn 공부 - 모방학습"
layout: post
date: 2025-06-26 14:55:00 +0900
categories: study
---

# IQ-Learn의 핵심 아이디어 요약
IQ-Learn은 다음 두 가지 문제를 동시에 해결하려는 목적에서 출발한 알고리즘이다.
1. **보상 함수 없이 전문가의 정책을 학습하고 싶다** (→ 모델이 직접 보상 추정 없이 Q 함수를 학습함)
2. **Offline 환경에서 학습하고 싶다.** (→ 환경 상호작용 없이 전문가 데이터만으로 학습)

# IQ-Learn의 학습 과정
1. 입력 데이터:
- 전문가 trajectory
- 현재 정책의 trajectory
2. Q 함수 업데이트:
- 전문가 데이터는 `Q(s, a)` 값을 올리는 방향으로 학습
- 정책 데이터는 `Q(s, a)` 값을 내리는 방향으로 학습
- 이 두 가지를 결합한 loss로 Q함수 업데이트
3. 정책 업데이트 (옵션):
- Q 함수가 학습된 후, Q(s, a)를 최대화하려는 정책으로 업데이트 가능 (e.g. SAC, DQN 등 활용)
4. 타깃 네트워크 사용:
- 안정성을 위해 Q 함수 업데이트에게 타깃 Q 함수 사용 (TD 방식)

# IQ-Learn의 장점
- 보상 함수 불필요
- Off-policy 지원
- 안정성 및 효율성

# 샘플 코드
## [Code](https://github.com/soonawg/iqlearn-sample/blob/main/iqlearn_sample.py)