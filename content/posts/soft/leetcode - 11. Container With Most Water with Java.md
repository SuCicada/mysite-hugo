[problem](https://leetcode.com/problems/container-with-most-water/)
```java
import java.lang.reflect.Method;
import java.util.*;

class Solution {
    public int maxArea(int[] height) {
        int max = 0;
        int left = 0;
        int right = height.length-1;
        int a,b;        
        while(left<right){
            a = height[left];
            b = height[right];
            max = Math.max(max, Math.min(a,b)*(right-left));
            if(a > b){
                right -- ;
            }else{
                left ++;
            }
            System.out.println(left+" "+right+" ");
        }
        return max;
    }
}



class Main {
    public static void main(String[] args) throws Exception{
        Solution solution = new Solution();
        int i = solution.maxArea(new int[]{1,8,6,2,5,4,8,3,7});
        System.out.println(i);
    }
}
```
