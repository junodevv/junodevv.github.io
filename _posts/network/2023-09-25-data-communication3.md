---
title: "[Network] 3. 데이터 링크 계층 1"
category: Network
tags: Network data-communication
---
데이터 통신 강의 3주차 정리 | 데이터 링크 계층 1

-----

### 3.1 데이터링크 계층 개요

#### 데이터링크 계층
> - "<b class="text-red">노드와 노드간의 오류가 없는 데이터 전송</b>"을 하기위한 목표로 전송규격을 정의
> - 상위계층(네트워크계층)에서 오루없는 물리계층 보이도록하는 역할을 함

<img width="488" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/259bdc0d-16c5-4344-93f1-fa35da2d1f19">

        * hop-to-hop == node-to-node

#### 데이터링크 계층의 핵심기능 6가지
> 1. 오류제어: 전송장애가 발생하면 수신노드에서 <b class="text-red">오류를 탐지, 복구</b>하는 기능
> 2. 흐름제어: 수신처리 속도를 고려하여 수신기가 송신기의 <b class="text-red">전송속도를 제어</b>하는 기능
> 3. 프레임(Frame)관리: <b class="text-red">프레임</b>(전송단위)형식으로 전송할 데이터를 <b class="text-red">생성/관리</b>하는 기능
> 4. 물리주소 지정: 송신자와 수신자의 물리주소를 헤더에 지정
> 5. 접근제어: 프레임을 전송하기 위해 <b class="text-red">물리계층으로 접근</b>하는 제어 기능
>
        * 충돌이 발생하지 않게 하기위해 -> 1. 시간조절방법, 2. 트레일 사용여부확인
> 6. 링크제어/관리: 링크를 생성/관리/삭제 하는 기능

### 3.2 오류제어

#### 오류의 원인
> - 물리계층의 전송장애로 오류발생(신호감쇄, 왜곡, 잡음, 신호간섭 등)
> - 송신노드 또는 수신노드의 장애(고장)
> - 전송매체의 절단, 파괴 등의 장애
> - 저속 전송매체(ex UTP1)로 데이터를 빠른속도로 무리하게 송신하는 경우 (== 전송매체의 성능에 적합하지 않게 사용하는 경우)

#### 오류유형
> - single bit error: 1비트만 에러
> - burst errors: 여러비트 에러

#### 오류검출방법
> - Parity비트 검출법
> - 2차원 parity비트 검출법
> - Checksums 검출법
> - Internet Checksums 검출법
> - CRC(Cycle Redundancy Checks)

#### 오류복구방법
> - 전방향 오류정정,FEC (수신측에서 알아서 복구)
>   - <b class="text-red">Hamming 코드기법</b>
>   - binary convolutional codes
>   - Reed-Solomon codes
>   - ...
><br><br>
> - 역방향 오류정정,REC (송신측에 에러응답프레임을 보내서 다시받음)
>   - NAK: 재전송 요청 후 다시 수신해서 정정
>   - Timeout: 응답을 못 받으면 다시 전송해 정정

### 3.3 오류검출

        1. Parity비트 검출법
        2. 2차원 parity비트 검출법
        3. Checksums 검출법
        4. Internet Checksums 검출법
        5. CRC(Cycle Redundancy Checks)

#### Parity bit 검출법
        에러가 별로 안나는 환경에서 사용

> - 데이터에서 1의 개수를 홀수(또는 짝수)로 맞추어 전송, 수신 후에 개수를 확인하는 기법
>
        - even parity bit
        - add parity bit
>
        ex1) 데이터 "1010011" - odd Parity일때 => parity bit == 1 -> 따라서, 1010011'1'
        ex2) ASCII 문자 'B'를 전송하는 경우 odd Parity bit =?
            'B' == 0x42 == 100 0010 -> 따라서, 1000010'1'
>
> <img width="390" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/970fc3b4-246d-4103-8849-8737b096bf20">
>
> - 문제점: 2비트 이상 오류(burst error)가 나면 검출 능력이 떨어짐

#### 2차원 Parity bit 검출법
        1차원보다는 좀더 견고한 검출법

<img width="181" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/baa82a72-742a-4190-97c4-7fee83295a19">
<img width="296" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/f487f5b7-bca3-4719-9e20-eacacf3dc6b3">
<img width="246" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/bdc9f207-0efa-4667-b794-cd923a0e3563">

#### Checksum (1 byte) 검출법
> - 데이터를 1 Byte(블록)단위로 XOR연산을 하여 Checksum을 생성후 전송﹒수신후에 받은 데이터들을 동일하게 XOR연산하여 만든것과 Checksum Byte를 비교/확인하는 방법

<img width="660" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/4719a03a-6dc8-48e8-bf28-00ff1f697710">

#### Internet Checksum 검출법 (2Byte단위)
        - 3,4계층에서 사용    

> - ones-complement addition 방식으로 계산
> - IP, TCP, UDP 에서 사용하는 오류검출방식 (-> 그만큼 성능이 좋음)
> - 수신측에서 Checksum했을 때 <b class="text-red">"FFFF"</b> 가 나오면 오류없음

<img width="632" alt="Pasted Graphic 5" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/7e01df91-3435-4949-8242-794362cd7939">
<img width="244" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/2727196e-a222-49d9-8957-bce3b7708941">

#### CRC (Cyclic Redundancy Check) 검출법

> - 다항식의 원리를 이용한 방법
> - 수신 후 데이터 부분을 나누어 CRC를 생성하고, CRC필드값과 비교하여 동일하면 오류없음 틀리면 오류발생
> - 검출능력이 좋아서 대부분의 LAN(Ethernet)에서 사용된다.
> - CRC-16, CRC-32, CRC-12

<img width="462" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/59b9f202-42b2-42c3-bd34-0981e519dabc">

### 3.4 오류복구/정정

#### 역방향 오류정정
                재전송으로 오류를 해결

> 1. Stop-and-wait ARQ: 프레임 1개를 전송할 때마다, ACK(응답프레임)를 기다림
                장점: 정확하다. | 단점: 속도가 늦다.
> 2. GO-back-N ARQ: 프레임을 계속 보내다가, NAK(부정응답프레임)가 온 이후의 모든 프레임을 재전송
                단점: 중복으로 인한 효율성 저하
> 3. Selective-rejeck ARQ: 프레임을 계속 보내다가, NAK(부정응답프레임)가 온 프레임만 재전송

#### 전방향 오류정정
                재전송하지 않고 오류를 해결하는 방법
> (전송 프레임내에 부가정보를 추가하여) 수신된 프레임만 가지고 오류정정을 할 수 있는 방식
>
> <b class="text-red">Hamming codes</b>

<img width="473" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/71ac206f-5d8d-4ff9-b299-458bab5195ca">

#### 해밍코드(Hamming code)를 이용한 오류정정법

> - 해밍코드로 <b class="text-red">parity bit를 생성</b>하는 과정

<img width="661" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/16d7658c-ec2f-4b90-9675-9c7cbcafe641">

                - 총 비트 위치 == 12
                - Parity Position == 1,2,4,8
                - 원본 비트 Position == 3,5,6,7,9,10,11,12

> - 해밍코드로 <b class="text-red">오류를 검출하고 오류를 정정</b>하는 과정

<img width="549" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/cc82c3dc-c332-4ca5-8e4b-da737ee73b8a">

> - 모든 Parity가 0 이면 오류가 없는것 ( 그렇지 않으면 오류가 있는 것)
> - 위 사진에선 결과가 "0101"이므로 오류가 있는것
> - 오류있는 결과 "0101"을 10진수로 변환하면 '5' 가됨 따라서, 5번째 비트에서 오류가 발생한것. -> 5번째 비트를 1에서 0으로 바꿔주면 오류가 정정된다.

### 3.5 흐름제어(Flow Control)

- 원인
    - 송신노드와 수신노드의 처리성능(속도차)이 다름
    - 송신속도가 너무 빨라서 수신노드가 처리하기 어려움
    - 수신버퍼링이 늦어지면 프레임이 분실될 수 있음

- 해결방법
    - 수신노드가 송신노드의 송신시점을 제어 

#### Feedback-based Flow Control

        1. Stop & Wait 흐름제어 기법
        2. Sliding Windows 흐름제어 기법

- Stop & Wait 흐름제어 기법
    - 오류제어(Stop & WaitARQ)와 같은방식
                
            장점: 단순함
            단점: 성능 ↓

    - 전송 프레임당 응답을 수신함
    - <b class="text-red">1</b>: **프레임 전송시간(transmission)**
            
            = 프레임의 끝 부분(모든부분)이 수신노드까지 도착하는 시간

    - <b class="text-blue">a</b>: **프레임 전송** <b class="text-blue">지연</b>**시간(Propagantion time)**

            = 프레임의 맨 앞부분이 수신노드까지 도착하는 시간( ex육상경기 )

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/48f971fa-8ccc-48c4-bd3b-f18546ea13c5">

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c9e0cf4f-ecc0-4ccf-a89d-0f0576456518">

- Sliding Windows 흐름제어 기법
    - 정해진 양(Window)만큼 보내고 응답을 기다리는 방식. 즉 정해진 양 이상으로는 보내지 않음
            ex) window가 4면 4개까지는 마음대로 보내고 나서 응답을 기다림

    - Window size: 응답없이 보낼 수 있는 프레임의 최대 개수

            ex) Window size = 4, 송신노드가 4개의 프레임을 최대로 보낼 수 있음

    - Window는 n-bit sequence counter로 구현, 윈도우 범위: 0 ~ 2ⁿ-1

            n-bit sequence counter: n비트로 구성된 매 입력마다 정해진 순서로 상태가 주기적으로 변하는 레지스터
            
            ex) 3-bit = 0~7, 6-bit = 0~63, 8-bit = 0~255
    - Window 동작원리
<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/cb372324-7c67-4044-8a8c-edd14e8b55e7">
    - 사례

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c7ac6eba-ae5e-4ce5-8dd5-47f50304c9e9">

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/a180a511-d0ab-4507-951a-245217dbcc7d">

- Piggybacking(피기배킹) 기법
        
        A <-> B 서로 데이터를 보낸다면, 각자 데이터를 보낼 떄 응답 신호를 실어 보내는것

    - 전이중(Ful deplex) 전송방식에서 전송 효율을 높이기 위한 방법
    - 데이터 프레임(I) 내부에 응답(ACK or NAK)을 포함시켜 보내는 기법
            - 두번걸쳐 보내는 프레임을 한번으로 줄일 수 있음

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/84da5327-7695-4b08-9105-7bf49121a9a5">

### 3.6 프레임 생성/관리

- 프레임(Frame)이란
    - 데이터링크 계층에서 전송하는 데이터의 단위
    - 프레임의종류
        - 데이터프레임
        - 응답프레임(ACK or NAK)
                ACKnowledgement or Negative AcKnowledgement

- 프레임 생성 방법
    - 네트워크 계층에서 내려운 bit stream을 프레임으로 생성
    - 구조: <b class="text-blue">Header + Payload(데이터) + Trailer</b>
    - 생성시 4가지 고려사항
        - Byte count
        - Character stuffing
        - Bit stuffing
        - 물리계층 코딩침해문제

### 3.7 프레임

#### 문자프레임(Character Frame)
- 데이터의 내용이 문자로 구성됨(= 8비트 단위의 고정크기), ASCII 코드로 정의
- 프레임의 시작(DLE, STX)과 끝(DLE, ETX)를 추가하여 프레임의 경계를 구분함

<img width="408" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/5cbea97f-8014-4ecb-a1df-dcc0c09236ab">
<img width="92" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c48ec0d4-9e3b-47fe-aad0-6dc3fe57ddd5">


#### 문자 스터핑(Character stuffing)

- 문자프레임 + <b class="text-red">"제어용 문자"</b>를 추가

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/37b53208-aa49-4b32-ac54-0773ea87bae5">

#### 비트프레임(Bit Frame)
- 프레임의 시작과 끝에 특정 비트패턴인 <b class="text-red">플래그(Flag,01111110)</b>를 추가해서 시작과 끝을 구별함

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/39937564-5eb4-46a9-85db-779d350442b2">

#### 비트 스터핑(Bit stuffing)
- 비트프레임 + <b class="text-red">"제어용 비트"</b> 추가

<img width="500" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/82878977-c98e-418e-9f00-aadffbec6434">

### 3.8 주소 및 링크 제어 관리

#### 점대점 링크, Point-to-Point 연결
- 두 노드간의 구성이 <b class="text-red">1:1</b>로 전송매체를 <b class="text-red">전용</b>으로 사용, <b class="text-red">주소 불필요</b>

#### 방송용 링크, Broadcasting
- 두 노드간의 구성이 <b class="text-blue">1:N</b>으로 전송매체를 <b class="text-blue">공용</b>으로 사용, 매체에 있는 모든 노드가 수신노드

#### 멀티드롭 링크, Multi drop 연결
- 노드간의 구성이 <b class="text-blue">1:N</b>으로 전송매체를 <b class="text-blue">공용</b>으로 사용하여 주소가 불필요
- 링크제어필요 (= Bus 제어)


<b class="text-red"></b>
<b class="text-blue"></b>