---
title: "객체 탐지 성능 향상: vision"
layout: post
date: 2025-07-16 15:16:00 +0900
categories: study
---

# 1. 데이터셋 향상
- Data Augmentation
    - 이미지 회전, 밝기 변경, 좌우 반전 등으로 데이터 다양성 증가
- Domain Randomization
    - 합성 데이터 생성 시, 배경/조명/크기 랜덤화
    - 실제 환경과 다른 다양한 조건에서 학습 -> 일반화 성능 향상
- 데이터셋 불균형 조정
    - 클래스마다 데이터 수가 너무 다르면 -> 학습 불안정
    - under/over sampling 또는 class weighting 필요

# 2. 라벨링 정확도 향상
- 정확한 bounding box 위치
    - 박스가 너무 크거나 작거나 어긋나면 성능 저하
    - 중심 좌표와 크기를 최대한 객체에 맞춰야 함
- 클래스 라벨 일관성
    - 같은 객체를 다른 이름으로 라벨링하면 혼란 발생
    - 클래스 정리를 미리 잘 해두는 것이 중요

# 3. 모델 아키텍처 선택
- YOLOv8n 선택 <-> YOLOv8x 선택
    - 성능을 높이려면 YOLOv8s -> YOLOv8m/l/x로 변경
    - 대신 속도와 메모리 사용량 증가를 감안해야 함

# 4. 하이퍼파라미터 튜닝 (Train-level)
- epoch 수 늘리기
    - 기본은 50~100, 더 정밀한 학습 위해서는 200 이상도 시도 가능
- imgsz 조정
    - `imgsz=640`이 기본이지만, `imgsz=800` 이상도 가능
    - 더 큰 이미지로 학습하면 작은 객체 탐지에 유리

# 5. 후처리 기술
## Confidence Threshold (`conf`)
Confidence Score란 YOLO가 객체라고 판단한 확신 정도이다. 0~1 사이의 값인데,
Confidence Threshold를 예를 들어, 0.5라고 설정을 하면 이 값보다 낮은 예측은 무시(drop)되게 된다.

## IoU Threshold (`iou`) - NMS에 사용됨
### IoU란?
- 두 박스가 얼마나 겹치는지를 0~1 사이 값으로 표현

### NMS란?
- 같은 객체에 대해 겹치는 여러 박스가 나올 수 있음
- 이 중 신뢰도가 가장 높은 것 하나만 남기고 나머지는 제거
