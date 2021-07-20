---
title: "Python basic(4) - Data processing"
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

# 데이터 전처리/시각화

> * 웹크롤링은 웹의 정보를 가져오는 것
> * 데이터를 가져와서 전처리 - 분석 - 결과 테이블 그래프를 그리는 방법 

1. 라이브러리 다운로드 
   * ​    #pip3 install matplotlib

<br>

* 데이터 그리기 

  * 데이터 표현에는 여러 방법이 있고, 필요할 때 검색해서 사용하면 된다. 
  * 기본적으로는 import후 그래프 시작은(한 개의 그래프를 그릴 경우) **plot**, 그래프 끝날 때는 **show**
    * 저장, 표현, 화면 구성 등은 plot과 show 사이에 구문을 작성한다. 

  ````python
  #graph
        #pip3 install matplotlib
  import matplotlib.pyplot as plt
  a=[1, 2, 3, 4, 5]
  b=[2, 4, 6, 8, 10]
  import random
  c=[]
  for i in range(1, 11, 1):
        c.append(random.randint(1, 10))
  #한 번에 한 개 그래프      
  plt.plot(a, b, 'o')#그래프 그리기 'o' > 점모양으로 그래프
  plt.title('graph')
  plt.xlabel('a')
  plt.ylabel('b')
  plt.savefig('test.png') # 그래프 저장하기
  plt.show()#그래프 보이기 
  
  print(c)
  plt.hist(c)#빈도수 체크 그래프 (막대그래프)
  plt.show()
  
  #모든 그래프 합쳐서 보기 
  
  plt.subplots()
  plt.plot(a, b)
  plt.plot(a, b, 'o')
  plt.hist(c)
  plt.show()
  
  #한 화면에 그래프 각각을 보기
  plt.subplot(2, 2, 1) #2*2영역에 1번
  plt.plot(a, b)
  
  plt.subplot(2, 2, 2)
  plt.plot(a, b, 'o')#2*2영역에 2번
  
  plt.subplot(2, 2, 3)
  plt.hist(c)#2*2영역에 2번
  plt.show()
  
  ````

  * hist: 중복값을 세어주는 역할 x값이 따로 필요 X 
  * subplot
    * () 변수 없이 선언 후 아래 plot으로 동일 x값에 여러 y값을 줄 경우, 한 x에 대한 y값이 나옴
    * plt.subplot(2, 2, 1)일 경우 2*2영역에 1번. 총 4개의 그래프가 화면에 나올 수 있다. 
  * 그래프 표현의 경우 다양한 표현이 가능하다. 그때그때 필요한 모양은 검색해서 추가할 것

<br>

### Spring과 연결하기

> 웹크롤링한 결과를 그래프화해서 파일에 저장하고 Spring파일에 업로드를 구현한다.

* 라이브러리 다운로드
  * #pip install requests



* GET방식

  ```python
  response = requests.get('http://127.0.0.1:9090/?id = asldkf ').text()
  ```

  

* POST방식

  * 아래는 서로 통신되는 과정 

  ````python
  import requests
  #웹크롤링 기상청정보 + 그래프 + 파일저장 + spring파일 업로드 구현
  #get
        #response = requests.get('http://127.0.0.1:9090/?id = asldkf ').text()
  #post
        #test.png 파일 Spring으로 업로드
  
  graphfile = open('test.png', 'rb')
  response = requests.post('http://127.0.0.1:9091/fileupload',
                           data={'name':'python', 'description':'file upload' },
                           files={'file1':graphfile , 'file2':graphfile })
  print(response.status_code)#error: 404/400/500 정상실행:200
  print(response.text) # @ResponseBody 문자열 응답
  ````



<br><br>


### 외부 데이터 가져와서 처리하기 

> 기상청 자료를 가져오는 것을 예시로 한다. 

* 웹크롤링

  * urllib.request로 먼저 url을 접속
  * BeautifulSoup으로 html코드 읽어오기

  ````python
  #url접속해서 모든 태그를 읽어서 출력
  import urllib.request as req
  from bs4 import BeautifulSoup as bs
  response = req.urlopen('http://www.kma.go.kr/weather/forecast/mid-term-rss3.jsp?stnId=108')
  soup = bs(response, 'html.parser')
  print(soup)
  ````

  * 소스를 select하기 전에 해당 소스가 어떤 식으로 그룹화되어있는지 파악하고 이용

    * 예를 들어 기상청이라면, 각 지역 태그 밑에 어떤 정보가 어떤 태그로 묶여있는지를 확인

    

* select하기

  * 필요한 소스는 list생성, 저장

  ````python
  city_list = []
  tmx_list = []
  tmn_list = []
  for loc in soup.select('location'): #str Type
        city_list.append( loc.select_one('city').string)
        tmx_list.append(loc.select_one('tmx').string)
        tmn_list.append(loc.select_one('tmn').string)
  tmx_list = list(map(int, tmx_list))#(숫자가 나열되지 않음) int로 변환 필요
  tmn_list = list(map(int, tmn_list))
  ````
  
  * select
  
    * select_one() :하나의 값을 조회
    * select(): 모든 값을 조회
  
  * find
  
    * find(): 하나의 값을 조회
  
    * findAll(): 모든 값을 조회
  
      

* 그래프 셋팅 

  * 한글어 폰트 지정을 해줘야 글자 처리 가능

  ````python
  #font setting ( 한글 처리 )
  
        #computer에서 사용 가능한 글꼴 정보 추출(조회하기)
        #'a':aaa.ttf 글꼴이름:ㅇㅇㅇ.ttf 이름 추출
  import matplotlib.font_manager as fm
  fname_list=[]
  for f in fm.fontManager.ttflist:
        fname_list.append(f.name)
  fname_list.sort() #정렬된 상태, return값 없음
  for name in fname_list:
        print(name)
  
  import matplotlib.pyplot as plt
  #graph setting
  plt.rcParams['font.family'] = 'AppleMyungjo'
  plt.rcParams['font.size'] = 8
  plt.rcParams['figure.figsize'] = (14, 4) #figure size  > width, heigth 
  plt.rcParams['xtick.labelsize'] = 8 #x축 데이터 글씨크기
  plt.rcParams['axes.labelsize'] = 10 #x축 레이블 글씨 크기
  plt.rcParams['lines.linewidth'] = 3
  plt.rcParams['lines.linestyle'] = '-.' #그래프 선모양
  plt.rcParams['axes.grid'] = True #그래프 그리드 표시
  ````



* 그래프 출력

  아래는 한 개의 그래프에 두 값을 표현

  ````python
  plt.subplots()
  plt.plot(city_list, tmx_list)
  plt.plot(city_list, tmn_list)
  plt.savefig('weather.png')
  plt.show()
  ````

  <br>

**파일 가져와서 데이터 가공 후 처리하기**

> 파일을 가져와서 수치로 각각 분류한 뒤, 데이터 생성 후 그래프 생성

````python
#usedcars.csv 
      #ex. 중고 상태 x축, 각 숫자 y축 
file = open('usedcars.csv', 'r')
car_state = []
car_state_values=['폐차직전', '양호 중고', '심각 중고']
car_state_cnt=[]
for line in file:
      line_list = line.split(',')
      mile = line_list[3]
      if mile.isdigit() and int(mile)>=20000:
            line_list.append(car_state_values[0])
      elif mile.isdigit() and int(mile)>=10000 and int(mile) < 20000:
            line_list.append(car_state_values[1])
      elif mile.isdigit() and int(mile) < 10000:
            line_list.append(car_state_values[2])
      else:
            line_list.append('차량상태')
      print(line_list)
      car_state.append(line_list[6])
file.close()

for car_val  in car_state_values:
      car_state_cnt.append(car_state.count(car_val))
import matplotlib.pyplot as plt

plt.rcParams['font.family'] = 'AppleMyungjo'
plt.title('중고차상태')
#plt.plot(car_state_values, car_state_cnt)
plt.hist(car_state[1: ]) #x축 (150) y 축 자동으로 빈도수 (차량상태를 세어줌 )
plt.show()
````



<br><br>

### Jupiter Notebook 사용

> 다운로드 방법은 anaconda 다운받거나, pip 명령어로 다운받거나 한다. 



아나콘다로 다운받기

> 아나콘다는 스프링의 maven같은 역할. 다양한 툴과 다양한 기본 라이브러리를 제공한다. 

* anaconda 접속, 64비트 다운로드 

  anaconda3-2020.11-macosx-x86_64.pkg

  * 다운로드폴더 가서 설치 시작

    default사항 그대로 두고 설치 

    경로는 내가 사용하고 있는 폴더로 변경(multi~)

    * 아나콘다는 파이썬이 같이 설치된다. 이미 설치했다면, 두 버전의 파이썬이 존재하게 된다.

    

* 시작 방법 

  * 아나콘다가 시작하고 jupiternotebook이 실행되어야 함

    bash창에서 쥬피터노트북 실행 

  * 주피터노트북 시작폴더 바꾸기

    * python_source로 변경 : /Users/lulala/JAVA_multi/python_source

    * jupyter notebook --generate-config : 파일 지정하기  

    * config에서 .py로 열고, 아래와 같이 _dir 코드에 시작폴더 지정

      c.NotebookApp.notebook_dir = '/Users/lulala/JAVA_multi/python_source' 

    * 쥬피터노트북 재시작

    * google로 사용 웹 변경할 시 

      c.NotebookApp.browser = 'C:/Program Files/Google/Chrome/Application/chrome.exe %s' // 경로는 내 경로에서 바꿀 것 

![스크린샷 2021-04-30 오후 3.59.08](%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.59.08.png)





* 쥬피터노트북의 확장자는 .ipynb

  * py확장자는 읽기만 가능, 실행은 불가능
  * 주피터노트북에서 해당파일을 실행하기 위해서는 주피터노트북용 파일을 통해 실행 

  * terminal 에서 python 명령어 가능, *.py하면 실행 가능, jupyter도 마찬가지 

* 기본 문법

  * ! 뒤에 따라오는 구문들은 도스창에서 실행되는 것

<br><br>


# Pandas Package

> pandas는 데이터 분석에 제일 많이 쓰이는 패키지이다. 



* Pandas Package

  pandas 패키지는 Series클래스와 DataFrame 클래스를 제공한다. 

  간단하게 정리해보자면, 

  Series의 경우 list와 dictionary를 합쳐놓은 일차배열(한 개의 열이 연속으로..)의 특징을 갖고, DataFrame은 Series의 이차원배열 형태를 갖는다. DataFrame은 이차원배열답게, 행과 열의 구조를 갖고 있는 것이 특징이다. 

  (DataFrame을 행만 추출해서 조회할 경우, 리턴된 타입은 Series타입이다.)

  

  * Series
    * index를  str타입으로 줄 수 있다.
      * 따로 index를 주지 않을 경우, 기본 숫자형 인덱스가 붙는다. 
    * list와 dictionary의 특징을 둘 다 갖는다. 

    딕셔너리 형태로 선언해도 되고, value값 따로 
  * dataframe
    * 2차원 배열의 형태를 가져온다. 

      * 행과 열의 구조를 갖는다. 
      * 행 index와 열을 정의하지 않은 경우 기본은 숫자로 지정 

      * [[ , , ], [ , , ], [ , , ]] 로 변수에 선언하면 기본은 행으로 추가, 
      * "콜롬명" : [, , , ]로 변수에 선언하면 열로 추가

    * 행 열을 바꿀 수도 있음 

      .T함수를 사용한다.  

  

  * 열 데이터 갱신, 추가, 삭제 
    * 특정 열을 가져와서 특정 열만 연산이 가능
    * 갱신: 데이터변수이름['열이름'] = 바꿀 값
    * 삭제: del 데이터변수이름['열이름']

    행데이터 변환의 경우, [: ] 슬라이싱 인덱싱이 필수. 안 할 경우 열로 취급

    혹은 loc함수 사용 

