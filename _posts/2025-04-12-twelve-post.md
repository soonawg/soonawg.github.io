---
title: "MCP를 사용하자 (feat. WSL 에러 해결)"
layout: post
date: 2025-04-12 23:30:00 +0900
categories: study
---

# MCP를 사용해보자
MCP는 요즘 AI 업계에서 굉장히 핫한 주제이다.
간단히 설명하자면, AI 모델과 다른 도구 등과의 상호작용을 제어하고 관리하기 위한 프로토콜이라고 보면된다.
이를 통해 Cursor나 Claude에서 AI를 통해 간단히 Github에 Commit을 하거나 Notion에 글을 쓸 수 있게 되는 것이다.

그래서 나도 이 파도에 올라타기 위해 Cursor에 MCP 기능이 생기자마자 사용하려 했는데...
WSL은 역시 오류가 있었다.
하지만, 개발자는 1억명이 넘고, 나같은 상황에 처한 사람은 무조건 있으니 인터넷에 검색해보니 해결방법이 있긴하였다.   

Smithery에서 기본적으로 제공하는 이 JSON 포맷은   
```
{
  "mcpServers": {
    "server-sequential-thinking": {
      "command": "wsl",
      "args": [
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/server-sequential-thinking",
        "--key",
        "@@@"
      ]
    }
  }
}
```
그냥 안된다.
내 문제인건지는 모르겠다만 그냥 안됨.   

인터넷에서 공통된 해결방법으로 제시하는 것은
wsl을 빼고 `cmd /c`를 추가하라는 것이다.
그래서 추가하고 나면,   
```
{
  "mcpServers": {
    "server-sequential-thinking": {
      "command": "cmd",
      "args": [
        '/c'
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/server-sequential-thinking",
        "--key",
        "@@@"
      ]
    }
  }
}
```
이렇게 되는데 이거 또한 안됬다.
해결법을 찾기 위해 계속된 구글링을 통해서 알아낸 것은 MCP를 사용하기 위해서는 nodejs와 npm 설치가 필요했다는 것이다.
그래서 wsl 환경과 windows 환경에 같은 버전으로 설치를 해주고 나니
해결이 되었다.