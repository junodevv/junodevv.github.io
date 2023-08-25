---
title: "[Java] int와 Integer의 차이"
category: Java
tags: java int Integer 자료형
---
string - int 변환 메소드를 살펴보다가 궁금해져서 찾아본 int와 Integer의 차이

> 1. [int란](#1-int란)
> 2. [Integer란](#2-integer란)
> 3. [3. int 와 Integer (Primitive type과 Wrapper class)의 차이점](#3-int-와-integer-primitive-type과-wrapper-class의-차이점)
> 4. [reference](#reference) 

-----

# 1. int란

> int는 변수의 타입(data type)이다.

<b class="text-red">변수란 "값을 저장할 수 있는 메모리 상의 공간"을 의미</b>한다.

```java
int i = 3;
cahr c = "j";
```

에서 <b>i</b> 와 <b>j</b>가 변수이다.

그리고 그 앞의 int, char이 변수의 type(자료형)을 지정해주고 있다.

즉, 이 변수의 type(자료형)은 <b class="text-red">"data의 type에 따라 값이 저장될 공간의 크기와 저장형식을 정의한것"</b> 으로 볼 수 있다.

이러한 자료형은 기본형(primitive type)과 참조형(reference type)으로 나뉘는데 

int나 char같은 자료형은 기본형에 속하는 것이다.

<br>
<br>
<p class="text-gray">*참고(기본형의 종류)</p>

![image](https://github.com/junodevv/junodevv.github.io/assets/126752196/ded91ea0-1526-4fc0-8280-8d72e3d98b14)

-----

# 2. Integer란

<b class="text-red">기본형 변수를 객체로 다루기 위해서 사용하는 클래스 = Wrapper class(래퍼클래스) 이다.</b>

Integer는 int의 wrapper class 인 것이다.

이러한 wrapper class 를 사용하는 경우는

1. 매개변수로 객체를 필요로 할 때
2. 기본형 값이 아닌 객체로 저장해야 할 떄
3. 객체 간 비교가 필요할 때

등이 있다.

<br>
모든 기본형은 wrapper class 를 생성할 수 있다.

<p class="text-gray">*참고(기본형 -> 래퍼클래스)</p>
![image](https://github.com/junodevv/junodevv.github.io/assets/126752196/5372d453-d299-4b9d-b562-d9376a85a2e2)

-----

# 3. int 와 Integer (Primitive type과 Wrapper class)의 차이점

int(Primitive type)

- 산술 연산 가능
- null로 초기화 불가능

Integer(Wrapper class)

- 산술연산 불가능 (기본형으로 변환 하여 할 수 있음)
- null 값 처리 가능

-----

# 끝

-----

## reference

[int와 Integer는 무엇이 다른가](https://velog.io/@hadoyaji/int%EC%99%80-Integer%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4-%EB%8B%A4%EB%A5%B8%EA%B0%80)