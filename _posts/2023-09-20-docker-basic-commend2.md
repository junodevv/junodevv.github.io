---
title: "[Docker] 도커 기본 명령어2"
category: Cloud
tags: cloud docker 
---

2023-09-20 클라우드 프로그래밍 3주차 강의 1

> 1. [컨테이너 통신](#컨테이너-통신)
> 2. [이미지 삭제](#이미지-삭제)

-----

# 컨테이너 통신

<p class="text-gray">외부에서 접속이 가능하게 하기 위해 통신에 대한 설정?이  필요</p>
<p class="text-gray">원래 https 를 사용해야 하지만 복잡해서 안함</p>


컨테이너 통신 체크 포인트
 
- 포트 : 컴퓨터 네트워크에서 통신을 위한 통로
- 호스트 포트 번호와 컨테이너 포트 번호 포트 포워딩
- 포트 포워딩 : 컨테이너 내부 포트와 호스트 포트를 매핑하는 기술
- 매핑 : 두 개의 포트를 연결하는 작업
- 포트포워딩 하는법 => -p 호스트_포트_번호 : 컨테이너_포트_번호 (-p 8080 : 80)
- 여러 개의 컨테이너를 생성하려면 호스트 포트 번호를 겹치지 않게 설정

> 부가설명
> - VMware의 ubuntu 를 개별 Host 라고 생각하기
> - httpd(아파치) 는 보통 80포트를 사용
> - MySQL은 3306포트 사용
> - ens33 -> 192.168.88.21
> - Host(Dhcp) -> 192.168.88.1
> 
> 결론: 포트포워딩을 해줘야한다. 외부에서 접속하려면 도메인 사서 해야함

> 역방향 프록시: hull.kr(:443) 로 들어오면 로컬 내부(192.168.0.2:30080)로 보내도록 리다이렉트 하고 
>
> -> 30080을 컨테이너의 80포트와 매핑한다.

- 아파치: 웹서버 기능을 제공하는 소프트웨어

```docker
# 아파치 컨테이너 생성
docker run --name apa000ex2 -d -p 8080:80 httpd
```

```docker
# 실행중인 컨테이너 목록 확인
docker ps
```
<img width="803" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/73103988-8577-4239-a0d0-432a2905aa86">

컨테이너가 실행 중 일때 웹 브라우저를 통해 localhost:8080 으로 접속하면 아파치컨테이너의 포트인 80 포트로 연결된다

PORTS 0.0.0.0:8080 -> 80/tcp ~

```docker
# 컨테이너 종료
docker stop apa000ex2

# 컨테이너 삭제
docker rm apa000ex2
```

> run 명령어: 이미지로부터 컨테이너를 새로 만들고 스타트 하는 것
>
> 따라서 stop 하고난 뒤에 다시 같은 컨테이너를 실행할땐 run이 안된다.

```docker
# 이미 생성된 컨테이너 실행
docker start apa000ex2
```

# 이미지 삭제

**이미지 삭제 체크 포인트**

- 이미지를 삭제하는 이유 : 컨테이너를 삭제해도 이미지는 그대로 남아 스토리지 용량을 압박하게 됨
- 이미지 삭제하는 방법 : 해당 이미지로 실행한 컨테이너가 남아 있으면 이미지를 삭제할 수 없으므로 컨테이너 삭제후 이미지 삭제 가능
- docker image rm 커맨드 
    - 이미지를 삭제하는 명령어
    - 공백으로 구분해 여러 이미지를 지정 가능
    - docker image rm [이미지_이름] [이미지_이름] [이미지_이름]

```docker
# 이미지 목록 확인
docker image ls
```

<img width="695" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/f741880e-962e-418a-9f48-514e3b6814fc">


- 이미지 목록의 주요 항목 표

<img width="767" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/1d8c1069-2bd7-436d-9b4d-4032cfaef9f1">


**이미지 삭제**

```docker
# 이미지 삭제
docker image rm httpd

# 버전지정 이미지 삭제
docker image rm httpd:2.2
```

> 이때, 해당 이미지가 컨테이너에서 사용 중이면 이미지를 삭제할 수 없다는 오류 메시지가 뜸
>
> 아파치 컨테이너를 종료(stop)만 하고 삭제(rm)르르 하지 않았기 때문에 이미지가 사용중인 것으로 간주됨

```docker
# 존재하는 컨테이너 목록확인
docker ps -a
```
<img width="697" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/233e473a-7b27-48d2-a6f5-c96d84ae955c">

```docker
# 컨테이너 삭제
docker rm apa000ex2

# 이후 오류메시지가 떴던 이미지 삭제
docker image rm httpd
```

-----

# 끝

-----

## reference

[개발자 hull, 교수님 강의블로그](https://hull.kr/cloud/7)