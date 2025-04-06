---
title: "Hugging Face Deep RL Course (2)"
layout: post
date: 2025-03-21 23:40:00 +0900
---

# 허깅페이스 Deep RL Course를 진행하며
이 글을 작성하는 시점에는 허깅페이스 Deep RL Course의 Unit 5까지 진행한 상태이다. 이 Course의 모든 실습은 Colab 노트북으로 진행되는데,
Colab의 가장 큰 장점은 무료로 GPU 자원을 쓸 수 있다는 것이고, 가장 큰 단점은 그 사용 시간이 정해져있다는 것이다.
그래서 나는 Course 페이지에 나와있는 실습을 다 진행하고, 나 혼자서 이를 이용해 뭔가를 따로 만들고 싶거나 아니면 오늘은 Unit을 하나더 공부하고 싶다고 해도 런타임 용량이 다 소모가 되었다면 진행을 못하는 것이다.
그래서 Colab Pro 결제 (달에 9.99 달러)를 할까 고민도 했지만, 대안을 생각해냈다.
현재 나는 WSL Ubuntu 환경에서 주로 개발을 진행하고 있는데, 여기에 CUDA, PyTorch 등 대부분의 개발 환경이 세팅되어있다.
그래서 여기서 `jupyter lab`을 키면 GPU 사용이 된다는 것을 알게되었고, 앞으로 혼자 실습을 진행할 때는 이걸 통해서 하면 될 것 같다.


# Progressing Through the Hugging Face Deep RL Course
At the time of writing, I have completed up to Unit 5 of the Hugging Face Deep RL Course. All the hands-on exercises in this course are conducted using Colab notebooks.

The biggest advantage of Colab is that it provides free access to GPU resources. However, its biggest downside is the limited runtime, meaning that once I exhaust my allocated usage, I can’t continue my experiments. This becomes a problem when I want to complete additional units or build something on my own using what I've learned.

I considered subscribing to Colab Pro ($9.99 per month), but instead, I found an alternative solution.

I primarily develop in a WSL Ubuntu environment, where I have already set up CUDA, PyTorch, and other necessary tools. I discovered that by launching jupyter lab in this environment, I can utilize my GPU.

From now on, I plan to use this setup for my own reinforcement learning experiments instead of relying on Colab.