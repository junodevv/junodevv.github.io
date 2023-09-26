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

<b class="text-red"></b>
<b class="text-blue"></b>