---
title: "[Docker] 도커 실습 5 - 역방향 프록시를 이용한 도메인 기반 서비스"
category: Cloud
tags: cloud docker 
---

2023-10-18 클라우드 프로그래밍 7주차 강의

-----

> 1. 역방향 프록시 개요
> 2. 역방향 프록시 컨테이너 설치

------

# 역방향 프록시 개요

역방향 프록시란?

역방향 프록시(reverse proxy)는 클라이언트와 서버 간의 통신에서 중간 역할을 함. 클라이언트가 서버에 직접 요청을 보내지 않고, 역방향 프록시를 통해 요청을 보냄. 역방향 프록시는 이 요청을 적절한 서버로 전달하고, 서버의 응답을 다시 클라이언트에게 전달함.

        아이패드 그림 참조

주요 기능
- 로드 밸런싱: 여러 서버가 있을 때, 트래픽을 균등하게 분산시킴.
- 캐싱: 자주 요청되는 리소스를 캐시에 저장해 빠른 응답을 가능하게 함.
- SSL 종료: SSL/TLS 암호화와 복호화 작업을 처리함.
- 압축: 데이터를 압축하여 전송 시간을 줄임.
- 보안: 보안 규칙을 적용하여 서버를 외부 위협으로부터 보호함.

예시
- Nginx, Apache HTTP Server, HAProxy 등이 있음.
- 역방향 프록시는 웹 애플리케이션의 확장성과 보안을 향상시키는 데 유용하게 사용됨.

구성 요소 설명
- Client: 웹 브라우저나 애플리케이션 등, 서비스를 사용하는 클라이언트.
- Reverse Proxy (Nginx): 클라이언트의 요청을 받아 적절한 도커 컨테이너로 전달하고, 컨테이너의 응답을 클라이언트에게 다시 전달하는 역할을 함. Nginx나 Apache 같은 소프트웨어가 이 역할을 수행함.
- Docker Containers: 실제 서비스를 제공하는 서버가 도커 컨테이너로 구성되어 있음. 각 컨테이너는 독립적인 서비스를 제공할 수 있음.

작동 과정
1. 클라이언트가 서비스에 접속하려고 요청을 보냄.
2. 역방향 프록시(Nginx 등)가 이 요청을 받아 로드 밸런싱, 캐싱, SSL 종료 등의 작업을 수행함.
3. 역방향 프록시가 요청을 적절한 도커 컨테이너로 전달함.
4. 도커 컨테이너가 요청을 처리한 후 응답을 역방향 프록시에게 보냄.
5. 역방향 프록시가 이 응답을 클라이언트에게 전달함.

-----

# 역방향 프록시 컨테이너 설치

### nginx 역방향 프록시 도커 허브
- https://hub.docker.com/r/jlesage/nginx-proxy-manager

### nginx 역방향 프록시 컨테이너 설치
```shell
docker run -d --name myproxy -p 8181:8181 -p 80:8080 -p 443:4443 -v /docker/appdata/nginx-proxy-manager:/config:rw jlesage/nginx-proxy-manager
```

        8181: 관리자 접속 포트
        80: http
        443: https

### nginx 역방향 프록시 접속
- http://게스트 우분트 아이피:8181
- http://192.168.47.128:8181
- id : admin@example.com
- pw : changeme

### nginx 역방향 프록시 admin 설정
- 아이디(메일), 패스워드 반드시 자신의 계정으로 변경
- 패스워드는 8자리 이상

**도메인 페이크 해야됨** aaa.com이 이미 존재하기떄문에 호스트 컴퓨터 내에서 aaa.com으로 접속을하면 기존에 있는 그 도메인이 아니라 내가 설정한 IP로 접속할 수 있게 해줘야 함


### 아파치 서버 및 nginx 서버 컨테이너 설치
```shell
mkdir apacheHome
mkdir nginxHome
docker run --name myapache -d -p 8001:80 -v ~/apacheHome:/usr/local/apache2/htdocs httpd
docker run --name mynginx -d -p 8002:80 -v ~/nginxHome:/usr/share/nginx/html nginx
```

### 각 폴더에 index.html 파일 생성
```shell
# 폴더 이동
cd /apacheHome
# 파일 생성
vi index.html
```
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>아파치 서버</title>
</head>
<body>
<h1>아파치 서버</h1>
</body>
</html>
```
```shell
# 폴더 이동
cd /nginxHome
# 파일 생성
vi index.html
```
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>엔진X 서버</title>
</head>
<body>
<h1>엔진X 서버</h1>
</body>
</html>
```

결과

<img width="353" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/280973c5-e964-475a-bd92-8c9c698e7244">
<img width="358" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e0c9117d-5a35-438b-8e79-a7ce5d11fc87">

# nginx proxy manager로 도메인 설정 과정

### 도메인 페이크(hosts 파일 변경)

루트 권한으로 hosts 파일로 이동
```shell
sudo vi private/etc/hosts
```
<img width="406" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/b9c9ff77-5826-43c4-8a0d-fffedcc83ece">

원하는 IP와 도메인 추가
- aaa.com
- bbb.com

### Nginx Proxy Manager 접속 후 Hosts 설정

172.16.147.129:8181로 Nginx Proxy Manager 접속

<img width="860" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e49b74dc-ffba-41ca-b1e3-1eea108a9ce5">

- Proxy Hosts 클릭
- add Proxy Host 버튼 클릭
- IP와 도메인 이름, 포트번호 설정 및 저장

<img width="497" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/03c4950d-1e0e-4d33-af30-3d2f85fd1f4d">

<img width="725" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/d70c44ba-26a0-40a9-b3cb-66c68c5a94f6">

도메인 주소를 통한 페이지 접속

<img width="303" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/d458aa1d-b998-44fa-9ea9-80613985654c">
<img width="284" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/24afd29b-ad1a-4fd2-a2eb-ba6707d5ddfb">

역방향 프록시를 통한 도메인 기반 서비스 만들기 성공!

# 끝

## reference 

[교수님 블로그 및 강의, hull.kr](https://hull.kr/cloud/14?page=2)