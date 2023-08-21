---
title: "[Java] substring 메소드(문자열 자르기)"
category: Java
tags: java method algorithm
---
알고리즘 문제 풀다가 알게된 substring 메소드

java.lang.String 클래스 속 메소드

-----

# substring(startIndex); 

- startIndex 부터 끝까지의 string을 잘라서 보관

# substring(startIndex, endIndex(불포함)); 

- startIndex 부터 endIndex까지의 string을 잘라서 보관

ex)
```java
string str = "hello";

str.substring(2); // "llo"

str.substring(2,4); // "ll", (4 불포함)
str.substring(1,4); // "ell", (4 불포함)
```

**주의**

```java
str.substring(5); // 안될거같지만 빈 string("")을 반환, 하지만 str.length()를 넘으면 안됨
```
-----

## reference

[[Java] 문자열 자르기](https://hianna.tistory.com/534)