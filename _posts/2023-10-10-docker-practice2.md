---
title: "[Docker] 도커 실습 2 - 도커 허브, 도커 컴포즈"
category: Cloud
tags: cloud docker 
---

2023-09-27 클라우드 프로그래밍 5주차 강의

-----

도커 실습 2

> 1. 도커 허브
> 2. 도커 컴포즈

-----

# 1. 도커 허브

### 도커 허브란
- 도커 제작사에서 운영하는 공식 도커 레지스트리
        
        이미지를 내려받는 곳, docker run 해서 내려받는 이미지들이 기본으로는 여기서 받는 거임

### 도커 레지스트리
- **도커 이미지를 저장하고 관리하는 중앙 저장소**로서 동작하는 서버
- 도커 이미지를 업로드, 다운로드, 검색, 삭제 등의 작업을 수행
- 도커 이미지를 공유하고 배포하기 위해 사용
    
### 리포지토리
- 도커 이미지의 집합을 나타내는 공간
- 이미지의 다양한 버전을 관리하고 구분


<img width="1015" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/f7dd2fa8-9717-4a0d-aaa5-c1bc8e09973a">

### 태그
- **도커 이미지의 버전을 식별하기 위한 라벨**
- 이미지의 특정 버전을 구분하고 관리하는 데 사용
- 레지스트리_주소(도커 허브는 ID)/리포지토리_이름:버전

        * 가장 마지막(최신) 버전: latest
        * 버전 이름짓기 ex) kky/httpd:01
        * 이전에 docker run httpd 를 했을 때 이 태그의 앞뒤가 생략되었던 것임
        공식적인 애들은 레지스트리 주소가 생략될수 있고, 버전을 안붙이면 가장 최신버전을 불러옴 

### 이미지에 태그를 부여해 복제하는 명령어(tag)
- docker tag [기존_이미지_이름] [레지스트리_주소]/[리포지토리_이름]:[버전]
- 명령어 실행 후 기존 이미지와 태그가 부여된 이미지가 둘 다 존재

        * 기존이미지_이름 ex) httpd
        * 레지스트리_주소: 거의 계정주소
        * 리포지토리_이름: 내가 계정안에서 생성한 리포지토리(저장소)
        
### 이미지를 업로드하는 명령어(push)
- docker push [레지스트리_주소]/[리포지토리_이름]:[버전]
- 리포지토리는 처음 업로드할 때는 존재하지 않고, push 커맨드를 실행하며 만들어짐

```shell
# 컨테이너 명 : mysql
# 네트워크 : php-mysql
# root 패스워드 : 123456
# database : php-mysql
# 사용자 계정 : php-mysql
# 사용자 패스워드 : 123456
# 포트포워드 : 33306:3306
docker run --name mysql 
-dit -p 33306:3306 
--net=php-mysql 
-e MYSQL_ROOT_PASSWORD=123456 
-e MYSQL_DATABASE=php-mysql 
-e MYSQL_USER=php-mysql 
-e MYSQL_PASSWORD=123456 
mysql --character-set-server=utf8mb4 
--collation-server=utf8mb4_unicode_ci 
--default-authentication-plugin=mysql_native_password
```