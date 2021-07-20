---
title: "Python basic(3) - Module/Exception"
categories:
  - Python
layout: archive
sidebar_main: true
author_profile: true
tags:
  - python
---

<br>

<br>

# Modul

> a, b 부품이 결합하면 완성품 x가 만들어지고, a, c가 결합하면 완성품 y가 만들어진다. 
>
> a, b, c라는 부품이 있으면 원하는 완성품을 만들 수 있다. 저런 부품단위가 모듈이다. 즉, 필요한 부품을 가져다 쓸 수 있는 개념. 
>
> 물리적 형태는 .py 즉 파이썬 파일 하나로 이루어져 있다. 

* pypi.org : 라이브러리 다운로드 가능 /파이썬 라이브러리 관리, 다운로드 가능 사이트.

  미리 만들어진 .py 파일이 있다. 이 파일을 import해서 사용하는 것 



##### <br>import 방법

> from이 붙으면 사용시 modul이름을 쓰지 않음

random/math/time/sys/

* import 방법

  1. import 모듈이름 import 뒤에 모듈 이름을 주면, 해당 모듈 내에 있는 것을 전부 사용 가능

  ```python
  import math
  math.trunc(3.14)
  math.log(10, 2)
  math.sin(50)
  ```

  

  2. **from import** 해당 모듈 안에 특정 함수를 쓸 것
     * 사용시 modul이름이 붙지 않는다. (붙이면 오류)

  ````python
  from math import trunc, log
  trunc(3.14)
  log(10, 2)
  ````

  

  3. 모든 함수 가져오기
     * 사용시 modul이름이 붙지 않는다. (붙이면 오류)

  ```python
  from math import *
  trunc(3.14)
  log(10, 2)
  sin()
  ```

  

  4. alias  별칭 주는 방법

  ````python
  import math as ma
  ma.trunc(3.14)
  ma.log(10, 2)
  ma.sin()
  ````

  

<br><br>

##### 기본 모듈

> 파이썬 기본 모듈은 상당히 많음. 아래는 그 중 몇 개만 예시로 가져온 것

* random

  ````python
  import random
  print(random.randint(1, 100))
  print(random.randrange(1, 101))
  name_list = ['abcd', '가나다라', 'ab가나', 'xyz']
  print(random.choice(name_list))      #random으로 하나 출력
  random.shuffle(name_list)     #리스트순서를 랜덤으로 섞는다 
  print(name_list)
  print(random.sample(name_list, 2)) #random으로 두 개 출력
  ````

  > 일반적으로 random은 전체데이터가 많으면 몇 개만 가져와 표본을 골라내고 분석해보고 나머지 데이터들에게 적용해봐야 할 때, 즉 집단에서 표본 샘플을 뽑아내는 작업에서 사용된다. 

<br>

* sys / os

  * sys
    * sys: 파이썬 system을 의미 

  ```python
  import sys
  #print(sys.argv[1])
  print(sys.version)#버전정보 
  print(sys.path)#내장모듈+사용자모듈+외부설치추가모듈 저장 가능 경로 
  ```

  * os 
    * 파일에 관련된 작업 

  ````python
  import os
  print(os.name)
  print(os.getcwd())#current working directory
  print(os.listdir())#directory list (파일목록)
  #os.rename('파일명', '')
  #os.remove("파일명")
  ````

  

<br>

* datetime

  시간을 알아오는 모듈

  * 현재시간 알아오기

  ````python
  import datetime
  now = datetime.datetime.now() #현재시간
  print(now.year)
  print(now.month)
  print(now.day)
  print(now.hour)
  print(now.minute)
  print(now.second)
  ````

  * 시간 바꾸기 

  ````python
  after_1year = now.replace(year=(now.year+1))
  print("one year after=", after_1year.year)
  ````

  <br>

* time

  ```python
  import time
  sec = time.time() #초단위 시간 표현
  now = time.localtime(sec) #년월일시분초 형태로 변환 
  print(now.tm_year)
  ```

  시간을 만들어서 초단위로 줬을 때, 현재 지역의 시간으로 변경 

<br><br>



##### 모듈 설치해서 사용하기 

> 파이썬IDLE설치시 pip명령어가 함께 설치되었고, 따라서 bash 창에서 pip python install p.. 명령으로 설치가 가능하다. 



* 웹 크롤링(웹서버 데이터 전송 방법 웹서버의 데이터를 가져오는 방법)을 사용하기 위해 beautifulsoup 라이브러리 패키지(*.py들 = 패키지)를 다운받는다. 

* 설치

  * pip.exe명령어가 파이썬 설치시 같이 설치됨

  * **pip3 install beautifulsoup패키지명**

  * pypi.org

    1. 다운로드 할 것 검색 

    2. beautifulsoup4 4.9.3 명령어 복사
    3. 도스창에 명령어 입력 (pip3 install beautifulsoup4)
    4. 확인(pip3 list)
    5. pip3 show beautifulsoup4-location
    6. sys.path 포함여부확인

    

    ![스크린샷 2021-04-29 오후 2.19.49](%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.19.49.png)

<img src="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.20.11.png" alt="스크린샷 2021-04-29 오후 2.20.11" style="zoom:50%;" />



<br>

##### 서버에 요청, 응답하기

>  beautifulsoup이용해서 서버에 접속하여 접속된 태그로 beautifulsoup에서 분석

* 요청 url을 가져오기 위한 module : urllib.request
* 웹서버 정보를 수집하는 방식이 beautifulsoup
  * 다른 웹의 정보를 python에서 커스터마이징이 가능 

```python
#서버에 연결하고, 종료
      #소스보기와 같은 결과
import urllib.request as req #요청 url가져오기
from bs4 import BeautifulSoup as bs
response = req.urlopen("http://localhost:9091/")
soup = bs(response, 'html.parser')#html tag parsing
#rescontents=response.read() #내용 갖고오기, 헥사데시말코드로 보여짐 
#print(rescontents)

#server 전체 내용을 출력
rescontents = soup.prettify()
print(rescontents)
#server 내용에서 특정 tag 찾기
print(soup.find('h3'))#type-dict string:xxx, style:xxx return #첫번째 태그만 찾아옴
print(soup.find('h3').string)
print(soup.find('img')['src'])#src key를 가져오는 것

for h3 in soup.findAll('h3'):#h3태그 전부 가져옴
      print(h3.string)
response.close()
```



<br><br>



# 예외처리

> * Java, c, c++, c# : 컴파일언어
>   * 자바소스파일 > 마지막 문법 검사=바이너리변경(컴파일) - 실행 
>
> * Js, Python: 인터프리터언어
>   * 한 문장마다 문법 검사, 실행



파이썬에서는 errror가 곧 exception 

<br>

* 예외처리방법

  ````python
  #Exception
  try:
        print(a)
  except NameError:
        print('a 변수 선언된 적 없음')
  ````

* 의도적 오류 발생

  **raise**

  ````python
  try:
        money = input('대출금액 상환개월수 입력') #'10000 12' .split(' ')
        two_items = money.split()
        loan = int(two_items[0])
        payback = int(two_items[1])
        if payback <= 0:#의도적 예외처리
              raise ValueError('상환개월은 음수값을 입력X')
        monthly_return = loan/payback
  except IndexError:
        print('대출금액이나 상환개월수 입력 확인')
  except ValueError as ve: #ValueError Type 변수 ve
         print(ve) #python의 오류메시지 출력
  else: #오류없이 실행 되었을 경우 
        print(monthly_return,  '을 매달 상환하셔야 합니다')
  finally: #반드시 실행
        print('영업종료')
  ````

  

