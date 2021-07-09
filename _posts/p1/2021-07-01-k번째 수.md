---
title: "k번째 수"
categories:
  - p1
layout: archive
sidebar_main: true
author_profile: true
---



<br>

<br>

- 문제 설명

  ### **문제 설명**

  배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

  예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

  1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
  2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
  3. 2에서 나온 배열의 3번째 숫자는 5입니다.

  배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

  ### 제한사항

  - array의 길이는 1 이상 100 이하입니다.
  - array의 각 원소는 1 이상 100 이하입니다.
  - commands의 길이는 1 이상 50 이하입니다.
  - commands의 각 원소는 길이가 3입니다.

  ### 입출력 예 설명

  [1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.[1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.[1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

  <br>

  <br>

문제 풀이

- 이차원 배열의 주어진 세 숫자의 index 0, 1은 주어진 일차원 배열을 자르는 범위이며 2는 자른 이후의 길이에서 추출할 index이다.
- 배열을 복사하는 copyOfRange 사용

```java
import java.util.Arrays;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];
        // 1. commands.length만큼 반복
        for(int i=0; i<commands.length; i++){
						// 2. array의 commands 범위(index 0, 1)만큼 복사
            int[] temp = Arrays.copyOfRange
                (array, commands[i][0]-1, commands[i][1]);
						// 3. 복사한 배열을 내림차순으로 정렬
            Arrays.sort(temp);
						// 4. 배열의 commands 3번째 index를 저장
            answer[i]=temp[commands[i][2]-1];
        }
   return answer;
    } 
}
```

### copyOfRange

: 배열을 복사하는 Arrays 매소드

```java
Arrays.copyOf(원본배열, 복사할 길이);
Arrays.copyOfRange(원본 배열, 복사할 시작인덱스, 복사할 끝인덱스);
```