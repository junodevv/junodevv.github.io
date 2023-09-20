---
title: "[Docker] 도커 실습 1 - 컨테이너 연동, 파일 복사, 볼륨 마운트, 도커 이미지 생성"
category: Cloud
tags: cloud docker 
---

2023-09-20 클라우드 프로그래밍 3주차 강의 2

-----

> 1. [컨테이너 연동](#컨테이너-연동)
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

도커 네트워크 생성
- 워드프레스 프로그램이 MySQL에 저장된 데이터를 읽고 쓰기 위해 두 컨테이너가 연결되어야 함
- 가상 네트워크를 생성하고 이 네트워크에 두 개의 컨테이너를 소속시켜 두 컨테이너를 연결

```docker
# 네트워크 생성
docker network create wordpress000net1
```

MySQL 컨테이너 생성 
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
> -e : 환경변수 설정
>
> -e MYSQL_ROOT_PASSWORD=myrootpass  -> root 비번
>
> -e MYSQL_DATABASE=wordpress000db  -> db이름
>
> -e MYSQL_USER=wordpress000kun  -> 유저이름
>
> -e MYSQL_PASSWORD=wkunpass mysql -> 계정비번
>
> --default-authentication-plugin=mysql_native_password -> 내가 설정한 계정과 비번으로 접속되게 하는 옵션
>

워드프레스 컨테이너 생성

```docker
# 워드프레스 컨테이너 생성
docker run --name wordpress000ex12 -dit --net=wordpress000net1 -p 8085:80 
-e WORDPRESS_DB_HOST=mysql000ex11 
-e WORDPRESS_DB_NAME=wordpress000db 
-e WORDPRESS_DB_USER=wordpress000kun 
-e WORDPRESS_DB_PASSWORD=wkunpass wordpress
```

워드프레스 가입

- 정보를 입력한 후 '워드프레스 설치' 버튼 클릭



MySQL 컨테이너 내부에 접속하여 워드프레스와 연동이 되어 있는지 가입한 계정 정보 확인
- 컨테이너에서 리눅스 명령어를 실행하려면 우리 명령을 전달하는 프로그램인 셸(Shell)이 필요

```docker
# MySQL 컨테이너 내부로 들어가서 'bash'라는 셸 프로그램 실행
docker exec -it mysql000ex11 bash
```