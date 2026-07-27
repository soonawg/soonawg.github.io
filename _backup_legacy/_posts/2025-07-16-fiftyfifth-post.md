---
title: "거리 및 자세 인식 심화: vision"
layout: post
date: 2025-07-16 15:33:00 +0900
categories: study
---

거리 및 자세 인식 심화라는 주제로 Vision 관련 내용을 공부한 것을 정리한 글이다.

# 카메라 캘리브레이션
실제 세계를 촬영한 이미지에는 왜곡이 있다. 카메라 캘리브레이션은 카메라 내부/외부 파라미터를 추정해서 "이미지 픽셀 <-> 실제 거리"를 정확하게 변환할 수 있도록 도와주는 과정이다.

## 왜 해야할까?
- 거리 측정 정확도 확보
    - YOLO로 객체를 인식해도, 실제 거리 계산은 카메라 보정 없이 왜곡될 수 있다.
- 자세 추정에 필요
    - 로봇이 물체를 집거나 추적할 때 정확한 위치/방향을 계산하려면 보정이 필수적이다.
- 왜곡 제거
    - 저가형 웹캠, 스마트폰 카메라는 일반적으로 렌즈 왜곡이 심하다


# Depth 계산 및 3D 좌표 추정
## 1. Depth란 무엇인가?
Depth는 카메라로부터 어떤 물체까지의 거리를 말한다.
일반적인 RGB 이미지에는 포함되진 않지만, Depth 카메라나 스테레오 방식 등을 쓰면 얻을 수 있다

## 핀홀 카메라 모델
실세계의 3D 좌표가 2D 이미지 평면에 투영되는 원리를 설명하는 수학적 모델이다.
### 투영 관계:
![projection_pinhall](/assets/images/2025-07-16/pinhall_camera.png)
- (X, Y, Z): 3D 월드 좌표
- (u, v): 2D 이미지 픽셀 좌표
- K: 카메라 내부 행렬 (intrinsic matrix)

### K (camera intrinsic matrix)
![projection_pinhall_K](/assets/images/2025-07-16/pinhall_camera_1.png)
- f_x, f_y: 초점 거리 (픽셀 단위)
- c_x, c_y: 중심점


## 픽셀 -> 3D 좌표 변환 공식
Depth와 Intrinsic 파라미터만 있으면 다음 공식으로 3D 좌표를 추정할 수 있음:
![pixel_to_3D](/assets/images/2025-07-16/pixel_to_3D.png)
- (u, v): 이미지 픽셀 좌표
- Z: 해당 픽셀의 depth 값
- (X, Y, Z): 카메라 기준 3D 좌표