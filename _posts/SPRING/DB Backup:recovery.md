# DataBase Backup/recovery

> drop table 할 경우, 데이터 복구 불가한 상황에 대비해 백업을 해놓는 과정



* 과정 

  SQL DB생성, 해당 DB을 서버 컴퓨터의 파일에 따로 카피(Backup)

  이후 복구해야 할 때, 해당 파일경로에서 데이터값을 가져와 복원 

  * 주의

    Oracle version, JDBC계정은 동일해야 한다. 



* 해당 과정은 Terminal을 통해 실행된다. 

  ````bash
  exp userid=jdbc/jdbc@xe file=c:/kdigital/backup tables=(inform)
  imp userid=jdbc/jdbc@xe file=c:/kditial/backup tables=(inform) 
  ````



| option       | Default Value |                                             |
| ------------ | ------------- | ------------------------------------------- |
| buffer       |               | evaluation buffer크기 byte단위              |
| file         | expat.dmp     | exp할 파일명                                |
| log          |               | 로그를 저장할 파일명                        |
| grants       | yes           | 해당 스키마에 설정된 권한도 export할 것인지 |
| indexes      | yes           | 인덱스 export유무                           |
| rows         | yes           | 데이터를 받을 것인가                        |
| constraints  | yes           | 제약조건을 받을 것인가                      |
| compress     | yes           | exp 데이터를 하나의 set으로 압축할 것인가   |
| full         | no            | 전체 데이터베이스 exp유무                   |
| owner        | current user  | exp 받을 사용자                             |
| tables       |               | exp 받을 테이블                             |
| table spaces |               | exp 받을 테이블 스페이스                    |



##### Docker의 SQL을 사용할 경우

* Docker 계정을 접속 후 root에서 실행

  * tables= 이름에 ()괄호는 생략해서 준다. 

  * 파일 권한 자꾸 에러뜨면 그냥 기본값으로 주고 받아도 됨 

    ````bash
    imp userid=jdbc/jdbc@xe file=expdat.dmp tables=test
    imp userid=jdbc/jdbc@xe file=expdat.dmp tables=test
    ````

  * 파일 지정하고싶으면 Datapump를 사용하는 것이 좋음 

    * 권한 지정 후 export를 해야함 

    <a href="https://myjamong.tistory.com/221">참조 링크</a>

