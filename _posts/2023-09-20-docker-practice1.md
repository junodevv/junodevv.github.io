---
title: "[Docker] 도커 실습 1.1 - 컨테이너 연동"
category: Cloud
tags: cloud docker 
---

2023-09-20 클라우드 프로그래밍 3주차 강의 2

-----
실습 1
> 1. [컨테이너 연동](#컨테이너-연동)
>       - [도커 네트워크 생성](#도커-네트워크-생성)
>       - [MySQL 컨테이너 생성](#mysql-컨테이너-생성)
>       - [워드프레스 컨테이너 생성](#워드프레스-컨테이너-생성)
>       - [워드프레스 사이트 구축 정리](#워드프레스-사이트-구축-정리)
> 2. 파일 복사
> 3. 볼륨 마운트
> 4. 도커 이미지 생성 

-----

# 컨테이너 연동

컨테이너 연동 실습 시나리오
- network 구성
- mysql 컨테이너 설치
- 워드프레스 컨테이너 설치
- 워드프레스 <-> mysql 연동

워드 프레스
- 웹 사이트를 만들기 위한 소프트웨어
- 워드프레스가 동작하려면 워드프레스 프로그램 외에도 아파치, 데이터베이스, PHP 런타임 필요
- 워드프레스 공식 이미지에 워드프레스 프로그램(본체)와 아파치, PHP 런타임 포함
- 워드프레스는 데이터베이스 중 MySQL 및 MariaDB를 지원

#### 도커 네트워크 생성
- 워드프레스 프로그램이 MySQL에 저장된 데이터를 읽고 쓰기 위해 두 컨테이너가 연결되어야 함
- 가상 네트워크를 생성하고 이 네트워크에 두 개의 컨테이너를 소속시켜 두 컨테이너를 연결

```docker
# 네트워크 생성
docker network create wordpress000net1
```

> 참고: 특별한 네트워크에 포함 시키느냐 아니냐에 따라서 사용하는 네트워크의 대역폭이 달라진다. 172.**17**.0.2, 172.**18**.0.2
>
> ```docker
> docker inspect [컨테이너 이름]
> ```
> 를 통해서 네트워크 대역폭을 확인 할 수 있다.

#### MySQL 컨테이너 생성 
- 데이터베이스 서버를 미리 구축하고 설정한 후 워드프레스 컨테이너와 연동하여 사용하기 위해 MySQL 컨테이너를 먼저 생성

```docker
#MySQL 컨테이너 생성
docker run --name mysql000ex11 
-dit 
--net=wordpress000net1 
-e MYSQL_ROOT_PASSWORD=myrootpass 
-e MYSQL_DATABASE=wordpress000db 
-e MYSQL_USER=wordpress000kun 
-e MYSQL_PASSWORD=wkunpass mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
```

**MySQL 컨테이너 생성시 각 옵션 및 인자의 역할**
- MySQL 데이터베이스를 초기화하고 워드프레스와 연동하기 위해 필요한 설정을 지정

> --net=wordpress000net1 -> "wordpress000net1" 네트워크에 컨테이너 연결
>
> ** -e : 환경변수 설정
>
> -e MYSQL_ROOT_PASSWORD=myrootpass  -> root계정 비번 설정
>
> -e MYSQL_DATABASE=wordpress000db  -> db이름 설정
>
> -e MYSQL_USER=wordpress000kun  -> 사용자 계정 유저이름 설정
>
> -e MYSQL_PASSWORD=wkunpass mysql -> 위 사용자 계정비번
>
> --character-set-server=utf8mb4 -> 서버 기본 문자셋 설정
>
> --collation-server=utf8mb4_unicode_ci -> 서버의 기본 콜레이션 설정
>
> \* 콜레이션: 문자열 비교와 정렬방식을 지정한다.
>
> --default-authentication-plugin=mysql_native_password -> 사용자 계정 기본 인증 플러그인을 native로 설정, 내가 설정한 계정과 비번으로 접속되게 하는 옵션
<br><br>

#### 워드프레스 컨테이너 생성

```docker
# 워드프레스 컨테이너 생성
docker run --name wordpress000ex12 -dit --net=wordpress000net1 -p 8085:80 
-e WORDPRESS_DB_HOST=mysql000ex11 
-e WORDPRESS_DB_NAME=wordpress000db 
-e WORDPRESS_DB_USER=wordpress000kun 
-e WORDPRESS_DB_PASSWORD=wkunpass wordpress
```


**워드프레스 컨테이너 생성시 각 옵션 및 인자의 역할**
- 워드프레스와 MySQL 간의 연동을 위해 설정한 데이터베이스 정보를 워드프레스 컨테이너에 전달

> --net=wordpress000net1 -> "wordpress000net1" 네트워크에 컨테이너 연결
>
> -p 8085:80 -> 호스트의 8085포트와 컨테이너의 80 포트를 매핑하여 외부에서 컨테이너에 접근 가능하게함
>
> -e WORDPRESS_DB_HOST=mysql000ex11 -> 워드프레스가 사용할 Mysql 데이터베이스 호스트 주소 설정
>
> -e WORDPRESS_DB_NAME=wordpress000db -> 워드프레스가 사용할 데이터베이스 이름 지정
>
> -e WORDPRESS_DB_USER=wordpress000kun -> 워드프레스와 연결할 데이터베이스 사용자 계정이름 지정
>
> -e WORDPRESS_DB_PASSWORD=wkunpass -> 위 사용자 계정의 비밀번호 지정

워드프레스 가입

- 웹브라우저에서 http://localhost:8085 로 접속한다.

- 정보를 입력한 후 '워드프레스 설치' 버튼 클릭

<img width="664" alt="Pasted Graphic" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/dac44301-a3d9-40e7-9ab1-5f849633955f">

설치성공 후

MySQL 컨테이너 내부에 접속하여 워드프레스와 연동이 되어 있는지 가입한 계정 정보 확인
- 컨테이너에서 리눅스 명령어를 실행하려면 우리 명령을 전달하는 프로그램인 셸(Shell)이 필요

```docker
# MySQL 컨테이너 내부로 들어가서 'bash'라는 셸 프로그램 실행
docker exec -it mysql000ex11 bash
```

<img width="711" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/aac802f2-86d7-4dcc-81a6-3a280fe38d8f">

-> 접속성공(bash-4.4#) 

이후 MySQL 데이터베이스 서버에 컨테이너 run(생성) 할 때 설정한 'wordpress000kun' USER로 접속

```docker
mysql -u wordpress000kun -p
```

> 참고: mysql 에서 최소한의 리눅스를 구성한 컨테이너를 제공하기 때문에 mysql000ex11이란 이름으로 만든 컨테이너 속 리눅스의 bash에 들어갈 수 있음
>
> 그 bash에서 MySQL 서버에 접속하는 것

계정 정보가 있는 데이터베이스 및 테이블 확인

```docker
# 사용할 데이터베이스 선택
use wordpress000db;

# 계정에 있는 모든 테이블 확인
show tables;
```

<img width="265" alt="Pasted Graphic 1" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/4321e9c8-396e-4a7b-810e-399c87b83a97">

> <p class="text-gray">근데 저 테이블들은 왜 생성되어있지?..</p>

계정정보가 있는 테이블 확인

```docker
select * from wp_users;
```

<img width="844" alt="Pasted Graphic 2" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/9f6bd927-2ced-41eb-ba2e-a5a1ff701a45">

-> http://localhost:8085 를 통해 가입한 정보가 테이블에 들어있다.

MySQL 서버 및 컨테이너 에서 나가기

```docker
# 나가기
mysql> exit

bash-4.4# exit
```

#### 워드프레스 사이트 구축 정리

- 컨테이너 종료
    - docker stop mysql000ex11
    - docker stop wordpress000ex12
    <br><br>
- 컨테이너 삭제
    - docker rm mysql000ex11
    - docker rm wordpress000ex12
    <br><br>
- 이미지 삭제 
    - docker image rm mysql
    - docker image rm wordpress
    <br><br>
- 네트워크 삭제
    - docker network rm wordpress000net1

------

# 끝

-----

## reference

[개발자 hull.kr, 교수님 강의블로그](https://hull.kr/cloud/9)