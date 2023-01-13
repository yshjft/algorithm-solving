## 문제
[2023 KAKAO BLIND RECRUITMENT 표현 가능한 이진트리](https://school.programmers.co.kr/learn/courses/30/lessons/150367)

## 설명
* 푸는데 고생한 이유
  * 포화 이진트리를 만들기 위해 왼쪽에 붙일 0의 개수를 완전히 잘 못 계산
  * 루트 노드만 있는 경우를 분리해서 생각하지 않고 한번에 처리하려고 시도함
  * 루트 노드 0, 왼쪽 0, 오른쪽 0인 경우 생각하지 못함
* [참고 풀이](https://velog.io/@seunghyun97/%ED%91%9C%ED%98%84-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%9D%B4%EC%A7%84%ED%8A%B8%EB%A6%AC)

## 풀이
```java
public class Solution {
    public int[] solution(long[] numbers) {
        int[] answer = new int[numbers.length];

        for(int i = 0; i < numbers.length; i++) {
            String binaryString = numberToBinaryString(numbers[i]);

            if(binaryString.length() == 1) {
                answer[i] = binaryString.equals("1") ? 1 : 0;
            }else {
                answer[i] = isValidBinaryTree(binaryString) ? 1 : 0;
            }
        }

        return answer;
    }

    private String numberToBinaryString(long number) {
        StringBuilder binaryStringBuilder = new StringBuilder(Long.toBinaryString(number));

        int length = binaryStringBuilder.length();
        int binaryTreeLength = 1;
        int h = 1;
        while(length > binaryTreeLength) {
            binaryTreeLength += (int)Math.pow(2, h++);
        }

        int need = binaryTreeLength - length;
        while(need-- > 0) {
            binaryStringBuilder.insert(0, "0");
        }

        return binaryStringBuilder.toString();
    }

    private boolean isValidBinaryTree(String binaryString) {
        boolean isValid = true;

        int mid = (binaryString.length()-1)/2;
        char root = binaryString.charAt(mid);
        String left = binaryString.substring(0, mid);
        String right = binaryString.substring(mid + 1);

        if(root == '0' && (left.charAt((left.length()-1)/2) == '1' || right.charAt((right.length()-1)/2) == '1')) {
            return false;
        }

        if(left.length() >= 3) {
            isValid = isValidBinaryTree(left);
            if(isValid) {
                isValid = isValidBinaryTree(right);
            }
        }

        return isValid;
    }
}

```
