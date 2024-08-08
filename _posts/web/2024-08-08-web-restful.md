---
title: "[WEB] RESTful"
category: Web
tags: RESTful RESTfulAPI
---
SsaGeMeogJa 프로젝트를 진행하면서 공부한 RESTful에 대한 이론

---

# 서론

이전에는 API를 설계하면서 어떤식으로 작성하는 것이 좋은 API인지 알지 못하고 설계해왔고 항상 이렇게하는게 가독성이 좋은가? API는 만드는 사람마음인가? 하는 생각을 하며 API를 설계해왔습니다. 하지만 이번 프로젝트를 진행하면서는 frontend팀원들과의 협업 및 유지보수 측면에서 API를 개선해야겠다고 생각했고 RESTful API를 공부하게 되었습니다.
<br>어렴풋이 들어보기만 했던 RESTful API에 대해서 제대로 알아야 겠다고 생각하여 공부해본 RESTful 이론에 대해서 정리해보겠습니다.

# REST란

REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미합니다.

### **즉 REST란**

1. HTTP URI(Uniform Resource Identifier)를 통해 **자원(Resource)을 명시**하고,
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

## **REST 구성 요소**

REST는 다음과 같은 3가지로 구성이 되어있다.

1. **자원(Resource) : HTTP URI**
2. **자원에 대한 행위(Verb) : HTTP Method**
    - GET, POST, PATCH, PUT, DELETE 등
3. **자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load**
    - 클라이언트에게 HTTP Method로 요청이 왔을때 **그에 대한 응답(JSON, XML 등)**

## REST의 특징

- Uniform Interface (인터페이스의 일관성)
    - HTTP표준에만 따른다면 특정 언어나 기술에 종속되지 않고 모든 플랫폼에 사용할 수 있다. URI로 지정한 리소스에 대한 조작이 가능한 아키텍처 스타일을 의미한다.
- Stateless (무상태성)
    - 상태정보를 따로 저장하고 관리하지 않는다. 세션,쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API는 요청만을 단순히 처리하면된다. 그래서 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.
- Cacheable (캐시 가능)
    - HTTP라는 기존 웹 표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용할 수 있다. 따라서 HTTP가 가진 캐싱 기능이 적용할 수 있다.
- Self-descriptiveness (자체 표현 구조)
    - REST API 메시지만 보고도 이를 쉽게 이해할 수 있도록 자체 표현구조로 되어있다.
    - JSON, XML등의 표현구조를 보고 메시지가 무엇을 어떤행위를 의미하는지 직관적으로 이해할 수 있다.
- Client - Server 구조
    - REST서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보) 등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로 간 의존성이 줄어들게 된다.
- Layered System (계층형 구조)
    - REST 서버는 다중 계층으로 구성될 수 있다. 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.
    - 클라이언트는 서버와 직접통신하는 것인지 중간의 어떤 서버와 통신하는지 알 수 없다. 따라서 클라이언트와 서버의 통신사이에 보안, 로드밸런싱 등의 계층을 추가할 수 있다.

## REST의 장단점

### 장점

1. HTTP 프로토콜의 인프라를 그대로 이용하기 때문에 별도의 인프라를 구축할 필요가 없다.
2. HTTP 프로토콜의 여러 장점을 함께 가져갈 수 있다.
3. HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용가능하다.
4. Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
5. REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
6. 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
7. 서버와 클라이언트의 역할을 명확하게 분리한다.

### 단점

1. 표준이 존재하지 않는다 → 하지만 여러 가이드가 존재
2. HTTP 메소드의 형태가 제한적이다.
    - CRUD(조회,생성,수정,삭제) 이외에 취소, 승인 등등 좀더 복잡한 과정에 대한 설계가 RESTful의 일관성을 해칠 수 있다.
3. 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
4. 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다. ex) 익스플로러

### 출처

- [https://khj93.tistory.com/entry/네트워크-REST-API란-REST-RESTful이란](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)

- [https://www.incodom.kr/REST](https://www.incodom.kr/REST)

- [https://tibetsandfox.tistory.com/19](https://tibetsandfox.tistory.com/19)