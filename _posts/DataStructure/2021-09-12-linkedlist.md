---
title: "List"
categories:
  - Datastructure
layout: archive
sidebar_main: true
author_profile: true
tags:
  - datastructure
---

<br>

<br>

### LIST

List란? (pointer를 기반)

- '일정한 순서'의 나열
- 어떤 정의에 의해서 결정된 '논리적인 순서'의 나열

**논리적 순서와 물리적 순서**

- 리스트의 '순서'는 데이터가 저장되는 물리적인 위치와 상관없이 사람들의 머리속에 인식되는 '논리적인 순서', 혹은 리스트에 나타나는 원소들간의 '의미적인 순서'를 의미
- pointer를 따라가면 순서대로 접근이 가능하다.
- 배열은 인덱스로 표현되는 '순서'가 배열 원소의 메모리공간(DDR)에서의 물리적인 위치를 의미
- 리스트의 '순서'개념은 <u>어떤 정의에 의해 결정된 '논리적인 순서'</u>
- 원소들의 물리적인 저장 순서나 위치와는 무관하게 <u>원소들간의 논리적인 순서만 유지</u>

**리스트의 구현 방법**

1. <u>포인터</u>를 이용한 리스트 구현 방법: 원소값을 저장하는 공간과 다음 원소를 가리키는 위치 정보를 저장하는 공간을 함께 구현하는 방법(연결 리스트)

2. <u>배열</u>을 이용한 리스트의 구현 방법

<br>

##### 배열을 이용한 리스트 구현

리스트를 배열로 구현하고 난 뒤에 중간에 다른 데이터를 삽입해야 한다면? 

- 배열의 확장: 초기 배열 선언에서 충분히 크게 하면 어느 정도는 배열의 추가 확장을 피할 수 있다. 그러나 원소를 리스트의 중간에 삽입하기 위해서는 리스트의 원소 값을 하나씩 뒤로 밀어야 한다. 

  - 배열이라는 자료구조는 직관적이지만, 고정적. 변동에 있어서 취약
  - 배열은 중간에 '빈 값'이 허용되지 않음

- '배열로 구현된 리스트'는 원소의 순서가 연속적인 물리적 주소에 저장

  - 원소를 삽입하거나 삭제하기 위해서는 해당 원소의 위치 뒤에 있는 모든 원소를 뒤로 물리거나 앞으로 당겨야 한다. 
  - 리스트 원소 값의 이동은 원소수가 많을수록 프로그램의 수행시간을 증가시킨다. 

  <br>

#### 포인터를 이용한 리스트의 구현

- 노드(node): 리스트위 원소(값) + 다음 원소를 가리키는 정보(메모리 주소값)

  노드는 데이터 요소(원소, 값)와 리스트의 다음 원소를 지시하는 포인터(주소, 링크(link))로 구성됨

  - 링크, 일종의 주소값

    <br>

##### 포인터 변수 및 리스트의 삭제/삽입

- 리스트의 생성

  정수값 data와 링크로 구성된 노드의 생성

  ````c
  struct linked_list_node {
    int data;
    struct linked_list_node*link;
  }
  ````

  

- 포인터의 구현

  - 포인터의 할당과 반환 예

  ```c
  int a, *p_a;
  float b, *p_b;
  p_a = (int*)malloc(sizeof(int));
  p_b = (float*)malloc(sizeof(float));
  *p_a = 10;
  *p_b = 3.14;
  printf("a is %d, b is %f\n", *p_a, *p_b);
  free(p_a);
  free(p_b);
  ```

  - malloc(memory allocation, 메모리 할당): 실행 중에 필요한 메모리를 할당받는 것

  - *은 포인터 

    `*p_a = 10` 은 포인터 자체의 값에 10이 저장되는 것이 아닌, p_a가 가르키는 (새롭게 메모리를 할당받은)주소값 공간. pointer가 선언되면 pointer의 주소값이 저장될 메모리와, 주소값이 가르킬 공간이 생성된다. <br>

    

    <img src = "/assets/images/posts/pointer.png">

    

##### 연결 리스트에서 노드의 삽입과 삭제

- 연결 리스트의 초기 모습 

  <img src = "/assets/images/posts/list.png">

- 리스트의 원소 삭제 연산 단계

  - 삭제할 노드의 선행 노드의 링크 필드를 삭제할 노드의 후행 노드를 가르키게 한다. 
  - 삭제할 노드를 메모리에 반환한다. 

  <img src = "/assets/images/posts/listdel.png">

  - 단계(순서)
    1. 메모리 공간을 할당받고 삽입할 내용을 저장하여 삽입할 x노드를 생성
    2. x노드의 링크부분이 후행 노드가 될 j노드를 가르키게 지시
    3. 삽입될 x노드의 선행 노드가 될 i 노드의 링크 필드가 x노드를 가르키게 함

  * 예시) 연결 리스트의 마지막에 삽입 연산

    ```c
    void addNode(linkedList_h* H, int x){
      //리스트 마지막 노드에 삽입 연산, x값은 100이라고 가정
      listNode*NewNode;
      listNode*LastNode;
      NewNode = (listNode*)malloc(sizeof(listNode));
      NewNode -> data = x;
      NewNode -> link = NULL;
    }
    ```

    ```c
    void addNode(linkedList_h* H, int x){
      if(H -> head == NULL) { //현재 리스트가 공백인 경우
        H -> head = NewNode;
        return;
      }
      LastNode = H -> head;
      while(LastNode -> link != NULL)
        LastNode = LastNode -> link;
      LastNode -> link = NewNode;
    }
    ```

    