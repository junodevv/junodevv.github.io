---
title: "[Java] StringBuilder(StringBuffer) 클래스와 메소드 정리"
category: Java
tags: java StringBuilder StringBuffer
---

살면서 사용해본 StringBuilder, StringBuffer 메소드에 대해 정리중인 게시글

-----

# 1. 생성

```java
StringBuilder sb = new StringBuilder(); // 기본 16개의 문자를 담을 수 있는 크기의 인스턴스 생성
StringBuilder sb = new StringBuilder(10); // 10개의 문자를 담을 수 있는 크기의 인스턴스 생성
StringBuilder sb = new StringBuilder("lee"); // "lee"값을 가지는 인스턴스 생성

```

# 2. append() 메소드

기존 StringBuilder 문자열의 <b class="text-red">뒤에</b> 새로운 문자를 더해준다.

```java
StringBuilder sb = new StringBuilder("lee");

sb.append("juno"); // sb = "leejuno"

sb.append('a');    // sb = "leejunoa"

sb.append(true);    // sb = "leejunoatrue"

char[] c = {'a','b'};
sb.append(c);       // sb = "leejunoatrueab"

sb.append(123.1);   // sb = "leejunoatrueab123.1"

sb.append(123.1f);  // sb = "leejunoatrueab123.1123.1"

sb.append(sb);      // sb = "leejunoatrueab123.1123.1leejunoatrueab123.1123.1"

sb.append(int i);
sb.append(long l);
...
```

예시에서 볼 수 있듯 append 메소드에 들어갈 수 있는 매개변수 타입은 ```String, char, char[], boolean, double, float, Object, int, long```이다.

# 3. .length()

문자열의 길이를 알려준다.

# 4. .charAt(idx)

해당 index의 문자(char)를 반환

# 5. .replace(int start, int end, String str)

index 위치 start 부터 end+1까지를 str로 바꿔준다.

```java
StringBuilder sb = new StringBuilder("leebabo");

sb.replace(3,7, "juno");
// sb = "leejuno"
```
# 6. .reverse()

문자열을 역으로 나열한다.

```java
StringBuilder sb = new StringBuilder("lee");

sb.reverse();
// sb = "eel"
```

# 7. .toString()

StringBuilder(Buffer)를 String 타입으로 변환 해준다.

# 8. .substring(int start, int end)

문자열의 index 위치 start 부터 end-1까지의 위치를 return 해준다.

```java
StringBuilder sb = new StringBuilder("leejuno");

sb.substring(3,7);
// sb = "juno"
sb.substring(3);
// sb = "juno"
```

# 9. .toString();

StringBuilder(Buffer) 타입의 인스턴스를 String 타입으로 바꿔준다.

```java
StringBuilder strB = new StringBuilder();
    	
strB.append("자바");

String str = strB.toString();
    	
System.out.println(strB); // "자바"    	
System.out.println(str);    // "자바"
```

출력결과는 같다.

이 메소드는 return 타입이 String인 메소드에서 StringBuilder를 사용했을때 유용하다.

이외에도 더 있지만 앞으로 공부하면서 사용하게 된 것들을 하나씩 정리할 예정

-----

# 끝

-----

## reference

[참고사이트](https://staticclass.tistory.com/82)