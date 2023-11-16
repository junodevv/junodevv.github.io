---
title: "[Etc] 하드 코딩"
category: etc
tags: hard-coding
---

우테코를 진행하며 알게된 하드 코딩

---

# 하드코딩이란

프로그램의 소스 코드에 데이터를 직접 입력해서 저장하는 것

파일주소, URL, IP주소, 비밀번소, 출력 문자열 등

ex)

```java
/** 예시 1 */
System.out.print("Hard Coding");

/** 예시 2: 0~9사이의 랜덤 값을 구하고 그 값에 따라 car 객체를 이동(or 멈춤)하는 메소드*/
public void moveOrNot(Car car){
        int randomNumber=Randoms.pickNumberInRange(0,9);
        if(randomNumber>=4){
        car.forward();
        }
}
```

예시 1의 `Hard Coding`처럼 출력값을 변수를 통해 입력하는 것이 아니라 직접 문자열을 넣은 경우

예시 2의 `(0,9)`,`4` 처럼 직접 숫자를 입력한 경우

위 같은 경우들이 하드코딩된 코드들이다.

# 하드 코딩의 문제점

1. 의미를 파악하기 어렵다.
2. 유지 보수하기 어렵다.

### 1. 의미를 파악하기 어렵다.

예를 들어 예시2 코드의 한부분을 가져와 보면

```java
int randomNumber=Randoms.pickNumberInRange(0,9);
```

여기서 `0`과 `9` 자체가 어떤 의미를 갖는지 파악하기 어렵다.

하지만 위 코드를

```java
private static final String MIN_NUMBER=0;
private static final String MAX_NUMBER=9;

        int randomNumber=Randoms.pickNumberInRange(MIN_NUMBER,MAX_NUMBER);
```

이런식으로 `상수변수`로 선언하여 사용한다면 `0`이 `최소값`을 의미하고 `9`가 `최댓값`을 의미한다는 것을 알 수 있다.

이렇게 사용한다면 메소드의 의미와 메소드에 들어가는 매개변수의 의미를 파악하기 쉬워져 가독성이 좋아질 것이다.

### 2. 유지 보수하기 어렵다.

이번에도 예시2의 같은 코드를 생각해보면

```java
int randomNumber=Randoms.pickNumberInRange(0,9);
```

이러한 코드가 한 곳이 아니라 여러 곳에 있고,

Random값을 도출 해내고 싶은 범위가 `(0,9)` 가 아닌 `(1,9)` 처럼 변경된다면 일일이 똑같이 `(0,9)` 로 작성된 코드를 찾아서 `(1,9)` 로 바꿔줘야 한다.

또한 변경해야할 코드를 미처 찾지못해서 변경하지 못하면 코드에 오류가 날 것이다.

하지만

```java
private static final String MIN_NUMBER=0;
private static final String MAX_NUMBER=9;

        int randomNumber=Randoms.pickNumberInRange(MIN_NUMBER,MAX_NUMBER);
```

이 코드에서는 상수변수인 `MIN_NUMBER`, `MAX_NUMBER` 의 값을 변경해주면 된다.

이러한 유지보수의 장점은 복잡하고 긴 코드일 수록 크게 드러날 것이다.

# 하드코딩을 피하자

의미가 명확한 코드, 가독성 좋은 코드, 유지 보수하기 좋은 코드를 작성하기 위해 하드코딩보다는 상수변수를 사용하는 것이 좋다.

### reference

[테코블, 하드코딩을 피해라.](https://tecoble.techcourse.co.kr/post/2020-05-07-avoid-hard-coding/)