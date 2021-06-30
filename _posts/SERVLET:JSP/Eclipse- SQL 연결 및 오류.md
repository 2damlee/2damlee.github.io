## Eclipse에서 SQL 연동하기

 

1) consol창에 있는 Data Sorce Explorer에서 Database Connection New... 

![스크린샷 2021-04-08 오전 10.18.33](/Users/lulala/Library/Application%20Support/typora-user-images/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.18.33.png)





2) 해당하는 SQL 프로그램 선택 (oracle)

<img src="/Users/lulala/Library/Application%20Support/typora-user-images/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.18.41.png" alt="스크린샷 2021-04-08 오전 10.18.41" style="zoom: 67%;" />





4) 설치된 oracle 버전 선택 (Oracle Thin Driver 11)

![스크린샷 2021-04-08 오전 10.19.13](/Users/lulala/Library/Application%20Support/typora-user-images/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.19.13.png)





5) 기존의 jdbc.jar 버전은 삭제, (맞지 않음) ojdbc6.jar의 경로를 찾아서 추가 

<img src="/Users/lulala/Library/Application%20Support/typora-user-images/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.19.35.png" alt="스크린샷 2021-04-08 오전 10.19.28" style="zoom:67%;" />

<img src="/Users/lulala/Library/Application%20Support/typora-user-images/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.20.08.png" alt="스크린샷 2021-04-08 오전 10.20.08" style="zoom:67%;" />



6) Database instance는 xe로 변경, Host는 나의 서버, 스키마(계정) username, password 입력해주고 finish 

<img src="/Users/lulala/Library/Application%20Support/typora-user-images/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.21.32.png" alt="스크린샷 2021-04-08 오전 10.20.58" style="zoom:67%;" />







## 오류와 오류 해결

나의 경우 SQL파일을 생성해 연동하거나, .java 파일에서 데이터베이스를 연동할 경우 문제가 생기지 않았으나 servlet파일만 실행하면 데이터베이스 연동이 되지 않았다. jdk파일 경로에 ojdbc6.jar도 잘 위치해 있었다. 

* 이 경우, 웹어플리케이션을 실행하는 톰캣과 커넥이 안 되는 문제. 

* tomcat이 설치된 파일 경로를 따라가서 lib 파일 내부에 .jar 파일들이 모여있는 곳에 jdk파일 내부에 위치한 ojdbc6.jar를 복사해 넣어주면 해결된다. 



끝!

 