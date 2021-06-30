---
title: "Recursion"
categories:
  - Algorithm
layout: archive
sidebar_main: true
author_profile: true
---



<br><br>

#### Recursion 순환

: 재귀, 자기 자신을 호출하는 함수

<br><br>

재귀는 항상 무한루프에 빠지는 것은 아니고, 적절한 조건을 줘서 기능을 작동할 수 있도록 만들 수 있다.

```java
public static void func(int k) {
	if (n <= 0)
		return;
	else {
		System.out.println("hello");
		func(n-1);
	}
}
```

<br>

**Recursion의 요구사항**

- Base case: 적어도 하나의 recursion에 빠지지 않는 경우가 존재해야 한다.
- Recursive case: recursion을 반복하다보면 언젠가는 반드시 base case에 수렴해야 한다.



<br><br>

**1~n 까지의 합**

```java
public static int func(int n) {
  if (n==0)
    return 0;
  else
    return n + func(n-1);
}
```

임의의 양 정수 k에 대하여 n<k인 경우 0에서 n까지의 합을 올바르게 계산하여 반환한다고 가정한다면, 0에서 k-1까지의 합이 올바로 계산되어 반환된다. 메소드 func는 그 값에 n을 더해서 반환한다. 

<br>

**Factorial: n!** 

````java
public static int factorial(int n) {
  if (n==0)
    return 0;
  else
    return n*factorial(n-1);
}
````

<br>

**Fibonacci Number**

````
public static int factorial(int n) {
  if (n<2)
    return n;
  else
    return fibonacci(n-1) + fibonacci(n-2);
}
````

<br>

**Euclid Method**

````java
public static int gcd(int m, int n) {
  if (m<n) {
  	int tmp=m; m=n; n=tmp;
  }
  if (m%n == 0)
  	return n;
  else
    return gcd(n, m%n);
}
````

m>=n인 두 양의 정수 m과 n에 대해서 m이 n의 배수이면 gcd(m, n)=n이고, 그렇지 않으면 gcd(m, n) = gcd(n, m%n)이다. 

<br>

👇위 식은 아래로도 변환이 가능하다. 

````java
public static int gcd(int p, int q) {
  if (q==0)
    return p;
  else
    return gcd(q, p%q);
}
````

<br>

✔️ 모든 순환함수는 반복문(iteration)으로 변경이 가능, 그 역도 역시 가능하다.

✔️ 순환함수는 경우에 따라 복잡한 알고리즘을 단순하고 알기 쉽게 표현하는 것을 가능하게 한다. 

✔️ 그러나 함수 호출에 따른 오버해드가 있고, 실행에 속도가 느려질 수 있다. 

<br><br>

👇아래는 순환을 사용한 여러 예제들 

<br>

**문자열 길이 계산**

```java
public static int length(String str) {
  if (str.equals(""))
    return 0;
  else
    return 1 + length(str.substring(1)); // 1 + 첫 글자를 제외한 나머지 문자열(반복) 
}
```

<br>

**문자열을 뒤집어 print**

````java
public static void printCharsReverse(String str) {
  if (str.length()==0)
    return;
  else {
    printCharsReverse(str.substring(1));
    System.out.print(str.charAt(0));
  }
}
````

<br>

**2진수로 변환하여 출력하기**

````java
public void printInBinary(int n) {
	if (n<2)
    System.out.print(n);
  else {
    printInBinary(n/2);
    System.out.print(n%2);
  }
}
````

<br>

**배열의 합 구하기**(0~n까지)

````java
public static int sum(int n, int[] data) {
  if (n <= 0)
    return 0;
  else
    return sum(n-1, data) + data[n-1];
}
````

data[0]에서 data[n-1]까지의 합을 구하여 반환

<br>

**데이터파일로부터 n개의 정수 읽어오기**

````java
public void readFrom(int n, int[] data, Scanner in) {
	if (n==0)
    return;
  else {
    readFrom(n-1, data, in);
    data[n-1] = in.nextInt();
  }
}
````



<br><br>

### Designing Recursion

순환적 알고리즘 설계 



- 암시적(implicit) 매개변수를 명시적(explicit) 매개변수로 바꾸는 것

<br>

##### 순차탐색

: 입력으로 어떤 배열이 있고 이 중에 원하는 특정한 값이 있다면 어디에 있는지를 찾을 때. (데이터가 정렬되어있다면 이진검색을 할 수 있음)

````java
int search(int[] data, int n, int target) {
  for (int i=0; i<n; i++)
    if (data[i]==target)
      return i;
  return -1; // 값이 존재하지 않을 경우
}
````

 이 경우 [0, n-1] n-1은 명시적이나 0은 암시적 매개변수. (인덱스는 보통 0부터 시작하니 암시적으로 사용)

<br>

👇아래는 매개변수의 명시화

````java
int search(int[] data, int begin, int end, int target) {
  if (begint>end)
    return -1;
  else if (target == data[begin])
    return begin;
  else
    return search(data, begin+1, end, target);
}
````



<br>다른 방식

````java
int search(int[] data, int begin, int end, int target) {
  if (begin>end) 
    return -1;
  else {
    int middle = (begin+end)/2;
    if (data[middle] == target)
      return middle;
    int index = search(data, bein, middle-1, target);
    if (index != -1)
      return index;
    else
      return search(data, middle+1, end, target);
  }
}
````

<br>

##### 최대값 찾기

````java
int findMax(int[] data, int begin, int end) {
  if (begin == end)
    return data[begin];
  else
    return Math.max(data[begin], findMax(data, begin+1, end));
}
````

<br>

👇 다른 방식

````java
int findMax(int[] data, int begin, int end) {
  if (begin == end)
    return data[begin];
  else {
    int middle = (begin+end)/2;
    int max1 = findMax(data, begin, middle);
    int max2 = findMax(data, middle+1, end);
    return Math.max(max1, max2);
  }
}
````



<br><br>

이진검색 찾기

: 데이터들이 크기순으로 정렬되어있어 배열에 저장되어있을 때 찾을 수 있는 방법 > 가운데 값을 기준으로, 내가 찾는 값이 크다면 뒷쪽 작다면 앞쪽을 탐색하는 방법

````java
public static int binarySearch(String[] items, String target, int begin, int end) {
  if (begin>end)
    return -1;
  else {
    int middle = (begin+end)/2;
    int compResult = target.compareTo(items[middle]);
    if (comResult == 0)
      return middle;
    else if (compResult<0)
      return binarySearch(items, target, begin, middle-1);
    else
      return binarySearch(items, target, middle+1, end);
  }
}
````

