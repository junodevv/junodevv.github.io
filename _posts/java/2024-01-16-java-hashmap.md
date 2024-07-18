---
title: "[Java] HashSet"
category: Java
tags: java hashset
---
[프로그래머스 폰켓몬, 해시문제](https://school.programmers.co.kr/learn/courses/30/lessons/1845?language=java)를 풀다가 알게된 HashSet

---

목차
- [HashSet 이란](#hashset-이란)
- [HashSet 메소드](#hashset-메소드)
- [HashSet의 장점(List대비)](#hashset의-장점list대비)

---

해당 문제를 풀떄 나는 `List`를 이용했다.

하지만 문제를 풀고나서 다른 사람들의 풀이를 살펴보니 종종 HashSet을 사용하는 사람들의 풀이가 보였고 코드도 더 간결해보였다. 뿐만 아니라 이 문제 자체가 Hash 라는 카테고리 안에 들어있는 문제였기 때문에 `HashSet`을 모르고 넘어갈 순 없었다.

# HashSet 이란
`Set` 인터페이스를 Hash로 구현한 자료구조로 중복을 허용하지 않는 집합이다.

내부적으로는 `HashMap`를 이용하여 해시 테이블로 만들어진다고 한다.

`HashSet`은 순서의 일정함을 보장해주지 않는다. 또한 `Null` 값을 허용한다.

<p class="text-red">한마디로 HashMap으로 만든 중복을 허용하지 않는 집합 자료구조.</p>

# HashSet 메소드

<img width="600px" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/32314b59-bcb8-4b09-8abb-85b79553e1d9">

`add(E e)`: 지정된 요소 e를 이 세트에 추가한다. 요소가 이미 존재하지 않는 경우에만 추가.(중복허용X)

`clear()`: 이 세트에서 모든 요소를 제거.

`clone()`: 이 HashSet 인스턴스의 얕은 복사본을 반환. 요소 자체는 복제되지 않는다.

`contains(Object o)`: 이 세트가 지정된 요소 o를 포함하고 있는지 여부를 반환.

`isEmpty()`: 이 세트에 요소가 없는 경우 true를 반환.

`iterator()`: 이 세트에 있는 요소들을 반복하는 Iterator를 반환.

`remove(Object o)`: 지정된 요소 o를 이 세트에서 제거. 요소가 존재할 경우에만 제거.

`size()`: 이 세트에 있는 요소의 수()를 반환.

`spliterator()`: 이 세트의 요소들에 대한 지연 결합(late-binding) 및 빠르고 오류가 없는(fail-fast) Spliterator를 생성.

# HashSet의 장점(List대비)

**1. 중복을 허용하고 싶지 않을때, 다른 부가적인 코드 없이 요소를 중복없이 추가할 수 있다.**

- List

```java
for(int i : nums){
    if(!types.contains(i)){
        types.add(i);
    }
}
```

- HashMap

```java
for(int i : nums){
    types.add(i);
}
```

위의 두 코드는 같은 요소를 포함한다.

**2. 위와 같은 원인으로 코드가 간결해진다.**

**3. 존재여부 확인시 성능향상**

`List`의 `contain` 메소드는 크기에 비례하는 시간(`O(n)`) 이 걸리지만 `HashSet`은 존재여부를 확인할때 상수 시간복잡도(`O(n)`)이 걸린다.

---

# reference
- [자바공식문서](https://docs.oracle.com/javase/8/docs/api/)
- [챗지피티](https://chat.openai.com/)