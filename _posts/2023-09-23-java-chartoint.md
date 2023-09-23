---
title: "[JAVA] Char -> Int 변환"
category: Java
tags: java method char int
---
프로그래머스 코딩테스트 문제를 풀다가 알게된 Char -> Int 형 변환 하는 법

-----

> 1. [자동 형변환](#1-자동-형변환)
> 2. [char - '0' (ASCII 코드 사용법)](#2-char---0-ascii-코드-사용법)
> 3. [Charactor.getNumericValue(char)](#3-charactorgetnumericvaluechar)

-----

# 1. 자동 형변환

```java
char c = '1';
int n = c; 
// int n = (int)c;

System.out.print(n); // 49 출력 
```

char 형인 c를 그대로 int 변수 n에 할당하게 되면 자동으로 형변환이 되어 저장된다.

이때 char에 저장된 문자인 '1'은 ASCII 코드 상에서 10진수값이 49 이므로 int 변수 n에는 정수 49 가 저장된다.

-----

# 2. char - '0' (ASCII 코드 사용법)

```java
char c = '1';
int n = c - '0';
// int n = (int)c - '0' 
//       == 49 - 48 = 1

System.out.print(n); // 1 출력
```

ASCII 코드상 0~9 까지 문자의 10진수 값은 48~57이다.

그리고 위 처럼 int 변수 n 에 '1' - '0'의 값을 넣게되면 자동으로 형변환된다.

따라서 문자 '1' - '0' 을 10진수 값으로 표현하면 49 - 48 이되어 정수 1을 얻을 수 있다.

<p class="text-gray"> 참고: ASCII 코드 표 일부 </p>

<img width="144" alt="image" src="https://github.com/junodevv/Algorithm/assets/126752196/6da4551b-ad5d-4c6d-a981-0ce8a8b4c8a4">

-----

# 3. Charactor.getNumericValue(char)

```java
char c = '1';
int n = Charactor.getNumericvalue(c);

System.out.print(n); // 1 출력
```

JAVA Charactor 클래스에서 제공하는 메소드이다.

-----

# 끝

-----

## reference

[자바 char to int : 문자를 숫자로 변환하기, Tstory blog, 나아가는중](https://dlee0129.tistory.com/230)

