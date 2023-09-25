---
title: "[Network] 2. 물리계층"
category: Network
tags: network data-communication
---
데이터 통신 강의 2주차 정리 | 물리계층

-----

# 2. 물리계층

### 2.1 물리계층

#### 물리계층의 정의/역할
> - 데이터를 전송하기 위한 <b class="text-red">전송매체의 기계적 규격을 정의, 전기적 신호의 전송규격을 정의</b>

#### 전송매체
> - (데이터를 전송하는데) 전류나 전압을 변화시키는 효과를 이용
> - 전류나 전압이 흐르는 매개체
> - 유선(전선, 구리선) vs 무선(공기매체)

### 2.2 유선전송매체

#### Twisted pair (TP선)
> - 두개의 구리도선을 꼬아서 엮은 전송매체
> - 외부 잡음 감소 효과
> - STP: Shielded TP | UTP: Unshielded TP
> - 전화선, LAN선, 신용카드결제기, 건물내부통신선 ...
> - 적은비용, 적은 데이터 전송률, 짧은 범위, 쉬운 설치
> - UTP1 ~ UTP8까지 존재
>       UTP1(전화선) -> UTP5(가장 대중적, 100Mbps~1Gbps) -> UTP8(40Gbps)

<img width="186" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c11c919b-a247-4df7-8396-920b18aa088d">


#### Coaxial Cable (동축케이블)
> - <b class="text-red">장거리 전송용</b>으로 성능이 좋은 전송매체
> - TV선, 장거리 다회선 전화선, LAN/MAN
> - <b class="text-red">잡음에 강하고 좋은 대역폭</b>을 가짐
> - 높은 주파수 범위의 반송파 지원
>
>       *반송파: 신호를 보다 멀리 보내기 위해 사용하는 고주파신호, 이 신호를 섞어서 신호를 변조하여 보낸다.

#### Fiber (Optics) Cable (광케이블)
> - 광섬유(유리or플라스틱)를 이용하여,<b class="text-red">빛에 데이터를 실어서 전송하는 전송매체</b>
> - 빛의 특성: 전자기적인 에너지 형태, 진공상태에서 전송속도(30만 km/s, 30cm/1ns)
> - 공기(물)와 같은 밀도가 높은 곳을 통과할 때는 속도가 감소됨
> - 장점: 잡음에 대한 저항력, 낮은 신호감쇄, 높은 대역폭 | 단점: 고비용, 설치와 관리 어렵
>
>       *대역폭: 한번에 보낼 수 있는 데이터의 크기
> - 전파방식: Single mode vs Multi mode(Step index, Graded index)

<img width="668" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e4dda130-369f-48c5-b722-bc795c7c8199">

### 2.3 무선전송매체

#### 무선 전송매체의 전파 유형
> - 지면 전파: 대기권의 낮은 부분으로 전자기파를 전파하는 방식<b class="text-red">(below 2 MHz)</b>
>
><img width="291" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/1fe736fb-2fb2-4af6-9f42-5f01b3431bdb">
>
> - 공중 전파: 안테나로 전파하거나, <b class="text-red">전리층에서 반사(굴절)하는 방식</b>으로 전파<b class="text-red">(2 to 30 MHz)</b>
>
><img width="335" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c7297655-5169-467a-be4b-81d537ad054a">
>
> - 가시선 전파: 안테나간 높은 주파수로 전파, 전선연결이 불가능한 경우 안테나가 서로 보이는 경우 사용가능<b class="text-red">(above 30 MHz)</b>
>
> <img width="322" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/22377562-eb99-4d31-9089-62fc1b0b6d9b">

#### Radio wave
> - 마이크로파에 비해 낮은 대역
> - 고체(벽), 진공, 대기를 모두 통과
> - <b class="text-red">3KHz ~ 300MHz, 전방향 전파(다향성)</b>
> - 같은 주파수를 사용하여 전송하는 안테나에 방해 받을 수 있음
> - 라디오, TV, 호출기, broadcasting(1:N)에서 활용됨
>
>       *broadcasting: 송신 호스트가 전송한 데이터가 네트워크에 연결된 모든 호스트에 전송되는 방식

<img width="557" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/edac5430-aa42-45c6-bee9-4403c3130195">

#### Micro wave
        ex) WIFI, Bluetooth, 전자레인지

> - <b class="text-red">300MHz ~ 300GHz</b>의 주파수를 갖는 전자기파
> - 단방향 전파(지향성), 가시선 전파, 벽을 통과하지 못함
> - 접시형 안테나로 전파를 집중시켜 전송할 수 있음(SNR🔺, 전파정확성🔺)
>
>       *SNR(Signal Noise Ratio): 신호대 잡음비, 잡음이 신호에 얼마나 영향을 끼지는지, 수신측에 도착한 신호의 신호전력 대 잡음전력의 비율
>
> - 지향성, 동시에 여러 전송기가 여러 수신기에서 간섭없이 전송할 수 있게 해줌
> - 지상마이크로파, 위성마이크로파

<img width="352" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/93454b2e-17ef-4bae-ba4c-e3bfe96e3ec6">

#### Infrared (적외선)
        ex) 리모컨

> - 단거리전파에 적합(ex 리모컨 <-> 가전제품, 노트북 <-> 프린터)
> - 지향성, 저가, 소형, 설치용이
> - 초고주파 주파수대역(300GHz ~ 400THz)
> - 고체 물체를 통과하지 못함 (= 단점이자 장점(보안성,간섭))

<img width="530" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/95faba64-1098-4cd3-98db-f755a6b1ba13">

### 2.4 전송매체

#### 주파수대에 따른 전파구간과 전송매체
        유선통신 & 무선통신용 주파수 스펙트럼
        * 주파수의 단위: Hz(헤르즈)
        KHz(=1,000Hz), MHz(=1,000,000Hz), GHz(1,000,000,000Hz), THz ...
        
<img width="537" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/04206bd4-7921-4e71-91c4-4afffab858f2">

### 2.5 신호전파와 데이터 전송

#### 전기적 신호와 데이터의 표현
> - 데이터 전송: 이진 데이터를 전기적 신호로 표현하고, 전파

<img width="380" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/05bbe769-d4e2-4ee5-8121-9f650afedc99">

### 2.6 신호변환기술

#### 신호와 전송
        - 신호(Signal) -> 데이터(Data)로 변환
        
#### 신호변환
        - 아날로그 신호 <-> 아날로그 데이터
        - 아날로그 신호 <-> 디지털 데이터
        - 디지털 신호 <-> 아날로그 데이터
        - 디지털 신호 <-> 디지털 데이터

#### 전송
        - 아날로그 전송
        - 디지털 전송

<img width="391" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/65322c2f-0dec-423d-9960-371c8a791097">


### 2.7 신호(Signal)

#### 개요
> - 신호는 송신기에서 생성되며, 전송매체를 통해서 전송됨
> - 신호를 <b class="text-red">시간함수(Time domain)</b>와 <b class="text-red">주파수 함수(Frequency doamin)</b>로 표현

#### Time domain(시간함수)

> - Analog signal -> continuous(연속적, 주기성)
> - Digital signal -> Discrete(비연속적, 비주기성)
> - 신호는 같은 패턴이 주기적으로 반복되는 주기(T)
> - peak amplitude(A), frequency(F) Phase(?)
> - <b class="text-red">주파수(F): 1초당 주기의 반복횟수, 단위 Hz</b>
>
        ex) 1초당 주기가 5번 반복됨 == 5Hz
            1초당 주기가 1000번 == 1KHz
            1MHz == 1초당 주기가 1,000,000번

<img width="335" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/5962379b-2317-49e6-a88a-74f88e6ffffd">

#### 신호
> - 진폭: 신호의 높이.(전압 Volt, 전류 Amperes, 전력 Watts)
>
>       ex) 3V, 0.7W, 1.5mA(밀리암페어)
> - 주기: 반복되는 패턴(cycle)을 완성하는데 걸리는 시간(단위: 초)
>
>       ex) 1s, 100ms(0.1초), 300µs(마이크로세컨드 == 1/1,000,000초)
> - 주파수: 1초당 주기의 반복횟수(단위: Hz)
>
>       ex) 1Hz, 3Hz, 1KHz(== 1000Hz), 310MHz(==310,000,000Hz)
>
> <b class="text-red">* 참고: 주파수와 주기는 역수 관계</b> -> 주파수 = 1/주기 
>
>       정현파가 2KHz의 주파수를 가지면 주기 == 1/2K == 1/2000 == 2*1/1000 == 2*milli == 2ms
>       정현파의 한 사이클이 25ms일때 주파수 == 1/25ms == 1/25*milli == 1/25*1/1000 == 1000/25 == 40Hz
>
> - 위상: 시간 0에 대한 파형의 상대적인 위치(= 동일 주파수에서 시간차로 어긋나는 각도)

<img width="441" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/632f5f73-cde0-429f-999a-70ed34c44c56">

#### <b class="text-red">주파수관점에서의 신호분석(Frequency Domain)</b>
> - 저주파 신호: 데이터를 멀리, 많이 보낼 수 없다는 한계점
> - 고주파 신호: 데이터를 많이 보낼 수 있다. ex) 반송파
> - <b class="text-red">송신기</b>: 입력신호를 반송파로 만들어 전송하는 기술이 필요 -> 저주파를 고주파에 혼합
> - <b class="text-red">수신기</b>: 수신된 반송파에서 원래신호를 분리해내는 기술필요 -> 고주파 성분을 제거(퓨리에변환함수)
>

<img width="664" alt="Pasted Graphic 2" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/fda9e61d-97d9-4406-9731-d3e650d2e6ca">

### 2.8 신호변환

#### 디지털변환(Digital Modulation)기법
> - 디지털 <b class="text-blue">데이터</b> -> 디지털 <b class="text-red">신호</b> 변환
> - RZ, NRZ, NRZI, Manchester, Differential Manchester, Biploar ...
>

<img width="613" alt="Pasted Graphic 3" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e4bb0fb5-4289-4bc4-83ec-7d0abe535d09">

> - (b) Non-Return to Zero(NRZ): 1 == high, 0 == low
> - (c) NRZ Invert (NRZI): (신호중간지점) 1 == Invert, 0 == No change
> - (d) Manchester: 1 == (신호중간지점) 1 == high->low, 0 == low->high
>
>                       (Clock 신호와 XOR연산에 맞춤)
>
> - (e) Bipolar encoding(AMI): 1 == +v -> -v 또는 -v -> +v, 0 == 0

#### 아날로그 변환(Analog Modulation) 기법
> - 디지털 데이터 -> 디지털 신호 -> 아날로그신호 변환
> - ASK (Amplitude Shift Keying): 1 == 진폭 high, 0 == 진폭 low
> - FSK (Frequency Shift Keying): 1 == 고주파, 0 == 저주파
> - PSK (Phase Shift Keying)
>   - BPSK(Binary PSK): 0, 180 degree
>   - QPSK(Quadrature PSK): 45, 135, 225, 315 degree

<img width="470" alt="Pasted Graphic 4" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/f3696a8e-9025-42d0-990a-535bc33b4e24">

#### PMC(Pulse Code Modulation) 기법
> - 아날로그신호 -> 디지털데이터 : PCM, Delta Modulation
> - 변환과정: ①샘플링 -> ②양자화 -> ③인코딩
>
>       * 샘플링: 특정주기마다 값을 추출
>       * 양자화: 추출한 (PCM)값을 레벨에 따라 레벨링하여 코드로 구분
> - 양자화 레벨🔺 -> 정밀도🔺 -> 인코딩비트수🔺 -> 전송량🔺

<img width="687" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/6175b0ee-a61d-4b5c-9b7b-761c1d316ff9">

#### 다중화(Multiplexing)
>       - "1송신기 - 전송매체 - 1수신기" 이렇게 되면 전송매체가 너무 놀아나게 된다 ->  다중화기법의 등장
>
> - 전송매체의 효율적 이용을 위해 동시에 여러신호를 전송하는 기법
> - FDM(Frequency Division Multiplexing): 주파수 분할
>
>       ex) 라디오, TV - 방송국에선 하나의 안테나로 여러 채널의 방송을 동시에 보내고 시청자는 여러 채널줄 하나의 (주파수)채널을 선택해서 시청함
>
> <img width="506" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/13110f62-415f-48c9-be19-e49028d4af29">
> - TDM(Time Division Multiplexing): 시간 분할
>
> <img width="461" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/ef334ec2-6e5c-4068-9987-fe0f33818642">

### 2.9 전송장애(전송오류)

#### 전송장애 원인
> 1. 신호감쇠(Attenuation)
>   - 전파거리에 따른 에너지(신호세기) 손실
>   - 증폭기/리피터로 신호증폭
>   - 감쇠 측정단위: dB
>
            * 디지털에서는 증폭기를 사용할수 X, 에러도 증폭됨
>
><br><br>
> 2. 왜곡(Distortion)
>   - 신호모양/형태가 변화되어 뒤틀어짐
><br><br>
> 3. 잡음(Noise)
>   - 열잡음, 유도선 잡음, 혼선, 충격잡음
><br><br>
> 4. 간섭
>   - 신호의 수신을 방해하는 에너지로 인한 장애
>
            * 이러한 신호들이 에러인지 아닌지는 디지털 계층에서 확인하게됨

<img width="355" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/efd57401-982f-40c5-bd15-3792c9f88216">


### 2.10 물리계층의 대표장비

#### 증폭기(Amplifier)
        - 감쇠된 신호크기를 증폭시켜 원래의 신호크기로 키워주는 장치
        - 감쇠 측정단위: dB
        - 아날로그 전용

#### 리피터(Repeater)
        - 수신한 신호를 다시 인코딩(재생)하여 송신하는 장치
        - 리피터로 신호재생
        - 디지털 전용

<img width="213" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/45af2f91-a8ad-46a8-9497-e692467ca372">

-----

# 끝

## reference

교수님 강의