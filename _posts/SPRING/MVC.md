## MVC

> Model - View - Controller 



##### 구성요소

| Component         | 설명                                                         |
| ----------------- | ------------------------------------------------------------ |
| DispatcherServlet | MVC의 Front Controller. 웹요청과 응답을 주관                 |
| HandlerMapping    | 웹요청시 해당 URL을 어떤 Controller가 처리할지 결정          |
| Controller        | 비즈니스 로직 수행 후 결과데이터를 ModelAndView에 반영       |
| ModelAndView      | Controller가 수행결과를 반영하는 Model데이터 객체와 이동할 페이지 정보(View 객체)로 구성 |
| ViewResolver      | 어떤 View를 선택할지 결정                                    |
| View              | 결과 데이터인 Model 객체를 display                           |

![스크린샷 2021-04-14 오후 10.07.58](%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.07.58.png)

> 1. DispatchServlet이 가장 먼저 요청받음. 
> 2. HandlerMapping이 요청에 해당하는 Controller를 return
> 3. Controller는 비즈니스 로직을 수행, 결과를 ModelAndView에 반영하여 return
> 4. ViewResolver는 view name을 받아 해당하는 View 객체를 return
> 5. View는 Model객체를 받아 랜더링.



* 각각의 용어 설명

  * Model

    * 어플리케이션 상태(data)를 의미
    * Java Beans

  * View

    * display 데이터
    * Model data의 렌더링 담당, html output을 생성
    * JSP

  * Controller

    * View와 Model 사이의 인터페이스 역할
    * Model/View에 대한 요청을 받고 그에 따른 로직 수행, 적절한 결과를 Model에 담아 View에 전달 (Model Object와 이 Model을 화면에 출력할 View Name을 반환)
      * Model은 데이터, View Name은 출력할 .jsp 이름을 의미
    * Controller —> Service —> Dao —> DB

  * DispatcherServlet

    * 사용자의 요청을 받음
    * 받은 요청은 HandlerMapping으로 넘어감

  * HandlerMapping

    * 요청을 처리할 Controller를 찾음 (Controller URL Mapping)

       클래스에 @RequestMapping(“/url”) annotaion을 명시하면 해당 URL에 대한 요청이 들어왔을 때 table에 저장된 정보에 따라 해당 클래스 또는 메서드에 Mapping
  
  * POJO(Old Java Object)
  
    * MVC에서 DAO와 VO는 서버로부터 독립적인 구조를 갖는데, 이를 POJO클래스라고 한다. 
    * 반드시 Spring을 사용하지 않아도 되는 class들을 일컫는 말 
  
  <a href = https://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html>참조</a>






#### 작동 방식



1. View 경로 및 설정 

web.xml : 웹 어플리케이션의 설정을 위한 배포 서술자. ViSpring의 환경 설정을 할 수 있음.

* servlet-context.xml(일부)

  ````html
  <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
  	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  		<beans:property name="prefix" value="/WEB-INF/views/" />
  		<beans:property name="suffix" value=".jsp" />
  		<!--뷰의 위치와 확장자 -->
  	</beans:bean>
  	
  	<context:component-scan base-package="multi.campus.spring" />
  	<!-- 어노테이션 사용 영역 -->
  
  ````

  * View의 이름, 확장자와 경로를 확인할 수 있다. 

  * 경로는 /web-inf/views/ 확장자 파일은 .jsp이다. 

    * /web-inf는 일종의 보안 영역. 브라우저가 접속한다는 것은 서버의 내용을 가져간다는 의미인데, 저 영역은 브라우저의 접근 불가 영역. 즉 직접 요청이 불가하고, mvc구조는 애초부터 모든 url은 dispatcher로 설계되어 있다.

       <u>Dispatcher-Controller-Modle/View return</u> 으로 호출

  * multi.campus.spring의 영역 내에서 annotation을 사용 가능 

  

* web.xml(일부)

  ````html
  <!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
  	<context-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>/WEB-INF/spring/root-context.xml</param-value>
  	</context-param>
  	
  	<!-- Creates the Spring Container shared by all Servlets and Filters -->
  	<listener>
  		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  	</listener>
  
  	<!-- Processes application requests -->
  	<servlet>
  		<servlet-name>appServlet</servlet-name>
  		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  		<init-param>
  			<param-name>contextConfigLocation</param-name>
  			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
  		</init-param>
  		<load-on-startup>1</load-on-startup>
  	</servlet>
  		
  	<servlet-mapping>
  		<servlet-name>appServlet</servlet-name>
  		<url-pattern>/</url-pattern>
  	</servlet-mapping>
  
  <!-- tomcat(tomcat이 filter를 받음) - spring - dispatcher  -->
  <filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>
  	org.springframework.web.filter.CharacterEncodingFilter
  	</filter-class>
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>utf-8</param-value>
  	</init-param>
  	</filter>
  	<filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>/*</url-pattern>
  	</filter-mapping>
  
  </web-app>
  ````

  * /* : 모든 확장자 가능. 사용자의 모든 요청을 받을 수 있다.



2. 설계하기

> 직접적으로 요청한 바를 주는 것이 아닌, 요청을 받고 이를 Controller를 통해 View와 model을 반환하여 응답하는 과정을 살펴본다. 

* DI와 IOC는 .xml과 annotation 둘 다 가능하나, 편리상 annotation을 사용하고, 웹 환경설정은 xml에서 진행된다. 



사용되는 anotation

@Controller : 컨트롤러를 정리하기 위해, 하나의 타입을 부여해주는 것이라고 보면 된다. 아래의 클래스가 컨트롤러임을 선언. 

@RequestMapping ("/"): 해당 url요청이 오면 아래의 메소드가 처리된다. 요청과 메소드의 매핑

````java
@Controller 
class HelloController{
  @RequestMapping("/login")
  void ModelAndView a(){
    add.모델
    setName.뷰 
    return modelandviewtype
  }
  @RequestMapping("/board")
  String b(){}
  @RequestMapping("/product")
  ModelAndView c(){}
}
````

@Controller는 해당 class를 Controller로 묶어주면서 (Interface의 역할을 하면서) 상속이 없어 여러 개의 메소드가 가능하고 선언부가 자유롭다.

ex)

````JAVA
@RequestMapping("/crud/list")
	public ModelAndView list() {
		String[] title = {"게시물1", "게시물2", "게시물3"};
		ModelAndView mv = new ModelAndView();
		mv.addObject("list", title); // model 저장
		mv.setViewName("/crud/list"); //ViewName지정-display를 보여줄 .jsp 경로 지정
		return mv; //데이터와 .jsp return 
	}
````



예제) 

CRUDController

````java
package multi.campus.spring;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controllerv//한 개 컨트롤러에 여러개 요청 메소드들 다수개 정의 가능 
public class CRUDController {
	//게시물 생성-조회-삭제-수정
	@RequestMapping("/crud/start")
	public void start() {
		//return 없는 경우 model 없음. 
		//view 이름은 url로 자동결정 web-inf/views/crud/sart.jsp
	}
	@RequestMapping("/crud/list")
	public ModelAndView list() {
		String[] title = {"게시물1", "게시물2", "게시물3"};
		ModelAndView mv = new ModelAndView();
		mv.addObject("list", title);
		mv.setViewName("/crud/list");
		return mv;
	}
  
	@RequestMapping(value="/crud/add", method=RequestMethod.GET)
	public String addform() {
		return "crud/addform";//addform.jsp
	}
	
	@RequestMapping(value="/crud/add", method=RequestMethod.POST)
	public ModelAndView addresult(String title, String contents, String writer) { 
		ModelAndView mv = new ModelAndView();
		mv.addObject("board", title +":" + contents + ":" + writer);
		mv.addObject("status", "게시물 1개 저장 완료"); 
		//mv.setViewName("crud/add.jsp"); url이름과 같음, 굳이 지정하지 않아도 된다. 
		return mv;	
	}
	
	@RequestMapping(value="/crud/update", method=RequestMethod.GET)
	public ModelAndView updateform() {
		//제목 내용 작성자를 미리 폼에 보여주고 수정
		ModelAndView mv = new ModelAndView();
		mv.addObject("title", "선택한 게시물 제목");
		mv.addObject("contents", "선택한 게시물 내용");
		mv.addObject("writer", "작성자");
		mv.setViewName("/crud/updateform");		
		return mv;
	}
	
	@RequestMapping(value="/crud/update", method=RequestMethod.POST
	public ModelAndView updateresult(String title, String contents, String writer) { 
		ModelAndView mv = new ModelAndView();
		mv.addObject("board", title +":" + contents + ":" + writer);
		mv.addObject("status", "게시물 1개 수정 완료"); 	
		return mv;
		
	}
	@RequestMapping("/crud/delete")
	public String delete(@RequestParam(value = "seq", required=false, defaultValue = "1") int seq) { 
		//전달값이 없어도 기본은 1
		System.out.println(seq + " 번의 글 삭제 ");
		//return "/crud/list";
		return "redirect:/crud/list"; //crud의 list라는 매핑 메소드로 이동하라
	}
}

````

* Controller

  * 반드시 ModelAndView를 return type으로 주지 않아도 된다. 이 경우, viewName은 매핑 url의 이름으로 결정된다.

    ex. @RequestMapping("/crud/start") -> web-inf/views/crud/sart.jsp

* Get/Post 

  * 동일한 요청 url에 대해 요청 메소드에 get과 post를 달리하면 두 개의 메소드를 실행할 수 있다. 
  * 이 경우 form을 get, get으로부터 값을 받아 다음 페이지를 출력해야 하는 경우 Post를 쓴다. 
    * get과 post의 url 노출 때문이다. post의 경우 보안이 더 높다. 

  ````java
  @RequestMapping(value="/crud/update", method=RequestMethod.POST)
  @RequestMapping(value="/crud/update", method=RequestMethod.GET)
  ````



view1)start.jsp

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>제 홈페이지의 첫 화면입니다.</h1>
	<a href="<%=request.getContextPath() %>/crud/list">게시물리스트 보러가기</a><br>
	<a href="<%=request.getContextPath() %>/crud/add">게시물 작성하러 가기</a><br>
	<a href="<%=request.getContextPath() %>/crud/update">게시물 수정하러 가기</a><br>
	<a href="<%=request.getContextPath() %>/crud/delete">게시물 삭제하러 가기</a><br>
	
</body>
</html>
````

view2)loginform.jsp

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action = "<%=request.getContextPath() %>/crud/update" method="post">
	제목<input type=text name="title" value = "${title }"><br> 
	내용<input type=text name="contents" value = "${contents }"><br> 
	작성자<input type=text name="writer" value = "${writer }"><br> 
	<input type=submit value="글쓰기"><br> 
	
	</form>
</body>
</html>
````

view3)list.jsp

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

</head>
<body>
	<c:forEach items="${list }" var = "title">
		<h1>${title }</h1>
	</c:forEach>
</body>
</html>
````

view4)addform.jsp

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action = "<%=request.getContextPath() %>/crud/add" method="post">
	제목<input type=text name="title"><br> 
	내용<input type=text name="contents"><br> 
	작성자<input type=text name="writer"><br> 
	<input type=submit value="글쓰기"><br> 
	
	</form>
</body>
</html>
````

view5)add.jsp

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	${board}<br>
	${status}<br>
</body>
</html>
````

view6)update.jsp

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	${board }
	${status }
</body>
</html>
````

* action url

  ````jsp
  <form action = "<%=request.getContextPath() %>/crud/add" method="post">
  ````

  * <%=request.getContextPath() %> 해당 요청 컨텍스트 경로를 자동 지정해줌



* Request매핑이 많아져 헷갈릴 경우 RequestMapping을 이용하면 request항목을 한 눈에 볼 수 있다. 

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.57.48.png" alt="스크린샷 2021-04-15 오후 8.57.48" style="zoom: 50%;" />





**Service Interface와 ServiceImple**

: Service는 DAO에서 받은 것을 가공, Controller에게 return, 그 데이터를 Controller가 model로 넣고 View와 함께 반환

: Service는 View에 종속적인 코드가 없음. 때문에 그대로 재사용이 가능(수정과 보완이 용이하다)

: 소스코드가 추가된다면, 바로 service 내의 소스 수정이 아닌 service Interface를 구현한 다른 클래스를 구현해 그 객체를 사용하게끔 하면서 개방-폐쇄 원칙을 따른다

**개방-폐쇄 원칙(OCP Open-Closed Princible): 소프트웨어개체(클래스, 모듈, 함수 등)는 확장에 대해 열려있어야 하고 수정에 대해서는 닫혀있어야 한다. 프로그램의 수정 보안 유지의 핵심. 