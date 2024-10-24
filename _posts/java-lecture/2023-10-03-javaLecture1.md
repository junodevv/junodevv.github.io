---
title: "[Java] 강의1 전자와 2진수의 이해 및 메모리의 이해"
category: Java
tags: java java강의 
published: false

---

자바를 더 깊게 이해하기 위한 itwill 자바 강의

-----

# 1. 전자와 2진수의 관계

### 1.1 전자

(-)전자와 (+)양성자가 서로 끌어들이는 힘으로 인해 전자가 이동을 하는데 이것을 보고 전류가 흐른다고 한다.

*전기선에서 흐르는게 전류

### 1.2 반도체

        전류가 흐르는 것만으로는 제어하기 어려움

- 부도체: 전류가 흐르지 않는 물질
- 도체: 전류가 잘 흐르는 물질
- 반도체: 온도에 따라 전류가 흐르기도하고 흐르지 않기도 함
        온도가 높으면 전류가 흐르지 않고, 온도가 낮으면 전류가 잘 흐름

### 1.3 2진수

반도체를 이용해 전류를 받거나 받지 않거나 할 수 있는데 이를 2진수(1,0)와 함께 응용하여 컴퓨터에 적용할 수 있다.

ex) 선풍기 꺼짐 켜짐(2가지 경우의수,1bit) | 꺼짐, 미풍, 약풍, 강품 (4가지 경우의수, 2bit) ...

### 1.4 캐패시터(축전기)

전류를 일시적으로 담아둘 수 있음 -> 일시적 배터리

-> 메모리(DRAM) - 캐패시터

# 2. 메모리 하나의 번지에 저장할 수 있는 데이터의 양은 8bit인 이유

### 2.1 통신의 시작

컴퓨터로 전류를 통해 2진수를 받아서 0,1로 표현된 수를 인간이 이해할 수 있는 부호로 변경해준다.
        *여기서 부호로 변경 = 부호화 = 문자인코딩

이러한 통신은 유럽에서 시작 되었는데 영어문자+기호+숫자+오류검출코드 를 모두 표현하는데 필요한 최소bit = 8bit였음

그리고 인간이 이해하는 최소 단위로 1byte(8bit)를 논리적으로 만들었음

### 2.2 컴퓨터 메모리

사람이 이해하기 좋은 최소 단위인 1byte씩 메모리의 공간을 분리해 놓음

따라서 1개의 주소에 1개의 문자가 저장됨

### 2.3 아스키코드의 한계

통신이 유럽뿐만아니라 전세계로 퍼지게됨, 유럽은 한 문자를 표현하는데 1byte면 충분하지만 다른 나라는 아니었음

한국은 2byte, 중국은 3byte, 일본도 1byte 초과 -> UniCode의 등장

-> 인터넷의 발전으로 세계화 되면서 UTF-8(3byte)가 등장하게됨, 3byte는 거의 모든 문자를 표현할 수 있음

**이모지는 UTF-8mb4

### 그래서 지금은 보통 UTF-8을 사용한다.

# 끝

## reference

[itwill, K-디지털 디지털기초역량 향상을 위한 웹개발 입문 과정](https://www.e-itwill.com/main/index.jsp)