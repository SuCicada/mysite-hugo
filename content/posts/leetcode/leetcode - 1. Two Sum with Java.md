https://leetcode.com/problems/two-sum/submissions/

```java
import java.util.*;
class Main {
    public int[] fun(int []nums,int target) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            int f = target - nums[i];
            if(map.get(f)!=null){
                return new int[]{map.get(f),i};
            }else{
                map.put(nums[i],i);
            }
        }
        return null;
    }
    public static void main(String[] args) {
        int []nums = {12,3,3,53,45,7,7,3};
        int []res = new Main().fun(nums,10);
        System.out.println(Arrays.toString(nums));
        System.out.println(Arrays.toString(res));
    }
}   
```

