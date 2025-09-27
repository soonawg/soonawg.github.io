---
title: "ViT"
layout: post
date: 2025-07-02 15:24:00 +0900
categories: study
---

# ViT
* ViT는 이미지를 Transformer 모델로 처리하는 방식이다.
* 기존 CNN의 local 필터와 달리 Transformer의 전역 self-attention 매커니즘을 이용해 이미지를 인식한다.
* 핵심 아이디어
    * 이미지를 패치로 나눔 -> 각 패치를 시퀀스로 보고 Transformer Encoder에 입력 -> 분류

# Vit의 전체 아키텍처
ViT는 구조상 Transformer Encoder만으로 사용하고, Decoder는 사용하지 않는다.
구성은 다음과 같음:
1. 패치 분할 (Patch Embedding)
    - 입력 이미지 H x W 크기에서 P x P 크기의 패치를 나눔
    - 예를 들어 224 x 224 이미지를 16 x 16 패치로 나누면 (224/16)^2 = 196개의 패치
2. 클래스 토큰 (CLS token)
    - BERT와 유사하게 [CLS] 토큰을 추가
    - 이 토큰은 Transformer 마지막에 이미지 분류에만 사용됨
3. 포지셔널 인코딩
    - 패치 토큰은 순서(위치)를 가지지 않으므로, 1D learnable position embedding을 더해 위치 정보를 주입
4. Transformer Encoder
    - L개의 Transformer block (Multi-head Self-Attention + MLP + Residual + LayerNorm)
    - 각 블록에서
        - Multi-head self-attention → 전역 정보 상호작용
        - MLP → 비선형성 학습
        - Residual → 안정적인 학습
        - LayerNorm → 정규화
    - 따라서 CNN보다 글로벌 피처 통합을 잘함
5. Classification Head
    - 최종적으로 [CLS] 토큰을 뽑아 MLP head에 연결하여 최종 분류 결과 출력

# ViT의 장단점
✅ **장점**

- CNN보다 훨씬 글로벌한 시야 (long-range dependency)
- 대규모 데이터(수억 장)에서 강력한 성능
- 구조가 간단 (Transformer block 재사용)

⚠️ **단점**

- 데이터가 부족하면 성능이 쉽게 떨어짐 (과적합)
- 로컬 피처 학습(CNN의 장점)을 충분히 대체하지 못할 수 있음
- 연산량 많음 → 대규모 GPU가 필요

# 다음은 샘플 코드이다.
**[Code](https://github.com/soonawg/vit_sample/blob/main/vit_sample.py)**