---
title: "[Java] string <-> int 변환"
category: Java
tags: java string int 변환
---
알고리즘 문제풀다가 알게된 string을 int로, int를 string으로 변환하는 메소드 정리

# - String -> int 

java.lang.Integer 클래스의 <b class="text-red">Integer.parseInt()</b> 와 <b class="text-blue">Integer.valueOf()</b> 메소드가 있다.

두 메소드의 차이점은 <b class="text-red">int</b> 로 변환되는가 vs <b class="text-blue">Integer</b>  로 변환되는가 이다.

자세한 내용 = [int와 Integer의 차이](/java/2023/08/21/java-intVSInteger.html)

#### 1. Integer.parseInt(String str)

```java
String str1 = "12";
String str2 = "-12";

int i1 = Integer.parseInt(str1); // i1 = 12
int i2 = Integer.parseInt(str2); // i2 = -12
```

#### 2. Integer.valueOf(String str)

```java
String str1 = "12";
String str2 = "-12";

int i1 = Integer.valueOf(str1).intValue(); // i1=12
int i2 = Integer.valueOf(str2).intValue(); // i2 =-12
```
사실 ```intValue()는 ``` 사용하지 않아도 자동으로 캐스팅 해준다.

-----

# - int -> String

java.lang.Integer 클래스의 Integer.toString(), java.lang.String 클래스의 String.valueOf() 메소드가 있다.

<b class="text-red">간단하게 ""(빈문자열) 과 +(더하기 연산자)를 사용하여 자동으로 캐스팅 되도록 할 수 있다.</b>

#### 1. Integer.toString(int i)

```java
int i1 = 123;
int i2 = -123;

String str1 = Integer.toString(i1); // str1="123"
String str2 = Integer.toString(i2); // str2="123"
```

#### 2. String.valueOf(int i)

```java
int i1 = 123;
int i2 = -123;

String str1 = String.valueOf(i1); // str1="123"
String str2 = String.valueOf(i2); // str2="-123"
```

#### 3. int + ""

```java
int i1 = 123;
int i2 = -123;

String str1 = i1 + ""; // str1 = "123"
Stting str2 = i2 + ""; // str2 = "-123"
```

위 처럼 빈 문자열("")에 int 를 이어 붙이면 자동으로 int 까지 String 으로 캐스팅된다.

이게 왜 되는지도 따로 다뤄 보겠다!

------

# 끝

------

## reference

[Sring을 int로 int를 String 으로 변환하기](https://hianna.tistory.com/524)