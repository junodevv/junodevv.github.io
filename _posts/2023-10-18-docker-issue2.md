---
title: "[Docker] docker 명령어 dk로 줄여서 사용하기"
category: Cloud
tags: cloud docker 
---

2023-10-18 클라우드 프로그래밍 7주차 강의

-----

# docker 명령어 dk로 줄여서 사용하기

.bashrc 내용을 수정

```shell
sudo nano ~/.bashrc
```

alias 설정 내용 작성후 저장

```shell
#제일하단에 내용입력
alias dk='docker'
```

.bashrc 내용을 적용
```shell
source ~/.bashrc
```

# 끝

-----

## reference

[교수님 블로그, hull.kr](https://hull.kr/cloud/16)