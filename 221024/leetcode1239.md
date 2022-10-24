## 문제
[LeetCode 1239](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)

## 설명
* 백트래킹 문제입니다.

## 풀이
```java
import java.util.*;

public class Solution {
    private int maxLength;

    public int maxLength(List<String> arr) {
        maxLength = 0;
        search("", 0, new HashSet(), arr);

        return maxLength;
    }

    private void search(String s, int idx, Set<Character> used, List<String> arr) {
        maxLength = Math.max(maxLength, s.length());

        for(int i = idx; i < arr.size(); i++) {
            String str = arr.get(i);
            Set<Character> characters = getStringCharacters(str);

            if(isValid(str, characters, used)) {
                used.addAll(characters);
                search(s + str, i + 1, used, arr);
                used.removeAll(characters);
            }
        }
    }

    private Set<Character> getStringCharacters(String str) {
        Set<Character> characters = new HashSet<>();
        for(int j = 0; j < str.length(); j++) {
            characters.add(str.charAt(j));
        }
        
        return characters;
    }
    
    private boolean isValid(String str, Set<Character> characters, Set<Character> used) {
        if(characters.size() != str.length()) {
            return false;
        }

        for(Character character : characters) {
            if(used.contains(character)){
                return false;
            }
        }

        return true;
    }
}

```
