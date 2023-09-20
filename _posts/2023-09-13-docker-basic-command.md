---
title: "[Docker] 도커 기본 명령어"
category: Cloud
tags: cloud docker 
---

2023-09-13 클라우드 프로그래밍 2주차 강의

-----

# 도커 명령어

도커 명령어란?
- 컨테이너를 다루는 명령어는 ‘docker’ 명령어로 시작   docker ~ 

도커 명령어의 기본적인 형태
- docker 커맨드(상위/하위) (옵션) 대상(이미지) (명령어) (인자)

커맨드(상위/하위)
- ‘무엇을‘, ‘어떻게’ 에 해당하는 부분 
<p class="text-gray">-> '무엇을'을 쓰지 않으면 container가 기본값이다.</p>

- 커맨드는 상위 커맨드(’무엇을’)와 하위 커맨드(‘어떻게‘)로 구성
- 상위 커맨드는 12종류(대상의 종류)
- docker **container start** penguin

옵션
- 옵션은 커맨드에 세세한 설정을 지정하는 용도
- 커맨드의 실행 방법 또는 커맨드에 전달할 값을 지정
- -d, --name penguin, -dit(백그라운드에서 입력을받는다)

대상(이미지)
- 커맨드와 달리 구체적인 이름을 지정
- docker container start [옵션] **penguin**

명령어 인자
- 대상에 전달할 명령어와 인자 값을 지정
- 문자 코드 또는 포트 번호 등을 전달 가능
- docker run -d python:3.8-alpine python -m http.server

-----

# 컨테이너 생성, 실행, 종료, 삭제

컨테이너 생성 및 실행
- 컨테이너를 생성하고 실행하는 커맨드 : docker run(docker container run)
    - docker image pull, docker container create, docker container start 기능을 하나로 합친 것과 같음
    - docerk run (옵션) 이미지 (인자) 
- 주요 옵션

<img width="946" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/4b06cbfe-9a60-4ce2-a909-c7393a9478b3">

- 서버(데몬)이 아닌 OS형태의 컨테이너의 경우 run 실행시 바로 종료
- docker run을 실행하되 백그라운드에서 실행하고(-d), 프로세스가 종료되지 않게 유지하고(-it)

컨테이너 정지 및 삭제 
- 컨테이너를 정지하는 커맨드 : docker stop(docker container stop)
    - 컨테이너를 삭제하려면 컨테이너를 정지시켜야 함
    - docker stop 컨테이너_이름
 
- 컨테이너를 삭제하는 커맨드 : docker rm(docker container rm)
    - 정지 상태가 아닌 컨테이너를 대상으로 실행하면 오류 발생 후 컨테이너가 삭제 되지 않음
    - docker rm 컨테이너_이름

컨테이너 목록
- 컨테이너의 목록을 출력하는 커맨드 : docker ps(docker container ls) = 도커 프로세스
    - 현재 실행 중인 컨테이너의 목록을 출력
    - docker ps -a 옵션을 추가하면 현재 존재하는 컨테이너의 목록 출력
- 컨테이너 목록의 주요 항목

<img width="942" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/61b00c1a-a56d-49f6-a7b5-73752f0f0203">

----- 

# 끝

-----

## reference

[교수님 블로그, 개발자 hull](https://hull.kr/cloud/7)