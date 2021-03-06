---
title: "Python basic(2) - Function"
categories:
  - Java
layout: archive
sidebar_main: true
author_profile: true
tags:
  - python
---



<br>

<br>

사용자 정의 함수 

* 특정 데이터에 포함되어있는 객체를 메소드라고 쓰고, 포함되어있지 않은 객체를 함수라고 한다.

* 함수: 입력값(매개변수)을 받아 특정 기능을 구현, 결과 return

* 함수이름은 내장함수 이름과 동일하게 두지 않는다.(동일하게 선언시 내장함수 사용 불가)

  <br>

  

  * 매개변수 없는 함수 정의

    ````python
    def hello_3times():
          print("hello")
          print("hello")
          print("hello")
    #함수 호출/실행
    hello_3times()      #print(hello_3times()) > None -return값이 없음 
    ````

  <br>

  * 매개변수 있는 함수 정의

    ````python
    #매개변수 있는 함수 정의
    def message_ntimes(message, n):
          for i in range(1, n+1, 1):
                print(message)
    
    #함수 호출
    message_ntimes('python', 5)
    ````

  <br>

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

  <br>

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

    <br>

##### return 있는 경우

* return값이 있는 경우

  ```python
  #return
  def returnl_func():
        print("리턴전")
        return 0
  #실행
  r1=returnl_func()
  print(r1)
  ```

  * return값이 없는 경우, 출력하면 None 
  * return 뒤에 문장 입력시, 실행되지 않음. return 이후는 함수 중단

* return값이 여러개인 경우

  ````python
  #return값이 여러개 함수 
  def return3_func():
        return 1, 2, 3, 4, 5
  #실행
  r1=return3_func()
  print(r1)
  #출력 (1, 2, 3, 4, 5)
  ````

  * <u>return타입을 tuple타입으로 취급함</u> 
    * 변경/추가/삭제 불가
  * list/set/dict으로 바꾸고 싶은 경우, return 타입을 list [ ] 나 set { } dict{ : }으로 만들어주면 된다. 선언 없을시(기본나열) 기본값이 tuple. 

<br>

<br>



##### 매개변수/지역변수/전역변수

````python
a = '전역변수'
def vartest(b):#매개변수
      c = '지역변수'
      
````

* 지역변수와 전역변수는 같이 사용 불가 

  * 즉 this의 개념이 불가능 

  ````python
  a = "전역변수"
  def vartest(b):#매개변수
        a="지역변수 a"
        c = "지역변수"
        print(a)
        print(b)
        print(c)
   # a는 지역변수 
  ````

* 전역변수를 지역에서 사용하기

  **global**: 전역변수임을 나타내는 말 

  * global을 선언한 뒤에 해당 전역변수 사용 가능

  ````python
  a = "전역변수"
  def vartest(b):#매개변수
        global a
        a="전역변수 값 수정"
        c = "지역변수"
        print(a)
        print(b)
        print(c)
  #전역변수a의 값이 변경
  ````

  * 지역변수를 전역에서도 사용하고싶을 경우는 변수값을 return

<br>

##### 함수의 재귀호출

* for문과 재귀호출로 factorial 구하기

* 재귀호출시

  * 반드시 <u>리턴값이 존재</u>해야 한다
  * <u>함수 종료조건이 필수</u> (없을 경우 무한루프)

  ````python
  #factorial(ex. 5! = 5*4*3*2*1) 구하기
  
  #반복문 factorial fucntion
  def fact1(n):
        result=1
        for i in range(1, n+1, 1):
              result = result*i
              print('{}!={}입니다'.format(i, result)) 
        return result
  
  r1 = fact1(5)
  print(r1)
  
  #재귀호출 factorial function
        #5!=5*4!    4!=4*3!     3!=3*2!
  def fact2(n):
        if n == 0:
              return 1
        else:
              result=n*fact2(n-1)
               print('{}!={}입니다'.format(i, result)) 
              return result;
  
  r1 = fact2(5)
  print(r1)
  
  ````

  * 구현은 단순하지만, 함수를 복사해서 여러개가 쌓이는 개념이기 때문에 메모리에 있어서는 비효율적
  * 좋은 성능을 요구하는 코드에서는 재귀함수를 반복문으로 대체하는 것이 낫다. 

<br>

##### 함수의 변수취급

* Python에서는 함수 변수 취급이 가능

  * 함수를 매개변수, 지역함수, 리턴함수로 쓸 수 있다.

  ```python
  def f1():
        print('출력')
  
  import time
  def call_func(f):
        #f 함수 5초 후 자동 실행
        time.sleep(5)
        f()
  call_func(f1)
  ```

  

* map ( 내장함수 )

  * list내부에 데이터 조작을 하는데 반복문을 사용하지 않아도 되는 함수. 매핑시켜준다. 
  * 예를 들어 a라는 list의 float데이터를 int로 바꾸고싶을때 반복문 대신 map 사용

  ````python
  #map 함수(함수를ㄹ 매개변수) mapping
  a = [2.5, 4.7, 3.4, 3.6]
  print(a)
  a = list(map(int, a))#각 데이터를 int로 mapping
  print(a)
  
  ````

  * 대문자로 바꿔보기

  ````python
  b = ['java programming과정', 'oracle sql', 'spring framwork', 'python programming']
  #앞 네 문자만 대문자로 바꾸는 함수
  def my_upper(s):
        return s[:4].upper() + s[4:]
        
  print(my_upper('java programming과정'))
  b = list(map(my_upper, b)) #4번을 알아서 실행해줌
  print(b)
  ````

<br>

##### 구현부가 없는 함수

* 함수 선언하고 구현하지 않으면 에러, 만약 구현부 없는 함수를 사용하고싶을 시

  ​	**pass**

  ```python
  def no_actioon():
        pass
  ```

  * 구현부를 미뤄두는 방법 

<br>

##### 람다식

* 매개변수와 리턴문만 가지는 무명의 함수

* 정의와 동시에 호출된다. 

  **lambda [매개변수] : 리턴값([매개변수값])**

````python
def lam1():
      return 0
print(lam1())

#아래는 위 함수를 람다식으로 바꾼 것 
print((lambda : 0)())
````

```python
print((lambda x: x*x)(10))
#x는 매개변수, ()에 매개변수 값을 넣어준다
print((lambda x, y: (x*y, x-y, x/y, x+y))(10, 2 ))
#return이 여러개일 경우, ( )Tuple로 return 
```

