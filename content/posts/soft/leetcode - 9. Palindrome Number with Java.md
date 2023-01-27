[题目](https://leetcode.com/problems/palindrome-number/)
```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0){
            return false;
        }
        int a = 0;
        int b = x;
        while(x>0){
            a = a * 10 + x%10;
            x = x/10;
        }
        return a==b;
    }
}
```
