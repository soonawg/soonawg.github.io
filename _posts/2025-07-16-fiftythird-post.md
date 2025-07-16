---
title: "YOLO 커스텀 데이터 학습 실습 진행: vision - 여름방학 공부기"
layout: post
date: 2025-07-16 13:28:00 +0900
categories: study
---

커스텀 데이터 학습은 다음과 같은 순서로 진행된다.


# 1. 데이터 준비
- 이미지 준비: 내가 탐지하고 싶은 대상이 포함된 사진들 보으기
- 라벨링: 이미지 내 객체 위치(박스)와 클래스 이름 표시
    - 라벨링 도구: labelImg, **Roboflow** 등 (내가 사용한 것은 Roboflow이다.)

# 2. 데이터셋 구성
- 라벨 파일은 YOLO 형식으로 변환되어 있어야 한다. (`.txt` 파일)
    - 각 라벨을 `[class_id, x_center y_center width height]` (모두 이미지) 
- 폴더 구조 예시
```yaml
/dataset
    /images
        /train
        /val
    /labels
        /train
        /val
```

# 3. YAML 설정 파일 작성
YOLO 학습 시 사용할 데이터셋 메타정보 설정
```python
train: /path/to/dataset/images/train
val: /path/to/dataset/images/val

nc: 2  # 클래스 개수
names: ['class1', 'class2']  # 클래스 이름 리스트
```

# 4. YOLO 모델 선택
- 사전 학습된 가중치 중 하나 선택 (e.g. `yolov8n.pt`, `yolo8s.pt` 등)

# 5. 학습 실행
```python
from ultralytics import YOLO

model = YOLO('yolov8n.pt')  # 사전학습된 모델 불러오기

model.train(data='/path/to/dataset.yaml', epochs=50, imgsz=640)
```

# 6. 결과 확인 및 평가