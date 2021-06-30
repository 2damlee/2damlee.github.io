# AJAX

> 기본 화면은 정적으로 특정 부분만 내용 변경 및 확장되는 비동기식 방식

AJAX(Asynchronous Javascript And Xml)



* 동기화 통신 방식(synchronous)

  요청->서버 html태그를 포함한 새로운 화면으로 응답->요청2 ->서버 html태그를 포함한 새로운 화면으로 응답2

  * 이같은 방식은 display 화면이 늘 새로 만들어져야 함 


  요청1 - 요청2 - 응답2 - 응답1의 방식이 가능하다면? Controller에게 결과를 받고, 그 결과만을 넣어줘 기존 화면에 확장만 할 수 있다면? 



* 비동기방식, **AJAX(Asynchronous Javascript And Xml)**
  * 화면 구성과 서버 요청 처리 결과가 구성되어있는 파일에 내용만 동적으로 바꾸는 방식
  * display는 display대로, 요청은 요청대로 처리하는 방식. 요청의 결과물을 받아서 현재 응답화면을 변경한다. 
  * 이 경우 <u>Controller에서 return하는 것은 view가 아닌 처리결과 태그, JSON</u>이 된다.



##### JSON(JavaScriptObjectNotation)

> Javascript가 Spring으로 넘어오는 경우 해당 언어가 JS인지 인지를 못한다. 이에 따라 다른 언어에서 호환성을 위해 약속한 것이 바로 JSON

* JSON형태

  {"valuename":"value", "valuename": 10,  	 }

  javascript와 html이 단일따옴표와 이중따옴표 둘 다 사용 가능하다면, JSON은 반드시 이중따옴표로 구분한다. (int 타입은 제외)

  * Controller는 return을 json 형태로 return 
    * Stringtype으로 return되기 때문에 JSON의 이중따옴표(" ") 앞에\를 추가
  * AJAX의 경우 클라이언트와 서버 양쪽 다 지원해야 그 결과를 볼 수 있다. 
  * 기존의 display내부에서 서버에서 처리된 결과를 AJAX로 보내면 화면에 추가한다.
    * 이 때 주고받는 데이터는 이전에 xml방식을 사용하였으나, 이것이 JSON으로 변경된 것 



##### 사용 환경설정



* client

  * html태그 안에서는 AJAX의 요청방식을 받을 수 없다.

  * javascript/jquery를 통해 요청한다. (스크립트 태그 내부)

    $.ajax({..});



* server(Spring mvc의 경우)

  * json 인식, 적용하기 위한 라이브러리 설치(pom.xml)

    ````html
    <!-- ajax , json -->
    <dependency>
    	<groupId>com.fasterxml.jackson.core</groupId>
    	<artifactId>jackson-databind</artifactId>
    	<version>2.9.3</version>
    </dependency>
    ````

    * 위 라이브러리같은 경우 return타입으로 객체를 보내는 경우, 자동으로 JSON으로 변경해주는 기능이 있다. (ex. VO자바객체를 JSON으로 자동변경)

  * <u>@ResponseBody/@RequestBody</u> annotation 필수

    @ResponseBody : 응답을 JSON으로 보내는 경우

    @RequestBody : 요청을 JSON으로 보내는 경우



##### 예제 살펴보기

Controller

```java
ackage ajax;

import java.util.ArrayList;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class LoginAjaxController {
	@RequestMapping(value="/ajax/login", method=RequestMethod.GET)
	public String loginform() {//form 출력(http요청) 
		return "/ajax/loginajax"; //뷰이름 
	}
	
	@RequestMapping(value="/ajax/login", method=RequestMethod.POST, produces = {"application/json;charset=utf-8"})
	@ResponseBody
	//{"id":$("#id").val(), "pw":$("#pw").val()}
	public String loginresult(String id, String pw) {//form 입력, 데이터 전송 ajax요청
		String result=null;
		if(id.equals("spring") && pw.equals("spring")) {
			result = "{\"process\":\"정상로그인\", \"role\":\"admin\"}";
      //변수명 process, 값 String"정상로그인"
		}else {
			result = "{\"process\":\"비정상로그인\", \"role\":\"user\"}";
		}
		return result;
	}//loginresult end
	
/*	 @RequestMapping("/ajax/board")
	 @ResponseBody 
	 public BoardDTO getBoardDTO(int seq) {
		 BoardDTO dto = new BoardDTO();
		 dto.setSeq(seq);
		 dto.setTitle("게시물제목");
		 dto.setContents("게시물내용");
		 dto.setWriter("작성자");
		 dto.setViewcount(100);
		 return dto;//json형태 변경하여 Return
	 }*/
	 
	// /ajax/board/1 - url 내부값 사용, 처리 
	 @RequestMapping("/ajax/board/{seq}")//{seq}들어간 값을 seq라는 변수로 name
	 @ResponseBody //return : 뷰이름이 아닌 json, 자동으로 변환
	 public BoardDTO getBoardDTO(@PathVariable("seq") int seq) {//url의 변수를 int seq로 사용 
		 BoardDTO dto = new BoardDTO();
		 dto.setSeq(seq);
		 dto.setTitle("게시물제목");
		 dto.setContents("게시물내용");
		 dto.setWriter("작성자");
		 dto.setViewcount(100);
		 return dto;//json형태 변경하여 Return
	 }
	 
	 @RequestMapping("/ajax/boardlist")
	 @ResponseBody //return : 뷰이름이 아닌 json, 자동으로 변환
	 public ArrayList<BoardDTO> getBoardDTO() {
		 BoardDTO dto = new BoardDTO();
		 ArrayList<BoardDTO> list = new ArrayList<BoardDTO>();
		 dto.setSeq(1);
		 dto.setTitle("게시물제목");
		 dto.setContents("게시물내용");
		 dto.setWriter("작성자");
		 dto.setViewcount(100);
		 list.add(dto);
		 list.add(dto);
		 list.add(dto);
		 list.add(dto);
		 list.add(dto);
		 list.add(dto);
		 return list;//json형태 변경하여 Return
	 }
```



````java
@RequestMapping(value="/ajax/login", method=RequestMethod.POST, produces = {"application/json;charset=utf-8"})
@ResponseBody
````

produces = {"application/json;charset=utf-8"}: jquery가 다시 요청을 보내기 때문에 한글 셋팅을 설정해준다.

@ResponseBody 어노테이션이 없다면 자동으로 view가 리턴된다. 

````java
@RequestMapping("/ajax/board/{seq}")//{seq}들어간 값을 seq라는 변수로 name
@ResponseBody 
public BoardDTO getBoardDTO(@PathVariable("seq") int seq) {//url의 변수를 int seq로 사용 
````

@RequestMapping("/ajax/board/{seq}"): 해당 url에 들어간 값을 seq라는 변수이름으로 사용

@PathVariable("seq"): url의 seq의 이름을 가진 변수값을 매개변수값으로 넣어준다 (이름이 모두 같아야 함)

​									  : url에 들어가는 형식, 파라미터값으로 일치시킬 수 있음.(글번호 활용)





loginajax.jsp

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src = "<%=request.getContextPath() %>/resources/jquery-3.2.1.min.js"></script>
<script>
$(document).ready(function(){
  $("#login").on('click', function(){
	$.ajax({ //login을누르면 a.jax가 실행 
		url: "<%=request.getContextPath()%>/ajax/login",  
		data: {"id":$("#id").val(), "pw":$("#pw").val()}, //전송했던파라미터 이름이 id, data라는 변수에 들어감 
		type: 'post', 
		dataType: 'json', 
		//함수의매개변수는 server로부터 받아온 변수
		success: function(server){ //server = "{\"process\":\"정상로그인\"}"
			alert(server.process);
			$("#result").html(server.role);
			if(server.process == '정상로그인'){
			$("#result").css("color", "blue");
			}else{
			$("#result").css("color", "red");
			}
		}
	});//ajax 요청함
  });//on end
  
 <%--/* $("#board").on('click', function(){
		//  $("#result").html("테스트완료");
		$.ajax({ //login을누르면 a.jax가 실행 
			url: "<%=request.getContextPath()%>/ajax/board",  
			data: {"seq":$("#seq").val()}, //전송했던파라미터 이름이 id, data라는 변수에 들어감 
			type: 'get', 
			dataType: 'json', 
			//함수의매개변수는 server로부터 받아온 변수
			success: function(server){ //server = "BoardDTO를 JSON으로 변경 자동"
				$("#result2").append(server.seq+"<br>");
				$("#result2").append(server.title+"<br>");
				$("#result2").append(server.contents+"<br>");
				$("#result2").append(server.writer+"<br>");
				$("#result2").append(server.viewcount+"<br>");
				
			}
		});//ajax 요청함
	  });//on end
	  */--%>
	  
	  $("#board").on('click', function(){
			//  $("#result").html("테스트완료");
			$.ajax({ //login을누르면 a.jax가 실행 
				url: "<%=request.getContextPath()%>/ajax/board/" + $("#seq").val(),  
				type: 'get', 
				dataType: 'json', 
				success: function(server){ 
					$("#result2").append(server.seq+"<br>");
					$("#result2").append(server.title+"<br>");
					$("#result2").append(server.contents+"<br>");
					$("#result2").append(server.writer+"<br>");
					$("#result2").append(server.viewcount+"<br>");
					
				}
			});//ajax 요청함
		  });//on end
  
	  $("#boardlist").on('click', function(){
			//  $("#result").html("테스트완료");
			$.ajax({ //login을누르면 a.jax가 실행 
				url: "<%=request.getContextPath()%>/ajax/boardlist",   
				type: 'get', 
				dataType: 'json', 
				//함수의매개변수는 server로부터 받아온 변수
				success: function(server){ //server = ArrayList<BoardDTO>를 Json은 배열로 자동 변경 
					// [{seq:1, title:"", }, {}, {}, {}, {}]
					for(var i in server){
					$("#result3").append("<div style='border:2px solid pink'>"+server[i].seq+"<br>");
					$("#result3").append(server[i].title+"<br>");
					$("#result3").append(server[i].contents+"<br>");
					$("#result3").append(server[i].writer+"<br>");
					$("#result3").append(server[i].viewcount+"<br></div>");
					
					}
				}
			});//ajax 요청함
		  });//on end
		  
  });
</script>
</head>
<body>
	<h1>ajax Login Form</h1>
	<form>
	<!-- 서버로 전송 - name속성= request.getParameter("id")
		 jquery 전송 - id속성 = $("#id")
	 -->
	아이디<input type=text name="id" id="id"><br>
	암호<input type=password name="pw" id="pw"><br>
	</form>
	<button id="login">ajax login</button>
	<div id="result"></div> 
	
	<input type=text id="seq"><button id="board">번 글 요청</button>
	<div id=result2></div>
	
	<button id="boardlist">모든 글 요청</button>
	<div id=result3></div>
</body>
</html>
````



````java
$.ajax({  
		url: "<%=request.getContextPath()%>/ajax/login",  
		data: {"id":$("#id").val(), "pw":$("#pw").val()},
		//전송했던파라미터 이름이 id, data라는 변수에 들어감 
		type: 'post', 
		dataType: 'json', 
		//함수의매개변수는 server로부터 받아온 변수
		success: function(server){ //server = "{\"process\":\"정상로그인\"}"
			alert(server.process);
			$("#result").html(server.role);
			if(server.process == '정상로그인'){
			$("#result").css("color", "blue");
			}else{
			$("#result").css("color", "red");
			}
		}
	});
````

Controller에서 받은 값을 처리해주는 ajax 요청문 

````java
$.ajax({ 
  url: 매핑할 url정보,
  data: {parameter이름:변수값, parameter이름:변수값},
  //해당 변수값을 Controller의 동일 parameter이름 값으로 보냄
  type: 메소드 타입(ex'post') 
  dataType:'json'
  success: function(server){ //위의 값을 Controller로 전송시켜서 해당 처리 값을 가져왔을 경우
    //function의 매개변수는 Controller(server)로부터 받아 온 변수
    server.process //서버 값 받아오기, 호출
  }
});
````

* 분리 ,(콤마) 필수
* function의 매개변수는 Controller(server)로부터 받아 온 변수
  * 매개변수이름.Controller에서 보낸 변수값이름 (server.process)



* ajax 전용 Controller 생성하기

  * AJAX 요청 처리 전용 컨트롤러 생성 가능

    **@RestController** : /Ajax 요청 처리 전용 Controller

  ````java
  package ajax;
  
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.ResponseBody;
  import org.springframework.web.bind.annotation.RestController;
  
  
  @RestController//Ajax 요청 처리 전용 Controller
  public class AllAjaxController {
  
  	@RequestMapping("/a")
  	public BoardDTO a() {
  		return new BoardDTO();
  	}
  	
  	@RequestMapping("/b")
  	public String b() {
  		return "";
  	}
  	
  	@RequestMapping("/c")
  	public Integer c() {
  		return 0;
  	}
  	
  }
  ````

  

