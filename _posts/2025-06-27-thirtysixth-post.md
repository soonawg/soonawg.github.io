---
title: "D4RL 공부 - Offline RL: 여름방학 공부기"
layout: post
date: 2025-06-27 13:22:00 +0900
categories: study
---

# D4RL 개요
D4RL은 Offline RL 연구를 위해 널리 사용되는 표준 데이터셋 및 벤치마크이다.
## 주요 목적:
- Offline RL 알고리즘들이 서로 공정하게 비교될 수 잇도록 표준화된 환경과 데이터셋을 제공
- Offline RL 연구자들이 데이터 수집부터 학습까지의 부담 없이 학습 알고리즘 성능에 집중할 수 있도록 지원
## 핵심 아이디어:
- 고정된 데이터셋 (replay buffer) 만으로 에이전트를 학습
- 환경은 MuJoCo, Adroit, AntMaze 등 시뮬레이터 기반이며, 각 데이터셋은 전문가/초보/랜덤 등 다양한 품질의 데이터 포함

# 데이터셋 품질
D4RL은 다양한 행동 품질의 데이터를 제공한다.

각 데이터셋 이름에 데이터 품질이 암시됨:

- **random**: 무작위 행동 데이터
- **medium**: 중간 수준 정책 (suboptimal)
- **expert**: 전문가 수준 정책
- **mixed**: medium + expert 혼합
- **cloned**: imitation-style dataset
- **diverse**: 다양한 행동 데이터 (특히 AntMaze)

다양성 덕분에:
- 알고리즘의 일반화 성능을 평가 가능
- 학습 난이도가 다양한 데이터셋을 통해 robust한 Offline RL 알고리즘 평가 가능

# D4RL SCORE
D4RL은 성능을 비교할 때 normalized score를 사용한다.
Normalied Score = 100 X {(RL agent score - random score) / (expert score - random  score)}

이 값은:

- 0 = random agent 수준
- 100 = expert 수준
- 100 초과: expert를 넘어선 경우

**장점**:

- 서로 다른 환경 간 점수를 비교할 수 있음
- 논문 간 결과를 동일한 스케일에서 비교 가능