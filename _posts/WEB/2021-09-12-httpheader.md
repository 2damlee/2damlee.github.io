---
title: "HTTP Header"
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

### HTTP

- Hyper Text Transfer Protocal

  : 인터넷에서 데이터를 주고받을 수 있는 통신 규약 

<br>

Request(Client)와 Response(Server)로 통신

### Request

예를 들어 http://asdf.asdf.com에 대한 정보를 달라고 요청한다면 서버에 요청을 보낼 때의 구성은 다음과 같다.

````
GET /tutorials/other/top-20-mysql-best-practices/ HTTP/1.1
Host: code.tutsplus.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.1.5) Gecko/20091102 Firefox/3.5.5 (.NET CLR 3.5.30729)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive: 300
Connection: keep-alive
Cookie: PHPSESSID=r2t5uvjq435r4q7ib3vtdjq120
Pragma: no-cache
Cache-Control: no-cache
````

* 시작줄 (GET/HTTP/1.1(version))
* 헤더 (요청에 대한 정보)

<br>

### Response

````
HTTP/1.x 200 OK
Transfer-Encoding: chunked
Date: Sat, 28 Nov 2009 04:36:25 GMT
Server: LiteSpeed
Connection: close
X-Powered-By: W3 Total Cache/0.8
Pragma: public
Expires: Sat, 28 Nov 2009 05:36:25 GMT
Etag: "pub1259380237;gz"
Cache-Control: max-age=3600, public
Content-Type: text/html; charset=UTF-8
Last-Modified: Sat, 28 Nov 2009 03:50:37 GMT
X-Pingback: https://code.tutsplus.com/xmlrpc.php
Content-Encoding: gzip
Vary: Accept-Encoding, Cookie, User-Agent
 
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "https://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Top 20+ MySQL Best Practices - Nettuts+</title>
<!-- ... rest of the html ... -->
````

- 시작줄 HTTP/1.1 <u>200 OK</u>  - 요청 성공 

- 헤더 정보
- 본문

<br>

### HTTP method

request시에 주소와 함께 메소드를 함께 보낼 수 있다.

| method | description                                                  |
| ------ | ------------------------------------------------------------ |
| GET    | 지정된 리소스(URI) 요청                                      |
| POST   | 클라이언트 폼 입력 필드 데이터 수락을 요청. server의 HTTP Body에 Data를 전송 |
| HEAD   | 문서의 헤더 정보만 요청, 응답 데이터(body)는 받지 않음.      |
| PUT    | 클라이언트가 전송한 데이터를 지정한 URI로 대체               |
| DELETE | 클라이언트가 지정한 URI를 서버에서 삭제                      |
| TARCE  | 클라이언트가 요청한 자원에 도달하기까지의 경로를 기록하는 loop back 검사 |

<br>

### HEADER

**공통 헤더** : 요청과 응답에 모두 사용되는 헤더

- Date: HTTP massage가 생성된 시각 (자동 생성)

- Connection: 기본적으로 keep-alive로 되어 있음

- Content-Length: 요청, 응답 메세지의 본문 크기를 바이트 단위로 표시 (자동 생성)

- Cache-Control: 

- Content-Type: 컨텐츠의 타입과 문자열 인코딩 명시 가능 (ex. Content-Type: text/html; charset=utf-8)

- Content-Language: 사용자의 언어 

- Content-Encoding: 컨텐츠가 압축된 방식

  <br>

**요청헤더**

- Host: 서버의 도메인 이름. host헤더는 반드시 하나만 존재한다.

- User-Agent: 현재 사용자가 어떤 클라이언트를 통해 요청 보냈는지 확인 가능하다. (ex. User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36) 운영체제, 앱, 브라우저 등의 정보가 담겨있다. 접속자 및 사용기기 통계에 활용되기도 한다. 

- Accept: 클라이언트가 허용 가능한 파일 형식

  - Accept로 원하는 형식을 보내고, 서버가 응답하면서 헤더의 Content를 설정하게 된다. 

  ex. 

  ````
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
  Accept-Encoding: gzip, deflate
  Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
  ````

- Cookie: 서버가 클라이언트에 쿠키를 저장해 놓았다면 해당 쿠키의 정보를 <u>이름-값</u>(쿠키는 map형식으로 구성) 쌍으로 웹서버에 전송한다. 

- Origin: POST등의 요청을 보낼 때 요청이 어느 주소에서 시작되었는가를 나타낸다. 보낸 주소와 받는 주소가 동일하지 않으면 문제가 발생.

- If-Modified-Since: 페이지가 수정되었으면 최신 버전 페이지를 요청(<a href = "/_posts/WEB/2021-09-08-cache .md">Cache</a>)한다. 이 요청을 위한 필드이다. 변경이 없다면, 데이터를 전송받지 않는다.

- Authorization: 인증 토큰 서버로 보낼 때 사용하는 헤더이다. 가령 API요청을 할 때 토큰이 없다면 거절당한다. 주로 JWT(Json Web Token)을 사용한 인증에서 사용한다. (ex. Kakao map)



<br>

**응답헤더**

- Server: 웹서버 정보
- Access-Control-Allow-Origin: Origin과 같이 req Host 와 res Host가 다르면 CORS에러가 발생하는데 이 헤더에 프론트 주소를 적어주면 에러X
- Allow: CORS요청 외에도 적용된다. (ex. Allow:GET -> GET요청만 받음)
- Content-Disposition: 응답 본문을 브라우저가 어떻게 표시해야하는지 알려주는 헤더 (ex. inline: 웹페이지 화면에 표시, attachment: 다운로드)
- Location: response가 300이거나 201Created일 때 어느 페이지로 이동할지 알려주는 헤더
- Content-Security-Policy: 외부 파일을 불러오는 경우, 차단하거나 허용할 소스를 명시할 수 있다. 