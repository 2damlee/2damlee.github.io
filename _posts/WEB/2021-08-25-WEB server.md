---
title: "WEB Server"
categories:
  - Web
layout: archive
sidebar_main: true
author_profile: true
tag:
  - web
---



<br>

<br>

웹 서버 구조를 다시 살펴보자 😭

Port(포트): 클라이언트에서 서버로 통하는 문이 있는데, 이 문들의 각자 고유한 번호라고 이해하면 된다. IP가 아파트 '동'까지의 주소라면 포트는 '호'라고 보면 된다. 

그리고 보통 생략이 가능하다. http://a.com:80 에 접속한다면, 서버는 a.com에 해당하는 컴퓨터를 찾은 뒤 80번 포트에서 리스닝하고있는 웹서버가 응답한다. 그러나 http는 웹브라우저를 통해 접속했음을 알려주고, 자동으로 80번 포트에 연결시켜준다. <u>http 접속은 곧 80번 포트를 쓰기로 약속</u>한 것. 

**HTTP(Hyper Text Transfer Protocol)**



<br>

**Static Pages,  Dynamic Pages**

- static pages: Port 80(Apache Server)
  - 

- Dynamic pages: Port 8080(Tomcat)
  - 인자에 따라 변화 (동적인 contents 반환)
  - 웹 서버에 의해 실행되는 프로그램을 통해 만들어진 결과물을 반환하는 것
    - WAS(Web Application Server) 위에서 돌아가는 Java Program = servlet
    - 개발자는 Servelt에 doGet()을 구현
      - 요청(request)에 따른 응답(request)을 구현하는 것

<br>

**WEB server와 WAS**

- Web Server

  - 소프트웨어와 하드웨어로 구분
    - 하드웨어: Web server가 설치되어있는 컴퓨터
    - 소프트웨어: 웹 브라우저(클라이언트)로부터 HTTP 요청을 받아 정적인 컨텐츠(html, images, css, js 등)를 제공하는 컴퓨터 프로그램

- Web Server의 기능

  - HTTP 프로토콜을 기반으로 하여 클라이언트의 요청을 서비스하는 기능

    - 기능 1) 
      - 정적인 contents 제공
      - WAS를 거치지 않고 바로 자원 제공
    - 기능 2)
      - 동적인 컨텐츠 제공을 위한 요청 전달(데이터 전달)
      - 클라이언트 요청(request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 답(response)한다. 

    Ex) Apache Server, Nginx, IIS 등

  <br>

**WAS(Web Application Server)**

- DB조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위한 것 
- HTTP를 통해 컴퓨터나 장치에 어플리케이션을 수행해주는 소프트웨어 엔진
- Web Container 혹은 Servlet Container라고 함
  - Container: JSP, Servlet을 실행시킬 수 있는 소프트웨어
  - 주로 DB서버와 같이 수행

- 기능

  1) 프로그램 실행 환경과 DB접속 기능 제공
  2) 여러 개의 트랜잭션 관리 기능
  3) 비즈니스 로직 수행

  Ex) Tomcat, JBoss, Jeus, Web Sphere 등 

<br>

<br>

WEB과 WAS를 분리해야 하는 이유? 

- 서버 부하 방지 
  - WAS는 데이터 조회 등의 로직 처리에 시간이 걸리기 때문에 단순한 정적 컨텐츠는 기능을 분리하는 것이 좋다. 
- 물리적으로 분리하여 보안 강화
  - <u>SSL</u>에 대한 암복호화 처리에 Web Server 사용

- 여러 대의 WAS를 연결 
  - Load Balancing을 위해 Web Server 사용 (로드밸런싱!)
  - fail over, fail back 처리에 유리 (장애 관련)
  - 특히 여러 개의 서버를 사용하는 대용량의 경우, 분리할 시 운영 중단에 대한 위험 방지 가능

-> 결론: 자원 이용의 효율성과 장애 극복, 배포 및 유지보수의 편의성을 위해 분리. 