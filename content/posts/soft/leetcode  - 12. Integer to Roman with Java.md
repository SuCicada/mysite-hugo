[problem](https://leetcode.com/problems/integer-to-roman/)

方法一，一位一位判断。
```java
class Solution {
   public String intToRoman1(int num) {
        StringBuilder sb = new StringBuilder();
        char ooo[] = new char[]{'I','V','X','L','C','D','M'};

        int wei = 0;
        while(num>0){
            int n = num %10;
            if(n<4){
                for(int i=0;i<n;i++){
                    sb.append(ooo[wei*2]);
                }
            }else if(n==4){
                sb.append(ooo[wei*2+1]);
                sb.append(ooo[wei*2]);
            }else if(n==5){
                sb.append(ooo[wei*2+1]);
            }else if(n>5 && n<9){
                for(int i=0;i<n-5;i++){
                    sb.append(ooo[wei*2]);
                }
                sb.append(ooo[wei*2+1]);
            }else if(n==9){
                sb.append(ooo[wei*2+1+1]);
                sb.append(ooo[wei*2]);
            }
            wei++;
            num = num/10;
        }
        return sb.reverse().toString();
    }
```

方法二，因为知道最多4位数，所以打表暴力。
```java
    public String intToRoman(int num) {
	    String s1[] = new String[]{"","I","II","III","IV","V","VI","VII","VIII","IX"};
        String s2[] = new String[]{"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"};
        String s3[] = new String[]{"","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"};
        String s4[] = new String[]{"","M","MM","MMM"};
        // return s4[(num/1000)%10]+s3[(num/100)%10]+s2[(num/10)%10]+s1[num%10];
        return new StringBuilder().append(s4[(num/1000)]).append(s3[(num/100)%10]).append(s2[(num/10)%10]).append(s1[num%10]).toString();
    }
}
```
