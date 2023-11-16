---
title: "[Java] 단위테스트"
category: Java
tags: java unittest testcode
---

우아한 테크코스를 진행하면서 알게된 단위테스트 방법

-----

# 단위테스트

- 구현한 개별 기능들이 의도대로 작동하는지 확인하는 행위
- 장점
    - 원하는 부분만 테스트 하면서 (중간 과정생략)결과를 빠르게 볼수 있다.
    - 미리 작성한 단위 테스트 기반으로 프로덕션 코드의 리팩토링을 안정적으로 할 수 있다 → 아직 이해하기 어려움

    * 리팩토링을 해보지 않아서 일단 과제 진행하면서 보자

  ex) 성공하는 단위테스트를 만들어놓고 기존 코드를 리팩토링하고 단위 테스트를 돌렸는데 실패 했다면 리팩토링 코드가 잘못된 것임을 알 수 있다.

    - 단위 테스트가 실패하는 지점에서 문제점을 빠르게 찾을 수 있다.
- 단위테스트 작성예시
    - 실행 코드

```java
public class Car{
	private int position;

	public Car(){
		this.position = 0;
	}
	
	// 명령한 수만큼 move() 메서드를 실행시키는 기능
	public void moveAsOrdered(final int orderCount){
		this.position += orderCount;
	}

	public int getPosition(){
		return position;
	}
}
```

- 테스트 코드

    ```java
    @Test
    @Displayname("자동차는 명령을 받은 만큼 이동거리가 증가한다")
    void car_moves_as_ordered(){
    	int orderedCount = 10;
    	Car car = new Car();
    	car.moveAsOrderd(orderedCount);
    	int distanceResult = car.getPosition();
    	assertThat(distanceResult).isEqualTo(orderedCount);
    }
    ```


## Given When Then 패턴

- 테스트 코드의 가독성을 높여주어 유지보수에 용이하다.
- 테스트 코드 작성의 표현방식
    1. Given: 테스트를 위해 **준비**하는 과정 → 테스트에 사용하는 **값을 정의**
    2. When: 테스트하고자 하는 **기능을 실행**
    3. Then: 기능을 실행 후 **결과를 검증**

* 필수가 아닌 선택

적용 예시

```java
@Test
@Displayname("자동차는 명령을 받은 만큼 이동거리가 증가한다")
void car_moves_as_ordered(){
	// given(주어진 값)
	int orderedCount = 10;
	Car car = new Car();

	// when(기능 작동)
	car.moveAsOrderd(orderedCount);
	int distanceResult = car.getPosition();

	// then (기능 작동 후 검증)
	assertThat(distanceResult).isEqualTo(orderedCount);
}
```

### @ParameterizedTest - 여러값을 테스트하는 어노테이션

- 여러 값을 검증 해야할때 같은 테스트 코드를 n번 만드는 대신 이 어노테이션을 사용할 수 있다.

예시 racingcar-6 과제 중에서

```java
@ParameterizedTest
    @CsvSource(value = {"4:-", "3:''", "9:-"}, delimiter = ':')
    @DisplayName("랜덤값이 4이상이면 전진하는 기능")
    void moveOrNot_랜덤값이_4이상이면_전진(int randomNumber, StringBuffer expected) {
        Car car = new Car("test");

        gameController.moveOrNot(car, randomNumber);

        assertThat(car.getForwardDistance().toString()).isEqualTo(expected.toString());
    }
```

## FIRST 원칙

- **Fast**
    - 단위테스트는 빨라야한다
- **Independent**
    - 테스트는 독립적으로 동작해야한다.
    - 이전 테스트에 영향을 받으면 실패원인을 찾기 어렵다.

    * ex) 첫번째 테스트에서 클래스안의 static 변수 값을 바꾸면 두번째 테스트에서 beforeEach 로 초기화 시켜주지않고 바로 사용하면 결과 값이 다르게 나올 수 있다.

- **Repeatable**
    - 어떤 상황에서든 예상한 대로 테스트 결과가 나와야 한다.
- **Self-Validateing**
    - 출력 혹은 로그가 아닌 테스트 자체적으로 결과가 나와야한다.

    * System.out.print(), logger.info() 같은 메소드 사용없이 테스트 자체적으로 결과가 나와야한다(T/F)

- **Timely**
    - 적시에 테스트를 철저하게 작성해야한다.

    * 테스트 코드를 차근차근 쌓아 나가야한다.

## reference

[우아한 테크, \[10분 테코톡\] 제이의 단위 테스트 ](https://youtu.be/mIO4Rbe_M74?si=yhhSIo-ow0mtyVYL)