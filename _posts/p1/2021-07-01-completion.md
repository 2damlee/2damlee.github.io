---

---

<br>

<br>

- 문제 설명

    ### **문제 설명**

    수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

    마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

    ### 제한사항

    - 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
    - completion의 길이는 participant의 길이보다 1 작습니다.
    - 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
    - 참가자 중에는 동명이인이 있을 수 있습니다.

    ### 입출력 예 설명

    예제 #1"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

    예제 #2"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

    예제 #3"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

    <br>

    <br>

* 문제 풀이
  * 참가자 배열과 완주한 배열 비교, 같지 않을 때 break 해당 index return
      - [0]부터 비교하기 때문에 두 배열 모두 내림차순 정렬을 해준 후 비교한다.
      - for문의 int i 를 밖에 변수로 선언
          - 불일치했을 때의 i번째 인덱스를 return한다.

```java
import java.util.Arrays;

class Solution {
    public String solution(String[] participant, String[] completion) {
			 // 내림차순 정렬
       Arrays.sort(participant);
       Arrays.sort(completion);
			 // 같지 않았을 경우의 i값을 return하기 위한 변수
       int i;

       for(i = 0; i < completion.length; i++){
           if(!completion[i].equals(participant[i])) break; // 다른 경우 실행 중단
       }
       return participant[i]; 
    }
}
```
