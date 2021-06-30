# Spring Boot

> 스프링부트는 기존 스프링이 가지고 있는 환경설정의 복잡함을 덜어주기 위해 생긴 것, 현재도 완성형은 아니고 계속해서 디벨롭되고 있는 상태이다. 

* maven - 스프링 라이브러리 자동 다운로드 기능이 포함된 프로젝트를 만들 수 있는 툴: sts
  * Spring은 xml설정이 늘 필수적. 
* spring boot project (=spring starter project)
  * xml설정의 복잡도가 좀 덜해진다.
  * web.xml + spring bean 설정파일.xml이 없어지고, 대신 applicaiton.properties에서 모든 파일 설정



#### 시작하기

1. new spring starter project로 시작

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-26%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.28.13.png" alt="스크린샷 2021-04-26 오전 9.28.13" style="zoom: 33%;" />

Maven : 사용 라이브러리

War: 저장할 파일 형태 (war의 경우 웹프로젝트)



2. 필요한 라이브러리 추가 
   * AJAX, MultipartFile API 등 여러 라이브러리들은 이미 부트에 포함되어 있다.

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-26%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.28.34.png" alt="스크린샷 2021-04-26 오전 9.28.34" style="zoom:33%;" />

필요한 라이브러리 추가를 mvc에서는 pom.xml에 dependency를 입력해 줬다면, 부트에서는 몇몇을 제외 필요한 것을 체크하면 자동 기입된다. 

나는 해당 설정시 Spring Web, Jdbc Api, MyBatis Framework, Oracle Driver, Spring Web을 선택  



#### 환경설정하기

> * Spring Boot에서는 sql mapping xml을 제외한 xml환경설정은 전부 application.propertis에서 한다.

* pom.xml에서 필요한 라이브러리 다운

  * dependency에서 선택가능한 항목이 아닌 것은 따로 기입해준다. 아래는 jsp와 jstl 라이브러리 추가
  
  ````html
  <!-- for jsp -->
  		<dependency>
          <groupId>org.apache.tomcat.embed</groupId>
          <artifactId>tomcat-embed-jasper</artifactId>
          <scope>provided</scope>
      </dependency>
  		<!-- for jsp jstl -->
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>jstl</artifactId>
      </dependency>
      <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-devtools</artifactId>
              <optional>true</optional>
      </dependency>
  ````



* application.propertis 환경설정/ 파일 위치 

  * Spring에서 본래 여러 각각의 xml에 환경설정을 해주었다면, 스프링부트에서는 하나의 xml에 모든 환경설정을 한다.
    * 아래의 경우 서버포트설정/view경로와 확장자 설정/mybatis 데이터베이스 연결 설정과 DB파일 경로 설정을 해줌

  ````html
  server.port=9002
  
  #view resolver (템플릿 대신 jsp 페이지를 사용할 경우)-pom.xml 추가사항도 있음!!!
  spring.mvc.view.prefix=/WEB-INF/views/
  spring.mvc.view.suffix=.jsp
  server.servlet.jsp.init-parameters.development=true
  
  #html,js,gif등은 static폴더에 저장이 기본설정
  
  #mybatis
  #Database config
  spring.datasource.url=jdbc:oracle:thin:@localhost:1521:XE
  spring.datasource.username=hr
  spring.datasource.password=hr
  spring.datasource.driverClassName=oracle.jdbc.driver.OracleDriver
  
  #mybatis config
  mybatis.config-location=classpath:db-config.xml
  #mybatis.mapper-locations=classpath:mybatis/mappers/sql-mapping.xml
  
  #매퍼 파일에서 alias를 쓰기위한 패키지 지정
  mybatis.type-aliases-package=spring_mybatis
  ````

  * mvc 프로젝트 web.xml에 view와 resources위치가 지정되어있던 것과 달리 부트에서는 경로를 지정해주고 <u>해당 파일을 직접 해당 경로에 생성</u>해줘야 한다.  
  * **html, js, gif 등은 static폴더** 저장이 기본 설정 
  * 부트에서 **classpath:** 는 **src/main/resources**를 가르킨다. 
    * 위 설정은 db-config는 classpath: 바로 아래 위치, sql-mapping.xml은 classpath: 내에 mybatis 폴더 안으로 경로를 설정
    * src/main/resources/templates/ 타입리프 포함 html파일(설정추가) (사용할 일x)



#### Spring Boot 특징 

* xml파일은 sql mapping xml을 제외 applicaiton.properties에서 파일 설정

* SpringBoot의 경우 내장된 톰캣서버가 있고, 부트 내장 앱에서 톰캣을 실행한다. 

  * 톰캣서버를 실행하는 main이 존재하고, 해당 main을 통해 톰캣서버를 실행한다 (application run)

    * Application.java main에서 run as > Spring Bott App

      자동 서버 열림 안됨, url 수동입력  

    * 기본 포트는 8080, application.properties에서 포트변경 가능 

* Context가 없다.

  * mvc에서는 기본 컨텍스트가 있고 다음 매핑 url이 왔다면, Boot의 경우 포트번호 끝나고 바로 url

    mvc: http://ip:port/현재context/xxxx	Boot:http://ip:port/xxxx

* Spring mvc프로젝트의 경우 sql-mapping.xml파일을 단독 설정하고 dao클래스 구현이 가능하나 <u>SpringBoot의 경우 sql매핑xml + dao 파일들을 같이 매핑만 가능</u>하다. (mvc는 둘 다 가능)

  * 따라서 Boot의 경우 session을 구현할 일이 없다



#### MyBatis DB연결

config.xml

```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <!-- mybatis db 연결정보 세팅 파일 -->
<configuration>
	<!-- 1.타입 alias : 
	mybatis.EmpVO : SQL결과 데이터 저장 
	별명 : emp 
	-->
	<typeAliases>
		<typeAlias type="spring_mybatis.EmpVO" alias="empVO"/>
	</typeAliases> 
	
	<!-- 3. sql mapping 파일 설정 -->
	<mappers>
		<mapper resource = "mybatis/sql-mapping.xml"/>
	</mappers> 
	
</configuration>

```



##### annotation 패키지 등록 : @ComponentScan

> 어노테이션 사용 경로 설정이 mvc에서는 server.xml에서 이루어진다면, boot에서는 서버가 실행되는 main class인 Applicaition.java에서 설정한다.

* Component를 추가하는 위치는 @SpringBootApplication가 붙어있는 클래스(main)

* 현재 파일 패키지일 경우 기본 포함되어 있다. 

* component패키지를 추가할 경우

  **@ComponentScan(basePackageClasses = xxxx.class)**

  :해당 클래스가 포함되어있는 패키지는 annotation사용 가능 

````java
package com.mulit.myboot01;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;
import spring_mybatis.EmpController;
import spring_mybatis.EmpDAO;

//부트시작클래스 @패키지명 
@SpringBootApplication
@ComponentScan(basePackageClasses=Myboot011Application.class)//포함되어있음 
@ComponentScan(basePackageClasses = EmpController.class)//component패키지 추가
@MapperScan(basePackageClasses = EmpDAO.class)
public class Myboot011Application {

	public static void main(String[] args) {
		//spring boot tomcat 내장서버 자동실행
		SpringApplication.run(Myboot011Application.class, args);
	}
}
````

main에서 해당 경로를 읽고, 경로에 위치하는 파일 내에 등록된 경로를 또 읽어서 연결된다. (main-config-sqlmapping)



##### DAO와 SQL Mapping

> SpringBoot의 경우 sql매핑xml + dao 파일들을 같이 매핑만 가능하고, 따라서 Boot의 경우 session을 구현할 일이 없다

* **DAO는 인터페이스로 변환**

  * MyBatis에서 DAO인터페이스의 메소드명과 동일한 id를 가진 것이 호출된다. (parameterType 호출되려면 역시 인터페이스 변수로 동일한 데이터타입이 선언되어있어야 한다.)
  * **sqlmapping 태그id와 DAO 메소드 이름 동일, parameterType과 변수타입 동일 필수**

  

* **각 태그의 Alias를 VO클래스의 bean객체 이름과 동일하게 변경**

  * ex) EmpVO클래스가 존재한다면, Alias는 empvo

  db-config.xml

````html
 <typeAliases>
		<typeAlias type="spring_mybatis.EmpVO" alias="empVO"/> 
	</typeAliases> 
````



* **sql-mapping.xml의 mapper이름은 DAO클래스이름으로 변경**
  * 서로가 서로의 mapper임을 동일하게 선언

````html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="spring_mybatis.EmpDAO">
````



* **DAO 인터페이스에서 @mapper 선언**

  * 서로가 서로의 mapper임을 동일하게 선언

  * 서버 실행 main클래스에서 <u>@MapperScan 설정 필수</u>

    @MapperScan(basePackageClasses = EmpDAO.class)



| EmpDAO                                                       | sql-mapping.xml                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| - 인터페이스로 변경<br />- @Mapper선언<br />- SqlSession session 삭제<br /> -메소드 구현부 삭제(인터페이스만 존재) | - <mapper namespace 변경<br />- 각 태그 id를 DAO 메소드와 동일하게 설정<br />- 각 태그의 alias를 VO클래스 bean객체명과 동일하게 설정 |

