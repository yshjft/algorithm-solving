## 문제
[개인정보 수집 유효기간](https://school.programmers.co.kr/learn/courses/30/lessons/150370)

## 풀이
```java
import java.util.*;

public class Solution {

    // 모든달 28일까지 있다고 가정
    // 날짜 정보를 class로 관리
    private static class Date {
        private int year;
        private int month;
        private int day;

        public Date(int year, int month, int day) {
            this.year = year;
            this.month = month;
            this.day = day;
        }

        public int getYear() {
            return year;
        }

        public int getMonth() {
            return month;
        }

        public int getDay() {
            return day;
        }

        public Date getNextDate(int month) {
            int updatedYear = this.year;
            int updatedMonth = this.month + month;
            int updatedDay = this.day;

            if(updatedMonth > 12) {
                updatedYear += (updatedMonth % 12 == 0 ? updatedMonth / 12 - 1 : updatedMonth / 12);
                updatedMonth = updatedMonth % 12 == 0 ? 12 : updatedMonth % 12;
            }

            updatedDay -= 1;
            if(updatedDay == 0) {
                updatedDay = 28;
                updatedMonth -= 1;

                if(updatedMonth == 0) {
                    updatedMonth = 12;
                    updatedYear -= 1;
                }
            }

            return new Date(updatedYear, updatedMonth, updatedDay);
        }

        // this가 date보다 뒤에 있냐?
        public boolean isAfter(Date date) {
            if(this.year > date.getYear()) {
                return true;
            }else if(this.year == date.getYear() && this.month > date.getMonth()) {
                return true;
            }else if(this.year == date.getYear() && this.month == date.getMonth() && this.day > date.getDay()) {
                return true;
            }

            return false;
        }
        
         @Override
        public String toString() {
            final StringBuilder sb = new StringBuilder();
            sb.append(year).append(".");
            sb.append(month).append(".");
            sb.append(day).append(".");
            return sb.toString();
        }
    }

    public int[] solution(String today, String[] terms, String[] privacies) {
        Map<Character, Integer> termMap = new HashMap<>();
        StringTokenizer st = null;

        for(String term : terms) {
            st = new StringTokenizer(term);
            termMap.put(st.nextToken().charAt(0), Integer.valueOf(st.nextToken()));
        }

        st = new StringTokenizer(today, ".");
        Date todayDate = new Date(Integer.valueOf(st.nextToken()), Integer.valueOf(st.nextToken()), Integer.valueOf(st.nextToken()));

        List<Integer> disposalTargets = new ArrayList<>();
        for(int i = 0; i < privacies.length; i++) {
            String privacy = privacies[i];

            st = new StringTokenizer(privacy);
            String date = st.nextToken();
            int month = termMap.get(st.nextToken().charAt(0));

            st = new StringTokenizer(date, ".");
            Date startDate = new Date(Integer.valueOf(st.nextToken()), Integer.valueOf(st.nextToken()), Integer.valueOf(st.nextToken()));

            // 종료일 구하기
            Date endDate = startDate.getNextDate(month);
            System.out.println(startDate+" "+endDate);

            // 비교
            if(todayDate.isAfter(endDate)) {
                disposalTargets.add(i+1);
            }
        }

        int[] answer = new int[disposalTargets.size()];
        for(int i = 0; i < answer.length; i++) {
            answer[i] = disposalTargets.get(i);
        }

        return answer;
    }
}

```
