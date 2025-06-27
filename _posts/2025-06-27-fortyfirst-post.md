---
title: "Dataset Quality & Selection in Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-27 18:11:00 +0900
categories: study
---

# 왜 Offline RL에서 데이터 품질이 중요한가?
- **Offline RL**은 이미 수집된 데이터셋에만 의존해서 학습하므로, 잘못된/편향된 데이터가 있으면 학습된 정책이 잘못될 수 있음
- 온라인 RL과 달리 추가 수집이 어렵기 때문에, 한 번 주어진 데이터셋의 품질이 **정책 성능의 한계**를 거의 결정
- 대표적인 문제점:
    - 전문가 수준이 아닌 sub-optimal 데이터 → 성능 제한
    - 데이터 커버리지 부족 → OOD(Out-ofdistribution) state-action에 약함

# 데이터셋의 레벨 (Human vs Expert vs Sub-optimal)
- Expert 데이터
    - 전문가 정책으로 수집 → 최적 궤적 중심
    - 가장 이상적, 하지만 수집비용 높음
- Sub-optimal 데이터
    - 낮은 수준의 정책으로 수집
    - 현실적이지만 sub-optimal policy로 학습할 위험
- Human 데이터
    - 휴먼 플레이어/조작자가 수집
    - 다양하고 현실적이나, 편차 
    
# 데이터 품질의 영향
- 데이터의 품질과 커버리지가 좋으면 → 학습된 정책이 강건하고 일반화 잘됨
- 품질이 낮거나 편향되면 → distribution shift 문제, overfitting, OOD 문제

# Dataset Selection 전략
- **커버리지**:
    
    **가능한 한 다양한 state-action 쌍을 포함해야 함**
    
    (→ 너무 국소적인 궤적만 있으면 일반화 어려움)
    
- **quality**:
    
    전문가 수준의 행동 비율이 높을수록 좋음
    
    (→ sub-optimal 데이터라도 잘 정제하면 쓸 수 있음)
    
- **task 적합성**:
    
    목표하는 downstream task와 유사한 도메인에서 수집해야 함
    
- **데이터 필터링**:
    
    잘못된 보상, 센서 오류, outlier 등은 전처리 단계에서 제거