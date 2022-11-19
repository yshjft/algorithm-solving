## 문제
[2022 KAKAO BLIND RECRUITMENT 양과 늑대](https://school.programmers.co.kr/learn/courses/30/lessons/92343)

## 풀이
```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private int n;
    private int max;
    private List<List<Integer>> graph;

    public int solution(int[] info, int[][] edges) {
        initField(info, edges);
        
        dfs(0, 0, 0, new boolean[n], info);
        
        

        return max;
    }

    private void initField(int[] info, int[][] edges) {
        n = info.length;
        max = 0;
        
        graph = new ArrayList<>();
        for(int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }

        for(int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
    }

    private void dfs(int now, int sheep, int wolf, boolean[] visited, int[] info) {
        if(info[now] == 0) {
            sheep++;
        }else if(info[now] == 1) {
            wolf++;
        }
        

        if(wolf >= sheep) {
            return;
        }

        max = Math.max(max, sheep);

        visited[now] = true;
        for(int i = 0; i < n; i++) {
            if(visited[i]) {
                for(int next : graph.get(i)) {
                    if(!visited[next]) {
                        dfs(next, sheep, wolf, visited, info);
                    }
                }
            }
        }
        visited[now] = false;
    }
}
```
