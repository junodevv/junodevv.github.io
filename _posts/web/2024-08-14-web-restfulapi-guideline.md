---
title: "[WEB] RESTful 작성 가이드라인"
category: Web
tags: RESTful RESTfulAPI
---
SsaGeMeogJa 프로젝트를 진행하면서 공부한 RESTful 작성에 대한 가이드라인

---
# 서론

API를 설계하면서 항상 내가 설계하고 있는 이것이 맞는것인가에 대해 고민을 해왔었는데 이번 프로젝트를 계기로 어렴풋이 들어왔던 RESTful API 설계에 대해서 공부하게 되었다.

우선 RESTful API는 표준이 없었다. 하지만 수많은 선배개발자들의 경험에 의한 가이드라인들이 있었다.

# RESTful API 설계 가이드라인

## 1.  용어

- Resource: 객체, 데이터의 표현 
ex) carts, dogs, cats 등등
- Method: 삭제, 추가, 업데이트 등 리소스에서 수행되는 작업
- Collections: Resource의 집합
ex) company==Resource, companies==Collections
- URL(Uniform Resource Locator): Resource를 찾을 수 있는 경로, 해당 Resource에서 메서드에 따라 작업을 수행

## 2.  API 엔드포인트

### API엔드포인트의 잘못된 예시 (장바구니 - 상품)

- `/getAllProducts`
- `/addNewProduct`
- `/updateProduct`

이러한 API들은 무수히 많이 생겨날 수 있다. 그리고 동일하거나 유사한 작업을 수행하는 API가 여러개 존재하게 될 수 있다. 그렇게 API 수가 증가할 수록 유지보수가 부담스러워질 수 있다.

### 올바른 방식

URL에는 리소스만 포함되어야하고 행동이나 동사는 포함되지 않아야한다. 하지만 위의 잘못된 예시에는 리소스 이름과 함께 액션(get, add, update)등이 포함되어있다.

옳은 예: `/products` → 엔드포인트에 아무런 액션(동작)이 포함되지 않고 리소스만 포함되어있다.

하지만 어떻게 `/products` 리소스에서 수행할 동작을 서버에 전달할까 

이때 HTTP 메서드(GET, POST, DELETE, PUT 등)가 그 역할을 한다. 그래서 URL에는 동사(액션)를 포함할 필요가 없는 것이다.

리소스는 API엔트포인트에서 항상 복수형이어야 하며 리소스의 한 인스턴스에 접근하려는 경우 URL에 ID를 전달한다.

**예시**

- [GET] `/carts` → 모든 장바구니 목록을 가져온다.
- [GET] `/carts/34` → 장바구니 ID가 34인 장바구니의 세부정보를 가져온다.
- [DELETE] `/carts/34` → 장바구니 ID가 34인 장바구니을 삭제한다.

**리소스 하위의 리소스가 존재하는 경우**

- [GET] `/carts/3/products` → 장바구니3의 모든 상품목록을 가져온다.
- [GET] `/carts/3/products/45` → 장바구니3에 속한 상품 45의 세부정보를 가져온다.
- [DELETE] `/carts/3/products/45` → 장바구니3에 속한 상품 45를 삭제한다.
- [POST] `/carts` → 새 장바구니를 만들고 새로만든 장바구니의 세부정보를 반환한다.

**결론**: **Resource(리소스)**에는 **복수형 명사**를 사용하고 **HTTP 메서드**를 활용해 해당 리소스에 대해 **수행할 동사**의 종류를 결정한다. URL은 명사(리소스)+동사(HTTP메소드)인 하나의 문장이다.

## 3. HTTP 메서드(동사)

- `GET` : 리소스 조회. 리소스에 대한 데이터를 요청. GET 메서드는 데이터를 가져올때만 사용.
- `POST` : 주로 데이터베이스에 리소스를 **생성** 하도록 서버에 요청. 서버로 데이터를 전송하여 데이터베이스에 새로운 리소스를 생성(등록)할 때 주로 사용.
- `PUT` : 요청 데이터를 사용하여 해당 데이터로 리소스를 업데이트, 리소스가 존재하지 않으면 새로운 리소스를 생성
- `PATCH` : 리소스를 부분적으로 변경.
- `DELETE` : 해당 리소스 삭제

**HEAD** : GET 메서드와 동일하지만, 상태 줄과 헤더만 반환받음.

**CONNECT** : 요청한 리소스에 대해 양방향 연결을 시작하는 메소드. 터널을 열기 위해 사용.

**OPTIONS** : 목표 리소스와의 통신 옵션을 설명하기 위해 사용됨. (주로 CORS에서 사용)

**TRACE** : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행.

## 4. HTTP 응답 상태 코드

https://developer.mozilla.org/ko/docs/Web/HTTP/Status

서버는 클라이언트가 요청한 API에대해 성공 했는지 실패 했는지에 대한 피드백을 주어야한다.
HTTP 상태코드는 다양한 시나리오에 대한 설명이 있는 표준화된 코드모임이다. 이는 회사마다, 프로젝트마다 다를 수 있다. 하지만 어느정도 표준은 있다고 한다.

### 200번대(성공)

요청된 작업이 성공적으로 수신되어 처리된 경우.

- **200 OK** | GET, POST 또는 PUT에 대한 성공을 나타내는 표준 HTTP응답
- **201 Created** | 새 인스턴스가 생성된 경우
- **204 NoContent** | 요청이 성공적으로 처리되었지만 반환된 컨텐츠가 없는 경우 
ex) DELETE를 한 경우, DELETE `/carts/3/products/45` 상품45를 삭제한 후 삭제한 데이터를 보내줄 필요는 없다.

### 300번대(리디렉션)

- **304 Not Modified** | 클라이언트가 이미 캐시에 응답을 가지고 있으므로 동일한 데이터를 다시 전송할 필요가 없다.

### 400번대(클라이언트 오류)

클라이언트가 잘못된 요청을 했을 경우

- **400 Bad Request** | 서버측에서 클라이언트가 무엇을 요청하는지 알 수 없어 클라이언트의 요청이 처리되지 않은경우
- **401 Unauthorized** | 클라이언트가 리소스에 접근할 수 없음을 나타낸다. 필요한 자격 증명을 가지고 다시 요청해야한다.
- **403 Forbidden** | 요청이 유효하고 클라이언트 인증이 되었지만 클라이언트가 다른 어떤 이유로 페이지나 리소스에 접근할 수 없는 경우
- **404 Not Found** | 요청된 리소스를 현재 사용할 수 없는 경우

### 500번대(서버 오류)

- **500 Internal Server Error** | 요청은 유효하지만 서버 오류로 처리하지 못한 경우
- **503** **Service Unavailable** | 서버가 다운되었거나 요청을 수신하고 처리할 수 없음을 나타냄, 보통 서버가 유지 관리 중일 때 사용된다.

## 5. 필드 이름 대소문자 규칙

어떤 규칙이든 따를 수 있지만 한 어플리케이션 내에서는 통일된 방식을 사용해아한다. 요청 본문이나 응답 데이터 형식이 JSON이라면 보통 camelCase를 따른다.

## 6. 검색, 정렬, 필터링, 페이지 번호

이러한 복잡해지는 리소스에 대한 매개변수, 속성 등은 쿼리 매개변수를 활용해준다.

### 정렬

쿼리 매개변수를 활용한다. 

ID에 따라 오름차순으로 정렬된 장바구니 목록을 가져오고자 하는 경우 → [GET] `/carts?sort=id_asc`

### 필터링

쿼리 매개변수 활용하여 옵션 전달

1번 장바구니에서 채소만 가져오는 경우 → [GET] `/carts/1/products?category=vegetable`

### 검색

1번 장바구니 상품 중 “당근”을 검색하는 경우 → [GET] `/carts/1/products?search=당근`

### 페이징

방식1: paga 사용

- 1번 장바구니에서 23번째 페이지의 상품 조회 → [GET] `/carts/1/products?page=23`

방식2: 상대위치(offset)과 범위(limit) 사용

- 1번 장바구니에서 23번째 페이지의 상품 10개 조회 → [GET] `/carts/1/products?limit=10&offset=23`

## 7. 버전관리

API를 업그레이드 하게되는 경우 버전을 명시해줄 수 있다.

예시 `https://api.myservice.com/**v1**/carts/1/products` 

## 8. 기타

### 부분응답

- 리턴을 원하는 특정 필드를 지정하려면 쉼표로 구분된 목록을 사용
- ex) `/carts/1/products?fields=name,image`

### 데이터베이스에 없는 자원에 대한 응답

- 리소스에 대한 응답이 아닌경우 동사를 활용한 요청을 보낸다.
- 계산, 번역, 변환 등 알고리즘 계산이나 번역, 환율 변환과 같은 요청은 동사를 활용한다.
- ex) `/convert?from=EUR&to=CNY&amount=100`

### 출처

<a href="https://www.mimul.com/blog/web-api-design-from-apigee/?fbclid=IwAR1Q5QmdYTLpgc9c5w9IKQM2i49JBsNuEwyJHntYjWytEYQK89m2fJUMPAE" target="_blank">
    Web API Design : 개발자에게 사랑받는 API 만들기, by Mimul
</a>

<a href="https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9" target="_blank">
    RESTful API Designing guidelines - The best practices, Mahesh Haldar
</a>