---
title: "[SQL] SELECT 쿼리의 실행순서"
category: SQL
tags: sql
---
SELECT 쿼리의 실행순서

---
기술면접에서 SELECT 쿼리의 실행순서를 질문 받았는데 대답하지 못했다.<br>
알아보니 실행순서는 쿼리성능의 개선과 문법 오류의 최소화를 위해 알아야할 중요한 문제인것 같다.

# 1. SELECT쿼리 실행순서

FROM and JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT (프조 웨 그해 셀오리)

# 각 단계의 동작

### FROM(+ JOIN)

조회할 테이블을 정의하고 JOIN이 있다면 JOIN을 실행한 하나의 가상테이블을 생성한다.

### WHERE

**조건에 맞는 데이터를 필터링**한다.

WHERE절은 FROM(JOIN)절의 결과(테이블)을 이용해 필터링한다.

### GROUP BY

**선택한 컬럼을 기준으로 조회한 레코드 목록을 그룹핑**한다.

### HAVING

그룹핑후 **각 그룹에 사용되는 조건절**, 그룹을 필터링 하는것(그룹핑된 테이블에 대한 WHERE 절)

<aside>
💡

HAVING절의 조건을 WHERE절에도 사용할 수 있는 경우라면 WHERE절에 사용하는것이 바람직하다.
HAVING절은 각 그룹에 조건을 걸기 때문에 퍼포먼스가 떨어지게 된다.
ex) MONEY > 10000은 모든 레코드에 MONEY가 10000이 넘어야한다는 조건이다. 이는 각 그룹에 따로 거는 것보다는 WHERE절로 한번에 거는것이좋다.

</aside>

### SELECT

여러 조건들을 처리한 후 남은 데이터에서 **어떤 열을 출력할지 선택**한다.

### ORDER BY

어떤 열까지 출력할지 정했다면 행의 순서를 어떻게 보여줄지 **정렬**해준다.

### LIMIT

결과중 **몇개의 행을 보여줄지** 선택한다.

# 2. 실행순서가 중요한 이유

쿼리의 실행순서를 아는 것은 쿼리를 효과적으로 작성할 수 있게 해준다.

## 1. 문법

### ORDER BY절에서 Alias 사용 (O)

ORDER BY절은 SELECT절보다 나중에 실행되기 때문에 SELECT절의 결과를 사용할 수 있다.

```sql
SELECT CONCAT(first_name, last_name) AS full_name
FROM user
ORDER BY full_name;
```

### WHERE 절에서 Alias 사용 (X)

WHERE절은 SELECT절 보다 먼저 실행된다. 따라서 WHERE절에서는 SELECT문에서 지정한 Alias를 활용할 수 없다.

```sql
SELECT CONCAT(first_name, last_name) AS full_name
FROM user
WHERE full_name = '이준오';
```

이렇게 WHERE절에서 Alias를 사용하려다가 원치 않는 결과를 받거나, ORDER BY절에서 SELECT절에서 사용한 함수를 다시 한번 호출하여 자원이 남비되는 이슈를 막으려면 실행순서에 대한 이해가 필요하다.

## 2. 성능

SELECT 쿼리의 실행동작을 시각화한 이미지

<img src="https://github.com/user-attachments/assets/ce5d1ef8-9547-4103-bf4b-5d0ebaa701f4" width="600">

JOIN할 테이블이 많고 레코드 수도 매우 많다면 수 많은 레코드를 가지고 WHERE, GROUP BY, ORDER BY등을 수행하게 된다.

이때 SQL 실행순서를 이해하고있다면 LIMIT절을 포함해 먼저 ID만을 필터링한 후 복잡한 JOIN을 포함한 나머지 컬럼 조회는 필터링된 레코드의 ID만을 가지고 후속 쿼리에서 실행할 수도 있다.

그러면 실제로 검색할 때 데이터의 행, 열 모두 크기가 매우 작아지므로 부하가 감소된다고 한다. 무거운 쿼리 1개를 가벼운 쿼리 2개로 나눴지만, 실행시간이나 부하는 10배 개선될 수도 있다고 한다.

이렇듯 SQL 실행순서를 이해하고 있다면 쿼리의 성능이 낭비되는 지점이 잘 보이고 튜닝을 할 수 있게 된다.

# 팁

SQL을 실행순서대로 작성하는 것.

실행순서대로 작성하다보면 중간에 성능적으로 튜닝 할 수 있는 요소를 발견할 가능성이 높다고 한다.

# 출처

https://jaehoney.tistory.com/191