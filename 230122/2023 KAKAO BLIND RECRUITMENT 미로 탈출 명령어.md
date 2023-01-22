## 문제
[2023 KAKAO BLIND RECRUITMENT 미로 탈출 명령어](https://school.programmers.co.kr/learn/courses/30/lessons/150365)

## 풀이
```java
import java.util.HashMap;
import java.util.Map;

// 가중치가 다르기(빠른 사전순) 때문에


public class Solution {
    public String solution(int n, int m, int x, int y, int r, int c, int k) {
        // n X m 격자
        // (x, y) -> (r, c), 거리는 총 k
        // 같은 격자 2번 이상 방문 가능 (중복 방문 가능)
        // 격자 밖으로 이동 불가능
        // 경로 문자열 표시 할 때 문자열이 사전 순으로 가장 빠른 경로로 탈출

        int dist = Math.abs(x - r) + Math.abs(y - c);
        k -= dist;

        if(k < 0 || k % 2 != 0) {
            return "impossible";
        }

        Map<Integer, Character> map = new HashMap<>();
        map.put(0, 'd');
        map.put(1, 'l');
        map.put(2, 'r');
        map.put(3, 'u');

        // d l r u
        int[] cnt = new int[4];
        if(x - r < 0) {
            // d
            cnt[0] += r - x;
        }else{
            // u
            cnt[3] += x - r;
        }

        if(y - c < 0) {
            // r
            cnt[2] += c - y;
        }else{
            // l
            cnt[1] += y - c;
        }

        StringBuilder answer = new StringBuilder();

        append(cnt[0], "d", answer);

        /**
         * k / 2 : 최대로 이동할 수 있는 수(limit 이라고 생각할 것)
         * n - x - cnt[0] : x에서 n까지 이동해야 하는수
         */
        int d = Math.min(k/2, n - (x + cnt[0]));
        append(d, "d", answer);
        cnt[3] += d;
        k -= 2*d;


        append(cnt[1], "l", answer);

        /**
         * k / 2 : 최대로 이동할 수 있는 수(limit 이라고 생각할 것)
         * y - 1 - cnt[1] : y에서 1까지 이동해야하는 수
         */
        int l = Math.min(k/2, y - 1 - cnt[1]);
        append(l, "l", answer);
        cnt[2] += l;
        k -= 2*l;
        
        /**
         * k가 남아 있다면 rl로 더 채워준다.
         * rl 조합이 사전순으로 가장 앞에 둘수 있는 조합
         */
        append(k/2, "rl", answer);

        
        append(cnt[2], "r", answer);
        append(cnt[3], "u", answer);

        return answer.toString();
    }

    private void append(int num, String s, StringBuilder sb) {
        for(int i = 0; i < num; i++) {
            sb.append(s);
        }
    }

}

```
