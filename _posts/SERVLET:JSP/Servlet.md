# Servlet



##### 클라이언트-서버 기반 프로그램 동작 방식

| http client                                                  | http server                                              |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| 1. 브라우저<br />url 입력 - 내용 실행 표현 - html<br /><br />4. 브라우저 표현 출력 | 2. 요청 분석<br />3. html파일을 찾아 클라이언트에게 전송 |

> 그러나 브라우저에서 db데이터나 서버 내부 파일 정보의 열람 및 작동은 html에서 할 수 없다. 영구적으로 저장해야하는 db를 위해, 자바언어를 이용해 jdbc.웹 서비스를 html 클라이언트에게 보내야 한다. 자바소스 실행은 웹브라우저가 실행하지 못하고, 웹서버가 자바소스를 실행하는 방법이 필요. 

| http client                                                  | http server                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 브라우저<br />url 입력 - 내용 실행 표현 - html<br /><br />5. 브라우저 표현 출력<br />(db 데이터, 서버내부 파일 정보 등) | 2. 요청 분석<br />3. 자바소스를 실행한다<br />4. 실행 결과를 전송한다 |

> 서버는 파일을 찾아서 (결과값을) 주고, 브라우저는 그것을 실행한다.



#### 웹어플리케이션

: 웹서버에서 요청 받은 파일을 실행하고 그 결과물을 html파일에 넣어 표현해주는 프로그램

| 웹클라이언트 실행 프로그램<br /> =웹브라우저 실행 | 웹 서버 실행 프로그램<br /> = 요청(url) + 자바 실행<br />=tomcat, apache, iis, oracle web server |
| ------------------------------------------------- | ------------------------------------------------------------ |
| html / css / javascript/ jQuery                   | asp / asp.net / php /cgi <br />이 중에 자바 작성, 웹서버 실행 : **servlet / jsp** |

* 웹어플리케이션은 웹서버에서 무언가 실행시켜주고 실행된 결과물을 html파일에 넣어 브라우저에게 전송. 이 때 호출되는 프로그램이 '웹 어플리케이션

  * 이클립스에서는 이를 만들어주는 것이 dynamic web project = 웹어플리케이션 저장 묶음 

* 웹서버(=tomcat) :외부로 요청을 받고 응답.  사이에 처리해주는 기능은 없음. html파일만 가능 

  웹어플리케이션 서버(=tomcat) : 요청받은 내용을 서버 내부에서 실행.  = 웹 어플리케이션 컨테이너 = 컨테이너 

  

##### WebContainer 

> * 웹컨테이너는 웹서버와 함께 존재 가능, ex. tomcat은 간이 웹서버 역할을 한다.
> * 웹컨테이너는 정적 페이지 처리 뿐 아니라 동적페이지에 대한 요청도 가능하다. 
> * 웹컨테이너는 HTTP request, HTTP response에 해당되는 객체를 생성한다.
> * 웹컨테이너는 요청URL에 대응되는 Servlet객체의 service 메소드를 호출함으로서 servlet을 활성화시킨다. 



##### Dynamaic Web Procject 구성

> eclipse를 통해 servlet/ jsp를 사용할 것

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-07%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.58.29.png" alt="스크린샷 2021-04-07 오전 9.58.29" style="zoom: 33%;" />

* src/*.java 폴더에는 자바 파일만 넣는다
  * library/tomcat libarary + jdk library : 기본 라이브러리
* webContent/*.html 에는 html css js gif mp3 ...   + *.jsp
  * WEB-INF/lib : 파일 위치 옮기지 말 것, 기본 라이브러리 외 추가 라이브러리파일은 이 위치에 넣는다
    * web.xml : 서블릿과 jsp 설정과 관련된 내용이 있다





## Servlet

> 서블릿은 자바로 작성, 톰캣과 같은 jsp/servlet 컨테이너에서 실행





* 서블릿 상속 

  * 서블릿이 상속받을 수 있는 것들은 다음과 같다.

    | 클래스/인터페이스 이름             |                                                              |
    | ---------------------------------- | ------------------------------------------------------------ |
    | class A extends **HttpServlet**    | http 요청에 특화되어진 처리만 할 수 있다. 원하는 것을 오버라이딩하 (ex. doGet/doPost 오버라이딩) |
    | class B extends **GenericServlet** | Servlet 메소드 일부 구현/나머지 오버라이딩                   |
    | class C implements **Servlet**     | 5개 메소드 모두 오버라이딩                                   |

  

  서블릿은 기본적으로 **HttpServlet**클래스를 상속받아야 한다. 

  <img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-07%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.31.25.png" alt="스크린샷 2021-04-07 오전 10.31.25" style="zoom:33%;" />



* 서블릿 매핑하기

  * 매핑의 이유는 간결성과 보안유지에 있다. 

    (클래스 이름 그대로 이름을 줄 시, 웹서버의 내부구조가 유출되기 때문)

    

  매핑방법

  1) web.xml에 태그를 생성

  ​	* dynamic web proect마다 하나씩 존재, 환경변수 설정이 되어 있음.

  * 버전 2 이전
  
  ````html
  <!-- servlettest.TestServlet 실제 클래스명 - > 다른 이름 호출 설정 -->
   <servlet>
   <servlet-name>test</servlet-name>
   <servlet-class>servlettest.TestServlet</servlet-class>
   </servlet>
  
  <servlet-mapping>
  <servlet-name>test</servlet-name>
  <url-pattern>/test</url-pattern>
  </servlet-mapping>
  </web-app>
  ````



​		2) servlet class를 통해 생성

​			 버전 4.0 이후 추가된 기능 

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-07%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.49.41.png" alt="스크린샷 2021-04-07 오전 10.49.41" style="zoom:33%;" />

 	annotation 필수

````
 @WebServlet("/test")
 @WebServlet("/매핑이름")
````

 

​	위와 같이 설정된다면 호출 시 

​		http://IP주소:포트번호/프로젝트이름(컨텍스트이름)/서블릿매핑이름

​	 으로 설정되게 된다. 

​		ex) http://localhost:8080/servlettest/test 



* 서블릿 LifeCycle (요청 - 실행 - 응답 과정) 

````
1. 클라이언트 1은 브라우저에서 Http://ip.port/프로젝트명/서블릿url 요청 

2. 서버는 웹서버가 웹어플리케이션 서버 쪽으로 url을 위임(자바를 실행할 능력이 x)

3. 위임받은 웹어플리케이션 서버가 메모리에 서블릿 객체가 로딩되어있지 않으면, 서블릿 객체 하나를 메모리에 로딩한다. 

5. init메소드를 호출한다 (overriding되어있다면) init: 서블릿 처리 이전을 초기화하는 것을 구현

4. 서블릿 객체를 멀티스레드형태로 실행한다. 
   * 수행하다가 다른 매서드를 수행하게 되면 여러개의 클래스가 같이 수행할 수 있는 형태

6. 요청 정보 HttpServletquest 객체와 응답 정보를 생성할 HttpResponse 객체를 생성한다.

7. doGet|doPost|Service 메소드 호출하여 (응답할 결과가 있다면) 응답한다.

7. 클라이언트 2은 브라우저에서 Http://ip.port/프로젝트명/서블릿url 요청

8. 서버는 웹서버가 웹어플리케이션 서버 쪽으로 url을 위임(자바를 실행할 능력이 x)
   * 이미 로딩이 된 상태라면  3-5번 생략
   
7. doGet|doPost|Service 메소드 호출하여 (응답할 결과가 있다면) 응답한다.
   
9. doGet 메소드 호출하여 (응답할 결과가 있다면) 응답한다. 
* 서블릿객체는 하나만 만들어지고 멀티스레드로 돌리는 것. 서블릿객체가 만들어질때 한 번 호출되는거고, doGet은 호출할 때 마다마다 응답하는 것

10. 서버 종료나 서블릿소스가 변경이되어 재컴파일시에 기존의 서블릿 객체를 메모리에서 삭제

11. destroy() 호출 : init 변수 메모리 삭제 구현
	  *서블릿 객체 메모리 삭제

12. 3번부터 다시 시작 
````

**doGet**: 서버 요청과 응답에 관한 내용, 오버라이딩이 필수. 응답할 결과가 있다면 객체 로딩 이후 호출할 때마다 응답

**init** : 서버에 메모리가 할당된 이후 처음 실행, 그 이후 응답하지 않음. 오버라이딩 필수 X

**destroy** : 서버가 종료되거나 소스가 변경되어 재컴파일 시 기존 객체 메모리에서 삭제, init 변수 메모리 삭제를 구현. 







#### 서블릿 요청과 응답 수행

* http 프로토콜의 규칙

  요청은 url의 형태를 가져올 것 

  * http://ip:port/경로/*.html
  * http://ip:port/경로/서블릿매핑url

  클라이언트는 서버에게 처리가 필요한 데이터 전송이 가능. 

  

* 방법: 

  ````java
  @WebServlet("/a")
  
  class A extends HttpServlet {
  
  //doGet메소드는 필수. 클라이언트 요청을 받아서 응답을 줘야 한다. 
  
  doGet(HttpServletRequest req, HttpServletResponse res){ // 요청과 응답의 두개의 매개변수 
  
  // 반복 , 요청받은 내용을 처리 , 응답(요청-처리-응답) 
  //HttpServletRequest req - ex. 요청(클라이언트 ip, id, pw form 태그 전송 받는 기능)
  /*HttpServletResponse res - ex.  처리(id, pw - member 테이블 id 존재여부/pw일치여부)
  		응답(id, pw 존재 -정상 응답, 비정상 응답 - 브라우저 출력 -html 파일 전송)*/
  
   	}
  
   }
  ````

  <img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.22.53.png" alt="스크린샷 2021-04-08 오전 9.22.53" style="zoom:50%;" />



예제 ) 

Html

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src = "jquery-3.2.1.min.js"></script>
<script>
</script>
</head>
<body>
<h1>로그인 폼</h1>
<form action = "login" method = "get"> <!--get은 메소드 생략 가능-->
아이디입력: <input type = text name = id><br>
암호입력: <input type=password name = pw><br>
로그인 가능 장소: 
<input type = checkbox name = "location" value="집" > 집
<input type = checkbox name = "location" value="학교" > 학교
<input type = checkbox name = "location" value="회사" > 회사
<input type = checkbox name = "location" value="기타" > 기타

<input type = submit value = "로그인버튼"><br>
</form>
</body>
</html>
````



Servlet

````java
package member;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/*<form action="login">
 * 아이디입력: <input type = text name = id><br>
 * 암호입력: <input type=password name = pw><br>*/
@WebServlet("/login")
public class LoginServlet extends HttpServlet {

 protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	 	 login(request, response);
		//요청받은 정보 추출  id, pw 전송 - 입력 
		String id =request.getParameter("id"); // 지어준 변수명과 일치
		String pw = request.getParameter("pw"); //getParameter: 요청정보 추출, 속성을 읽어오는 것 
		String[] locations = request.getParameterValues("location"); // 여러 값 (배열)을 읽어오는 메소드
    
		//로그인처리 - id = user, pw = 1234 > 정상 로그인 되었습니다 응답
		String result = " ";
		if(id.equals("user")&&pw.equals("1234")) {
			result = "<h3 style=color:green>정상 로그인되었습니다.</h3>"; // <body>태그 내부 구성으로 간주
		}else {
			result = "<h3 style=color:red;>비정상 로그인되었습니다. </h3>";
			result += "<h3><a href = 'loginform.html'>로그인창으로 이동</a></h3> ";		
		}
		// 서블릿이 응답 결과 브라우저 출력 text/html - mime type(브라우저타입) 
		response.setContentType("text/html;charset=UTF-8"); //html로 내보내고, utf-8로 텍스트를 보이겠다.
		PrintWriter out = response.getWriter();
		out.println(result);

		out.println("<h3>선택한 장소는 다음과 같습니다.</h3>");
		for(String loc : locations) {
			out.println(loc + "<br>");
		}		
	}
}
````

​	* 요청시: 

 protected void doGet(HttpServletRequest request, HttpServletResponse response)

 	request와 response 변수값을 통해 html로부터 입력된 값을 받고, 응답할 수 있다.

 * **request:** 

   request.getParameter("id");  

   // html에서 name이 id인 입력요소를 가져옴. <u>반드시 같은 이름</u>을 기입. 

   request.getParameterValues("location"); 

   //여러 값을 가져오는 메소드 (ex. option multible이나 checkbox)

* **response:** 

  <u>response.setContentType("text/html;charset=UTF-8");</u> 

  // 응답 컨텐트 타입을 html, 문자설정은 utf-8로 설정  

  PrintWriter out = response.getWriter(); // 결과를 내보낼 객체 생성

  out.println(result); // 응답



* 응답 메소드 종류

  1) doGet

  2) doPost

  3) service ( 1) 과 2) 둘 다 가능 )

| get                                                          | post                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **url뒤에 데이터 전송**<br />url?name=value&name2=value2 ...<br />url의 ? 뒷부분에 데이터가 들어간다. <br />전송 데이터 길이가 정해져있음.  <br />get 요청 방식: <br />< form action = "login"><br />< form action = "login" method=get><br />브라우저 주소에 값 직접 입력<br />< a href="login" >(서블릿 이동)<br />get은 기본 방식(default)<br /><br />**doGet(...)** | **url과 별도 분리 데이터 전송**<br />(url 뒤 정보 (url+ip+post데이터들)보이지 않음) <br /><br />: 보안에 강함 <br />파일 업로드같은 길이가 긴 데이터 전송에 유리<br /> (url길이제한이 없기 때문에)<br />서블릿에서는 속도가 느리다<br />**post 요청 방식은 하나:**<br />**< form action = "login" method=post >**<br /><br />**doPost(...)** |

** html에서 서로 뒤섞여 사용하면 안 되며 사용하는 쪽으로 일치되어야 한다.

** get은 생략이 가능(기본값)하나 <u>post는 반드시 명시</u>해야 한다. 

````java
<form action = "login" method = "post"> 
````

** <u>post의 경우 요청값을 읽어올 때 인코딩설정이 필요</u>하다. 

​	 (get의 경우 "url?" 부분이 한글 인코딩 자동설정하나, post의 경우 url + [데이터]의 형태로 되어 있어 별도의 characterset이 필요)

````java
 request.setCharacterEncoding("UTF-8");
````





예제) 구구단 출력하기

*html

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<title>Insert title here</title>
<script src = "jquery-3.2.1.min.js"></script>
<script>
 $(document).ready(function(){
	 //숫자가 아닌지 체크 
 });
</script>
</head>

<body>
<form action = "gugu" method="post">
	구구단을 입력:
	<input type=text name=number>
	<input type="submit" value="click">
</form>
</body>
</html>
````



*servlet

````java
package servlettest;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/gugu")
public class GugudanServlet extends HttpServlet {

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doPost(request, response);
	}
	
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//request.setCharacterEncoding("utf=8");
		
		String number = request.getParameter("number");
		int outputnum = 0;
		//2 숫자타입으로 바꾸기 
		if(number == null || number.equals("")) {
			outputnum = 2;
		}
		else {
			int outputnum2 = Integer.parseInt(number);
			if(outputnum2 >= 2 && outputnum2 <=9) {
				outputnum = outputnum2;
			}else {
				outputnum = 10;
			}
			
		}
		String format = " ";
		
		format += "<h1>구구단</h1> ";
		format += "<form action = 'gugu' method='post'>";
		format += "구구단을 입력:<input type=text name=number><input type='submit' value='click>'";
	    format += "</form>";
		
		if(outputnum != 10) {
		 format += "<table border = 3>";
		for(int i = 1; i <=9; i++){
			format += "<tr><td>" + outputnum + " * " + i + " = " + (outputnum*i) + "</td></tr>";
		}
		format += "</table>";
		}else {
			format = "<table border = 3>";
			for(int i = 1; i <=9; i++){
				format += "<tr>";
				for(int j = 2; j <= 9; j++) {
					
					format += "<td>" + j + " * " + i + " = " + (j*i) + "</td>";
				}
				format += "</tr>";
		 }
			format += "</table>";
		}
		format += "<a href = 'gugudan.html'>구구단입력</a> ";
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println(format);
		
	}

}
````



