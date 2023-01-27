[where is problem ](https://leetcode.com/problems/zigzag-conversion/)
搞笑的空间复杂度
使用long，最后进行比较会更小，不知道为什么
```java
class Solution {
    public int reverse(int x) {
        boolean minus = x<0;
        x *= minus?-1:1;
        int res = 0;
        while(x>0){
            if(res*10/10 != res){
                return 0;    
            }
            res = res*10 + x%10; 
            x = x/10;
        }
        return res * (minus?-1:1);
    }
}
```

