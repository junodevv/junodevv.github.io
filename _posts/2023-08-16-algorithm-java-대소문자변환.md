---
title: "[Java] 대소문자 변환 메소드(+ 대소문자 구분)"
category: Java
tags: algorithm java 대소문자변환 대소문자구분
---
알고리즘 문제를 풀다가 알게된 (string, char)대소문자 변환 메소드 정리

1. [str.toUpperCase() / str.toLowerCase()](#1-strtouppercase--strtolowercase)

2. [Character.toUpperCase(char) / Character.toLowerCase(char)](#2-charactertouppercasechar--charactertolowercasechar)

3. [번외: Character.isUpperCase(char) / Character.isLowerCase(char)](#3-번외-characterisuppercasechar--characterislowercasechar)

----------

# 1. str.toUpperCase() / str.toLowerCase() 

<p class="text-red">(char는 사용 불가)</p>

- str.toUpperCase() => string을 대문자로 변환

```java
String str = "abc";
str = str.toUpperCase(); // "ABC"
```

- str.toLowerCase() => string을 소문자로 변환

```java
String str = "ABC";
str = str.toLowerCase(); // "abc"
```

-------------

# 2. Character.toUpperCase(char) / Character.toLowerCase(char)

<p class="text-red">(string는 사용 불가)</p>

- Character.toUpperCase(char) => char를 대문자로 변환

```java
Char c = "a";
c = Character.toUpperCase(c); // "A"
```

- Character.toLowerCase(char)

```java
Char c = "A";
c = Character.toLowerCase(c); // "a"
```

----------

# 3. 번외: Character.isUpperCase(char) / Character.isLowerCase(char)

입력 받은 파라메터가 영문 대문자 / 소문자 인지 를 구분하여 True/False 값을 return 하는 함수

```java
char c = "a";
System.out.print(Character.isUpperCase(c)); // false
System.out.print(Character.isLowerCase(c)); // true
```