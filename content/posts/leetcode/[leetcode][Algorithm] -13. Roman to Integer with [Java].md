[problem](https://leetcode.com/problems/roman-to-integer/)

```java
class Solution {
    public int romanToInt(String s) {
        int N = s.length();
        int sum = 0;
        char a = 0;
        int b = 0;
        int c = 0;
        for(int i=0;i<N;i++){
            c = b;
            a = s.charAt(i);
            if(a == 'I'){
                b = 1;
            }else if(a == 'V'){
                b = 5;
            }else if(a == 'X'){
                b = 10;    
            }else if(a == 'L'){
                b = 50;
            }else if(a == 'C'){
                b = 100;
            }else if(a == 'D'){
                b = 500;
            }else if(a == 'M'){
                b = 1000;
            } 
            // System.out.println(i+" "+a+" "+c+" "+b);
            if(i>0 && c<b){
                c = -c;
            }
            sum += c;
            // System.out.println(sum);
        }
        sum += b;
        return sum;
    }
}
```
