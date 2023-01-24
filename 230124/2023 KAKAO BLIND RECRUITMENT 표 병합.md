## 문제
[2023 KAKAO BLIND RECRUITMENT 표 병합](https://school.programmers.co.kr/learn/courses/30/lessons/150366)

## 풀이
```java
package KAKAO_BLIND_RECRUITMENT_2023_표_병합;

import java.util.ArrayList;
import java.util.List;

public class Solution {

    public String[] solution(String[] commands) {
        // 병합
        // (r1, c1)  (r2, c2)
        // 같은 셀인 경우 무시 -> 병합이 되면 같은 셀이다
        // 인접 ㅇㅇ -> 걍 병합
        // 인접 ㄴㄴ -> 선택된 셀들만 영향
        // 두셀 중 한 셀 -> 있는 값으로 통일
        // 두셀 모두 -> (r1, c1)
        // (공통) 어느 위치를 선택하여도 병합된 셀로 접 -> 서로 연결 되어 있어야 한다(하나의 그룹으로) -> 유니온 파인드


        // 병합 해제
        // 해당 셀의 모든 병합 해제 -> 연결된 모든 셀들의 병합을 해제
        // 프로그램 초기 상태로 돌아감 -> 아무 것도 가지고 있지 않은 상태, 다만 r,c는 해지 저 값을 가지고 있는다


        int[] parents = new int[2501];
        String[] values = new String[2501];
        for(int i = 0; i <= 2500; i++) {
            parents[i] = i;
            values[i] = "";
        }


        List<String> prints = new ArrayList<>();
        for(String command : commands) {
            String[] words = command.split(" ");

            switch (words[0]) {
                case "UPDATE":
                    update(words, values, parents);
                    break;
                case "MERGE":
                    merge(words, parents, values);
                    break;
                case "UNMERGE":
                    unMerge(words, parents, values);
                    break;
                default:
                    print(words, parents, values, prints);
                    break;
            }
        }

        String[] answer = new String[prints.size()];
        for(int i = 0; i < prints.size(); i++) {
            answer[i] = prints.get(i);
        }

        return answer;
    }

    // 항상 root(parent)가 중요
    private void update(String[] words, String[] values, int[] parents) {
        if(words.length == 4) {
            int r = Integer.parseInt(words[1]);
            int c = Integer.parseInt(words[2]);
            values[find(convert(r, c), parents)] = words[3];
        }else{
            String value1 = words[1];
            String value2 = words[2];
            for(int i = 1; i < values.length; i++) {
                if(values[i].equals(value1)) {
                    values[i] = value2;
                }
            }
        }
    }

    private void merge(String[] words, int[] parents, String[] values) {
        int idx1 = convert(Integer.parseInt(words[1]), Integer.parseInt(words[2]));
        int idx2 = convert(Integer.parseInt(words[3]), Integer.parseInt(words[4]));
        int p1 = find(idx1, parents);
        int p2 = find(idx2, parents);

        // 같은 root(parent)는 무시
        if(p1 == p2) {
            return;
        }

        // 같은 root(parent)로 만듬 (p1을 root로 만듬)
        union(p1, p2, parents);

        // 값을 변경
        String value = values[p1].equals("") ? values[p2] : values[p1];
        values[p1] = "";
        values[p2] = "";
        values[p1] = value;
    }

    private void unMerge(String[] words, int[] parents, String[] values) {
        int r = Integer.parseInt(words[1]);
        int c = Integer.parseInt(words[2]);
        int idx = convert(r, c);

        int p = find(idx, parents);
        String value = values[p];

        List<Integer> tmp = new ArrayList<>();
        for(int i = 1; i < values.length; i++) {
            if(find(i, parents) == p) {
                tmp.add(i);
            }
        }

        for(int i : tmp) {
            parents[i] = i;
            values[i] = "";
        }

        values[idx] = value;
    }

    private void print(String[] words, int[] parents, String[] values, List<String> prints) {
        int idx = convert(Integer.parseInt(words[1]), Integer.parseInt(words[2]));
        int p = find(idx, parents);
        prints.add(values[p].equals("") ? "EMPTY" : values[p]);
    }

    // 중요! 좌표 -> 번호
    private int convert(int r, int c) {
        int tmp = (r - 1) * 50  + c;
        return tmp;
    }

    private void union(int p1, int p2, int[] parents) {
        p1 = find(p1, parents);
        p2 = find(p2, parents);

        if(p1 != p2) {
            parents[p2] = p1;
        }
    }

    private int find(int a, int[] parents) {
        if(a == parents[a]) {
            return parents[a];
        }else{
            return parents[a] = find(parents[a], parents);
        }
    }

}

```
