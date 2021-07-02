---
title: "MyBatis-1"
categories:
  - Spring
layout: archive
sidebar_main: true
author_profile: true
tag:
  - Spring
---

<br><br>

JDBC

: 통합 프레임워크 중 jdbc가 있는 Spring Jdbc, 그리고 DB연동만을 목적으로 하는 프레임워크 MyBatis가 있다. MyBatis는 스프링의 독립적인 사용과 연결이 가능하다



<br><br>

## MYBATIS

기존의 jdbc의 코드 반복의 단점이 보완된 프레임워크. sql소스는 xml파일에 작성하기 때문에 sql의 변환이 자유롭다. 

주의: IBATIS와 MYBATIS는 호환이 안 되는 API가 종종 있다. 오류의 경우 확인할 것

<br>

* 특징
  * con-mybatis 설정 파일 한 번만 가져오게 하고, connectionpooling을 한다.(중복코드x)
  * SQL실행을 MYBATIS API에 위임한다. (pt, st 사용 x)
  * SQL과 JAVA 언어 작성 영역을 분리한다
    * 자바+SQL만 정의해놓은 XML파일 + MyBatis 라이브러리

<br>

#### 시작하기

* Maven Tool: STS Tool 내부에 필요한 스프링 라이브러리를 자동으로 다운로드해주고 버전을 관리해주는 툴

  * Spring MVC Project는 Maven을 사용하고, 따라서 MYBATIS 라이브러리 또한 자동 다운로드 된다.

  <a href = "MVNRepository.com">Maven 중앙 저장소 사이트</a>
  *.jar의 모든 파일들이 위 사이트에 존재한다. (필요할 경우 위에서 다운)

  

  * 원하는 파일을 선택, 아래 Maven을 선택해서 클릭하면 해당 소스가 복사된다. 

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.48.56.png" alt="스크린샷 2021-04-15 오후 9.48.56" style="zoom: 33%;" />

* 복사된 소스를 Spring mvc프로젝트 바로 아래 위치한 <u>pom.xml</u>에 가져온다.
  * </dependencies>태그가 끝나기 전, 태그 안에 붙여넣어준다. 
  * 해당 소스를 복붙하면, 그 이후로 파일이 다운로드되고 라이브러리에 .jar파일이 생성된다.

  ````html
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.6</version>
  </dependency>
  		        
   <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.2</version>
  </dependency>
  
  <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>4.3.18.RELEASE</version>
  </dependency>
  
  <!-- JDK 8, 9 사용 중일 경우 -->
  <!-- https://mvnrepository.com/artifact/com.oracle.database.jdbc/ojdbc8 -->
  <dependency>
      <groupId>com.oracle.database.jdbc</groupId>
      <artifactId>ojdbc8</artifactId>
      <version>19.7.0.0</version>
  </dependency>
  <!-- ajax 파일 업로드 jar -->
  
  	</dependencies>
  
  ````

  <br><br>

#### 연결, 사용하기

<br>

* 설정

  * url에서 MySQL이 설치된 서버의 주소와 사용할 DB스키마를 db-config.xml에 다음과 같이 작성

  ````html
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
  		<typeAlias type="mybatis.EmpVO" alias="emp"/> <!-- mybatis.EmpVO = emp로 정의 -->
  	</typeAliases> 
  	
  	<!-- 2.DataSource 설정 -->
  	<environments default="development">
  		<environment id="development">
  			<transactionManager type="JDBC" />
  			<dataSource type="POOLED">
  				<property name="driver" 
  				value="oracle.jdbc.driver.OracleDriver"/>
  				<property name="url" 
  				value="jdbc:oracle:thin:@localhost:1521:xe"/>
  				<property name="username" value="hr"/>
  				<property name="password" value="hr"/>								
  			</dataSource>
  		</environment>
  		</environments>
  	<!-- 3. sql mapping 파일 설정 -->
  	<mappers>
  ````

  oracle hr계정에 접속

  * DB와 연결 할 class파일이 있다면, 해당 파일을 Alias로 이름 지정이 가능하다. 

    ````html
    <typeAliases>
    		<typeAlias type="mybatis.EmpVO" alias="emp"/> 
      <!-- mybatis.EmpVO = emp로 정의 -->
    	</typeAliases> 
    ````

    <br>

* 연결하기: 

  JDBC에서의 Connection은 mybatis에서 Session으로 이해된다. 

  * 처음 단 한 번만 생성하면 되고, jdbc처럼 메소드마다 생성할 필요가 없다.
  * 연결하기
    * 아래는 java main source에서 연결

  ````java
  public static void main(String[] args) throws IOException {
  		//db 연결 요청 xml 
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  SqlSessionFactory factory = builder.build(Resources.getResourceAsReader("mybatis/db-config.xml"));
  SqlSession session = factory.openSession(true);
   //수동 commit rollback > true >자동commit 
  System.out.println(session.getConnection());//연결상태 확인
   //bean 태그가 없음. @ComponentX
  ````

  * 설정으로 sql commit을 자동으로 생성 가능 

<br>

* MyBatis와 Spring 연결

  * context-mapper.xml파일 생성, 아래와 같이 sql 코드는 이 xml파일에 작성하면 된다.

  ````html
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="kdigital">
  	<select id="oneEmp" resultType="mybatis.EmpVO" parameterType="int">
  	select*from employees where employee_id=#{employee_id} <!-- ?와 같음 -->
  	</select>
  	<select id="manyEmp" resultType="emp"><!-- db-config에 이름 정의됨 -->
  	<![CDATA[select*from employees where employee_id < 200]]> <!-- 문자 그대로 읽어오는 CDATAsection(character data) -->
  	</select>
  	
  	<insert id="insertEmp" parameterType="emp">
  	insert into employees values(#{employee_id}, #{first_name}, #{last_name}, #{email},
  	 #{phone_number}, sysdate, #{job_id}, #{salary}, null, #{manager_id}, #{department_id})
  	<!-- emp에 있는 값과 같아야 함, Type 생략 가능 -->
  	</insert>
  	<update id="updateEmp" parameterType="emp">
  	update employees 
  	set first_name=#{first_name} where employee_id=#{employee_id}
  	</update>
  	<delete id="deleteEmp" parameterType="int">
  	delete employees where employee_id=#{a}
  	</delete>
  	
  	<select id="cnt" resultType="int">
  	<!-- resultType에 데이터타입이 와도 o -->
  	select count(*) from employees
  	</select>
  	
  	<select id="pageEmp" resultType="emp" parameterType="int[]">
  	
  	select r, employee_id, first_name from
  	(select rownum r, employee_id, first_name from
  	(select*from employees order by hire_date desc))
  	where r between
  	<!-- 배열을 반복할 수 있도록 item은 선언된 변수명 그대로. bigin close separator -->
  		<foreach collection="array" item="num" separator="and">
  		#{num}
  		</foreach>
  	</select>
  	<insert id="insertEmp2" parameterType="emp">
  		<selectKey resultType="int" keyProperty="employee_id"> <!-- parameter emp안에 employee_id가 있다면 int 타입으로 넣어줌. insert into employees values(select emp_seq.nextval from dual) -->
  		select emp_seq.nextval from dual
  		</selectKey> <!-- insert할 때 임시적으로 select 생성 -->
  	insert into employees values(#{employee_id}, #{first_name}, #{last_name}, #{email},
  	 #{phone_number}, sysdate, #{job_id}, #{salary}, null, #{manager_id}, #{department_id})
  	</insert>
  </mapper>
  
  ````

  1. **XML과 DTD선언**

     ````html
     <?xml version="1.0" encoding="UTF-8" ?> 
     <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     ````

     < mapper namespace="SQL이름"> : 외부에서 sql호출시 사용할 이름 

  2. **Root Element - <mapper>**

     ````html
     <mapper namespace="kdigital">
     </mapper>
     ````

     namespace 속성은 package처럼 여러 SQL문을 묶어준다. SQL문은 <mapper>하위에 작성

  3. **< select>< insert>< update>< delete>**

     각각 SQL select, insert, update, delet로 사용

     * 속성

     | 속성         | 기능                                                         |
     | ------------ | ------------------------------------------------------------ |
     | id           | 각 SQL문에 이름 지정                                         |
     | resultType   | Select문 실행 결과를 담을 객체<br />resultType="패키지이름.클래스이름" |
     | resultMap    | Select문 실행 결과를 담을 객체<br />resultType과 resultMap 중 택 1 |
     | paraeterType | 이 속성에 지정한 객체의 속성값이 SQL문의 입력 parameter에 전달 |

     예시)

     ````html
     <select id="oneEmp" resultType="mybatis.EmpVO" parameterType="int">
     select*from employees where employee_id=#{employee_id} 
     </select>
     ````

     * **#{ }** : Jdbc에서 ?와 같은 의미. parameterType을 지정해줘야 하며, resultType 내에 있는 변수이름과 동일한 값을 안에 넣어 줄 경우, Type에서 받아온 변수 값이 전달되게 된다.  

     ````html
     </select>
     	<select id="manyEmp" resultType="emp"><!-- db-config에 이름 정의됨 -->
     	<![CDATA[select*from employees where employee_id < 200]]> <!-- 문자 그대로 읽어오는 CDATAsection(character data) -->
     	</select>
     ````

     * **< ![CDATA[  ]]**>: 만약 < 같은 부등호를 넣을 경우, SQL문을 해당 문장 그대로 넣어줄 수 있도록 < ![CDATA[ SQL문 ]]> 안에 넣어준다 

     ````html
     <insert id="insertEmp2" parameterType="emp">
     		<selectKey resultType="int" keyProperty="employee_id"> 
           <!-- parameter emp안에 employee_id가 있다면 int 타입으로 넣어줌. insert into employees values(select emp_seq.nextval from dual) -->
     		select emp_seq.nextval from dual
     		</selectKey> <!-- insert할 때 임시적으로 select 생성 -->
     	insert into employees values(#{employee_id}, #{first_name}, #{last_name}, #{email},
     	 #{phone_number}, sysdate, #{job_id}, #{salary}, null, #{manager_id}, #{department_id})
     	</insert>
     ````

     * **< selectkey>** : 해당 SQL문이 실행 될 때만 임시적으로 select를 생성한다. 

<br><br>

##### 예제) MVC와 DB활용

main

````java
package mybatis;

import java.io.IOException;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class EmpMain {

	public static void main(String[] args) throws IOException {
		//db 연결 요청 xml 
		SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
		SqlSessionFactory factory = builder.build(Resources.getResourceAsReader("mybatis/db-config.xml"));
		SqlSession session = factory.openSession(true); //수동 commit rollback > true >자동commit상
    
		EmpService service = new EmpServiceImpl();
		EmpDAO dao = new EmpDAO();
		dao.setSession(session);
		((EmpServiceImpl)service).setDao(dao);
    //test1
		EmpVO vo = service.getOneEmp(200);
		System.out.println(vo);
		
		//test2. Parameter 없는 경우(select*)
		List<EmpVO> list = service.getAllEmp();
		for(EmpVO vo : list) {
		System.out.println(vo);
		
		//test3. insert
	  EmpVO vo = new EmpVO(300, "사원", "김", "kim@a.com", "010222333", "IT_PROG", 100, 30000, 50);
		service.insertEmp(vo);
		System.out.println("한 명의 사원 저장 완료");
		EmpVO vo2 = service.getOneEmp(300);
		System.out.println(vo2);
		
		
		
		//test4. update 300번 사번 사원 이름을 대리로 변경 
		EmpVO vo3 = new EmpVO();
		vo3.setEmployee_id(300);
		vo3.setFirst_name("대리");
		service.updateEmp(vo3);
			
		//test5. delete
		int employee_id = 300;
		service.deleteEmp(employee_id);
		
		//test6. page 지정 출력
		int[] nums = {11, 20};
		List<EmpVO> list = service.getPageEmp(nums);
		for(EmpVO vo : list) {
			System.out.println(vo);
		}
````

VO

````java
package mybatis;

public class EmpVO {
	//employees 테이블 칼럼 =변수
	String first_name, last_name, email, phone_number, hire_date, job_id;
	int employee_id, manager_id, department_id;
	double salary, commission_pct;
	
	public EmpVO() {}
	
	public EmpVO(int employee_id, String first_name, String last_name, String email, String phone_number, String job_id,double salary, int manager_id, int department_id) {
		super();
		this.first_name = first_name;
		this.last_name = last_name;
		this.email = email;
		this.phone_number = phone_number;
		this.job_id = job_id;
		this.employee_id = employee_id;
		this.manager_id = manager_id;
		this.department_id = department_id;
		this.salary = salary;
	}

	public String getFirst_name() {
		return first_name;
	}
	public void setFirst_name(String first_name) {
		this.first_name = first_name;
	}
	public String getLast_name() {
		return last_name;
	}
	public void setLast_name(String last_name) {
		this.last_name = last_name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPhone_number() {
		return phone_number;
	}
	public void setPhone_number(String phone_number) {
		this.phone_number = phone_number;
	}
	public String getHire_date() {
		return hire_date;
	}
	public void setHire_date(String hire_date) {
		this.hire_date = hire_date;
	}
	public String getJob_id() {
		return job_id;
	}
	public void setJob_id(String job_id) {
		this.job_id = job_id;
	}
	public int getEmployee_id() {
		return employee_id;
	}
	public void setEmployee_id(int employee_id) {
		this.employee_id = employee_id;
	}
	public int getManager_id() {
		return manager_id;
	}
	public void setManager_id(int manager_id) {
		this.manager_id = manager_id;
	}
	public int getDepartment_id() {
		return department_id;
	}
	public void setDepartment_id(int department_id) {
		this.department_id = department_id;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public double getCommission_pct() {
		return commission_pct;
	}
	public void setCommission_pct(double commission_pct) {
		this.commission_pct = commission_pct;
	}
	@Override
	public String toString() {
		return "EmpVO [first_name=" + first_name + ", last_name=" + last_name + ", email=" + email + ", phone=" + phone_number
				+ ", hire_date=" + hire_date + ", job_id=" + job_id + ", employee_id=" + employee_id + ", manager_id="
				+ manager_id + ", department_id=" + department_id + ", salary=" + salary + ", commission_pct="
				+ commission_pct + "]";
	}
	
}

````

DAO

````java
package mybatis;


import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class EmpDAO {
	SqlSession session;

	public void setSession(SqlSession session) {
		this.session = session;
	}
	
	public EmpVO getOneEmp(int employee_id) {
		EmpVO vo = session.selectOne("kdigital.oneEmp", employee_id);
		return vo;
	}
	
	public List<EmpVO> getAllEmp() {
		List<EmpVO> list = session.selectList("kdigital.manyEmp");
		return list;
	}
	public void insertEmp(EmpVO vo) {
		session.insert("kdigital.insertEmp", vo);
	}
	public void updateEmp(EmpVO vo) {
		session.update("kdigital.updateEmp", vo);
	}
	public void deleteEmp(int employee_id) {
		session.delete("kdigital.updateEmp", employee_id);
	}
	public List<EmpVO> getPageEmp(int[] nums){
		return session.selectList("kdigital.pageEmp", nums);
	}
	public void insertEmp2(EmpVO vo) {
		session.insert("kdigital.insertEmp2", vo);
	}
}
````

Service(Interface)

````java
package mybatis;

import java.util.List;

public interface EmpService {
	public EmpVO getOneEmp(int employee_id);
	public List<EmpVO> getAllEmp();
	public void insertEmp(EmpVO vo);
	public void updateEmp(EmpVO vo);
	public void deleteEmp(int employee_id);
	public List<EmpVO> getPageEmp(int[] nums);
	public void insertEmp2(EmpVO vo);
}
````

ServiceImpl

````java
package mybatis;

import java.util.List;
//서비스 - 기능 단위 
public class EmpServiceImpl implements EmpService {

    EmpDAO dao; 
	
	public void setDao(EmpDAO dao) {
		this.dao = dao;
	}

	@Override
	public EmpVO getOneEmp(int employee_id) {
		return dao.getOneEmp(employee_id);
	}

	@Override
	public List<EmpVO> getAllEmp() {
		//dao 다른 메소드 호출 실행 
		return dao.getAllEmp();
		
	}

	@Override
	public void insertEmp(EmpVO vo) {
		 dao.insertEmp(vo);
		
	}

	@Override
	public void updateEmp(EmpVO vo) {
		dao.updateEmp(vo);
		
	}

	@Override
	public void deleteEmp(int employee_id) {
		dao.deleteEmp(employee_id);
		
	}

	@Override
	public List<EmpVO> getPageEmp(int[] nums) {
		return dao.getPageEmp(nums);
	}

	@Override
	public void insertEmp2(EmpVO vo) {
		dao.insertEmp2(vo);
		
	}
}
````

