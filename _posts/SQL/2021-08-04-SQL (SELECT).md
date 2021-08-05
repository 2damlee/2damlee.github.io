---
title: "SQL BASIC Lv.1 (SELECT)"
categories:
  - Sql
layout: archive
sidebar_main: true
author_profile: true
tag:
  - Sql
---

<br>

<br>

example Table

`ANIMAL_INS` 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. `ANIMAL_INS` 테이블 구조는 다음과 같으며, `ANIMAL_ID`, `ANIMAL_TYPE`, `DATETIME`, `INTAKE_CONDITION`, `NAME`, `SEX_UPON_INTAKE`는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

| NAME             | TYPE       | NULLABLE |
| ---------------- | ---------- | -------- |
| ANIMAL_ID        | VARCHAR(N) | FALSE    |
| ANIMAL_TYPE      | VARCHAR(N) | FALSE    |
| DATETIME         | DATETIME   | FALSE    |
| INTAKE_CONDITION | VARCHAR(N) | FALSE    |
| NAME             | VARCHAR(N) | TRUE     |
| SEX_UPON_INTAKE  | VARCHAR(N) | FALSE    |

 <br>

#### MIN && MAX

* 최댓값 구하기 (MAX)

  ```sql
  SELECT MAX(DATETIME) FROM ANIMAL_INS;
  ```

  ANIMAL_INS 테이블의 DATETIME(날짜) 중 가장 늦은 날짜(큰 수) 가져오기

* 최소값 구하기(MIN)

  ```sql
  SELECT MIN(DATETIME) FROM ANIMAL_INS;
  ```

  

<br>

#### NULL값 조회하기

* `WHERE column_name IS NULL`  : NULL인 값 조회
* `WHERE colum_name IS NOT NULL` : NULL이 아닌 값 조회

<br>

#### 정렬하기

* 오름차순 정렬

  ```sql
  SELECT * FROM ANIMAL_INS 
  ORDER BY ANIMAL_ID ASC;
  ```

  `ORDER BY` 오름차순 할 column `ASC`;

* 내림차순 정렬

  ````sql
  SELECT * FROM ANIMAL_INS 
  ORDER BY ANIMAL_ID DESC;
  ````

  `ORDER BY` 오름차순 할 column `DESC`;

  ```Sql
  SELECT NAME, DATETIME 
  FROM ANIMAL_INS
  ORDER BY ANIMAL_ID DESC
  ```

  : animal_id역순으로 name, datetime 조회

  <br>

  ````SQL
  SELECT ANIMAL_ID, NAME, DATETIME 
  FROM ANIMAL_INS 
  ORDER BY NAME, DATETIME DESC
  ````

  `ORDER BY` 뒤에 오는 컬럼명에 따라 우선순위를 결정할 수 있다. 👆의 경우 NAME 기준 내림차순으로 정렬, 중복될 경우 DATETIME정렬을 따른다. 

<br>

#### 상위의 레코드 조회(subQuery)

- DATETIME이 가장 큰 NAME 구하기 

  ```sql
  SELECT NAME FROM (
      SELECT*FROM ANIMAL_INS 
      ORDER BY DATETIME
  )
  WHERE ROWNUM = 1;
  ```

  <br>

  - ROWNUM은 일종의 순번 
  - DATETIME을 내림차순으로 조회한 후, 그 값에서 순번이 1인 것을 조회 



