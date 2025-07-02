---
title: "DeiT, Swin - ViT: 여름방학 공부기"
layout: post
date: 2025-07-02 15:25:00 +0900
categories: study
---

# DeiT와 Swin
ViT는 장점도 있지만, 여전히 많은 단점이 존재했고, 이를 해결하기 위해 생겨난 모델인 DeiT와 Swin이 있다.

# DeiT 개요
- ViT는 대량의 데이터가 없으면 성능이 크게 떨어지는 문제가 있었다.
- DeiT는 Vit의 데이터 효율성을 크게 개선한 모델이다.
- 소량의 라벨 데이터만 있어도 좋은 성능을 낼 수 있도록 학습법과 데이터 증강, 지식 증류(knowledge distillation) 방법을 새롭게 도입하였다.

# DeiT의 주요 특징 및 기법
## 데이터 효율적 학습
* ViT는 수백만 장 이상의 데이터(예: ImageNet-21k) 없이는 잘 학습이 안 되는데, DeiT는 ImageNet-1k(약 120만 장) 정도의 데이터로도 좋은 성능을 달성했다.
* 데이터 효율성을 위해 다양한 데이터 증강 및 정규화 기법이 활용된다.

## 지식 증류 (Knowledge Distillation)
- Teacher-student 프레임워크를 활용한다.
- CNN 기반 강력한 ResNet 모델을 teacher로 사용하여, DeiT(student)가 teacher의 출력을 모방하도록 학습한다.
- 이 과정에서 student 모델이 더 적은 데이터로도 효과적으로 학습한다.
- 즉, teacher가 만든 'soft label'이 student를 보조해 준다.

## 토큰 및 구조는 ViT와 유사
- 입력 이미지를 패치 단위로 분할하고, 각 패치를 Transformer 입력 토큰으로 변환한다.
- 기존 ViT와 동일하게 Transformer Encoder 블록을 쌓아 처리한다.
- 위치 인코딩도 똑같이 적용된다.

# DeiT 학습 방법과 손실 함수
- 일반적인 분류 손실(크로스 엔트로피) 외에, 지식 증류 손실(distillation loss)이 추가된다.
- 총 손실 함수는 다음 두 가지를 더한 것:
    1. 기본 분류 손실: 학생 모델의 출력과 실제 라벨 간 크로스 엔트로피
    2. 증류 손실: 학생 모델의 출력과 교사(ResNet) 모델의 출력 간 차이
- 이 두 손실을 적절히 가중합하여 최적합한다.

# DeiT의 아키텍처 및 구조
- 입력: 224×224 RGB 이미지 → 16×16 패치로 나눔 (총 196개 토큰)
- 패치 임베딩: 각 패치를 고차원 임베딩 벡터로 변환
- Transformer Encoder: 12개의 Transformer 블록(기본 DeiT-Base 모델 기준)
- Classification Token: 학습 가능한 특수 토큰 [CLS]를 입력 맨 앞에 추가하여 최종 분류용 정보로 활용
- 최종 출력: [CLS] 토큰의 최종 상태를 분류기(MLP)에 전달해 클래스 확률 

# DeiT의 성능 및 장점
- 동일한 데이터(예: ImageNet-1k)에서 ViT 대비 높은 정확도 달성
- ImageNet-1k만으로도 ImageNet-21k 사전학습한 ViT와 비슷한 수준 성능
- CNN 기반 모델과 경쟁 가능한 수준의 성능
- 데이터가 제한적일 때도 잘 작동
- 추가적인 컴퓨팅 리소스 없이도 학습 효율 개선

--------------------------

# Swin 개요
- Swin Transformer는 Vit의 한계(계산량, 로컬 정보 학습 부족)를 개선하기 위해 고안된 모델이다.
- 기본적으로 ViT 기반이지만, CNN처럼 로컬-투-글로벌 표현을 학습할 수 있도록 설계되었다.
- 핵심 아이디어는 윈도우 기반의 self-attention + 윈도우를 shift해서 정보가 교류되도록 하는 구조이다.

# Swin의 구조
## 🟦 **윈도우 기반 self-attention**
- ViT처럼 전체 패치에 global self-attention을 적용하면 계산량이 매우 커진다.
- Swin에서는 고정된 윈도우(예: 7x7 크기)의 패치 안에서만 self-attention을 수행한다.
    - → 이렇게 하면 local self-attention으로 메모리와 계산량을 줄인다.
- 대신 **윈도우를 매번 약간씩 이동(shift)** 시켜 다음 계층에서 다른 윈도우끼리 정보를 교환하게 한다.
    - → shift windowing으로 윈도우 간 정보 흐름을 보장한다.

## 🟦 **계층적 구조 (hierarchical)**
- CNN처럼 resolution을 점차 줄이는 단계적 구조
    - 예: Stage 1 → Stage 2 → Stage 3 → Stage 4
- 초기에는 작은 패치(4x4) 단위로 입력을 나누고, stage가 진행되면서 patch merging으로 해상도를 줄여 나가며 feature dimension을 키운다.
    - → 이로 인해 계층적 특징 추출(Hierarchical feature extraction)이 가능하다.

## 🟦 **Patch Merging**
- Swin에서는 pooling처럼 작동하는 patch merging 연산을 사용해 feature map의 해상도를 절반으로 줄인다.
- 이때 단순 pooling 대신 patch들을 concatenate 후 선형변환을 거쳐 표현력을 유지한다.

# Swin의 장점
- 윈도우 기반 attention으로 계산량 효율
- shift window로 인접 윈도우 간 정보 교류
- 계층적 구조 -> CNN과 유사한 multiscale feature 추출 가능
- 다양한 downstream task (분류, 검출, 분할 등)에 유연하게 적용 가능