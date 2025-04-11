---
title: "Firebase Studio를 사용해보자."
layout: post
date: 2025-04-11 21:45:00 +0900
categories: study
---

# Firebase Studio를 사용해보자.

Firebase studio란, 이번 주에 구글에서 발표한 따끈따끈한 
**AI 기반 어플리케이션 개발 프레임워크**이다.

간단히 설명하면, 기존에 ChatGPT나 Claude처럼 프롬프트로 
"길찾기 앱을 만들어줘" 하면, **얘가 알아서 프로토타입을 만들어준다는 것**이다. 
(물론 프롬프트는 이거보다 자세한게 좋을 수도 있다.)

나의 관심사와 밀접한 관계가 있는 것은 아니지만, 요즘 AI 도구가 많이 나오고 
한번씩 사용해보는 재미가 있기에 이거 또한 사용해보았다.

나는 간단히 To-do 앱을 만들어달라고 부탁하였다. 프롬프트는 다음과 같이 하였다:

> "Make a simple to-do list web app. Users can add, delete, and mark tasks as complete."

이렇게 부탁을 하면, Firebase Studio가 BluePrint를 만들어서 보여준다.

![Firebase_blueprint](/assets/images/2025-04-11/firebase_todo_blueprint.PNG)

그 후, **Prototype this App** 버튼을 누르면, 알아서 코드들을 작성하기 시작한다.  
그리고 결과물은 다음과 같다.

![Firebase_prototype](/assets/images/2025-04-11/firebase_todo_prototype1.PNG)

UI도 준수하고, 직관적이여서 개인적으로 사용할거면 나쁘지 않았다.  
해야할 일을 추가한 모습은 다음과 같다.

![Firebase_prototype1](/assets/images/2025-04-11/firebase_todo_prototype2.PNG)

그리고, 실제로 Firebase가 작성한 코드도 볼 수 있고 수정을 할 수 있는 기능 또한 존재한다.

## 다만, 내가 느낀 단점을 얘기해보자면:
1. 아직 너무 어려운 앱을 지시하면 해결하지 못하는 것 같다.  
   휴머노이드 로봇 행동 학습 시각화 대시보드라는 주제로 관련 내용과 함께 지시해보았는데 
   오류도 나고, 결과물도 영 좋지 않았다.

2. 이런 프레임워크는 비개발자를 위한 것일텐데, 비개발자가 해서 만들어도 퀄리티가 구리다.  
   비개발자는 코드를 실제로는 작성하지 못하고, 프롬프트를 통해서만 개발을 할것이다. 하지만, 
   프롬프트로 지시를 해서만 나오는 결과물이 영 좋지않아서 의미가 있나 싶다.

하지만, 프로토타입이라는 것에 의미가 있는 것이기에 앱을 만드는 분들에게는 
개발 속도를 빠르게 올려주는데 앞으로 이바지 할 것 같다.