# 파일 업로드

> 지금까지 서버-클라이언트 전송 datatype이 문자 데이터들이었다면, 파일을 사용한다. 



파일전송을 위해 클라이언트와 서버에서 해야 하는 사항들은 다음과 같다.

* 클라이언트

  * 파일은 크기의 한계 때문에 url로 데이터값 전송이 불가능하다. 

    즉 <u>GET방식의 전송은 불가능</u> 

    **form method= post** 방식은 필수, 여기에 더해서 form에 다음을 추가한다.

    **form enctype=multipart/form-data** 이 역시 필수

    **input type=file** 필수

    ````html
    <form action="<%=request.getContextPath() %>/fileupload"
    	method=post enctype="multipart/form-data">
    <input type=file>
    ````

  * 본래 데이터가 넘어가는 형태는 ?구분자를 기준으로 각 값을 &로 구분(ex. ?id=dd&pw=xx)한다면, 파일의 경우 각 값이 완전히 분리된 형태로 넘어간다. 



* 서버

  * pom.xml에 환경설정

    * fileupload관련된 라이브러리 추가

    ````html
    <!-- 파일 업로드 -->
    <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.1</version>
    </dependency>
    
    <!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.6</version>
    </dependency>
    ````

  * servlet-context.xml 환경설정

    ````html
    <beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>
    ````

    * resolver가 붙은 것들은 일종의 해석자(읽어오는 자)역할을 한다. 이 bean 객체는 file을 읽어오는 객체

    

  * 읽어오는 방식은 **MultipartFile**

    (파일명, 내용, 길이 타입..)과 같은 식으로 읽어옴. 읽어온 파일은 특정 서버의 경로(해당 경로를 기입)에 지정해야 한다. 

    ````html
    String savePath="/Users/lulala/upload";
    ````



#### Spring Boot의 경우

부트의 경우 해당 라이브러리가 이미 내장되어 있기 때문에 위와 같은 환경설정 필요 없이 바로 코드작성이 가능하다. 



* 예제

  * UploadVO

  ```java
  package com.mulit.myboot01;
  
  import org.springframework.web.multipart.MultipartFile;
  
  public class UploadVO {//uploadform.jsp 4개 전달 
  	String name;
  	String description;
  	//*.jar - pom.xml(maven tool(auto Library download)
  	MultipartFile file1;
  	MultipartFile file2;
  	
  	public String getName() {
  		return name;
  	}
  	public void setName(String name) {
  		this.name = name;
  	}
  	public String getDescription() {
  		return description;
  	}
  	public void setDescription(String description) {
  		this.description = description;
  	}
  	public MultipartFile getFile1() {
  		return file1;
  	}
  	public void setFile1(MultipartFile file1) {
  		this.file1 = file1;
  	}
  	public MultipartFile getFile2() {
  		return file2;
  	}
  	public void setFile2(MultipartFile file2) {
  		this.file2 = file2;
  	}
  	
  }
  ```
  
  * UploadController
  
  ````java
  package upload;
  
  import java.io.File;
  import java.io.IOException;
  import java.util.UUID;
  
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.ModelAttribute;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RequestMethod;
  import org.springframework.web.multipart.MultipartFile;
  
  @Controller
  public class UploadController {
  	
  	public static String getUuid() {
  		return UUID.randomUUID().toString().replaceAll("-","").substring(0,10);
  	}
  	
  	@RequestMapping(value="/fileupload", method=RequestMethod.GET)
  	public String uploadform() {
		return "/upload/uploadform";
  		
  	}
  	@RequestMapping(value="/fileupload", method=RequestMethod.POST)
  	public String uploadresult(@ModelAttribute("vo") UploadVO vo) throws IOException {
  		//업로드한 파일 객체 
  		MultipartFile multipartfile1 = vo.getFile1();
  		MultipartFile multipartfile2 = vo.getFile2();
  		
  		//업로드한 파일명 추출
  		String filename1 = multipartfile1.getOriginalFilename();
  		String filename2 = multipartfile2.getOriginalFilename();
  		
  		//서버 저장 경로 설정
  		String savePath="/Users/lulala/upload"; // 
  		
  		// 중복파일처리1 : 
  		// api : 랜덤암호화변경이름
  		// a.txt --> 123wsdjhfckdjf.txt
  		String path []= filename1.split("[.]");
  		for(String s : path) System.out.println("==="+s);
  		//String ext1 = "."+filename1.split("[.]")[filename1.split("[.]").length-1];
  		String ext1 = filename1.substring(filename1.lastIndexOf("."));
  				
  		//String ext2 = "."+filename2.split("[.]")[filename2.split("[.]").length-1];
  		String ext2 = filename2.substring(filename2.lastIndexOf("."));	
  				
  		System.out.println(ext1+":"+ext2);
  		String saveFileName1 = getUuid() + ext1;
  		String saveFileName2 = getUuid() + ext2;
  				
  		File file1 = new File(savePath + filename1);
  		File file2 = new File(savePath + filename2);
  		
  		//서버 저장
  		multipartfile1.transferTo(file1);
  		multipartfile2.transferTo(file2);
  		
  		return "/upload/uploadresult";//view에서 업로드 파일명 출력 
  	}
  }
  ````
  
  * 클라이언트로부터 파일을 받게 될 경우, 파일을 받는 해당 폴더에서 동일 이름으로 파일이 충돌하지 않기 위해 랜덤이름을 지정 



# 파일 다운로드



````java
package com.mulit.myboot01;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.OutputStream;

import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.util.FileCopyUtils;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class DownloadController {
	@RequestMapping("/filedownload")
	public ModelAndView downloadform(){
		//upload폴더 저장 파일리스트 모델로 생성 뷰로 저장 
		ModelAndView mv = new ModelAndView();
		File path = new File("/Users/lulala/upload/");
		String[] filelist = path.list();
		mv.addObject("details", filelist);
		mv.setViewName("upload/downloadform");
		return mv;
	}
	
	@RequestMapping("/downloadresult")
	public void downloadresult(String file, HttpServletResponse response) throws IOException
	{//다운로드해야 파일명 
		//file 다운로드 이후 다른 뷰 전송x
		File f = new File("/Users/lulala/upload/", file); 
		
		//파일 다운로드를 응답하겠다고 선언. 파일명, 길이 포함
		response.setContentType("application/download"); //다운로드 할 파일
		response.setContentLength((int)f.length()); //다운로드 받아야 할 파일의 길이 지정, long타입 리턴
		response.setHeader("Content-Disposition", "attachment;filename=\""+ file + "\"");
		
		//파일명에 해당하는 파일을 읽어서 클라이언트에게 복사 출력
		OutputStream out = response.getOutputStream();//클라이언트에게 출력
		FileInputStream fin = new FileInputStream(f);//파일을 읽어서 
		FileCopyUtils.copy(fin, out); //복사
		fin.close();
		out.close();
	}
}
````





# 파일 이미지 매핑

> 파일 이미지를 업로드 할 때, 서버 컴퓨터 내의 파일(외부파일)을 출력할 경우, 이미지파일이 해당 프로젝트 resources나 static에 위치해 있지 않다면 외부경로이기 때문에 출력할 수 없다. 
>
> jsp내부에서 외부이미지(예를 들면 업로드했던 이미지)를 사용하려면, 서버 내부의 경로를 직접주지 않고 그 파일에 대한 mapping을 해줘야 한다.



* 업로드받은 파일을 업로드 할 경우

  * Server: < img src="c:/upload/a.png">
  * Client: < img src="server내부지정 경로/">

  c:/upload 폴더 > **/upload uri 요청 추가 매핑**이 반드시 필요



````java
package com.mulit.myboot01;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/upload/**")
                .addResourceLocations("file:///Users/lulala/upload/");
        // 파일 저장하는 경로를 upload url로 매핑  
    }
}
````

* @Configuration 

  : 현재 클래스에서 설정하는 모든 결과를 bean객체로 해석

  * override 해야 함 

