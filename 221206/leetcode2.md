## 문제
[2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)

## 풀이
```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode ans = new ListNode();

        ListNode l1Now = l1;
        ListNode l2Now = l2;
        ListNode now = ans;

        int carry = 0;
        while (l1Now != null || l2Now != null) {
            int sum = carry;
            sum += (l1Now == null ? 0 : l1Now.val);
            sum += (l2Now == null ? 0 : l2Now.val);

            if(sum >= 10) {
                sum -= 10;
                carry = 1;
            }else{
                carry = 0;
            }
            now.val = sum;


            l1Now = l1Now == null ? null : l1Now.next;
            l2Now = l2Now == null ? null : l2Now.next;

            if(l1Now != null || l2Now != null) {
                now.next = new ListNode();
                now = now.next;
            }

            if(l1Now == null && l2Now == null && carry != 0) {
                now.next = new ListNode();
                now = now.next;
                now.val = carry;
            }
        }

        return ans;
    }

}

```
