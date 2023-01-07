## 문제
[2023 KAKAO BLIND RECRUITMENT 이모티콘 할인행사](https://school.programmers.co.kr/learn/courses/30/lessons/150368)

## 풀이
```java
public class Solution {
    private int maxJoiner;
    private int maxRevenue;

    public int[] solution(int[][] users, int[] emoticons) {
        // 1. 플러스 가입자 최대 (우선)
        // 2. 판매액 최대

        maxJoiner = Integer.MIN_VALUE;
        maxRevenue = Integer.MIN_VALUE;
        searchDiscountCombination(0, new int[emoticons.length][2], users, emoticons);

        int[] answer = new int[2];
        answer[0] = maxJoiner;
        answer[1] = maxRevenue;

        return answer;
    }

    private void searchDiscountCombination(int idx, int[][] discounts, int[][] users, int[] emoticons) {
        if(idx == discounts.length) {
            updateMax(discounts, users);
            return;
        }

        for(int rate = 1; rate <= 4; rate++) {
            discounts[idx][0] = rate * 10;
            discounts[idx][1] = emoticons[idx] - (emoticons[idx] * discounts[idx][0] / 100);
            searchDiscountCombination(idx + 1, discounts, users, emoticons);
        }
    }

    private void updateMax(int[][] discounts, int[][] users) {
        int joiner = 0;
        int revenue = 0;
        for(int[] user : users) {
            int userDiscount = user[0];
            int userPrice = user[1];

            int priceSum = 0;
            for(int[] discount : discounts) {
                if(discount[0] >= userDiscount) {
                    priceSum += discount[1];
                }
            }

            if(priceSum >= userPrice) {
                joiner++;
            }else{
                revenue += priceSum;
            }
        }

        if(maxJoiner < joiner) {
            maxJoiner = joiner;
            maxRevenue = revenue;
        }else if(maxJoiner == joiner && maxRevenue < revenue) {
            maxRevenue = revenue;
        }

    }
}

```
