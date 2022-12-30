## 문제
[2022 KAKAO TECH INTERNSHIP 코딩 테스트 공부](https://school.programmers.co.kr/learn/courses/30/lessons/118668)

## 풀이
```java
public class Solution {
    public int solution(int alp, int cop, int[][] problems) {
        int goalAlp = 0;
        int goalCop = 0;

        for(int[] problem : problems) {
            goalAlp = Math.max(goalAlp, problem[0]);
            goalCop = Math.max(goalCop, problem[1]);
        }

        if(alp >= goalAlp && cop >= goalCop) {
            return 0;
        }

        if(alp >= goalAlp) {
            alp = goalAlp;
        }

        if(cop >= goalCop) {
            cop = goalCop;
        }

        int[][] dp = new int[goalAlp + 2][goalCop + 2];
        for(int i = alp; i <= goalAlp; i++) {
            for(int j = cop; j <= goalCop; j++) {
                dp[i][j] = Integer.MAX_VALUE;
            }
        }
        dp[alp][cop] = 0;

        for(int i = alp; i <= goalAlp; i++) {
            for(int j = cop; j <= goalCop; j++) {
                // 알고력 높이기
                dp[i + 1][j] = Math.min(dp[i + 1][j], dp[i][j] + 1);

                // 코딩력 높이기
                dp[i][j + 1] = Math.min(dp[i][j + 1], dp[i][j] + 1);

                // 문제를 푸는 경우
                for(int[] problem : problems) {
                    if(i >= problem[0] && j >= problem[1]) {
                        if(i + problem[2] > goalAlp && j + problem[3] > goalCop) {
                            dp[goalAlp][goalCop] = Math.min(dp[goalAlp][goalCop], dp[i][j] + problem[4]);
                        }else if(i + problem[2] > goalAlp) {
                            dp[goalAlp][j + problem[3]] = Math.min(dp[goalAlp][j + problem[3]], dp[i][j] + problem[4]);
                        }else if(j + problem[3] > goalCop) {
                            dp[i + problem[2]][goalCop] = Math.min(dp[i + problem[2]][goalCop], dp[i][j] + problem[4]);
                        }else if(i + problem[2] <= goalAlp && j + problem[3] <= goalCop) {
                            dp[i + problem[2]][j + problem[3]] = Math.min(dp[i + problem[2]][j + problem[3]], dp[i][j] + problem[4]);
                        }
                    }
                }
            }
        }

        return dp[goalAlp][goalCop];
    }
}

```
