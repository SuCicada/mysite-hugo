[题目链接](https://leetcode.com/problems/longest-substring-without-repeating-characters/submissions/)
```java
import java.lang.reflect.Method;
class Solution {
    /*两个方法都是：end无所谓的在一个一个移动，begin则是追寻之前的循环体留下的信标
      关键在于信标是怎么设置下的，第一种方法是每一次都从序列中寻找信标。
      第二种直接记录了下来。
      一个用时间换空间，一个用空间换时间。
    */
    public int lengthOfLongestSubstring1(String s) {
        int sum = 0;
        int begin = 0;
        int end = 0;
        for(end = begin;end<s.length();end++){  /*每次都移动尾指针*/
            for(int i = begin;i<end;i++){     
            /*验证重复，和end处比较，如果没有重复，说明这个子串可以加入end处的字符
              而无论是否重复，都不影响移动尾指针end，影响的只是begin的位置。
            */
                if(s.charAt(i) == s.charAt(end)){
                    sum = Math.max(sum,end-begin);
                    begin = i+1;
                    break;
                }
            }
        }
        /*最后的end会指向最后一个的下一个，所以需要再判断一下夹住的是不是大的，
          eg: ab or aab  
        */
        return Math.max(sum,end-begin);
    }

    public int lengthOfLongestSubstring2(String s) {
        int ascii[] = new int[128];
        int sum = 0;
        int begin = 0;
        for(int i=0;i<s.length();i++){ /*i是end*/
            /* begin是慢慢推进的，循环体就像是在插眼，begin一个一个跳过去*/
            begin = Math.max(begin,ascii[s.charAt(i)]);  
            sum = Math.max(sum,i-begin+1);
            ascii[s.charAt(i)] = i + 1;  /* 在当前的下一个插眼，告诉begin要跳下一个*/
        }
        return sum;
    }

}


class Main {
    public static void main(String[] args) throws Exception{
        String solutionMethod = "lengthOfLongestSubstring2";
        Method lengthOfLongestSubstring =  Solution.class.getMethod(solutionMethod,String.class);
        Solution solution = new Solution();
        System.out.println(lengthOfLongestSubstring.invoke(solution,""));
        System.out.println(lengthOfLongestSubstring.invoke(solution,"a"));
        System.out.println(lengthOfLongestSubstring.invoke(solution,"bb"));
        System.out.println(lengthOfLongestSubstring.invoke(solution,"ab"));
        System.out.println(lengthOfLongestSubstring.invoke(solution,"abb"));
        System.out.println(lengthOfLongestSubstring.invoke(solution,"aab"));
        System.out.println(lengthOfLongestSubstring.invoke(solution,"abcabcbb"));
    }
}   
```

---
就是这样，一道一道刷，要找回以前的感觉
