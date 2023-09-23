---
title: "[Network] 1. 데이터 통신 / 네트워크의 개요"
category: Network
tags: Data-communication network
---
데이터 통신 강의 1주차 정리 | 데이터 통신과 네트워크의 개요

-----

# 1. 통신 / 네트워크 개요

### 1.1 통신의 개념 / 목표

#### - 통신의 목표
        한 지점에서 지정된 다른 지점으로 데이터를 정확히 전송하는 것

### 1.2 데이터 통신
        두대 이상의 컴퓨터가 전송매체를 통해서 데이터를 송수신 하는 기술

#### - 데이터 통신 모델
        사용자가 다른 곳으로 메시지 "m"을  전송하고자 할 때
        
<img width="695" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c3187fd8-b879-4fb7-83cb-15e8b794f648">

①,② "m" 이라는 정보(soruce)를 디지털 시그널(신호)로 송신

③ Trans-mitter(ex 모뎀, 랜카드, 랜선)을 통해 아날로그 시그널 형태로 변환

④ Trans-mission system을 통해 아날로그 시그널을 수신

⑤ Receiver 에서 수신받은 아날로그 시그널을 다시 디지털 시그널로 변환

⑥ "m" 수신완료

### 1.3 네트워크(Networks)
        통신장비(ex 스위치, 라우터 등)로 여러 대의 컴퓨터가 그물같은 통신망을 구성하는 구조
        
        *그물같은 통신망 == communication network

#### LAN(Local Area Network)
        - Topology(위상, 네트워크요소(노드,링크)가 물리적으로 연결된 형태﹒방식)
        
            > Star(성형) - 전송로 중앙에 통신장비(허브/스위치 등)가 배치되어 연결된 형태
                장점: 복잡도를 줄여준다.    |   단점: 중앙 통신장비가 고장나면 다 먹통됨.

            > Bus(버스형) - 노드간의 공유된 전송로로 서로 연결된 형태
                단점: 버스를 공유하기 때문에 한 컴퓨터가 버스로 전송중일때 다른 컴퓨터는 사용불가(1차선)

            > Tree(트리형) - 최상위 루트노드에서부터 계층적 구조로 연결된 형태

            > Ring(링형) - 반지 같은 구조, 데이터 전송방향이 한쪽으로 정해져 있음
                단점: ① 12시방향에서 11시 방향으로 보낼때 비효율적임 ② 중간에 하나가 고장나면 먹통
            
            > Mesh(메쉬형) - 노드들이 상호간 모두 연결된 상태
                장점: 안정적이다    |   단점: 고비용, 복잡 (많은 랜선, 랜카드)
            
<img width="590" alt="Pasted Graphic" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/66f197c2-84cc-4174-8d7d-eca933f23806">

#### WAN(Wide Area Network == internet) 
        - LAN 보다 넓은 범위, LAN들이 모여서 WAN이 된다.

<img width="762" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e79a64f9-9b92-4208-b28f-b85e00015d5c">

#### PAN(Personal Area Network), BAN(Body Area Network)
        - 거리기준: PAN(10~100m), BAN(max 3m)

<p class="text-gray">*참고: 크기에 따른 네트워크 분류</p>
<img width="156" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/d9f161f7-0b98-4f2c-b221-07d05ae8c693">

### 1.4 프로토콜 구조(Protocol Architecture)

#### Protocol의 정의와 의미
        - 데이터를 정확히 주고 받기 위해 계층화시키고, peer 계층간에 정의된 전송규약
        *peer계층: 송신기(Source)와 수신기(distination)를 말한다.
        
        - 송신기와 수신기는 반드시 프로토콜을 지켜야 데이터를 정확히 송수신 할 수 있다(상호운용성)

#### Protocol의 계층적 모듈구조, 모듈화
        - 복잡한 시스템을 모듈화

        - 수직적 형태로 계층화

        - 따라서, 각 계층의 모듈이 변경되어도 다른 계층에는 영향을 주지 않는 구조

        - 상위 계층에서 필요한 기능을 하위 계층에서 함수(Primitive) 또는 인터페이스로 서비스를 제공(즉, 특정모듈이 다른 모듈에 서비스를 제공)

        - 독립적인 고유기능을 수행하는 모듈들이 서로 연결되어 동작함

#### Protocol의 3대 요소
        - Syntax(형식) - 송수신기 간의 "전송데이터의 포맷"을 정의 해야함

        - Semantic(의미) - 상호 협력을 위한 "제어정보"를 정의 해야함(ex 오류처리)

        - Timing(타이밍) - 전송데이터를 주고 받을 때 "속도와 전송절차"를 정의해야 함

#### Protocol 설계시 고려사항 (9가지)
> 1. <b class="text-red">주소설정</b> - 네트워크상 송신﹒수신 호스트를 구별하기 위한 <b class="text-red">식별자</b>
>
> 2. <b class="text-red">오류제어</b> - 신호 감쇄/왜곡으로 인한 오류발생 -> 오류탐지 -> 오류처리(복구)
>
> 3. <b class="text-red">흐름제어</b> - 수신호스트의 버퍼처리속도가 늦을 경우 송신호스트의 속도를 제어
>
> 4. <b class="text-red">연결제어</b> - 데이터를 전송하기 전 상호 송수신 가능한 상태로 설정/해제/관리 제어
>
> 5. <b class="text-red">순서제어</b> - 전송데이터에 **순서**를 매겨서 메시지 분실여부 
>
> 6. <b class="text-red">전송메시지의 단편화/재조합</b> - 전송효율을 높이기위해 작은 크기로 줄여 <b class="text-red">단편화</b>하여 전송후 수신할 때 응용 프로그램에서 원래의 크기로 <b class="text-red">재조합</b>하는 과정
>
> 7. <b class="text-red">캡슐화</b> - 데이터에 제어정보를 덧붙임
>
> 8. <b class="text-red">동기화</b> - 여러시스템이 동시에 통신할 수 있는 기법
>
> 9. <b class="text-red">전송모드</b> - 단방향(only 한방향), 반이중(동시에 한방향), 전이중(양방향)

#### 서비스 프리미티브 (Service primitive)
> - 프로토콜에서 하위 계층이 상위 계층에게 제공하는 서비스
>
>   - <b class="text-red">연결형 서비스</b> - 정확
>   - <b class="text-red">비연결형 서비스</b> - 빠름(대신 정확도 낮음)

<img width="619" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/eec62c2d-c3d8-4ba8-8b0c-e7b6887e0a8b">

#### Protocol Architecture & Networks
> - 대부분 송수신과정에서의 주체 == 어플리케이션, 컴퓨터, 네트워크
> - 각 주체별 역할과 기능을 효과적으로 수행하기 위해 3개로 모듈화/계층화
>   - <b class="text-red">Application 계층</b>: 프로그램 본래의 기능과 역할
>   - <b class="text-red">Transport 계층</b>: 포트주소 역할
>   - <b class="text-red">Network access 계층</b>: Network 접속/전송 역할(이기종 네트워크)

#### 전송과정의 이해
> - PDU: Protocol Data Unit (header+body, encapsulation(캡슐화))
> - segment == transport PDU
> - packet == network PDU

<img width="442" alt="7bad6038-fb6d-4db5-92bd-b64aacb74da3" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/d805b7f2-d322-4bb6-b28b-1f20c2b4b3f1">

### 1.5 프로토콜 참조 모델
> - OSI 7계층 참조모델 -> 국제 표준기구(ISO)에서 만든 모델(정확성 좋음, 성능낮음)
> - TCP/IP 모델 -> 현실적으로 많이 사용하는 모델 (성능좋음)

<img width="454" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/43d471b9-f64d-4003-89f3-2e2fb6f0ebec">

#### OSI 7계층 참조모델

> - 1계층: 물리 계층 | Physical - 기계적, 전기적, 기능적, 절차적 특성을 정의, <b class="text-red">비트스트림을 물리적 매체를 통해 전송</b>
>
> - 2계층: 데이터 링크 계층 | Data Link - 물리적인 링크를 통해 동기화, 에러제어, 흐름제어 등을 통해 패킷을 전송
>
> - 5계층: 세션 계층 | Session - 동기화 유지, 데이터 교환 관리, 전송계층에서 설정된 종단 간 논리적 연결에 추가 서비스를 제공
>
> - 6계층: 표현 계층 | presentation - 데이터의 구조를 하나의 통일된 형식으로 표현, 데이터의 압축 암호화 기능 수행
>
> - 7계층: 응용프로그램 계층 | Application - 유저, <b class="text-red">응용프로그램 간의 데이터 교환</b>을 가능케 하는 계층
>
>       ex) HTTP, FTP, 터미널서비스, 메일프로그램, 디렉토리서비스
<b class="text-red"></b>