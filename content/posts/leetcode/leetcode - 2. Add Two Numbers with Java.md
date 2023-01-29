https://leetcode.com/problems/add-two-numbers/submissions/
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode head = res;
        int flag = 0;
        while(l1!=null || l2!=null || flag!=0){
            int num = (l1==null?0:l1.val) + (l2==null? 0: l2.val) + flag;
            res.next = new ListNode(num%10);
            flag = num/10;
            res = res.next;
            if(l1!=null)
                l1 = l1.next;
            if(l2!=null)
                l2 = l2.next;
        }
        return head.next;
    }
}
```
