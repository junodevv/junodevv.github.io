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
>       *반송파: 신호를 보다 멀리 보내기 위해 사용하는 고주파신호, 이 신호를 섞어서 신호를 변조하여 보낸다.

#### Fiber (Optics) Cable (광케이블)
<b class="text-red"></b>