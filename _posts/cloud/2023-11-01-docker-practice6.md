---
title: "[Docker] 도커 실습 6 - 역방향 프록시를 이용한 마이크로 서비스"
category: Cloud
tags: cloud docker 
---

2023-11-01 클라우드 프로그래밍 9주차 강의

-----

### 도커 (Docker)
- 도커는 컨테이너 기반의 오픈소스 플랫폼임.
- 애플리케이션과 그 의존성을 컨테이너로 패키징하여, 어디에서나 동일하게 실행할 수 있게 함.
- 도커 컨테이너는 가볍고 이식성이 높아서, 마이크로서비스 아키텍처와 잘 어울림.

### 마이크로서비스 (Microservices)
- 마이크로서비스는 하나의 큰 애플리케이션을 작은, 독립적인 서비스 단위로 분리한 아키텍처 패턴임.
- 각 서비스는 독립적으로 배포하고 운영할 수 있으며, 다른 서비스와 네트워크를 통해 통신함.
- 마이크로 서비스 아키텍처는 <b class="text-red">애플리케이션의 확장성, 유지보수성, 장애 격리 등을 향상</b>시킬 수 있음.

        기능적으로 컨테이너들을 분리하는 것 -> 이러면 이 점이 많음
        Client -> API Gateway -> Microsevices
        * 마이크로 서비스의 최대 단점: 세션관리

### 도커와 마이크로서비스의 관계
- 도커 컨테이너는 마이크로서비스를 구현하고 배포하는 데 이상적인 방법을 제공함.
- 각 마이크로서비스는 독립적인 컨테이너로 실행할 수 있으며, 이 컨테이너들은 동일한 호스트나 다른 호스트에 배포할 수 있음.
- 도커 컨테이너를 사용하면, 개발부터 테스트, 스테이징, 프로덕션까지 일관된 환경에서 애플리케이션을 실행할 수 있음.

즉, 도커는 마이크로서비스 아키텍처를 쉽게 구현하고 관리할 수 있는 강력한 도구를 제공하며, 둘은 서로 보완적인 관계임.

        도커가 있기 때문에 마이크로 서비스를 사용할 수 있는 것이기도함

### 역방향 프록시를 이용한 도커 기반 마이크로 서비스 시나리오
1. front 프로그램 서버 컨테이너
2. task 마이크로 서비스 서버 컨테이너
3. user 마이크로 서비스 서버 컨테이너
4. mysql 컨테이너(기존 실습)
5. nginx 역방향 프록시 컨테이너(기존 실습 활용)

        교수님이 직접 짠 방식 Gateway는 또 따로 배워야 해서 역방향 프록시를 이용하기로 함

클라우드서버(우분투 게스트)에 필요한 폴더 생성

```shell
# front 프로그램은 기존 html 폴더와 연결
mkdir php
mkdir ./php/task
mkdir ./php/user
```

        php를 사용하는 이유: 문법이 쉬워서

각 컨테이너 생성 및 실행

```shell
docker run -d --name myfront -p 8011:80 --net php-mysql -v ~/html:/var/www/html myphpimage
docker run -d --name mytaskapi -p 8012:80 --net php-mysql -v ~/php/task:/var/www/html myphpimage
docker run -d --name myuserapi -p 8013:80 --net php-mysql -v ~/php/user:/var/www/html myphpimage
```

        2개는 기존에 만들어 놓은 컨테이너 활용

기존 역방향 프록시 삭제 후 다시 설정(--net php-mysql  추가)

```shell
docker run -d --name myproxy --net php-mysql -p 8181:8181 -p 80:8080 -p 443:4443 -v /docker/appdata/nginx-proxy-manager:/config:rw jlesage/nginx-proxy-manager
```

역방향 프록시 설정

host 파일에 도메인 추가

mac은 방식이 달라서 찾아봐야함 -> [호스트(hosts) 파일 수정하는 법](https://likedev.tistory.com/entry/MAC%EB%A7%A5%EB%B6%81%EC%97%90%EC%84%9C-hosts-%ED%8C%8C%EC%9D%BC-%EC%88%98%EC%A0%95-%EB%B0%A9%EB%B2%95)

> 1. 터미널에서 `sudo vi /private/etc/hosts` 열기
> 2. 원하는 IP와 도메인 주소 입력
> \* 혹시모를 일을 위해`sudo cp /private/etc/hosts /private/etc/hosts.bak`을 사용해 백업파일을 만드는 방법도 좋음

        * 만약 hosts 파일변경이 안된다면 
            원인1: 관리자로 들어가지 않은경우
            원인2: 백신프로그램이 막는경우
            원인3: hosts 파일을 .txt 파일로 저장한 경우


크로스 오리진 문제 : 

이기종 서버?

포트를 안치게 하면서 같은 도메인네임 gctask.com:8011(8012)를 사용하지 않고 /task/를 8012포트로 커스텀해서 gctask.com/task/ 으로접속하게 만들어서 같은 도메인에 접속하고 있는걸로 보이게 만듦

그리고 IP에 myfront를 넣으면 왜 내부에서 통신해서 80포트로 설정하는 거지

원래는 포트번호를 노출하는 것이 좋지는 않음 그래서 위처럼 task 로 했으면 8012를 명시해서 접속하는걸 막는게 좋음



<b class="text-red"></b> 


# 진행중

-----

# reference

[교수님 블로그 및 강의, hull.kr](https://hull.kr/cloud/17)