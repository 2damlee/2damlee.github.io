---

---

<br>

기본 명령어 

| 명령어 | 설명                                                         |
| ------ | ------------------------------------------------------------ |
| ls     | 파일 목록 조회                                               |
| file   | 파일의 종류 조회                                             |
| pwd    | 절대 경로 방식으로 현재 작업 디렉토리를 조회                 |
| cd     | 작업 디렉토리 이동(미지정시 사용자 홈 디렉토리로 이동)<br />* 지정 방법은 절대경로와 상대경로가 있음 |
| mkdir  | 디렉토리 생성<br />-p는 필요한 경우 상위 디렉토리를 생성<br />-m은 디렉토리를 만들면서 접근 권한을 설정 |
| rmdir  | 디렉토리 삭제<br />* 비어있는 디렉토리만 삭제 가능, 모든 디렉토리 삭제할 시 rm -r idr 또는 rm -rf dir 사용 |
| cp     | 파일이나 디렉토리 복사<br />* 대상 파일 존재시 덮어쓰기 수행 |
| mv     | 파일의 이름을 변경 또는 다른 디렉토리로 이동                 |
| rm     | 파일 삭제 명령                                               |

<br>

##### ls 명령어

| 짧은 옵션 | 긴 옵션       | 설명                                                         |
| --------- | ------------- | ------------------------------------------------------------ |
| -a        | --all         | 숨김 파일 포함 모든 파일을 보여줌                            |
| -d        | --directory   | 디렉토리 자체에 대한 정보                                    |
| -F        | --classify    | 우측에 파일 종류를 알려주는 문자 붙임<br />(실행 파일은 *, 디렉토리는 /, 심볼릭 링크는 @) |
| -l        | --format=long | 긴 포맷으로 결과 보여줌                                      |
| -R        | --recursive   | 재귀적으로 수행되는데 서브 디렉토리의 내용도 나열            |
| -S        | --sort=size   | 파일의 크기 순서로 결과 보여줌                               |
| -t        | --sort=time   | 최종 수정 시간 순으로 보여줌                                 |

<br>

<br>

#### 파일의 접근 권한

- 사용자 부류
  - 소유자(u), 그룹(g), 기타(o)
- 권한
  - 읽기(r) : 파일 내용 보기
  - 쓰기(w) : 파일의 내용 수정과 삭제, 파일 이름 변경
  - 실행(x) : 파일 실행 

* chmod 명령어

  파일 소유자가 파일의 접근 권한을 변경하는 명령

  `chmod [option] mode files`

  option: `chmod -R 755 dir1` or `[ugoa][+-=][rwx]`(ex.`chmod u+x fiel1`)

* umask 명령어

  접근권한의 기본값을 출력하거나 설정하는 명령

  (보통 /etc/bashrc에 설정되어 있음)

  `umask [-S][mask]`

  ex. `umask 002` : 기타 사용자에게 쓰기 권한을 부여하지 않겠다. (775라는 접근권한 *파일의 경우 실행 권한은 부여되지 않으며 접근권한은 664)

- chown 명령어

  - root 사용자가 파일이나 디렉토리의 소유자를 변경하는 명령

  `chown [option] newnowner files`

  : newowner에 소유자만 주어지면 소유자를 변경

  - <u>소유자:그룹</u> 또는 <u>소유자.그룹</u>의 형태로 지정 가능

<br>

#### 파일 내용 확인

* more 명령

  * 파일의 내용을 화면 단위로 출력하는 명령

    - 한 화면을 보여준 상태에서 멈춤

    `more [option] files`

* less 명령

  * more 명령의 개선된 버전
    * 위 아래 스크롤 가능, 다양한 내부 명령이 있음 (ex. 검색)

* head 명령

  * 파일의 맨 앞 부분을 출력하는 명령

  `head [option] [files]`

* tail 명령

  * 파일의 마지막 부분을 출력하는 명령

  `tail [option] [files]`

  `tail -f /var/log/messages` : -f옵션을 사용하면 변화하는 파일의 내용을 계속 감시 가능

<br>

#### 명령의 연결과 확장

- cat 명령

  - 하나의 파일 또는 여러 파일을 연결(concatenate)시켜 화면에 출력

  `cat [option] [files]`

  - 파일을 지정하지 않으면 표준 입력으로부터 읽는다.
    - cat > file을 수행하여 텍스트 파일 생성 가능
  - 여러 파일의 내용을 연결시킬 때 사용 가능(redirection)
  - 옵션 -n을 사용하면 라인 번호 o