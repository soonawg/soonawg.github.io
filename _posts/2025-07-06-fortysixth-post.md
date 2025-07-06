---
title: "ViT, CLIP, BLIP, Flamingo 실습 - 멀티모달: 여름방학 공부기"
layout: post
date: 2025-07-06 14:44:00 +0900
categories: study
---

# 실습을 해보자
ViT, CLIP, BLIP, Flamingo 이론을 공부하고 간단히 실습을 진행해야지 좀 더 이해가 쉬울거같아 실습을 진행하고자 한다.

# ViT 실습
ViT 실습 코드는 다음 링크를 통해 확인
[Code](https://github.com/soonawg/multimodal_practice/blob/main/vit_sample.py)

ViT 실습을 통해 공부한 것을 간단히 정리하자면 다음과 같다.

## 1. ViT 아키텍처 탐색
- `vit_tiny_path16_224` 모델이라는 경량화 된 모델을 사용했는데, 
- Patch Size가 (16, 16) -> 즉, 이미지를 16x16 블록으로 나눠서 토큰처럼 처리한다
- Embedding Dimension : 192
- Head 수: 3개
- Block Depth: 12개의 Transformer block
### 주요 컴포넌트
- 패치 임베딩: Conv2d 기반으로 16x16 단위로 나눔
- 위치 : (1, 197, 192) -> 197 = 196 patches + 1 [CLS] 토큰
- Classification head: 1000 클래스용 Linear

## 2. 패치 임베딩 시각화
- 224x224 이미지를 16x16 패치로 쪼갬
    - 총 196개 패치
    - 14 x 14 배열 형태
- 시각화를 해서 패치 그리드 확인 가능했고, 일부 패치를 따로 출력하여 토큰 단위 입력의 개념을 직관적으로 볼 수 있었음
-> ViT가 "토큰" 단위로 영상을 처리한다는 개념을 구체적으로 확인한 단계

## 3. 어텐션 맵 시각화
![ViT_attention_map](/assets/images/2025-07-06/ViT_attention_map.png)

## 4. 빠른 학습 데모 및 모델 비교 및 패치 크기 실험 (CIFAR10 소규모)
- CIFAR10의 1000개 subset -> 2 epoch 학습
- Accuracy는 10%대로 매우 낮았음
    - 성능이 낮게 나온 이유는 CIFAR10에 특화된 데이터 증강을 사용하지 않았고
    - ViT는 소량 학습 데이터에 약한 성질

### 모델 비교
- 파라미터 수
    - ViT-tiny: 5.7M
    - ViT-small: 22M
    - ResNet-18: 11M
- 추론 속도 (100회 평균):
    - ViT-tiny: 4.24ms
    - ViT-small: 3.98ms
    - ResNet-18: 1.85ms

### 패치 크기 실험
- 8x8 패치 -> 784개
    - 예상 메모리: 588KB
- 16x16 패치 -> 196개
    - 예상 메모리: 147KB
- 32x32 패치 -> 49개
    - 예상 메모리: 36KB

즉, 패치가 작아질 수록
- 패치 크기 증가, 시퀀스 길이 증가, 메모리와 연산량 증가
    - trade-off 관계를 보여준다.


# CLIP 실습
CLIP 실습 코드는 다음 링크를 통해 확인
[Code](https://github.com/soonawg/multimodal_practice/blob/main/clip_sample.py)

## 1. CLIP 아키텍처 탐색
- 모델: ViT-B/32 (Vision Transformer + Transformer)
- 임베딩 차원: 512
- 어휘 크기: 49,408
- 파라미터 수
    - 이미지 인코더 약 88M
    - 텍스트 인코더 약 38M
    - 전체 약 151M
- 출력 크기
    - 이미지 특징: `[1, 512]`
    - 텍스트 특징: `[1, 512]`

## 2. Zero-Shot 분류 실험
- CIFAR-10 데이터셋을 대상으로
- 다양한 프롬프트 템플릿으로 평가:
    - "a photo of a {}" -> 78%
    - "a picture of a {}" -> 77.5%
    - "an image of a {}" -> 76.7%
-> 즉, 템플릿 문구에 따라 Zero-shot 성능이 1~2% 정도 차이가 난다는 걸 알 수 있었음

## 3. 창의적 Zero-shot 실험
- CIFAR-10 샘플 이미지에
    - "majestic lion", "cyberpunk style", "happy emotion" 같은 비정형 프롬프트를 줬더니
    - 대부분 0.0 확률 -> 매칭이 거의 되지 않음
    - 일부는 특정 프롬프트(예: soaring eagle, baroque art)로 1.0 출력
-> CLIP이 학습 시 보지 못한 비주얼-텍스트 조합에는 취약하다는 것을 보여줬고, 특히 CIFAR-10처럼 제한된 데이터셋 이미지를 대상으로 "예술적 스타일"이나 "감정적 표현" 같은 프롬프트는 거의 매칭되지 않는다

## 4. 텍스트-이미지 검색
- 100개 CIFAR-10 이미지로 데이터베이스를 만들어
- 쿼리:
    - "a flying animal"
    - "a vehicle with wheels"
    - "something that lives in water"
    - 등
- 상위 유사도:
    - 대체로 0.23 ~ 0.27 수준

## 5. 멀티모달 유사도 분석
- 8개의 텍스트 설명간 코사인 유사도 시각화
- 가상 유사한 문장쌍:
    - "a bird flying in the sky" <-> "a fish swimming in the ocean" -> 0.835
    - "a dog playing in the park" <-> "a horse running in a filed" -> 0.773
- 전반적으로 0.6~0.8 사이의 유사도가 많이 나옴
- 텍스트 표현끼리도 CLIP 멀티모달 공간에서의 상당히 의미적으로 가까운 표현을 포착한다는 보여줌

## 6. 다국어 및 창의적 프롬프트 싫험
- “고양이 사진” (한글) → 26.4
- “a photo of a cat” (영어) → 26.8
- “une photo d'un chat” (프랑스어) → 28.2
👉 한글, 불어, 독일어, 스페인어 등 **다국어 표현도 꽤 높은 스코어(20~28)**
→ CLIP이 다국어 zero-shot에 대해 어느 정도 강건성을 가진다는 점을 잘 보여줬어.
- “in the style of Van Gogh”, “as a cyberpunk artwork” 같은 창의적 스타일 프롬프트:
    - 18 ~ 24 범위의 유사도
    - 즉 **스타일 프롬프트에도 대응 가능**하지만 정확한 매칭은 영어식 직설적 프롬프트보다 약간 떨어진다는 특징.

## 7. Similarity matrix between text descriptions
![text_matrix](/assets/images/2025-07-06/text_matrix.png)


# BLIP 실습
CLIP 실습 코드는 다음 링크를 통해 확인
[Code](https://github.com/soonawg/multimodal_practice/blob/main/blip_sample.py)

## 1. BLIP 아키텍처 이해
- 비전-텍스트 융합 구조: Vision Transformer + 텍스트 디코더의 이중 구조
- 파라미터 규모: 2.47억 개의 파라ㅣ미터
- 입력 처리: 384x384 해상도 이미지와 최대 50 토큰 텍스트 동시 처리 가능

## 2. 이미지 캡셔닝 특성
- 객체 인식 정확도:
    - 명확한 객체(개, 배, 비행기)에서도 높은 정확도
    - 유사 클래스 (고양이 vs 개) 혼동 발생 (CIFAR-10의 저해상도 한계)
- 생성 다양상:
    - 온도 파라미터 조정으로 다양한 표현 생성 가능
    - 동일 이미지에 대해 "a boat floating" vs "a boat sailing" 같은 뉘앙스 차이 생성
- 프롬프트 영향력:
    - "a photo of" -> 간결한 설명
    - "this picture contains" -> 더 상세한 설명

## 3. VQA 능력 분석
- 질문 유형별 성능(정확도):
    - 기본 사실 질문: 65
    - 공간적 추론: 40
    - 감정/추상적 질문: 20
- 주목할 만한 특성
    - 객체 수 파악 능력 우수 (”How many objects?” → 정확한 숫자 응답)
    - 색상 인식 높은 정확도 (배경색, 객체색 구분)
    - 추상적 질문에는 약함 (”What emotions?” → 단순 응답)

## 4. 멀티모달 상호작용
- 교차 모델 이해력:
    - 이미지-텍스트 정렬 능력 확인 (e.g. “배” 이미지에 “harbor” 응답)
    - 클래스별 설명 차이: “비행기” → 기술적 설명 vs “개구리” → 생태학적 설명
- 한계점:
    - 저해상도 이미지에서 세부 특징 인식 어려움
    - 문화적/상황적 맥락 이해 부족

## 5. 생성 품질 평가
- BLUE/ROUGE 지표:
    - ROUGE-L(0.25) > ROUGE-2(0.05): 의미적 유사성 > 어휘적 정확성
    - 평균 BLUE 0.035: 참조 캡션과의 정확한 매칭 부족 (CIFAR-10의 한계)
- 생성 특성:
    - 평균 9.95단어의 간결한 설명
    - “a”, “the” 등 관사 과잉 사용 경향

## 6. 프롬프트 실험 교훈
- **효과적인 전략**:
    - 직관적 프롬프트("What color?") > 추상적 프롬프트("Describe poetically")
    - 구체적인 맥락 제공("In a museum...")이 생성 품질 향상
- **주목할 실패 사례**:
    - 스타일 프롬프트에서 토큰 반복 현상
    - 관점 프롬프트("From artist's view")의 응답 단순화

![finial](/assets/images/2025-07-06/final.png)

# Flamingo 실습
Flamingo 실습 코드는 다음 링크를 통해 확인
[Code](https://github.com/soonawg/multimodal_practice/blob/main/Flamingo_sample.py)

## 1. Gated Cross-Attention 매커니즘
1. 게이트 제어 학습 패턴
    - 모든 레이어에서 게이트 값이 0으로 초기화된 상태에서 분석
    - 실험 결과 평균 게이트 값: 0.0 (레이어 1~4 모두)
    - 이는 모델이 초기 학습 단계에서 이미지 특징을 점진적으로 통합하기 시작함을 의미
2. 정보 흐름 계층화
    - 레이어가 깊어질수록 활성화 값의 norm이 감소하는 패턴 (평균 0.5 -> 0.3)
    - 초기 레이어: 저수준 특징 추출
    - 후기 레이어: 고수준 추상화 및 융합 수행

## 2. Few-shot Learning 특성
1. 샷 수에 따른 성능 변화
```
# Few-shot vs Zero-shot 비교 결과
shot_results = {
    0: 0.15,  # Zero-shot
    1: 0.05,   # 1-shot
    2: 0.10,   # 2-shot
    3: 0.05,   # 3-shot
    5: 0.10    # 5-shot
}
```
- 샷 증가-> 성능 향상 패턴 확인 (단순화된 실험 환경)
2. 태스크별 최적 샷 수
    - 분류 태스크: 1샷에서 100% 성공률 (과적합 가능성)
    - 개수 세기/설명 태스크: 샷 증가에도 낮은 성공률 -> 언어 생성 태스크의 어려움 반영

## 3. Attention 매커니즘의 핵심 특성
1. 계층별 어텐션 분포
    - 레이어 1: 높은 엔트로피(8.66) -> 다양한 특징 포착
    - 레이어 4: 낮은 엔트로피(7.84) -> 선택적 집중
```python
# 레이어별 어텐션 통계
layer_stats = [
    {'mean': 1.0278, 'entropy': -8.66},  # Layer1
    {'mean': 1.0417, 'entropy': -8.78},  # Layer2
    {'mean': 0.9722, 'entropy': -8.19},  # Layer3
    {'mean': 0.9306, 'entropy': -7.84}   # Layer4
]
```
2. 헤드별 전문성 분화
    - 헤드 3,4,6: 높은 집중도(100M) -> 특정 특징에 집중
    - 헤드 1,2,3,7,8: 중간 집중도(4.56~8.10) -> 일반적 특징 처리
    - "분업화 어텐션" 매커니즘 확인: 각 헤드가 서로 다른 특징 처리 전략 학습

## 4. 모델 크기별 성능 특성
1. 규모 확장 효과
```python
benchmark = {
    'Small': {'params': 2.5M, 'throughput': 3010},
    'Medium': {'params': 5.5M, 'throughput': 2024},
    'Large': {'params': 14.9M, 'throughput': 1520}
}
```
- 모델 크기 2배 증가 -> 처리량 33% 감소 (트레이드 오프 존재)
- 대규모 모델일수록 복잡한 패턴 학습 가능 but 리소스 요구량 증가
2. 메모리 효율성
    - Large 모델 (14.9M params)도 0.05MB 미만 메모리 사용 -> 최적화된 아키텍처
    - 이미지 특정 압축 (7x7 grid)이 메모리 효율성의 핵심 요소

