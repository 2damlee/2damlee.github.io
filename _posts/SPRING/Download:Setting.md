# Spring

> 자바 개발을 위해 어느정도의 틀을 제공하는 일종의 프레임워크 중 하나. 

* MVC: Model, view, Controller Structure ( Model2 )

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.01.47.png" alt="스크린샷 2021-04-13 오후 10.01.47" style="zoom:50%;" />

* 각 역할
  * 서블릿: Controller 역할 (요청 파라미터를 추출, 분석, 처리 할 DAO를 결정 -> 결과를 보여줄 JSP를 결정. 일종의 교통정리)
  * DAO, DTO: model(데이터를 만들어주는 역할)
  * DAO, DTO: model(데이터를 만들어주는 역할)

<u>Spring에서는 위와 같은 MVC 구조가 강제</u>된다.  





### Spring 시작하기

1. spring.io 에서 해당 파일을 다운, 원하는 폴더에 압축해제 

2. 시작할 때 workspace 지정 

3. help>Eclipse Marketplace에서 STS 검색, 버전에 맞는 STS 다운로드(web, server항목 등이 있음)

   * 나의 경우 SpringToolSuite4 버전을 사용, 따라서 Spring Tool3 Add-On for Spring Tools 4 3.9.16.REALEASE를 다운

4. **Changes**

   1) Change <java-version> to 1.8
   	Change <org.aspectj-version> to 1.6.10 

   ![스크린샷 2021-04-16 오후 7.28.24](%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.28.24.png)

   

   

   2) Change Compiler

   * Preferences>Java Compiler> change java version(1.8)
   * Edit Library>JRE System Library> change java version(1.8)
   * Project Facets>  change java version(1.8)
   * Project Facets> Runtimes Apache Tomcat (add libarary)

   

   3) Connect Tomcat (after connect, check libarary)

    

5. 파일 위치 파악하기 

   <img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.03.38.png" alt="스크린샷 2021-04-13 오후 10.03.38" style="zoom: 50%;" />

   ​												: 자바 소스를 생성 공간

   <img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.04.00.png" alt="스크린샷 2021-04-13 오후 10.04.00" style="zoom:50%;" />

   ​													: eclipse로 따지면 WebContent의 공간

   ​													: classes ( = eclipse에서 lib )





##### Dynamic Web Installation

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.23.34.png" alt="스크린샷 2021-04-16 오후 7.23.34" style="zoom:50%;" />







#### Create Spring mvc project

* new > Spring Legacy Project > Spring MVC Project > package name > finish
  * 초기 패키지 생성은 필수, test.my.mvc로 이름을 정할 시, 서버가 시작되면 url의 끝은 /mvc로 시작하게 된다. 따라서 여러 패키지를 만들게 될 시 세번째 이름은 중복되지 않도록 한다.(충돌 오류발생함)



### Spring Dependency Injection

: 클래스 객체를 개발자가 코드에서 생성하지 않고 프레임워크가 생성하여 사용하는 방법 



##### 의존성 주입(Dependency Injection)

* coupling: 객체 하나를 변경하니 같이 변경해야 할 코드가 동반된다면, 이 경우 커플링이 높다(결합도가 높다)고 한다. 

* 결합도 낮추기

  * 인터페이스 활용

    * 유사 기능의 메소드, 그러나 다른 코드를 정의 할 인터페이스를 생성하고 그 안에 메소드를 선언
    * 인터페이스를 상속한 하위 클래스들을 표준화시키고 메소드를 오버라이딩
    * 인터페이스는 여러가지 하위 클래스 타입이 호환 가능해진다. 

    하나의 인터페이스 타입에 여러가지 하위 객체들 생성이 가능 

* 주입: 값을 다른 객체로부터 전달받아오는 것을 주입 받는다고 표현한다. 

  * 해당 클래스가 결정한 객체가 아닌 외부로부터 전달받은 객체를 의미. 

DAO VO의 예시)

​	VO1

`````java
package emp;

public class EmpVO implements VO{
	int id;
	String name;
	double salary;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	
}
`````

​	VO2

````java
package emp;

public class MemberVO implements VO {
	int id;
	String name;
	String email;
	
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	
}
````

​	DAO

````java
package emp;
public class EmpDAO {
	void insertEmp(VO vo){
		// dtod 받아온 객체를 SQL에 저장할 코드
		System.out.println("db등록 완료 했습니다.");
	}
}
````

​	Main

````java
package emp;

public class EmpMain {

	public static void main(String[] args) {
		VO vo = new EmpVO();
		vo.setId(100);
		vo.setName("이사원");
		
		VO vo2 = new MemberVO();
		vo2.setId(100);
		vo2.setName("이사원");

		EmpDAO dao = new EmpDAO();
		dao.insertEmp(vo)
		dao.insertEmp(vo2); 
		System.out.println("회원 등록 마쳤습니다. ");
	}

}

````

