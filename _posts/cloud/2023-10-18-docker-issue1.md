---
title: "[Docker] 도커 컨테이너 자동 재시작"
category: Cloud
tags: cloud docker 
---

2023-10-18 클라우드 프로그래밍 7주차 강의

-----

> 1. 도커 컨테이너 옵션을 활용한 재시작 방법
> 2. 우분투 systemd 활용한 재시작 방법

-----

# 1. 도커 컨테이너 옵션을 활용한 재시작 방법

```shell
docker run --restart=always -d your-image
```

### 도커 컨테이너 재시작 정책
- no : 컨테이너가 멈추면 재시작하지 않음. 이것이 디폴트 설정임.
- on-failure : 컨테이너가 비정상적으로 종료되었을 때(즉, 종료 상태 코드가 0이 아닌 경우)에만 재시작함.
- unless-stopped : 컨테이너가 사용자에 의해 명시적으로 정지되지 않는 한 항상 재시작함.
- always : 컨테이너가 멈추면 언제나 재시작함.

이미 실행 중인 컨테이너의 재시작 정책을 변경하려면 docker update 명령어를 사용할 수 있음

```shell
docker update --restart=always your-container-id-or-name
```

# 2. 우분투 systemd 활용한 재시작 방법

```shell
sudo nano /etc/systemd/system/your-container.service
```

your-container.service 파일 내용

```
[Unit]
Description=Your Container Description
After=docker.service
Requires=docker.service
[Service]
ExecStart=/usr/bin/docker start -a your-container-name-or-id
ExecStop=/usr/bin/docker stop -t 2 your-container-name-or-id
Restart=always
RestartSec=5s
[Install]
WantedBy=multi-user.target
```

systemd service 옵션 내용
- Description : 서비스에 대한 설명
- After와 Requires : 이 서비스가 docker.service에 의존한다는 것을 나타냄
- ExecStart : 서비스가 시작될 때 실행할 명령어
- ExecStop : 서비스가 중지될 때 실행할 명령어
- Restart : 재시작 정책 설정
- RestartSec : 재시작 딜레이 시간 설정
- WantedBy : 이 서비스가 설치될 목적지를 나타냄

Systemd 데몬 리로드
- 변경 사항을 적용하기 위해 systemd 데몬을 리로드함.

```shell
sudo systemctl daemon-reload
```

서비스 활성화
- 서비스를 활성화해서 부팅 시 자동으로 시작되게 함.

```shell
sudo systemctl enable your-container.service
```

서비스 시작 및 상태 확인
- 최초는 서비스 수동 시작 필요

```shell
sudo systemctl start your-container.service
sudo systemctl status your-container.service
```

#끝

-----

## reference

[교수님 블로그, hull.kr](https://hull.kr/cloud/15)