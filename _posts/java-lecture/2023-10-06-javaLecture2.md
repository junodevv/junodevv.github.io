---
title: "[Java] 강의2 컴퓨터 구조"
category: Java
tags: java java강의 
published: false

---

자바를 더 깊게 이해하기 위한 itwill 자바 강의

-----

# 1. CPU, RAM

- CPU: 연산장치
- RAM(메모리): 저장장치

CPU가 RAM에 데이터를 ①요청하고 불러와서 ②연산을 수행하고 그결과를 다시 RAM에 ③저장한다.

# 2. 영구적 저장공간(하드디스크)

메모리는 일시적인 저장공간 -> 컴퓨터 저장시 데이터가 사라짐(휘발성)

이래서 필요한 것이 **하드디스크** 

장점: 컴퓨터가 꺼져도 데이터가 사라지지 않음(영구적 저장), 가격이 저렴함

단점: 데이터를 저장하거나 찾아오는데 시간이 오래걸림

# 3. 메모리 vs 하드디스크

- 메모리가 많을 수록 컴퓨터는 빨라질 수 있음

- CPU --요청--> RAM --요청--> 하드디스크

        * RAM에도 필요한 데이터가 없는 경우 = 페이지 폴트
        * RAM에 필요없어진 데이터를 지우고 (하드디스크에서)새로운 데이터를 가져오는 것 = 페이지 교체

# 4. CPU 레지스터(저장장치)

- 지스터는 가장빠름

- RAM의 데이터를 레지스터에 저장해서 CPU가 처리하는 것임

        * 캐싱(레지스터,RAM): 레지스터에 이미 저장되어있는 데이터라면 RAM에 요청할 필요없이 바로 불러와 쓰는것
        캐싱은 각 장치의 입장마다 다름, RAM입장에선 RAM에 데이터가 있어서 하드디스크에 요청하지 않으면 그게 캐싱

- 레지스터의 크기가 중요(5번으로)

# 5. 32bit 컴퓨터

- CPU가 한번에 처리할 수 있는 데이터 양 = 32bit(42억9천)

- CPU에 붙어있는 레지스터의 총 크기가 4MB라면 레지스터 하나당 32bit를 처리할 수 있음 -> 32bit 컴퓨터

        그래서 32bit 컴퓨터로 64bit 프로그램을 돌리면 안돌아감
        그리고 64bit 컴퓨터로 32bit 프로그램을 돌리면 돌아감

- 32bit = 2^32 = 약 42억9천 -> 이때, 8GB(80억Byte) RAM을 사용하면 1byte에 1개의 번지를 가지므로 32비트로 표현할수 있는 최대의 수인 42억9천을 넘어서는 번지는 사용할  수 가 없음

따라서 32bit 컴퓨터는 4GB를 초과하는 RAM을 사용할수가 없음

# 끝

## reference

[itwill, K-디지털 디지털기초역량 향상을 위한 웹개발 입문 과정](https://www.e-itwill.com/main/index.jsp)