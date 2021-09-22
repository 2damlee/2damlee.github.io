---
title: "linked list(2)"
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

### 연결 리스트의 응용

<br>

##### 연결 리스트의 종류

- 단순 연결 리스트: 하나의 링크만 있고, 각각의 노드의 링크는 <u>후행 노드만을 가리키는</u> 구조

  특정 노드의 후행 노드는 쉽게 접근 가능하지만 특정 노드의 선행 노드에 대한 접근은 헤드 노드부터 재검색해야하는 문제점이 발생

  -> 포인터를 두 개로 해서 해결? 

  ​	:프로그래밍 코드가 비효율적이게 될 수 있음. 

- **이중 연결 리스트**

<img src = "/assets/images/posts/linkedlist(1).png">

코드의 변경을 최소화 -> Llink만 추가해주면, 선행 노드도 가르킬 수 있음. 

- 이중 연결 리스트: 특정 노드는 선행 노드를 가르키는 링크와 후행 노드를 가르키는 링크를 가진다. 

  -> 특정 노드에서 선행 노드와 후행 노드에 간단한 프로그램 코드를 통해 접근 가능

- 원형 이중 연결 리스트 
  - 데이터를 쉽게 추가하고 삭제할 수 있다는 장점
  - 연결 리스트는 마지막 노드의 링크 필드는 언제나 null값
    - 리스트의 마지막 원소 뒤에는 아무 원소도 없기 때문에 연결 리스트의 마지막 노드의 링크필드는 언제나 Null
    - 그래서 마지막 노드의 링크 필드를 활용하면서도 프로그래밍 성능에 도움이 되도록 하기 위해 원형 연결 리스트가 제안
  
  <img src = "/assets/images/posts/linkedlist(2).png">

*참조: null값이란 영역이 할당되어있으나 그 안에 데이터가 없는 상태를 의미

```c
typedef struct ListNode{ // 원형 연결 리스트의 노드 구조 정의
  int data;
  struct ListNode*link;
} listNode;
typedef struct { // 원형 연결 리스트의 헤드 노드 구조 정의
  listNode*head;
} linkedList_h;
linkedList_h* createLinkedList_h(void) { // 원형 연결 리스트의 헤드 노드 생성
  linkedList_h*H;
  H = (linkedList_h*)malloc(sizeof(linkedList_h));
  H -> head = NULL;
  return H;
}
```

우선적으로 헤더값은 0

삽입연산👇

```c
void addFirstNode(linkedList_h*H, int x) {
  // 원형 리스트 첫 번째 노드 삽입 연산, x값은 100으로 가정
  listNode*tempNode;
  listNode*NewNode;
  NewNode = (listNode*)malloc(sizeof(listNode));
  NewNode -> data = x;
  NewNode -> link = NULL;
}
```

원형 연결 리스트의 노드 삽입👇

```c
void addFirstNode(linkedList_h*H, int x) {
  if(H -> head == NULL) { //현재 리스트가 공백인 경우
  	H -> head = NewNode;
    NewNode -> link = NewNode;
    return;
  }
  tempNode = H -> head; // 호출 값으로 포인터를 주는 것
  while(tempNode -> link != H -> head) // null이 아닌지 check (head노드가 마지막 노드를 찾는 과정 )
    tempNode = tempNode -> link;
  NewNode -> link = tempNode -> link;
  tempNode -> link = NewNode;
  H -> head = NewNode;
}
```



