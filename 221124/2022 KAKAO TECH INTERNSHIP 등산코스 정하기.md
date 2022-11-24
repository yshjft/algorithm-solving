## 문제
[2022 KAKAO TECH INTERNSHIP 등산코스 정하기](https://school.programmers.co.kr/learn/courses/30/lessons/118669)

## 풀이
```java
import java.util.*;

public class Solution {
    private static class Point {
        private int idx;
        private int dist;

        public Point(int idx, int dist) {
            this.idx = idx;
            this.dist = dist;
        }

        public int getIdx() {
            return idx;
        }

        public int getDist() {
            return dist;
        }
    }
    private static final int INF = Integer.MAX_VALUE;
    private List<List<Point>> map;

    public int[] solution(int n, int[][] paths, int[] gates, int[] summits) {
        initField(n, paths, gates, summits);
        return dijkstra(n, gates, summits);
    }

    private void initField(int n, int[][] paths, int[] gates, int[] summits) {
        map = new ArrayList<>();
        for(int i = 0; i <= n; i++) {
            map.add(new ArrayList<>());
        }

        for(int i = 0; i < paths.length; i++) {
            int a = paths[i][0];
            int b = paths[i][1];
            int dist = paths[i][2];

            if(isGate(a, gates) || isSummit(b, summits)) {
                map.get(a).add(new Point(b, dist));
            }else if(isGate(b, gates) || isSummit(a, summits)) {
                map.get(b).add(new Point(a, dist));
            }else{
                map.get(a).add(new Point(b, dist));
                map.get(b).add(new Point(a, dist));
            }
        }
    }

    private int[] dijkstra(int n, int[] gates, int[] summits) {
        Queue<Point> q = new ArrayDeque<>();
        int[] intensity = new int [n + 1];

        Arrays.fill(intensity, INF);

        //
        for(int gate : gates) {
            q.add(new Point(gate, 0));
            intensity[gate] = 0;
        }

        while(!q.isEmpty()) {
            Point now = q.poll();

            if(now.dist > intensity[now.getIdx()]) {
                continue;
            }

            for(Point next : map.get(now.getIdx())) {
                int nextIntensity = Math.max(intensity[now.getIdx()], next.getDist());

                if(intensity[next.getIdx()] > nextIntensity) {
                    q.add(new Point(next.getIdx(), nextIntensity));
                    intensity[next.getIdx()] = nextIntensity;
                }
            }
        }

        int minSummit = INF;
        int minIntensity = INF;

        Arrays.sort(summits);
        for(int summit : summits) {
            if(intensity[summit] < minIntensity) {
                minSummit = summit;
                minIntensity = intensity[summit];
            }
        }

        return new int[]{minSummit, minIntensity};
    }

    private static boolean isGate(int num, int[] gates) {
        for (int gate : gates) {
            if (num == gate) return true;
        }
        return false;
    }

    private static boolean isSummit(int num, int[] submits) {
        for (int submit : submits) {
            if (num == submit) return true;
        }
        return false;
    }
}

```
