[题目地址](https://leetcode.com/problems/zigzag-conversion/)
这道题涉及到String, StringBuffer, StringBuilder的知识点
String在拼接时需要new新的String对象, 而它除了hash之外都是final属性, 分配空间会造成时间损耗.
所以String不适合大量拼接.  使用String可是耗时18ms呢
StringBuffer解决了这个问题, 它提升了速度, 是线程安全. 
而StringBuilder 和StringBuffer类似, 但它是线程不安全, 这也使得它比StringBuffer拼接更快. 本题只需3ms
有没有更快的呢, 当然有, 还就是它了 char[], 只需2ms(笑)
![100%什么的就是给我的恩赐](https://img-blog.csdnimg.cn/20190911224716179.png)
```java
import java.lang.reflect.Method;
import java.util.*;
class Solution {
    public String convert(String s, int numRows) {
        int N = s.length();
        // StringBuilder res = new StringBuilder();
        char res[] = new char[N];
        int cycle = (numRows-1)*2;
        int index = 0;
        if(numRows==1){
            return s;
        }
        for(int row=0;row<numRows;row++){
            /* row 
                0 6 
                1 4 2
                2 2 4
                3 0 
              */
            for(int i=row;i<N;i+=cycle){
                /* 一个锯齿*/
                // res.append(s.charAt(i));
                res[index++] = s.charAt(i);
                int next = i+(cycle-2*row); // i + cycle - 2*row < i + cycle  => row > 0
                if(next < N && row > 0 && next != i){
                    // res.append(s.charAt(next));
                    res[index++] = s.charAt(next);
                }
            }
        }
        return new String(res);
    }
}


class Main {
    public static void main(String[] args) throws Exception{
        Solution solution = new Solution();
        String i =solution.convert("PAYPALISHIRING",3);
        System.out.println(i);
    }
}   
```

