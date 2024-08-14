---
title: "[MySQL] 날짜 포맷 함수, DATE_FORMAT()"
category: SQL
tags: mysql date-format
---
좀 지나면 까먹는 MySQL DATE_FORMAT() 사용 하는 법

---
<img width="743" alt="image" src="https://github.com/user-attachments/assets/ef28620d-77d9-4679-8585-83ce47af6e6e">

포맷문자들

위 표에 있는 포맷문자들과 SQL을 작성하는 개발자가 원하는 구분자(`-`, `.`, `/` 등)를 조합하여 여러 형식의 날짜 포맷을 만들 수 있다.

```sql
SELECT DATE_FORMAT('포맷할 컬럼', '원하는 포맷 형식') 
FROM table;
```

예시

```sql
SELECT DATE_FORMAT('REGISTER_DATE', '%Y-%m-%d')
FROM table;
```

결과예시

```sql
2024-08-14
```

### 출처
<a href="https://lanuarius19.tistory.com/entry/MySQL-날짜-포맷-사용법-정리-DATEFORMAT-함수" target="_blank">
    https://lanuarius19.tistory.com/entry/MySQL-날짜-포맷-사용법-정리-DATEFORMAT-함수
</a>
