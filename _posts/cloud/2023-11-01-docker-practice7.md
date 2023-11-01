---
title: "[Docker] 도커 실습 7 - 역방향 프록시를 이용한 마이크로 서비스의 세션 처리"
category: Cloud
tags: cloud docker 
---

2023-11-01 클라우드 프로그래밍 9주차 강의 2

-----

> 목차
> 1. Redis 개요
> 2. Redis Session
> 3. Redis 컨테이너 설치 및 세션 테스트

-----

# 1. Redis 개요

Redis(Remote Dictionary Server)는 고성능 키-값 저장소로, 데이터베이스, 캐시, 메시지 브로커 등 다양한 용도로 사용됨. Redis는 메모리 기반으로 동작하기 때문에 디스크 기반의 데이터베이스보다 훨씬 빠른 속도를 자랑함. 다양한 데이터 타입을 지원하며, 단순한 문자열뿐만 아니라 리스트, 셋, 해시 등도 저장할 수 있음.

### 특징

- 메모리 기반: 빠른 읽기/쓰기 속도
- 지속성: 필요에 따라 디스크에 데이터를 저장할 수 있음
- 레플리케이션: 데이터 복제를 통한 고가용성 지원
- 트랜잭션: 여러 연산을 원자적으로 처리
- Pub/Sub: 메시지 브로커 기능도 제공

-----

# 2. Redis Session
웹 애플리케이션은 사용자의 상태를 유지하기 위해 세션을 사용함. 그러나 세션 데이터를 애플리케이션 서버에 저장하면 서버가 여러 대일 경우 세션 공유에 대한 문제가 있음. 이런 문제를 해결하기 위해 Redis 같은 메모리 저장소를 사용하여 세션 정보를 중앙에서 관리함.

### Redis Session 장점

- 빠른 접근: 메모리 기반으로 빠르게 세션 데이터에 접근 가능
- 확장성: 여러 서버 간의 세션 공유가 쉬움
- 지속성: 필요에 따라 세션 데이터를 디스크에 저장하여 안정성 확보

Redis는 빠른 속도와 다양한 용도로 인해 많이 사용되고 있으며, 특히 세션 관리에 유용함.

        그리고 이기종 서버에서 돌아가는 다양한 종류의 프로그램들이 같은 세션을 유지하도록 도와준다. 

-----

# 3. Redis 컨테이너 설치 및 세션 테스트

### redis 도커 허브
- https://hub.docker.com/_/redis
- https://www.docker.com/blog/how-to-use-the-redis-docker-official-image/

redis 컨테이너 설치 명령어

```shell
docker run -d --name myredis --net php-mysql -p 6379:6379 redis
```

redis session store 사용을 위한 각 컨테이너(mytaskapi, myuserapi) 모듈 설치

```shell
docker exec -it mytaskapi /bin/bash
pecl install redis
docker-php-ext-enable redis
exit
docker exec -it myuserapi /bin/bash
pecl install redis
docker-php-ext-enable redis
exit
```

        각 서버에서 redis 확장 모듈이 필요하기 때문에 bash들어가서 직접 설치해줌

redis_session.php 설정 파일 생성

```php
// Redis에 연결
$redis = new Redis();
$redis->connect('myredis', 6379);
```

'myredis'부분이 원래 ip? 도메인? 이 들어가야하는데 우리는 도커에서 php-mysql network 로 연결되어 있어서 'myredis'로 사용할 수 있는 거임

-----

# 진행중

-----

# reference
[교수님 블로그 및 강의, hull.kr](https://hull.kr/cloud/18)