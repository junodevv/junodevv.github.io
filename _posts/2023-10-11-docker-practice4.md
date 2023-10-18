---
title: "[Docker] 도커 실습 4 - php mysql 연결 및 tasks app 개발"
category: Cloud
tags: cloud docker 
---

2023-10-11 클라우드 프로그래밍 6주차 강의

-----

> 1. php, mysql 컨테이너 실행
> 2. 개발 환경 구축 및 tasks app 설치
> 3. front, back end 컨테이너 분할

-----
# 1. php, mysql 컨테이너 실행

# 2. 개발 환경 구축 및 tasks app 설치

# 3. front, back end 컨테이너 분할

크로스 오리진 문제(2가지)

        소스IP     <--->     리소스IP
        - 192.168.88.128:80(프론트 프로그램 서버 IP) <---> 192.168.88.128:8080
        - 192.168.88.128:80(윈도우 IP) <---> 192.168.88.128:8080