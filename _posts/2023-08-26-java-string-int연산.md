---
title: "[Java] String + int 연산(\"\"+i = \"i\")"
category: Java
tags: java String int +연산
---
알고리즘을 풀다가 궁금해진 "" + i(int) 연산에 대해서 알아본 글 + (char+int)

-----

<br>
알고리즘 문제를 풀다보니

```java
int i = 123;
System.out.print(""+i); // "123"
```

이렇게 빈 문자열("")과 int형 변수를 "+" 연산자로 더해주면 int형 변수까지 문자열(String)으로 변한다는 사실을 알게 되었다.

이게 왜 이렇게 되는지 궁금해 졌다.

<br>

-----

# 결론
<br>
그 이유는 <b class="text-red">+ 연산자를 수행할 때 어느 한쪽이 String이면 다른 한쪽도 String 형태로 변환 한 후 두 String 을 결합하기 때문에</b> 그렇다.

ex)

```java
System.out.println(1+3);    // 숫자+숫자 = 4
System.out.println(""+1+3); // 빈String + 숫자 = "13"
System.out.println("A"+1);  // String+숫자 = "A1"
System.out.println("A"+"A");// String + String = "AA"
```

<br>

-----

# 번외(char형)
<br>
만약 char형 이라면 어떨까

ex)

```java
char c = 'A';
System.out.println(c);      // 'A'
System.out.println(c+3);    // 68  (65+3)
System.out.println(c+"A");  // "AA"
System.out.println(c+c);    // 130 (65+65)
System.out.println(""+c+1); // "A1"
```

c+3 (char + int) 는 68 이라는 값이 나오게 된다.

그 이유는

1. char형 변수는 컴퓨터 내부에 저장될때 아스키 코드로 값을 저장하고 있기 때문에
<br> 변수c에는 'A'의 아스키코드 값인 정수65가 저장된다.

2. 어떤 수식이 계산될 때 가장 범위가 넓은 피연산자의 타입으로 변환된다. 
<br> 즉, int(4byte) 이하의 변수 타입을 사용하는 경우 연산시에 int형으로 자동 변환된다.

따라서, c가 int형인 65 로 변환되어 65+3 이 실행되는 것이다.

<br>
<p class="text-gray">*참고: 기본 자료형들의 범위크기 순서</p>

> byte < short = char < int < long < float < double

<p class="text-red">*주의</p>

```java
char c = 'A';
char c2 = c+1; // 컴파일 에러
```
이런 코드는 안된다.

왜냐면 <b class="text-red"> 리터럴 간의 연산 </b> 이기 때문이다.

이거는 다음시간에 [리터럴 간의 연산]()

----

# 끝

----

## reference

[String + int 연산](https://blog.naver.com/jin93hj/220574721347) : 전체적인 참고

[char형 연산](https://cheerant.tistory.com/43) : char형, 리터럴간 연산 참고

['char'와 "String"의 진실](https://kang-james.tistory.com/entry/JAVA-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-%EB%AC%B8%EC%9E%90-%ED%83%80%EC%9E%85-char-%EC%99%80-%EB%AC%B8%EC%9E%90%EC%97%B4-String-%EC%9D%98-%EC%A7%84%EC%8B%A4) : char형의 저장 방식 참고