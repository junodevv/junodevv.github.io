---
title: "[Java] startsWith, endsWith 메소드"
category: Java
tags: java method startsWith endsWith
---
특정 문자로 시작하거나 특정문자로 끝나는 지 체크해주는 메소드

프로그래머스 코딩테스문제를 풀다가 알게된 startsWith, endsWith 메소드

-----

> 1. startsWith()
> 2. endsWith()

-----

# 1. startsWith()

String.class에서 제공하는 startsWith() 메소드

특정 문자열로 <b class="text-red">시작하는지</b> 체크하여 True / False 값을 return 해준다.

ex)

```java
String str = "java";

str.startsWith("j");  // true
str.startsWith("av"); // false
```

<b class="text-red"> *주의</b>: 띄어쓰기(공백)도 포함된다.

```java
String str = "java";

str.startsWith(" j");  // false
```

-----

# 2. endsWith()

String.class에서 제공하는 startsWith() 메소드

특정 문자열로 <b class="text-red">끝나는지</b> 체크하여 True / False 값을 return 해준다.

ex)

```java
String str = "java";

str.endsWith("a");  // ture
str.endsWith("va"); // true
str.endsWith("j");  // false
```

<b class="text-red"> *주의</b>: 띄어쓰기(공백)도 포함된다.

```java
String str = "java";

str.startsWith("a ");  // false
```

-----

# 끝

## reference

[[JAVA] 자바_startsWith/endsWith (특정 문자로 시작하거나 끝나는지 체크)](https://mine-it-record.tistory.com/128)