---
title: "RT-2"
layout: post
date: 2025-07-06 23:58:00 +0900
categories: study
---

# RT-1 요약
![rt-1](/assets/images/2025-07-06/rt-1.png)
- RT-1 목표:
    - 전통적인 task-specific 방식의 한계를 극복하고 하나의 범용 모델로 다양한 작업과 환경에서 잘 작동하는 로봇 제어 시스템을 목표로 한다.
- 입력:
    - 로봇 카메라 이미지 (300x300x3 픽셀)와 자연어 명령을 받는다
- 처리 과정:
    - 이미지는 사전 학습된 EfficientNet-B3 CNN을 통해 처리되며, 9x9x512 크기의 피처 맵으로 처리된다
    - 자연어 명령은 Universial Sentence Encoder를 거쳐 512차원 벡터로 변환된다.
    - FILM 레이어를 통해 이미지 채널에 감마와 베타 값을 곱하고 더하여 중요한 특징을 강조한다.
    - 9x9 피처 맵을 81개의 비전 토큰으로 평탄화하여 트랜스포머에 입력한다.
- 출력: 트랜스포머는 로봇의 행동을 위한 11개의 정수 값을 순차적으로 출력하며, 이는 디코딩 과정을 통해 실제 제어 신호로 변환된다.


# RT-2 요약
- 목표: RT-1을 확장하여 로봇 시연 데이터뿐만 아니라 웹에서 학습한 지식까지 로봇 제어에 활용할 수 있도록 한 모델이다,
- 핵심: RT-2는 Vision-Language-Action(VLA) 모델로, 기존 이미지와 언어만 처리하던 Vision-Language Model을 로봇 액션까지 확장해서 웹 지식을 활용해 복잡한 명령을 수행하게 한다.
- 처리 과정:
    - 자연어 명령:
        - 예를 들어 사용자가 "pick up the red cup"라는 문장을 입력하면
        - 이 문장을 먼저 Tokenizer에 의해 단어 단위로 나뉘고, 각 단어는 고유한 정수 ID로 변환된다
        ![rt-2-tokenizer](/assets/images/2025-07-06/rt-2-tokenizer.png)
        - 이 단어들이 Embedding layer를 통과하면서 각 단어는 고정된 차원의 벡터로 변환된다
        ![rt-2-embedding-layer](/assets/images/2025-07-06/rt-2-embedding-layer.png)
        - 이 벡터들은 이후 Transformer의 Self-Attention 블록을 거치며 단어들 간의 의미 관계를 파악한다.
    - 이미지 입력
        - 이미지는 먼저 ViT 또는 PaLI-X 이미지 인코더로 들어간다.
        - 여기서 이미지는 패치 단위로 잘려 처리된다.
        - 각 패치 벡터들은 Position Embedding을 더해주고 Self-Attention 연산을 통해 이미지 내에서 어떤 위치가 중요한지를 판단하게 된다.
    - 자연어-이미지 결합
        - 텍스트 토큰과 이미지 토큰을 같은 시퀀스로 이어서 트랜스포머에 넣는 구조이다.
        - 이렇게 붙여진 시퀀스 구조는 Multi-Modal Transformer Encoder로 들어가고 여기서 Cross-Attention이 발생한다
        ![rt-2-cross-attention](/assets/images/2025-07-06/rt-2-cross-attention.png)
        - 이미지 토큰이 문장 중 어떤 단어와 연결되는지를 판단하고, 문장 토큰은 이미지 내에 어떤 영역이 중요한지를 판단한다.
    - 디코드 액션 출력
        - 앞서 설명된 인코더에서 설명된 통합 정보들은 디코더로 넘어오고, 트랜스포머 디코더는 마치 문장을 생성하듯 숫자 토큰 시퀀스를 생성한다.
        ![rt-2-token-sequence](/assets/images/2025-07-06/rt-2-token-sequence.png)
        - 이런 숫자 하나가 로봇의 제어 파라미터에 해당한다.
        ![rt-2-parameters](/assets/images/2025-07-06/rt-2-parameters.png)
        - 여기서 RT-2의 가장 혁신적인 부분은 로봇의 행동, 즉 액션을 VLM이 출력하는 텍스트 토큰의 일부로 통합하여 출력한다는 점이다.
            - RT-2는 기존 VLM이 가지고 있는 인터넷 지식을 유지하면서 시연 데이터도 같이 학습하는  Co-fine-tuning 방식을 사용한다.
                - 쉽게 얘기하여 웹에서 학습한 데이터로는 ‘지식’을 로봇 시연 데이터로는 ‘행동 감각’을 학습해 두 데이터를 섞어서 같이 학습시킨다는 의미이다.
                - 이런 학습 방식 덕분에 이전 로봇 데이터에 없던 명령어나 상황도 잘 이해하고 수행할 수 있다.


# RT-1 vs RT-2
![rt-comparison-1](/assets/images/2025-07-06/rt-comparison-1.png)
![rt-comparison-2](/assets/images/2025-07-06/rt-comparison-2.png)

여기서 RT-2가 RT-1보다 진화했다는 핵심 포인트는

1. 언어와 행동의 통합 관점에서 RT-2는 로봇의 행동을 자연어의 토큰처럼 다루며 텍스트와 액션을 동일한 시퀀스로 학습했다는 점
2. 사전학습된 VLM으로부터 이미 학습된 개념, 상식, 관계 이해 능력을 로봇 제어에 전이했다는 것이 중요한 점
3. 웹 지식을 잃지 않으면서 로봇 데이터를 추가 학습해 두 세계의 균형을 유지한 co-fine-tuning