[题目](https://leetcode.com/problems/reverse-integer/)
```java
class Solution {
    public int myAtoi(String str) {
        int N = str.length();
        int minus = 0;
        long res = 0;
        for(int i=0;i<N;i++){
            char ch = str.charAt(i);
            System.out.println(ch+" "+minus);
            if((ch == '-' || ch == '+') && minus == 0){
                minus = ch=='+'?1:-1;
            }else if(ch ==' ' && minus==0){
                
            }else if(ch <='9' && ch>='0'){
                if(minus == 0){
                    minus = 1;
                }
                res = res*10 + ch-'0';
                if(res != (int)res){
                    return minus==1?2147483647:-2147483648;
                }
            }else{
                break;
            }
        }
        return (int)res*minus;
    }
}   
```

---
简单题做的真舒服呜呜呜我真是废物
