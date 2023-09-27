---
title: "[Docker] 도커 실습 1.2 - 파일복사, 볼륨마운트, 도커 이미지 생성"
category: Cloud
tags: cloud docker 
---

2023-09-27 클라우드 프로그래밍 4주차 강의 1

-----

도커 실습 1

> 1. [컨테이너 연동](/docker/2023/09/27/docker-practice1.html)
> 2. 파일 복사
> 3. 볼륨 마운트
> 4. 도커 이미지 생성 

-----

# 2. 파일복사

### 컨테이너 내부와 호스트 간에 파일을 복사하는 작업의 필요성

 - 데이터 복원 
    - 컨테이너 안의 데이터를 호스트로 백업하거나, 호스트에서 데이터를 컨테이너로 복원할 때 필요
    - 중요한 데이터를 보존하고 컨테이너가 삭제되더라도 안전하게 데이터를 유지

- 파일 공유
    - 호스트와 컨테이너 간 파일 공유 시 사용
    - 컨테이너 내에서 생성된 결과물이나 로그 파일 등을 호스트에서 확인하고 사용
    - 컨테이너에서 생성된 파일을 호스트에서 사용해야 할 때

### 파일 복사 명령어
    
- 파일 복사는 양방향 모두 가능
- 컨테이너로 파일을 복사하는 커맨드 사용 예(호스트 -> 컨테이너)
    - docker cp [호스트_경로] [컨테이너_이름] : [컨테이너_경로]
- 호스트로 파일을 복사하는 커맨드 사용 예(컨테이너 -> 호스트)
    - docker cp [컨테이너_이름] : [컨테이너_경로] [호스트_경로]

### 컨테이너와 호스트 간에 파일 복사 실습
- 호스트의 html 파일을 아파치 컨테이너 내부로 복사하여 초기 화면 변경

##### 디렉토리 생성 및 html 파일 생성

```docker
#홈 디렉토리 아래 '~/example_html' 디렉토리 생성
mkdir ~/example_html

#nano 텍스트 에디터를 사용 '~/example_html' 디렉토리에 새로운 HTML 파일 생성
nano ~/example_html/index.html
```
##### HTML 내용 작성

```HTML
<html>
    <meta charset="utf-8"/>
    <body>
        <div> 안녕하세요! </div>
    </body>
</html>
```

        우분투 서버 ssh 에서 직접 index.html 을 만드는 것이 아님
        외부에서 우분투 ssh 에 접속하는 방식을 써서 편집을 해야함(== 원격 ssh접속)
        교수님이 추천해준 프로그램 -> https://blog.naver.com/pgh7092/221256069394, https://hull.kr/cloud/12 

        이때, <html> 같은 태그들을 하나하나 다쓰는 건 힘듦
        이클립스 Remote Systems 쓸 수 있음 -> 확장프로그램설치해야함 -> 그럼 우분투의 ssh의 폴더를 이클립스에서 수정할 수 있음
        그러면 태그들의 자동완성기능을 사용할 수 있게 됨.

##### 아파치 컨테이너 생성

```docker
# 아파치 컨테이너 생성
docker run --name apa000ex19 -d -p 8089:80 httpd
```

        -d: 백그라운드로 실행
        -p: 포트번호 지정
        8089: 포트번호
        httpd: apache 서버의 도커제공 서버 공식이름

##### 복사 명령어로 호스트에서 컨테이너로 index.html 파일 복사

```docker
#'~/example_html/index.html' 파일을 아파치 컨테이너인 'apa000ex19'의 '/usr/local/apache2/htdocs/' 경로로 복사
#아파치 컨테이너의 경로는 아파치 서버가 웹 브라우저에 제공하는 웹 루트 디렉토리
docker cp ~/example_html/index.html apa000ex19:/usr/local/apache2/htdocs/
```

<img width="635" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/6ddb54b6-8d8f-4651-a730-0ee1c4ecfd3d">

        * 이게 오타가 나기 쉬워서 나중에는 볼륨 마운트를 사용하게됨
        그래서 이건 잘 사용안함
        근데 cp가 왜 필요한가 -> 볼륨마운트로 할때는 머시기 뭐가 안된대, 마운트 안된 파일에서 수정해서 마운트된 파일로 cp해야할 일이 생길 수 있대

# 3. 볼륨 마운트

컨테이너의 휘발성
- 컨테이너가 시작되고 실행되는 동안에는 컨테이너 내의 모든 데이터 및 상태가 보존
- 컨테이너가 종료되면 컨테이너 내부의 데이터와 상태는 사라짐(파일, 프로세스 상태, 메모리 내용 등)
- 컨테이너의 휘발성을 극복하기 위해 볼륨이라는 저장 공간을 이용해 데이터의 영속성을 보장

            * 영속성: 계속 유지하는 속성

볼륨
- 컨테이너와 호스트 머신 간 데이터를 안전하게 저장, 관리 및 공유하기 위한 독립적인 저장 공간
- 도커 엔진에 의해 관리되며 호스트 머신의 파일 시스템과는 분리되어 작동
- 데이터의 영속성을 보장하고 여러 컨테이너 간에 데이터를 공유 및 백업이 가능

        * docker volume @@ 하면 백업용 파일이 만들어짐?
        ex) 로그 저장 파일

데이터 퍼시스턴시(data persistency)
- 데이터를 옮기는 작업 대신 처음부터 컨테이너 외부에 데이터를 보관하고 이를 컨테이너에서 사용하는 개념

마운트
- 호스트의 파일 시스템이나 볼륨을 도커 컨테이너 내부의 특정 경로에 연결하는 작업

마운트의 종류
- 바인드 마운트
    - 호스트 머신의 디렉토리를 컨테이너 내부에 마운트하여 데이터를 실시간으로 동기화하는 방식   
    - 주로 개발 중인 소스 코드나 파일을 컨테이너와 컴퓨터 간에 실시간으로 공유하고 변경 내용을 바로 확인할 때 사용
    - 개발 작업을 편리하게 하고 컨테이너와 호스트 간에 데이터를 손쉽게 주고받을 수 있음

            보통 "마운트"라고 말하는 건, "바인드 마운트" 이다.

- 볼륨 마운트
    - 볼륨은 컨테이너에 디스크 형태로 마운트되어 안전하게 데이터를 저장하고 공유하는 방식
    - 데이터베이스나 중요한 설정 파일과 같은 컨테이너 안의 파일을 안전하게 보관하고 다른 컨테이너와 공유할 때 사용
    - 컨테이너 간 데이터를 안전하게 공유하고 영속성을 확보

바인드 마운트 실습
- 호스트 머신에 디렉토리 생성 후 컨테이너의 내부 경로와 마운트 및 초기 화면 변경
    
```docker
# 홈 디렉토리 아래 마운트 할 디렉토리 생성
mkdir ~/apa_folder

# 디렉토리와 마운트하는 아파치 컨테이너 생성
# '~/apa_folder' 디렉토리를 아파치 컨테이너의 '/usr/local/apache2/htdocs' 경로로 연결
docker run --name apa000ex20 -d -p 8090:80 -v ~/apa_folder:/usr/local/apache2/htdocs httpd
```

        -v: 마운트 옵션

웹 브라우저에서 로컬호스트와 지정된 포트 번호로 접속하여 초기 화면 확인
- http://localhost:8090
- 아무 파일도 없는 경우 "It works"와 같은 기본 초기 화면이 출력되지만, 폴더가 마운트되면 해당 폴더 내의 파일 및 디렉토리 목록이 "Index of /" 초기 화면에 표시

<img width="353" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/ddf20877-ea2f-4e17-adca-da8ab390fad4">

        * 마운트된 폴더에 아무 파일도 없으면 "Index of/" 가 뜸

마운트된 디렉토리 경로에 HTML 파일을 넣어 초기 화면을 실시간으로 변경

```docker
# nano 텍스트 에디터를 사용 '~/apa_folder' 디렉토리에 새로운 HTML 파일 생성
nano ~/apa_folder/index.html
```

html 파일 작성 후 저장

```html
<html>
    <meta charset="utf-8"/>
    <body>
        <div> 안녕하세요! </div>
    </body>
</html>
```

이후 웹 브라우저에서 로컬호스트와 지정된 포트 번호로 접속하여 업데이트된 초기 화면을 확인

<img width="344" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/8e103d11-979a-4de3-986a-7d723d72c0d8">

        호스트(리눅스)에서 바인드 마운트된 파일을 수정하자 컨테이너속 파일 또한 업데이트 되었음을 알 수 있다.

<img width="447" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/5428d8a2-884b-480f-b08f-cd15c4b60db0">

        ↑ docker exec -it apa000ex20 bash 를 통해 컨테이너속 리눅스 shell에서 확인한 htdocs/index.html

볼륨 마운트 실습
- 볼륨 생성 후 컨테이너의 내부 경로와 마운트 및 초기 화면 변경

```shell
# 볼륨 생성
docker volume create apa000vol1

# 볼륨을 마운트하는 아파치 컨테이너를 생성
docker run --name apa000ex21 -d -p 8091:80 -v apa000vol1:/usr/local/apache2/htdocs httpd
```

<img width="715" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c145c65e-5953-445d-a036-c6f8886051b5">
 

```shell
# 볼륨 상세 정보 확인 -> 터미널 화면 확인하기
docker volume inspect apa000vol1
```

<img width="597" alt="Pasted Graphic" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/ff66226d-4f14-40e8-985b-3e0b36c111fd">


```shell
# 컨테이너 상세 정보 확인 -> 터미널 화면 확인하기
docker container inspect apa000ex21

```

<img width="559" alt="Pasted Graphic 1" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/b57b339e-2fdd-488c-bf15-5e06d15a81a3">


<img width="338" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/2f1c99b2-7c08-48ad-8614-8daa4b489983">


        여기서 만들어진 볼륨 파일을 보려하면 접근권한이 없음음 알 수 있다.
        ls -al 명령어로 확인해보면 권한을 확인 해 볼 수 있음

        이러고 8091 포트로 접속을 해보면 아까 바인드와는 다르게 정상적으로 "it work" 가 뜬다 -> 이거는 볼륨에 그대로 복사해 가기 때문에 있대

        그리고 호스트의 볼륨 디렉토리에서 직접 수정을 해야한다.

```shell
# 볼륨 경로에서 'index.html' 파일 변경
nano /var/lib/docker/volumes/apa000vol1/_data/index.html
```

<img width="563" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/920dc3e0-f30f-4f69-b37e-8e07d6171daf">


# 4. 도커 이미지 생성

### 컨테이너에서 이미지를 추출해 생성 실습
        이미지 만드는 방법1

```shell
# 아파치 컨테이너 생성
docker run --name apa000ex22 -d -p 8092:80 httpd

# 컨테이너에서 현재 실행 중인 상태를 그대로 새로운 이미지로 생성
# docker commit [컨테이너_이름] [새로운_이미지_이름]
docker commit apa000ex22 ex22_original1

# 이미지 목록 확인
docker image ls
```

        * commit = 이미 있는 컨테이너기반으로 이미지 생성

<img width="641" alt="Pasted Graphic 3" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/8b7204f9-0449-403f-8b9a-0c8d27e2d420">


### Dockerfile
        이미지 만드는 방법2

- 도커 이미지를 빌드하기 위한 텍스트 기반의 스크립트 또는 설정 파일
- docker 스크립트를 재료 폴더에 컨테이너에 넣을 파일과 함께 두어 이미지 생성
- docker build -t ex22_original2 [재료_폴더_경로]

<img width="547" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/340f3496-8418-4884-b5fb-6f80def9eaf9">

##### Dockerfile로 이미지 생성 실습

```shell
# Dockerfile을 홈 디렉토리 아래 'apa_folder' 디렉토리에 생성
nano ~/apa_folder/dockerfile
```

```docker
FROM httpd
COPY index.html /usr/local/apache2/htdocs/
# /tmp 디렉토리를 생성하고 권한을 설정합니다.
RUN mkdir -p /tmp && chmod 1777 /tmp
# 패키지 관리자를 업데이트하고 패키지를 설치합니다. -> 컨테이너 안의 리눅스에서 설치
RUN apt-get update && \
    apt-get install -y apt-utils
```

        1777 - 특수권한

```shell
# Dockerfile로 이미지 생성
# docker build -t [이미지_이름_지정] [재료_폴더_경로]
docker build -t ex22_original2 ~/apa_folder
```

```shell
# 이미지 목록 확인
docker image ls
```

```shell
# 생성 이미지로 컨테이너 생성
docker run --name ex22 -d -p 8099:80 ex22_original2
```

** 아파치 컨테이너속 shell 들어가는 법 **

```shell
docker exec -it [컨테이너명] bash
```
        -it: interactive terminal 의 약자, 해당 명령어를 추가하면 실행중인 도커에 접근해서 입력한 명령어를 실행하고 그 상태를 유지시켜 주는 역할이라고 한다.

컨테이너 개조
- 컨테이너를 실행 중인 상태에서 컨테이너 내부의 파일 시스템, 환경 변수, 네트워크 설정 등을 변경하는 작업

컨테이너를 개조하는 방법
- 마운트와 파일 복사를 이용
- 컨테이너에서 리눅스 명령어 실행

컨테이너에서 명령어를 실행하려면 셸이 필요
- 컨테이너는 가장 일반적으로 사용되는 셸인 'bash'가 설치 되어 있음
- 컨테이너를 아무 설정 없이 실행하면 bash가 동작하지 않는 상태로 실행되기 때문에 bash를 실행해 명령을 입력받을 수 있는 상태로 만들어야 함
- 'bash'를 실행하려면 '/bin/bash' 인자를 전달해야 함 -> bash만 넣어도 됨
- docker run 또는 docker exec 커맨드와 함께 사용 

docker exec
- 컨테이너 속에서 명령어를 실행하는 커맨드
- 'bash' 없이 어느 정도 명령을 직접 전달할 수는 있지만 초기 설정이 없어 동작하지 않는 경우도 있기 때문에 기본적으로는 셸을 통해 명령을 실행
- docker exec 옵션 [컨테이너_이름] /bin/bash

docker run
- 컨테이너에 들어있는 소프트웨어를 실행하는 대신 'bash'가 실행되 컨테이너는 실행 중이나 소프트웨어는 실행 중이 아닌 상태가 됨
- 'bash'를 사용한 컨테이너 조작이 끝나고 나면 다시 docker start 커맨드로 컨테이너를 재시작해야함
- docker run (옵션) [이미지_이름] /bin/bash

도커 엔진을 통해야 하는 명령
- 도커 엔진의 시작/종료
- 네트워크, 디스크 설정, 실행중인 컨테이너 목록 확인 등 컨테이너 전체에 대한 관리 작업
​​​​​​
컨테이너 내부에서 실행하는 명령
- 컨테이너 속 새로운 소프트웨어 추가
- 컨테이너 속 소프트웨어의 실행/종료/설정 변경
- 컨테이너 안과 밖의 파일 복사/이동/삭제 작업


시험 예시 - 네트워크를 만들어서 두개의 컨테이너에 연결하는 명령어를 써라


