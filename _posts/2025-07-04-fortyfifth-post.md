---
title: "CLIP, BLIP, Flamingo - 멀티모달: 여름방학 공부기"
layout: post
date: 2025-07-04 14:13:00 +0900
categories: study
---

# CLIP, BLIP, Flamingo
지난 시간 공부했던 ViT에 이어 그 기반 아이디어를 둔 CLIP, BLIP, Flamingo에 대해 공부를 하였고, 그를 간단히 정리하고자 글을 작성한다.

# CLIP
## CLIP이란?
- CLIP은 "이미지와 자연어 설명을 함께 처리"하는 Vision-Language 모델의 대표적인 형태이다.
- 핵심 아이디어는 대량의 웹 이미지-텍스트 쌍을 contrastive learning으로 학습하여,
    - 텍스트와 이미지의 의미를 공통 임베딩 공간에 매핑
    - zero-shot 인식이 가능하게 한다는 점이다.

## CLIP의 학습 목표
- CLIP은 이미지와 텍스트의 쌍이 올바르게 매칭되는지를 구별하는 학습을 수행한다.
- 수백만 개의 이미지와 그에 대한 캡션(문장)을 함께 사용해
    
    **“이미지와 설명이 일치하면 임베딩을 가깝게, 일치하지 않으면 멀어지게”** contrastive loss를 통해 학습한다.
    
즉,
- 이미지 인코더로부터 나온 벡터
- 텍스트 인코더로부터 나온 벡터를 **공통의 임베딩 공간**에 위치시키는 방식이다.

## CLIP의 아키텍처
CLIP은 두 개의 인코더를 사용한다.
✅ **이미지 인코더**
- ResNet 또는 ViT 기반 아키텍처
- 입력 이미지를 patch embedding → Transformer로 인코딩 → feature vector 추출

✅ **텍스트 인코더**
- Transformer 기반 언어 모델
- 입력 텍스트(문장)를 토큰화 → positional embedding → Transformer 인코딩 → feature vector 추출
→ 최종적으로 이미지와 텍스트 인코더의 출력을 **512차원** 정도의 벡터 공간으로 투영한다.

## CLIP의 장점
- 인터넷에서 쉽게 수집한 대규모 이미지-텍스트 쌍으로 학습
- **라벨링 데이터 없이**도 zero-shot 분류가 가능
- 새로운 class가 등장해도 텍스트 프롬프트만 바꾸면 인식 가능
- 이미지-텍스트 매칭, retrieval, open-vocabulary detection 등에 활용


## Zero-shot이란?
여기서 공부를 하는데 Zero-shot이라는 용어가 익숙치 않아 공부를 해보았다.

Zero-shot이란 **모델이 학습하지 않은(또는 훈련 데이터에 포함하지 않음) 새로운 태스크나 새로운 클래스를, 추가적인 파인튜닝 없이 바로 수행하는 능력**을 말한다고 한다.

# BLIP
## 배경과 목표
- 기존의 CLIP은 이미지-텍스트 쌍을 사용해 언어-이미지 간의 매칭을 학습했다.
- 하지만 CLIP은 텍스트가 **자연스러운 설명**이 아니라 짧은 라벨 수준인 경우가 많아서 시각적 장면을 더 깊이 이해하거나 풍부하게 묘사하는 데 한계가 있다.
- BLIP은 그에 더 나아가서
    - 이미지 캡셔닝
    - 비주얼 질문 응답 (VQA)
    - 자연스러운 텍스트 생성
    을 통합적으로 학습하여, 보다 풍부하고 정교한 비전-언어 표현을 배우도록 설계되었다.

## BLIP의 핵심 아이디어
- Bootstrapping:
    인터넷에서 수집한 **라벨이 부정확하거나 noisy한 이미지-텍스트쌍**을
    스스로 더 정제하고 개선해서, 더 나은 학습데이터로 만들어버리는 자기학습 전략을 사용
- Unified Vision-Language Pretraiing:
    하나의 거대한 모델이
    - 이미지->텍스트 생성 (Captioning)
    - 텍스트->이미지 매칭 (Retrieval)
    - 비주얼 질문 응답 (VQA)
- 미세 조정 (Fine-tuning)
    Pre-training 후에는 각 태스크(예: VQA, 이미지 검색)에 맞게 미세조정

## BLIP 아키텍처
BLIP의 전체 구조는 다음과 같다:
- Image Encoder: ViT 계열의 Transformer 기반 이미지 인코더
- Q-Former: Query Transformer로, 이미지 특징을 질의(query)처럼 뽑아서 언어모델로 전달하기 전에 중간 표현을 조정
- Text Decoder: GPT나 BERT 계열의 구조로 텍스트를 생성하거나 매칭 점수를 계산

## BLIP의 특징
- 기존 CLIP보다 풍부한 설명과 문장 생성 능력
- noisy한 웹데이터를 스스로 정제(bootstrap)
- Q-Former를 통한 정보 필터링
- 여러 태스크를 한꺼번에 학습

## VQA란 무엇인가?
여기서 공부를 하는데 VQA라는 용어가 익숙치 않아 공부를 해보았다.

VQA란 이미지를 보고 사람이 질문하면 AI가 답변하는 문제를 뜻한다.


# Flamingo
## 개념
- Flamingo는 비전-언어 멀티모달 모델로, few-shot VQA, 이미지 캡셔닝, 비주얼 다이얼로그 등 다양한 태스크를 단일 아키텍처로 처리할 수 있도록 설계된 모델이다.
- 특히 few-shot 학습을 목표로 하여, 작은 샘플로도 새로운 태스크에 빠르게 적응할 수 있도록 한다

## 구조
Flamingo의 구조는 다음과 같다.
- Vision Encoder
    - CLIP-ViT를 기반으로 한 강력한 Vision Transformer 사용
    - 이미지 -> 시각 피처 벡터 추출
- Perceiver Resampler
    - 이미지 피처 벡터를 압축하고 가공하는 모듈
    - 이미지 피처를 시퀀스 형태로 요약하여 텍스트 스트림에 주입하기 좋게 만듦
- LLM
    - GPT 같은 대형 언어 모델
    - 텍스트 스트림과 resampled 비전 스트림을 이어받아 멀티모달 토큰 시퀀스로 처리
- Cross-attention layers
    - LLM의 일부 레이어를 cross-attention으로 확장
    - 텍스트 토큰과 이미지 피처를 자유롭게 상호 참조할 수 있게 함
즉 **이미지 -> ViT -> Perceiver Resampler -> LLM -> cross-attention -> 답변/생성**
의 파이프라인으로 이루어진다

## 특징
✅ 텍스트·이미지 시퀀스를 **하나의 토큰 시퀀스**로 처리
✅ Perceiver를 사용해 **효율적으로 이미지 피처를 요약**
✅ LLM의 일부 레이어에 cross-attention을 넣어 multimodal reasoning
✅ 사전학습 LLM을 거의 그대로 사용하기 때문에 기존 언어 능력을 해치지 않으면서 시각 정보만 덧붙여서 활용 가능
✅ 몇 장의 예제만으로도 새로운 태스크를 빠르게 적응 (few-shot)

## 장점과 한계
### 장점
- 적은 샘플로도 빠르게 대응 가능한 few-shot 능력
- 기존 LLM을 거의 수정 없이 재활용
- Perceiver 기반의 효율적 피처 요약
### 한계
- Perceiver + LLM 결합 구조라 파라미터 수가 매우 커짐
- 실제 응용에는 대규모 컴퓨팅 자원 필요

## Cross-attention이란?
여기서 공부를 하는데 Cross-attention이라는 용어가 익숙치 않아 공부를 해보았다.
- Self-attention이란 자기 자신(같은 시퀀스) 안에서 토큰끼리 상호작용(의존성)을 학습하는 매커니즘이다. 예를 들어, 문장 안의 단어들끼리 문맥을 고려할 때 사용한다.
- Cross-attention은 서로 다른 두 시퀀스 사이의 상호작용을 학습하는 매커니즘이다.
    - 하나는 query (주료 텍스트)
    - 다른 하나는 key/value (예: 이미지 피처)
    이렇게 서로 다른 modality의 정보를 연결할 때 쓰인다.

## Few=shot이란?
여기서 공부를 하는데 Few-shot이라는 용어가 익숙치 않아 공부를 해보았다.
BLIP에서 사용하는 zero-shot과 마찬가지로 **새로운 태스크를 다루지만,** 이번에는 **몇 개의 예시 (few-shot examples)**를 프롬프트에 함께 넣어줘서, 모델이 그 예시를 참조하며 문제를 풀도록 하는 방식이다.
→ 파인튜닝은 여전히 하지 않고, 단지 프롬프트  안에 “문제-정답 쌍”을 몇 개 보여주는 것