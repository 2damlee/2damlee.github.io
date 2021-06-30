# PYTHON

> Java가 웹서버를 개발하기 위한 언어라면, Python은 빅데이터 분석에 강점을 갖고 있다. 



#### 파이썬 사용 환경

* 파이썬을 사용하는데 여러 환경이 있음

  * python idle
  * pycharm
  * eclipse 내 추가 프로그램 설치
  * jupyter notebook
  * google colab

  

  

##### python idle로 시작하기

* python.org에서 다운로드
  * 설치시 경로는 원하는 곳에 설치
* Python IDLE 실행
  * IDLE Shell은 한 문장만 실행 가능 
  * 여러 문장 실행시, newfile>새로운 폴더 생성 후 작성
    * 파일이 저장될 경로는 지정
    * 기본 확장자는 .py 
  * 작성 후 실행은 run 





### 변수타입

* 변수선언

  : JAVA에서는 변수를 선언할 때 타입을 함께 선언해야 한다면, PYTHON에서는 바로 선언이 가능

  ````python
  k = None
  b = 30
  i ='abc'
  ````

  * Python에서 null 타입은 None 

  * 출력문은 print 함수 사용 

    ````python
    print(type(k))
    ````

    

* 파이썬의 변수타입

  > 파이썬에 많은 변수타입이 있으나 아주 기본적인 것만 정리해두고, 나머지는 추가해서 숙지하도록 한다.

  | 변수타입 |                  |
  | -------- | ---------------- |
  | None     | null             |
  | int      | 정수             |
  | float    | 실수             |
  | bool     | 참/거짓(boolean) |
  | str      | 문자열           |

  * python에서는 Java처럼 변수를 선언만 해둘 수 없음(ex. int i;) 대신 None타입으로 선언



### 연산자 

* 산술연산자 

| 연산자          |                       |
| --------------- | --------------------- |
| +, -, *         | 덧셈, 뺄셈, 곱셈      |
| /               | 나눗셈 (소수점 출력)  |
| //              | 나눗셈 (소수점 생략)  |
| %               | 나머지                |
| A**B, pow(A, B) | 지수 (A의 B 거듭제곱) |

* 관계연산자

| 연산자 |             |
| ------ | ----------- |
| >      | 크다        |
| >=     | 크거나 같다 |
| <      | 작다        |
| <=     | 작거나 같다 |
| ==     | 같다        |
| !=     | 같지 않다   |

* 논리연산자

| 연산자 |                    |
| ------ | ------------------ |
| and    | 둘 다 참 True      |
| or     | 둘 중 하나 참 True |
| not    | 거짓이면 True      |

* 문자연산자

  * python에서 문자열 구분은 " ",' ' 둘 다 가능 

  * 문자열에서 + 는 같은 타입끼리만 결합이 가능 

    * 자동형변환 X

  * 문자열 * int 사용시 int만큼 반복 

    ````python
    a='abc'
    print(a*3)
    #abcabcabc 출력
    ````

  * slice연산자 

    **변수이름[index:index]** :첫번째 인덱스값부터 두번째 인덱스값까지 

    * 첫 인덱스만 선언: 해당 인덱스부터 나머지 문자열
    * 마지막 인덱스만 선언: 0번 인덱스부터 해당 인덱스 이전까지

    ````python
    a='abc'
    print(a[2:4]) #2번 인덱스부터 4번 인덱스 이전까지 부분 문자열
    print(a[2:])  #2번 인덱스부터 나머지 문자열
    print(a[:4])  #0번 인덱스부터 4번 인덱스 이전까지
    ````

  * **in** : 포함 여부 - bool값을 return

    ````python
    a="multicampus"
    print("cam" in  a) #True
    ````

  * **find**: 해당 문자열의 포함 위치가 몇 번에 위치해있는지를 return해주는 함수 

    ````python
    print(a.find('cam'))
    print(a.rfind('cam')) #오른쪽부터 조회
    //int return
    ````

  * **count**: 문자열에서 특정 문자의 개수를 세어준다

    * ex. multicampus에 m이 몇 개 들어있는가

    ```python
    a="multicampus"
    print(a.count('m')) # 2
    ```

  * **len**: 총 문자 개수

    * 문자열타입 뿐 아니라 정수 타입, 배열 타입 모두 사용 가능

    ````python
    a="multicampus"
    print(len(a)) #총 문자 개수 //11
    ````

  * **.format()**: 블럭 안에 변수값을 넣어주는 기능

    ````python
    print("{}는 정수이고 {}는 문자열입니다".format(1, 'a')) #1는 정수이고 a는 문자열입니다
    print("{:10.2f}".format(10/3)) #정수 10자리에 소수점 둘째자리까지 // 3.33
    print("{:10d}".format(10//3)) #정수만 //3
    ````

    

### 타입변환

* 문자열 -> 정수 

  **str(intType)**

  ````python
  print(type(100)) #int
  print(type(str(100))) #str
  print("abc"+"def"+ str(100))
  ````

* 정수->문자열

  **int(strType)**



### Scanner input

* 파이썬에서 키보드 입력받기 

  * 입력시 입력 값이 변수값이 됨

  ````python
  a = input();
  in1 = input('정수 1개를 입력');
  ````



### 명령행 입력변수 (Customize Run)

> 자바로 치면 run configuration

* 입력변수 만들기
  * import sys 필수 
  * 첫번째 값, [0] 값은 모듈 이름 

```python
import sys
print(sys.argv[1])
print(sys.argv[2])
print(sys.argv[3])
# sys 출력시 : ['/Users/lulala/JAVA_multi/python_source/a.py', 'python', '100', '3.14']
```



### 내장함수 



* 데이터 저장 함수 

  | 저장타입   |                                                              |
  | ---------- | ------------------------------------------------------------ |
  | list[ ]    | 동적인 특성<br />여러 타입의 데이터 저장 가능, index O       |
  | Tuple ( )  | 값을 변경할 수 없는 리스트                                   |
  | set{ }     | 저장 순서 없음(index X)<br />중복데이터 불가(추가될 시 무시) |
  | dictionary | 저장 순서 없음(index X)<br />key:value로 구성                |

  

  * list

  ````python
  a2=[1, 2, 3, 4, 5]
  a=["title", 2, 45000.05, True, [90, 30]]
  ````

  * 데이터 추가

    * append : 배열 맨 뒤에 추가 

      ````python
      a.append(100)
      ````

    * insert: 배열 중간에 끼워넣기

      ```python 
      a.insert(1, 200)
      #1번 인덱스에 200 value 추가 
      #끼워넣기, 원래 존재했던 index-value는 한 칸씩 뒤로 밀려남
      ```

  * 데이터 변경

    ````python
    a[1]=300 #1번 인덱스의 값을 300으로 변경
    ````

  * 데이터 삭제

    * pop() , remove( ), del

    ````python
    #리스트의 마지막 데이터 삭제
    a.pop()
    #해당 data 삭제
    a.remove(데이터값)
    #해당 인덱스 데이터 삭제
    del a[0] 
    ````

    

  * Tuple

  ```python
  d=(1, 2, 3, 4, 5)
  ```

  ( )없는 Tuple

  ```python
  a=1, 2, 3, 4, 5#unpacking
  a, b= 1, 2  #두 개의 변수를 동시 선언. a는 TupleX 변수
  ```

  

  * set 

    * value만 존재, 중복 데이터X

    ```python
    b={1, 2, 3, 4, 5, 5} #중복데이터 추가하면 무시
    b.add(5) #중복데이터 추가하면 무시
    ```

    추가

    **.add(데이터값)**

    

  * dictionary

    * key:value - key값 중복X

    ```python
    c={"name":"lee", "2":200, "id":"lee"}
    #출력시 
    print(c.items())
    print(c.keys())
    print(c.values())
    ```





* 문자열 관련 내장함수

  

  * **in** : 포함 여부 - bool값을 return

    ````python
    a="multicampus"
    print("cam" in  a) #True
    ````

  * **find**: 해당 문자열의 포함 위치가 몇 번에 위치해있는지를 return해주는 함수 

    ````python
    print(a.find('cam'))
    print(a.rfind('cam')) #오른쪽부터 조회
    //int return
    ````

  * **count**: 문자열에서 특정 문자의 개수를 세어준다

    * ex. multicampus에 m이 몇 개 들어있는가

    ```python
    a="multicampus"
    print(a.count('m')) # 2
    ```

  * **len**: 총 문자 개수

    * 문자열타입 뿐 아니라 정수 타입, 배열 타입 모두 사용 가능

    ````python
    a="multicampus"
    print(len(a)) #총 문자 개수 //11
    ````

  * **.format()**: 블럭 안에 변수값을 넣어주는 기능

    ````python
    print("{}는 정수이고 {}는 문자열입니다".format(1, 'a')) #1는 정수이고 a는 문자열입니다
    print("{:10.2f}".format(10/3)) #정수 10자리에 소수점 둘째자리까지 // 3.33
    print("{:10d}".format(10//3)) #정수만 //3
    ````

    

* 절대값, 버림 반올림

  | 함수    |                         |
  | ------- | ----------------------- |
  | abs()   | 절대값 계산 함수        |
  | round() | 반올림 계산 함수        |
  | trunc() | 버림계산 함수(math모듈) |





### 조건문/반복문

JAVA와 달리 { }대신 들여쓰기로 포함관계여부를 결정. 동일 포함 관계에 있는 구문은 같은 들여쓰기 내에 작성

해당 키워드가 종료되면 반드시 **:**

종료는 **return**

 

1. 조건문

* **if문**

  ````python
  if 10>5:
        print(1)
        print(100)
  elif 10==10:
    		print(2)
  else:
        print(2)
  			print(200)
  ````

  * else if 는 elif



2. 반복문 

* **while문**

  ````python
  import random
  lottoset=set() #data가 없는 set 생성(중복데이터 X, 무시)
  cnt=0
  while True:
        if len(lottoset) == 6:
              break
        cnt = cnt+1
        lottonum=random.randint(1, 45)
        print("{}번째 난수 {}를 생성합니다.".format(cnt, lottonum))
        lottoset.add(lottonum)
  print(lottoset)
  
  ````

  * **random.randint(1, 45)**
    * random함수: 1-45사이에서 랜덤 정수 하나를 출력 



* **for문**

  for 변수 in 반복횟수

  ````python
  print(range(1, 11, 1)) #1부터 시작해서 10까지(11보다 작은)의 범위, 1씩 증가 즉 1-10까지의 범위
  range(11) #시작값 default=0 / 증가값 default=1
  for i in range(11):#0-10까지 반복(11반복)
        print(i)
  ````

  * **range**

    range(시작값, 끝나는값, 증가값)

    range(끝나는값) : 기본 시작값은 0, 기본 증가값은 1

  ````python
  a=[1, 2, 3, 4, 5]
  for i in a:#배열 안의 값이 없을때까지 출력
        print(i)
  ````

  ```python
  #index포함해서 출력하기
  for i in range(0, len(a), 1):#배열 안의 값이 없을때까지 출력
        print("{}번째 인덱스 값은 {}입니다.".format(i, a[i]))
  ```

  ```python
  #dictionary출력하기 
  d={"k1":1, "k2":2, "k3":3, "k4":4, "k5":5}
  # d.items() : duple형태로 출력 (소괄호), key:value 출력
  for key, value in d.items():
        print("{}키의 값은 {}입니다".format(key, value))
  ```

  

### 함수

> 사용자 정의 함수 
>
> * 특정 데이터에 포함되어있는 객체를 메소드라고 쓰고, 포함되어있지 않은 객체를 함수라고 한다.
> * 함수: 입력값(매개변수)을 받아 특정 기능을 구현, 결과 return
> * 함수이름은 내장함수 이름과 동일하게 두지 않는다.(동일하게 선언시 내장함수 사용 불가)



* 매개변수 없는 함수 정의

  ````python
  def hello_3times():
        print("hello")
        print("hello")
        print("hello")
  #함수 호출/실행
  hello_3times()      #print(hello_3times()) > None -return값이 없음 
  ````



* 매개변수 있는 함수 정의

  ````python
  #매개변수 있는 함수 정의
  def message_ntimes(message, n):
        for i in range(1, n+1, 1):
              print(message)
  
  #함수 호출
  message_ntimes('python', 5)
  ````



* 기본 매개변수가 있는 함수 정의 

  ````python
  #매개변수 입력 안할 시 기본 매개변수로 실행
  def basic_ntimes(message='java', n=5):
        for i in range(1, n+1, 1):
              print(message)
          
          
  #함수 호출
  #basic_ntimes()
  basic_ntimes(message='pypypython')     #keyword 매개변수/순서 상관 X
  ````



* 가변 매개변수 함수 정의

  * 가변 매개변수는 하나만 선언O 일반매개변수 뒤에만 선언
  * 일반매개변수가 앞에 선언되어 둘이 같이 선언될 수 X(일반매개변수에 값이 전달되지 않음)

  ````python
  def dynamic_message(n, *msg):
        for i in msg:
              print(i)
              
  dynamic_message(4, 'python')
  dynamic_message('python', 'java')
  dynamic_message('python', 'css', 'spring')
  ````

  





