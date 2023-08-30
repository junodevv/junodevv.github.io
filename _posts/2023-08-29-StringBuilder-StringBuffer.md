---
title: "[Java] StringBuilder와 StringBuffer"
category: Java
tags: java String StringBuilder StringBuffer Algorithm
---
알고리즘 문제를 풀다가 궁금해진 Stringbuilder 와 StringBuffer에 대한 정리

----

<h1 class="text-blue"> 1. StringBuilder</h1>

<b class="text-red">결론부터 말하면 StringBuilder는 기존 String객체와 다르게 변형이 가능한 객체이다.</b>

기존 String객체는 변형이 불가능하다.

```java
String str1 = "one";
String str2 = "two";
System.out.pringln(str1 + str2); // "onetwo"
```
이렇게 작성되었을 때 마지막 str1+str2 코드는 새로운 String 객체를 생성하게 만든다.

즉, Srting 객체와 + 연산자를 사용하게 되면 메모리의 할당(객체생성)을 발생시키게되고 
많은 연산이 이루어지면 그만큼 성능적으로 좋지 않고 비효율적인 코드가 된다.

이럴때 사용하는 것이 StringBuilder 클래스이다.

자바에서 제공하는 StringBuilder 객체는 객체를 새로 생성하지 않고 기존의 String데이터에 원하는 내용을 추가해준다.

ex)
```java
StringBuilder sb = new StringBuilder();

sb.append("가나다");

System.out.print(sb); // "가나다"

/* 객체 생성과 동시에 문자열 초기화 */
StringBuilder sb2 = new StringBuilder("ABC"); 

sb2.append("DEF");

System.out.print(sb2); // "ABCDEF"
```
위 처럼 작성된 코드는 새로운 String객체를 생성하지 않고 기존의 StringBuilder 객체에 새로운 문자열을 추가(append) 한다.

<br>

<p class="text-red">*주의</p>
주의할점은 StringBuilder로 만들어진 문자열을 String 으로 return 할 때는 ```toString()``` 메소드를 통해 

StringBuilder 객체를 String객체로 변환 시켜줘야 한다는 것이다.

ex) 
```java
public String print (String str) { 
    StringBuilder sb = new StringBuilder();
    	
    sb.append(str);
    	
    return sb.toString();
}
```
위처럼 String을 return하는 메소드를 만들었다면 ```toString()``` 으로 String객체를 만들어 줘야한다.

<br>
혹은 메소드의 return값을 StringBuilder 객체로 만들어준다.

ex)
```java
public StringBuilder print2 (String str) {
    StringBuilder sb = new StringBuilder("프린트2");
    	
    sb.append(str);
    	
    return sb;
    }
```

<p class="text-gray">*참고</p>
[자바공식문서 및 StringBuilder관련 메소드](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html)

-----

<h1 class="text-blue">2. StringBuffer</h1>

StringBuffer는 StringBuilder 와 거의 유사하고 사용하는 메소드 또한 동일하다.

StringBuilder 와의 차이점은 <b class="text-red">동기화(Synchronization)</b>이다.

StringBuffer는 동기화를 지원하여 **멀티 스레드 환경에서도 안전하게 동작한다.**

그 이유는 StringBuffer 메소드에서 **synchronized 키워드**를 사용하기 때문이다.

> **synchronized 키워드**는 여러개의 스레드가 한 개의 자원에 접근 하려고 할 때, 
>
> 현재의 데이터를 사용하고 있는 스레드를 제외하고 나머지 스레드들이 데이터에 접근할 수 없도록 막는 역할을 한다.

따라서, 멀티 스레드 환경에서 프로그램이 작동되는 경우 StringBuffer가 사용된다고 한다.

이 사이트에서 자세한 내용을 확인 할 수 있다. [String ,StringBuffer, StringBuilder 차이점 및 성능비교](https://inpa.tistory.com/entry/JAVA-%E2%98%95-String-StringBuffer-StringBuilder-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%84%B1%EB%8A%A5-%EB%B9%84%EA%B5%90)

-----

# 3. 결론

결국 어떤 상황에 어떤 클래스를 사용해야 하는가에 대한 의문이 든다.

정리하자면 

- String을 사용하는 경우

> 1. 문자열의 연산이 적거나 변경하지 않을때
> 2. 멀티 쓰레드 환경일 때(불변이기 때문에 안전)

- StringBuilder를 사용하는 경우

> 1. 문자열의 추가, 수정, 삭제 등이 빈번한 경우
> 2. 단일 쓰레드이거나 동기화를 고려하지 않아도 되는 경우
> 3. 속도는 StringBuffer보다 빠르다.

- StringBuffer를 사용하는 경우

> 1. 문자열의 추가, 수정, 삭제 등이 빈번한 경우
> 2. 멀티 쓰레드 환경이거나 동기화를 고려해야 하는 경우

-----

# 끝

-----

<h2 class="text-gray">reference</h2>

**[👍String ,StringBuffer, StringBuilder 차이점 및 성능비교](https://inpa.tistory.com/entry/JAVA-%E2%98%95-String-StringBuffer-StringBuilder-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%84%B1%EB%8A%A5-%EB%B9%84%EA%B5%90)**

[StringBuilder참고사이트1](https://onlyfor-me-blog.tistory.com/317)

[StringBuilder 참고사이트2](https://www.codejava.net/java-core/the-java-language/why-use-stringbuffer-and-stringbuilder-in-java)

[StringBuilder 참고사이트3](https://hardlearner.tistory.com/288)

