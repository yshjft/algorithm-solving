## 문제
[2023 KAKAO BLIND RECRUITMENT 택배 배달과 수거하기](https://school.programmers.co.kr/learn/courses/30/lessons/150369)

## 풀이
```java

import java.util.Stack;

public class Solution {
    public long solution(int cap, int n, int[] deliveries, int[] pickups) {
        long answer = 0;

        // stack의 꼭대기에는 거리가 먼 것이 있다.
        Stack<Integer> dStack = new Stack<>(); // 거리가 먼 배달
        Stack<Integer> pStack = new Stack<>(); // 거리가 먼 회수
        for(int i = 0; i < n; i++) {
            if(deliveries[i] > 0) {
                dStack.add(i);
            }

            if(pickups[i] > 0) {
                pStack.add(i);
            }
        }

        // -> : 가장 멀리 있는 곳 까지 가면서 배달할 곳 계산한다.
        // <- : 되돌아 오면서 회수한다.
        while(!dStack.isEmpty() && !pStack.isEmpty()) {
            // 거리가 먼것 부터 처리해야 이득(거리가 가까운건 여러번 움직이게 된다.)
            // 멀리 갔다가 다시 시작점으로 돌아오는 것을 고려 해야 한다(*2)
            answer += Math.max((dStack.peek()+1)*2, (pStack.peek()+1)*2);

            // box : 배달할 수 있는 box의 개수
            int box = 0;
            while(!dStack.isEmpty() && box <= cap) {
                if(deliveries[dStack.peek()] + box <= cap) {
                    box += deliveries[dStack.peek()];
                }else{
                    deliveries[dStack.peek()] -= (cap - box); // 전부는 배달 못하고 배달할 수 있는 만큼만 배달
                    break;
                }

                dStack.pop();
            }

            // box : 회수할 수 있는 box의 개수
            box = 0;
            while(!pStack.isEmpty() && box <= cap) {
                if(pickups[pStack.peek()] + box <= cap) {
                    box += pickups[pStack.peek()];
                }else{
                    pickups[pStack.peek()] -= (cap - box); // 전부 회수는 못하고 회수할 수 있는 만큼만 회수
                    break;
                }
                pStack.pop();
            }
        }

        // 남은 배송을 처리하자
        while(!dStack.isEmpty()) {
            answer += (dStack.peek()+1)*2;

            int box = 0;
            while(!dStack.isEmpty() && box <= cap) {
                if(deliveries[dStack.peek()] + box <= cap) {
                    box += deliveries[dStack.peek()];
                }else{
                    deliveries[dStack.peek()] -= (cap - box);
                    break;
                }
                dStack.pop();
            }
        }

        // 남은 회수를 처리하자
        while(!pStack.isEmpty()) {
            answer += (pStack.peek()+1)*2;

            int box = 0;
            while(!pStack.isEmpty() && box < cap) {
                if(pickups[pStack.peek()] + box <= cap) {
                    box += pickups[pStack.peek()];
                }else{
                    pickups[pStack.peek()] -= (cap - box);
                    break;
                }
                pStack.pop();
            }
        }


        return answer;
    }
}

```
