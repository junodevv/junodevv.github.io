---
title: "[Network] 9. 네트워크 계층 3"
category: Network
tags: Network data-communication
---

데이터 통신 강의 9주차 정리 | 네트워크 계층 3

-----

# 네트워크 계층

## 인터넷계층(IP = Internet Protocol)

### Internet 개요
- 인터네트워킹의 줄임말

![image](https://github.com/junodevv/junodevv.github.io/assets/126752196/3f974276-1e0d-4a38-8800-cf8a7d8c4bc9)

### IP계층에서의 프로토콜
- **IP[Internet Protocol]** (IPv4, IPv6)
    - IP계층의 핵심 프로토콜, datagram 전송방식을 지원(Connectionless service)
    - 실제 전송할 메시지(전송계층에서 송신요청하는 데이터)를 패킷으로 변환하여 전송

- **라우팅 프로토콜** (<b class="text-red">RIP</b>, <b class="text-red">IGRP</b>, <b class="text-blue">OSPF</b> 등) <b class="text-red">Disdance Vector 라우팅알고리즘</b> / <b class="text-blue">Link state 라우팅알고리즘</b>
    - 효율적 라우팅을 위해 라우팅정보(delay, topology, failure)를 송수신하는 프로토콜

- **주소변환 프로토콜** (ARP, RARP)
- **네트워크 관리용 프로토콜** (ICMP)
    - IP프로토콜을 이용해서 네트워크의 장애유무, 트래픽, 혼잡정보등을 송수신하는 프로토콜
- **멀티캐스트 그룹관리 프로토콜** (IGMP)

<img width="600" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/95148866-8970-4733-b346-0ee8e7afa037">

## IPv4 Protocol
### IPv4에서의 중요한 이슈 
`주소`, `단편화,재조립`, `생명주기`, `에러﹒흐름제어`, `라우팅`
- 소스 스테이션에서 여러 네트워크를 거켜 목적지 스테이션으로 메시지를 전송하려면 필요한 것들
    - **Addressing**: 각 노드(라우터포함)를 식별하기 위해 주소가 필요
    - **Fragmentation & Reassembly**: 전송테이터를 여러 개의 패킷으로 분리하고 조립
    - **Datagram lifetime**: 패킷의 무한Loop 문제와 패킷트래픽을 효율적으로 관리하기 위한 생명주기
    - **Error control & Flow control**: 패킷의 전송이 보장되지 않는 Connectionless 서비스에서 최소한의 안전기능
    - **Routing**: 패킷의 효율적 전송을 실현하기 위한 라우팅

### IPv4 - Addressing
- 5개의 Class로 분류
- IP주소 = Network주소 + Host주소
- 표기법: 10진수(ex. 124.0.12.6)
- A클래스(네트워크식별 갯수:2⁷(=128), 호스트식별 갯수:2²⁴)
- B클래스(네트워크식별 갯수:2¹⁴, 호스트식별 갯수:2¹⁶)

<img width="600" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/74c61180-037e-4760-95b3-862b6a315509">


<b class="text-red"></b>
<b class="text-blue"></b>