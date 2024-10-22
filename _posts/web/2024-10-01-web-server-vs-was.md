---
title: "[WEB] Web Server와 WAS"
category: Web
tags: WEB Web-Server WAS Web-Application-Server 면접대비
---
최근 리마인드하게된 Web Server와 WAS(Web Application Server)

---

# 1. Static pages와 Dynamic pages

Web Server와 Web Application Server를 알기 전에 Static pages와 Dynamic pages를 이해하면 좋다.

## Static pages(Web Server)

<img src="https://github.com/user-attachments/assets/163b29e3-0089-4511-a33d-aae2b76967fd" width="300">

- 정적 페이지, 말그대로 정적인 페이지(컨텐츠)들을 가져온다.
- 여기서 Web Server는 파일 경로의 이름을 받아 경로와 일치하는 file Content를 반환한다.
- 그래서 항상 동일한 페이지를 반환한다. (해당 파일을 직접 변경하지 않는 이상)

## Dynamic pages(WAS)

<img src="https://github.com/user-attachments/assets/c7a6737f-0012-4ff5-8aae-53e32c1d88a0" width="300">

- 인자(파라미터)의 내용에 맞게 동적인 contents를 반환한다.
- 웹 서버에 의해서 실행되는 프로그램을 통해서 만들어진 결과물을 반환한다.
    
    > Servlet: WAS 위에서 실행되는 Java Program
    > 
- 개발자는 Servlet에 doGet()을 구현한다. (Spring의 `@GetMapping`)

# 2. Web Server 와 Web Application Server

<img src="https://github.com/user-attachments/assets/853e024a-7ba3-47b4-805a-f5bc42263247" width="500">

## Web Server

### Web Server의 개념

- 하드웨어적으로는 Web 서버가 설치되어있는 컴퓨터를 의미
- 소프트웨어적으로는 **웹 브라우저 클라이언트로부터 HTTP요청을 받아 정적 리소스(`.html`, `.css` 등)를 제공하는 컴퓨터 프로그램**

### Web Server의 기능

요청에 따라 두가지 기능중 적절히 선택하여 수행한다.

**기능 1)**

- 정적리소스 제공
- WAS를 거치지 않고 바로 자원 제공

**기능 2)**

- 동적리소스 제공을 위한 요청 전달 → WAS
- 클라이언트의 Request를 WAS에 보내고 WAS에서 처리한 결과를 클라이언트에게 Response한다.

### Web Server의 대표적인 예시

Apache Server, Nginx 등

### Web Server 작동방식

1. 브라우저(클라이언트)는 URL을 사용하여 서버의 IP주소를 찾는다.
2. 브라우저는 정보에 대한 HTTP 요청을 보낸다.
3. Web Server는 DB서버와 통신하여 관련 데이터를 찾는다.
4. Web Server는 HTTP 응답으로 HTML, 이미지, 비디오 등 정적 리소스를 브라우저에 반환한다.
5. 브라우저는 받은 정보를 표시한다.

블로그, 기사 등과 같은 정적 컨텐츠를 호스팅하는 웹사이트들을 Web Server를 통해 실행할 수 있다.

## WAS(Web Application Server)

### WAS의 개념

- **DB조회나 비즈니스 로직의 처리가 필요한 요청에 대해 동적 리소스를 제공하기 위해 만들어진 Application Server**
- HTTP를 통해 컴퓨터나 장치에 어플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)이다.
- **웹 컨테이너 또는 서블릿 컨테이너를 제공**한다. → **이러한 컨테이너에서 동적인 프로세스를 처리**한다.
    - 컨테이너란 JSP, Servlet을 싱행시킬 수 있는 소프트웨어이다.
    - 즉, WAS는 JSP, Servlet 구동 환경을 제공한다.

### WAS의 역할

- **WAS = Web Server + Web Container**
- Web Server의 기능들을 구조적으로 분리하여 처리하고자 하는 목적으로 제시되었다.
    - 분산 트랜잭션, 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산환경에서 사용된다.
    - 주로 DB서버와 함께 수행된다.

### WAS의 주요기능

1. 프로그램 실행 환경(컨테이너) DB접속기능 제공
2. 여러 트랜잭션(논리적인 작업단위) 관리 기능
3. 업무 처리를 위한 비즈니스 로직 수행

### WAS의 대표적인 예시

Tomcat, JBoss, Jeus, Web Sphere 등

### WAS 작동방식

1. 브라우저(클라이언트)가 URL을 사용하여 서버의 IP주소를 찾는다.
2. 브라우저는 정보에 대한 HTTP요청을 보낸다.
3. Web Server는 요청을 WAS로 전송한다.
4. WAS는 비즈니스 로직을 적용하거나 다른서버 및 서드파티 시스템과 통신하여 요청을 수행한다.
5. WAS는 새 HTML페이지를 렌더링하고 이를 응답으로 Web Server에 반환한다.
6. Web Server는 브라우저에 응답을 반환한다.
7. 브라우저가 정보를 표시한다.

# 3. Web Server와 WAS의 차이

그렇다면 둘의 차이는 뭘까

- Web Server
    - 클라이언트에게 HTTP 요청을 받아 **정적 리소스를 제공**
    - 동적인 처리가 필요한경우 WAS에 요청을 보내줌
- Web Application Server
    - HTTP 요청을 받는 Web Server와 동적인 처리과정을 담당하는 **Web(Servlet) Container**를 함께 포함하고 있어 **동적인 리소스를 제공**한다.

# 4.  Web Server 와 WAS를 분리하는 이유

## Web Server가 필요한 이유

- 클라이언트(웹 브라우저)에 이미지파일(정적 리소스)을 보내는 과정이라면
    - 이미지 파일과 같은 정적인 파일들은 웹 문서(HTML)가 클라이언트로 보내질 때 함께 가는것이 아니다.
    - 클라이언트는 HTML을 먼저 받아오고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하여 그때서야 이미지파일을 받는다.
    - Web Server를 통해 정적인 파일들을 Application Server 까지 가지 않고 앞단에서 빠르게 보내줄 수 있다.
- 따라서 Web Server에서는 **정적 리소스만 처리하도록 기능을 분배**하여 서버의 부담을 줄일 수 있다.

## WAS가 필요한 이유

- 웹페이지는 정적 리소스와 동적 리소스가 함께 존재한다.
    - 사용자 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야한다.
    - 이때, Web Server만을 이용하게되면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어놓고 서비스해야한다
    - 하지만 이는 불가능에 가깝고 자원이 절대적으로 부족하다.
- 따라서 WAS를 이용해 요청에 맞는 데이터를 DB에서 가져와 비즈니스 로직에 맞게 그때 그때 결과를 만들어 제공함으로써 자원을 효율적으로 사용할 수 있다.

## WAS가 Web Server기능까지 수행하면 안되나?

1. 기능을 분리하여 서버 부하 방지
    - WAS는 DB조회나 비즈니스 로직을 처리하느라 바쁘기 때문에 단순한 정적 리소스는 Web Server에서 빠르게 클라이언트에게 제공하는 것이 효율적이다.
    - 정적 리소스까지 WAS에서 처리하게 된다면 정적 리소스처리로 인해 부하가 커지고, 동적 리소스의 처리가 지연되어 수행속도가 느려진다.
    - 즉, 이로 인해 페이지 노출 시간이 늘어나게 된다.
2. 물리적으로 분리하여 보안 강화
    - SSL에 대한 암복호화 처리에 Web Server를 사용
3. 여러대의 WAS를 연결 가능
    - Load Balancing을 위해 Web Server를 사용 할 수 있다.
    - fail over(장애 극복,조치), fail back(장애 복구) 처리에 유리하다.
    - 대용량 웹 어플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장래극복에 쉽게 대응할 수 있다.
    - 예를 들어, Web Server에서 오류가 발생한 WAS를 이용하지 못하게 하고 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.
4. 여러 웹 어플리케이션 서비스 가능
    - 예를 들어, 하나의 서버에 PHP, JAVA application을 함께 사용하는 경우
5. 기타
    - 접근 허용 IP관리, 2대 이상의 서버에서의 세션관리 등도 Web Server에서 처리하면 효율적이다.

### 요약

**자원 이용의 효율성, 장애극복, 배포 및 유지보수의 편의성** 을 위해 Web Server 와 WAS를 분리한다.

# 출처

---

[굉장히 잘 정리된글](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)

[잘 요약된 글](https://hstory0208.tistory.com/entry/Web과-WAS란-웹-서비스의-구조에-대해-알아보자)

[AWS제공, 웹서버와 WAS의 동작과정이 잘 정리됨](https://aws.amazon.com/ko/compare/the-difference-between-web-server-and-application-server/)

