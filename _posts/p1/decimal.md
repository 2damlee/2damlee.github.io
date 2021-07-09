---
title: "로또 확률 구하기"
categories:
  - p1
layout: archive
sidebar_main: true
author_profile: true
tag:
 - level1
---



<br>

<br>

- 문제 설명

    `로또 6/45`(이하 '로또'로 표기)는 1부터 45까지의 숫자 중 6개를 찍어서 맞히는 대표적인 복권입니다. 아래는 로또의 순위를 정하는 방식입니다. 




    - 순서와 상관없이, 구매한 로또에 당첨 번호와 일치하는 번호가 있으면 맞힌 걸로 인정됩니다.
    - 알아볼 수 없는 두 개의 번호를 각각 10, 6이라고 가정하면 3등에 당첨될 수 있습니다.
        - 3등을 만드는 다른 방법들도 존재합니다. 하지만, 2등 이상으로 만드는 것은 불가능합니다.
    - 알아볼 수 없는 두 개의 번호를 각각 11, 7이라고 가정하면 5등에 당첨될 수 있습니다.
        - 5등을 만드는 다른 방법들도 존재합니다. 하지만, 6등(낙첨)으로 만드는 것은 불가능합니다.
    
    민우가 구매한 로또 번호를 담은 배열 lottos, 당첨 번호를 담은 배열 win_nums가 매개변수로 주어집니다. 이때, 당첨 가능한 최고 순위와 최저 순위를 차례대로 배열에 담아서 return 하도록 solution 함수를 완성해주세요.
    
    ### 제한사항
    
    - lottos는 길이 6인 정수 배열입니다.
    - lottos의 모든 원소는 0 이상 45 이하인 정수입니다.
        - 0은 알아볼 수 없는 숫자를 의미합니다.
        - 0을 제외한 다른 숫자들은 lottos에 2개 이상 담겨있지 않습니다.
        - lottos의 원소들은 정렬되어 있지 않을 수도 있습니다.
    - win_nums은 길이 6인 정수 배열입니다.
    - win_nums의 모든 원소는 1 이상 45 이하인 정수입니다.
        - win_nums에는 같은 숫자가 2개 이상 담겨있지 않습니다.
        - win_nums의 원소들은 정렬되어 있지 않을 수도 있습니다.
    
    ---

<br>

<br>

문제풀이

- 최소 경우와 최대 경우를 계산

    : 최대의 경우 '0'을 맞는 경우로 ++ , 최소의 경우는 번호가 일치할 경우만 ++ 

    → 최대 경우 = 최소 경우 + 최대 경우(0을 +한 값)

- 등수 출력 메소드 생성

    : 맞은 개수에 따른 등급 출력, 1 이하(0, 1)는 6 출력

```java
class Solution {
    public int[] solution(int[] lottos, int[] win_nums) {

        int[] answer = new int[2];
        int min_case = 0;
        int max_case = 0;

        for(int i = 0; i < win_nums.length; i++) {
            for(int k = 0; k < lottos.length; k++) { // win_nums와 lottos 비교
                if(win_nums[i] == lottos[k]) { // 0은 모두 당첨번호에서 제외 
                    min_case += 1;
                }
            }

            if(lottos[i] == 0) { // 0을 당첨번호로 추가 
                max_case += 1;
            }
        }

        if(max_case == 0){ // win_nums에 0이 없을 경우 
            max_case = min_case;
        } else {
            max_case = min_case + max_case;
        }

        answer[0] = grade(max_case);
        answer[1] = grade(min_case);

        return answer;
    }

		// 등급 매기기 
    public int grade(int cnt){
        switch(cnt){
                case 6: return 1;
                case 5: return 2;
                case 4: return 3;
                case 3: return 4;
                case 2: return 5;
                default: return 6;
        }
    }
}
```
