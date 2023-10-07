---
title: "[java] 강의5 웹서버, ISO 7계층"
category: Java
tags: java java강의 
---

자바를 더 깊게 이해하기 위한 itwill 자바 강의 5

-----

# 웹서버

# 1. 서버

- 요청에의해서 폴더 하나를 열어서 공유해 주는것

<img width="478" alt="image" src="https://github.com/junodevv/Algorithm/assets/126752196/5e28c361-f441-4655-9d96-031e2b44ac7c">

- 웹서버는 수동적
        클라이언트의 PULL 요청에 따라 동작함, PULL 방식
        웹서버가 클라이언트에게 주는 걸 PUSH

# 2. Stateful

- 상태지속
        두 개의 컴퓨터?(서버-클라이언트)가 연결된 상태로 지속됨
- A와 B가 원할 때 언제든 메시지를 보낼 수 있음

# 3. Stateless

- 서버-클라이언트 관계에서 클라이언트는 요청만하고 서버는 응답만 하고 그 통신이 끝나면 연결이 끊김