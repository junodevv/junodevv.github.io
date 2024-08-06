---
title: "[Mockito] Mockito 테스트 프레임워크의 적용"
category: TestCode
tags: testCode mockito
---
SsaGeMeogJa(싸게먹자) 프로젝트를 진행하면서 배우고 적용해본 Mockito 테스트 프레임워크

---

# Mockito 적용 계기

최근 SsaGeMeogJa(싸게먹자) 프로젝트를 진행하면서 단위테스트를 작성했다.<br>
단위테스트를 작성할 때 나는 우리 팀에서 임의로 넣은 더미데이터들을 기준으로 테스트코드를 작성했다.<br>
하지만 더미데이터는 개발과정에서 테스트를 위한 데이터일 뿐이다.<br>
따라서 프로젝트를 배포하게된다면 더미데이터를 주입하지 않게되어 나의 테스트코드가 성공하지 못하는 코드가 될 것이다.<br>
이를 깨닫고 나는 어느 환경에서든 항상 성공할 수 있는 테스트코드를 작성해보려 했다.

우선 어떤 방식이 존재하는지 찾아봤다.

1. `@BeforeEach`를 이용한 기본 DB데이터 주입
2. 별도의 테스트 DB 사용
3. `Mockito` 사용
4. 테스트용 클래스 생성
5. 테스트 컨테이너 라이브러리 사용

내가 찾은 바로는 이정도의 방식이 있었다.

나는 이번에 그 어떤 로컬의 리소스에 의존하고 싶지 않았기 때문에 1번, 2번, 5번 방식은 제외 했고 테스트를 위해 새로운 클래스를 만들어야한다는 것이 비효율적이라는 생각이 들어 4번도 제외하였다.<br>
또한 Mockito는 SpringBoot에 대한 의존성이 없기 떄문에 SpringBoot서버를 띄우는 과정이 생략되어 테스트 속도도 빨라진다는 장점있어서 결국 Mockito 테스트 프레임워크를 사용하기로 했다.

# Mockito의 적용

## Mockito란
Mockito를 적용하기전에 이 Mockito가 무엇인지 알아봤다.
Mockito란 개발자가 동작을 직접 제어할 수 있는 가짜 객체를 만들도록 지원해주는 테스트 프레임워크이다. 일반적으로 Spring 어플리케이션은 여러객체들이 의존성을 가지고 있따. 이러한 의존성이 테스트의 작성을 어렵게하는데 이때 Mockito를 활용하여 가짜객체를 주입하고 내가 원하는 결과를 반환하도록 조정하여 단위 테스트를 진행할 수 있게 해준다.

## Mockito 사용법
### 의존성 추가
일반적으로 `testImplementation 'org.springframework.boot:spring-boot-starter-test'`에 Mockito 라이브러리가 포함되어있다. 특별한 버전설정이 필요하다면 Maven Repository의 Mockito-core의 의존성을 추가해주는 것이 필요하다.

### 실행시 적용하는 법
`@ExtendWith(MockitoExtension.class)`을 테스트 클래스의 범위에 추가해준다.

```java
@ExtendWith(MockitoExtension.class)
class CartServiceTest {
    ...
}
```
### 가짜 객체인 Mock객체를 생성하고 주입하는 방법
1. `@Mock` : 가짜 객체인 Mock 객체를 생성하는 어노테이션, 해당 객체의 로직은 스터빙을 통해 제어된다.
스터빙하지 않고 해당 객체를 호출하면 primitive type은 0, reference type은 null을 반환한다.
2. `@Spy` : 진짜 객체를 생성하는 어노테이션, 해당 객체의 로직을 스터빙을 통해 제어할 수 있다. 
하지만 스터빙하지 않으면 실제 객체의 로직을 수행하고 실제 값을 반환한다.
3. `@InjectMock` : `@Mock`이나 `@Spy`로 만들어진 객체를 자동 주입해주는 어노테이션.

### 스터빙(Stubbing)하는 방법
#### 1. `OngoingStubbing` 메서드
- when에 넣은 메소드의 리턴값을 정의해주는 메소드, 진행중인 스터빙이라는 뜻

```java
when(스터빙할 메서드).then메서드;
```

|메서드명|내용|
|:---:|:---:|
|`thenReturn`|어떤 값을 반환할 것인지 정의|
|`thenThrow`|어떤 예외(Exception)을 Throw할 것인지 정의|
|`thenAnswer`|어떤 작업을 할지 정의하는 것<br>공식문서에서 이 메서드 대신 thenReturn이나 thenThrow를 사용할 것을 권장한다고한다.|
|`thenCallRealMethod`|실제 메서드 호출|

#### 2. `Stubber` 메서드
- when에 메서드가 아닌 클래스를 작성해준다.

```java
Stubber 메소드.when(스터빙할 클래스).스터빙할 메소드
```

|메서드명|내용|
|:---:|:---:|
|`doReturn`|어떤 값을 반환할 것인지 정의|
|`doThrow`|어떤 예외(Exception)을 Throw할 것인지 정의|
|`doAnswer`|어떤 작업을 할지 정의|
|`doNothing`|어떤 행동도 하지 않게 정의|
|`doCallRealMethod`|실제 메서드 호출|

#### OngoingStubbing과 Stubber의 차이
- OngoingStubbing은 메서드 체이닝을 통해서 <b class="text-red">여러동작</b>을 연속적으로 수행할 수 있도록해준다.

```java
List<String> mockedList = mock(List.class);

// OngoingStubbing 사용 예제
when(mockedList.get(0)).thenReturn("firstElement").thenThrow(new RuntimeException());

System.out.println(mockedList.get(0)); // 출력: "firstElement"
System.out.println(mockedList.get(0)); // RuntimeException 발생
```
- Stubber는 주로 void 메서드나 메서드 체이닝을 사용할 수 없는 경우에 사용된다.

```java
List<String> mockedList = mock(List.class);

// Stubber 사용 예제
doReturn("firstElement").when(mockedList).get(0);

System.out.println(mockedList.get(0)); // 출력: "firstElement"
```

- 이둘은 메서드 호출시점에따라 분류 할 수 있다.<br>
OngoingStubbing은 메서드 호출 시점에 반환값을 정의하고 Stubber는 메서드 호출 이전에 반환 값을 정의할 수 있다.(그래서 void를 반환하는 메서드나 예외를 던지는 상황에 유용)

보다 자세한 사용법은 아래 사이트들에서 볼 수 있다.

>[Mockito 공식문서](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)
>
>[JUnit과 Mockito 기반의 Spring 단위 테스트 코드 작성법, [MangKyu's Diary:티스토리]](https://mangkyu.tistory.com/145)
>
>[[Java] Mockito 사용법 (3) - 스터빙 (Stubbing)](https://effortguy.tistory.com/143)