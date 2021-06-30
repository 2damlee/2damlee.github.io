# Cookie & Session



* client와 server 통신

| client                                                       | server                                                      |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| http://ip:port/컨텍스트명/a?id=jdbc                          | a -> AServlet -> doGet(){<br />request.getParameter("id")}; |
| http://ip:port/컨텍스트명/b?id=jdbc<br />서버 종료 시점까지 현재 컨텍스트의 모든 서블릿끼리 공유 O | b -> BServlet -> doGet(){<br />request.getParameter("id")}; |



#### 쿠키와 세션

 : 브라우저 종료, 혹은 시기를 정해서 그 기간만 공유를 가능하게 하는 기술



* http프로토콜 처리 과정

  ````
  1. 요청1 - 처리1(ex.게시물리스트) - 응답1(출력)
     doGet(HttpServletRequest req, HttpServletResponse res)
     매개변수 매소드 시작 - 메모리 생성 - 지역변수 - 메소드 종료 - 메모리 삭제 
  
  2. (동일 게시물)요청2 -- 응답 - 처리 - 응답 
  		: 다시 같은 것을 요청했을때, 그 동일 메모리는 아무것도 남아있지 않음
     doGet(HttpServletRequest req, HttpServletResponse res)
     매개변수 매소드 시작 - 메모리 생성 - 메소드 종료 - 메모리 삭제
  ````

  *  웹서버는 이전 클라이언트 처리 결과를 남기지 않는다. (기본적인 http 프로토콜의 특성)
    * 한 번 웹서버 요청한 클라이언트가 또 요청한다면, 이전의  처리 결과를 얻을 수 없다.



이러한 웹서버의 한계점에서 http의 해결은, **쿠키와 세션**

* 쿠키와 세션을 이용하면 이전 클라이언트의 처리 결과를 남길 수 있다. 



#### 쿠키와 세션 기능 및 특징



| 쿠키                                                         | 세션                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 클라이언트측에 저장<br /><br />크기에 한계가 있음(4kb)<br /><br />한 개의 서버당 한 개<br /><br />문자열값이어야 함(자바객체 안됨)<br /><br />브라우저 종료시: 브라우저 쿠키<br /><br />/종료 이후 저장여부 결정 가능: 파일쿠키(탐색기 폴더 파일형태로 존재)<br /><br />c1.setMaxAge(초단위 시간)<br /><br />보안 취약 | 서버측에 저장<br /><br />자바객체 전달 활용 O<br /><br />크기 한계 없음<br /><br />한 개의 클라이언트 당 한 개 <br /><br />브라우저  종료시 끝<br /><br />보안 유리 |

사용 예시:

쿠키: 현재 서버 xx째 방문입니다. / 최근 방문시간은 ㅇㅇ 등 보안과 상관없는 기록

세션: 로그인 아이디, 장바구니 등 정보와 관련된 일들





* **쿠키**
  * 웹 페이지들 사이의 공유 정보를 클라이언트 pc에 저장해놓고 필요할 때 여러 웹페이지들이 공유해서 사용할 수 있도록 하는 매개역할 

| client                                                       | server                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 클라이언트 요청<br /><br />4. 쿠키를 저장한다<br /><br />5. 클라이언트 요청하면 서버 쿠키가 생성되어 있다면, 쿠키 정보를 같이 전송 | 2. 처리한 결과로 저장 정보 만든다<br />Cokkie c =  new Cookie("이름", "값")<br /><br />3. 응답 내부 포함 클라이언트 전송 = 쿠키<br />response.addCookie(c);<br /><br />6. 요청 내부 포함 쿠키 확인 후  처리 추가/쿠키 활용 ...<br />Cookie[] coo = request.getCookies(); |



##### 쿠키 API

````java
package cookie;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/cookie1")
public class CookieServlet1 extends HttpServlet {
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//사용자 id-jdbc, pw-program 을 쿠키로 만들어보는 것 
		Cookie c1 = new Cookie("id", "jdbc"); // 쿠키문자열 규칙 - 영문자 숫자 몇 특수문자
		Cookie c2 = new Cookie("pw", "jdbc");
		//브라우저가 종료된 이후에도 일정 시간동안(초단위) 쿠키가 지속될 것을 설정 
		c1.setMaxAge(60*60*24); //24시간동안 유지
		c2.setMaxAge(60*60*24);
		//c1.setMaxAge(-1);//브라우저 종료시까지만 지속 default는 -1  
		
		//클라이언트로 전송
		response.addCookie(c1);
		response.addCookie(c2);
		response.setContentType("text/html;charset=utf-8");
		PrintWriter o = response.getWriter();
		o.println(c1.getName() + "=" + c1.getValue() + "<br>");
		o.println(c2.getName() + "=" + c2.getValue() + "<br>");
		o.println("위와 같은 쿠키를 클라이언트로 전송하였습니다.");
		
	}

}
````

```java
package cookie;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/cookie2")
public class CookieServlet2 extends HttpServlet {
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
		response.setContentType("text/html;charset=utf-8");
		PrintWriter o = response.getWriter();
	

		//사용자 id-jdbc, pw-program 을 쿠키로 만들어보는 것 
		//현재 서버가 만들어서 전송했던 쿠키들을 클라이언트로부터 생성된 모든 쿠키를 전달받는 것
		Cookie [] coos = request.getCookies(); //getCookiees() 배열타입 리턴
		for(Cookie c : coos) {
			if(c.getName().equals("id")) {
			o.println((c.getName() + "=" + c.getValue() + "<br>"));
			}
		}		
		o.println("클라이언트로부터 전달받은 쿠키는 위와 같습니다..");
	
	}

}
```





* **세션**

| client                                            | server                                                       |
| ------------------------------------------------- | ------------------------------------------------------------ |
| 1. 클라이언트 요청<br /><br />5. 클라이언트 요청2 | 2. 처리한 결과로 저장 정보 만든다<br />2-1. 세션 객체 생성<br />HttpSession session = request.getSession();<br />2-2. 필요한 정보를 저장<br />session.getAttribute("속성명1", 객체1)<br />요청1의 session:id(클라이언트마다 식별자가 있음)<br />[속성명1: 객체1][속성명2: 값2 ..<br /><br />4. 응답<br /><br />6. 서버 내부 세션 가운데 요청2 클라이언트의 저장 정보(세션확인)를 확인 <br /><br />7. 세션 정보를 활용<br />session.getAttribute("속성명1")<br />session.getAttribute("속성명2") |



````java
package session;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

 
@WebServlet("/session1")
public class SessionServlet1 extends HttpServlet {

	 
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		 //클라이언트 요청 속에 세션이 포함되어있는지 여부 확인 
		//브라우저 열고 요청1 - 요청2 - 브라우저 종료 - 세션정보 삭제 
		//세션정보 서버측 저장, 세션정보사용기능식별자 클라이언트측 저장(=세션id 쿠키에 저장 - JSESSIONID)
		String id = "none", pw = "none";
		
		HttpSession session = request.getSession(); // sesion 객체 생성
		/*HttpSession session = request.getSession(); 기능
		 * 클라이언트 요청속에 세션이 포함되어있는지를 확인 - 있다면 서버 요청한 적이 있던 것으로 간주
		 * - 세션 객체 생성 필요 없으니 기존 생성된 세션을 사용
		 * 아니라면, 서버에 요청한 적(브라우저 열고 최초 요청인가)은 없기 때문에 세션객체를 생성 */
		
		if(session.isNew()) {//브라우저를 열고 최초로 세션이 열렷는지, 클라이언트 요청에 세션 없다 = 서버 남긴 정보가 없다 = 최초 요청 
			session.setAttribute("id", "jdbc");
			session.setAttribute("pw", "jdbc");
			
		}else {
			id = (String)session.getAttribute("id");
			pw = (String)session.getAttribute("pw");
		}
		response.setContentType("text/html;charset=utf-8");
		PrintWriter o = response.getWriter();
		o.println("세션정보확인=" + id + ":" + pw);
	}

}

/*1. 브라우저 열고 http:// .../ session1을 실행한다
 * 2. 세션 2개 정보 저장한다
 * 3. 브라우저 열고 http:// .../ session2 실행한다
 * 4. 세션에 저장된 2개 정보 추출한다
 * 5. http://.../session1을 실행한다
 * 6. 세션에 저장된 2개 정보 추출한다
 * 7. 브라우저 닫는다
 * 8. 브라우저 열고 session2를 실행하게 된다면, 
 * 9. 세션에 저장되어진 정보는 없어진 상태. null값. 
 * */
````

````java
package session;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

 
@WebServlet("/session2")
public class SessionServlet2 extends HttpServlet {

	 
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		 //클라이언트 요청 속에 세션이 포함되어있는지 여부 확인 
		//브라우저 열고 요청1 - 요청2 - 브라우저 종료 - 세션정보 삭제 
		//세션정보 서버측 저장, 세션정보사용기능식별자 클라이언트측 저장(=세션id 쿠키에 저장 - JSESSIONID)
		// 이 세션은 세션 정보값만 받게 되어있음. (브라우저 닫으면 정보값 사라짐)
		String id = "none", pw = "none";
		
		HttpSession session = request.getSession();
		 id = (String)session.getAttribute("id");
		 pw = (String)session.getAttribute("pw");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter o = response.getWriter();
		o.println("세션정보확인=" + id + ":" + pw);
	}

}
````

: 클라이언트의 브라우저가 서버에 최초 접속하면 서버의 서블릿은 세션 객체를생성 후 세션 객체에 대한 세션 id를 브라우저에 전송. 브라우저는 이 세션 id를 브라우저가 사용하는 세션 쿠키에 저장, 쿠키 이름은 <u>jsessionId</u>

: 재접속해 세션 쿠키에 저장된 세션 id를 다시 서버로 전송시 서버는 이 id를 통해 브라우저의 세션 객체에 접근, 브라우저에 대한 작업 수행