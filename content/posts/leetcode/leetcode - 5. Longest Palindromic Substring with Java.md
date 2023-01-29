[题目在我这里](https://leetcode.com/problems/longest-palindromic-substring/)
方法一，大暴力
```java
 public String longestPalindrome1(String s) {
        /*
            very force: O(s.length()^2)
            空间复杂度  O(1)
        */
        int i=0;
        int j=0;
        boolean flag = false;
        int maxi = i;
        int maxj = j;
        for(i=0;i<s.length();i++){
            for(j=s.length()-1;j>i;j--){
                if(s.charAt(i)==s.charAt(j)){
                    int ii = i;
                    int jj = j;
                    for(jj=j;jj>ii;jj--,ii++){
                        if(s.charAt(ii)!=s.charAt(jj)){
                            // flag = true;
                            break;
                        }
                    }
                    System.out.println(ii+"  -  "+jj);
                    if(ii>=jj){
                        flag = true;
                        break;
                    }
                }
            }
            if(flag){
                if(j-i > maxj-maxi){
                    maxj = j;
                    maxi = i;
                }
            }
        }
        if(s.length()==0){
            return "";
        }
        if(maxi>=maxj){
            return s.substring(0,1);
        }
        return s.substring(maxi,maxj+1);
    }
```

方法二，dp  ( 69ms )
```java
    public String longestPalindrome2(String s) {
       /*
            DP
            空间复杂度  O(N^2)
            时间复杂度  O(N^2)
            f(i,j)  = { true    (f(i+1,j-1)==true && s[i]==s[j])
                      { false    otherwise
            f(i,i)   = true
            f(i,i+1) = s[i] == s[i+1]
        */
        int N = s.length();
        boolean dp[][] = new boolean[N][N];
        int max = 0;
        int a=0,b=0;
        if(N==0){
            return "";
        }
        for(int j=0;j<N;j++){
            for(int i=0;i<=j;i++){
                // System.out.println(i+" "+j+" "+s.charAt(i) +" "+ s.charAt(j));
                dp[i][j] = s.charAt(i) == s.charAt(j);
                if(i+1<N && j-1>=0 && j-i>=2){
                    dp[i][j] = dp[i][j] && dp[i+1][j-1];
                }
                // System.out.println(j-i+1);
                if(dp[i][j] && j-i+1 > max){
                    max = j-i+1;
                    a = i;
                    b = j;
                }
            }
        }
        for(boolean i[]: dp){
            System.out.println(Arrays.toString(i));
        }
        System.out.println(max+" "+a+" "+b);
        return s.substring(a,b+1);
    }
```

方法三，中心扩展法 （递归版）   6ms
```java
    public String longestPalindrome3(String s) {
        /*中心扩展法
            空间复杂度  O(1)
            时间复杂度  O(N^2)
        */
        if (s == null || s.length() < 1) return "";
        int a = 0;
        int b = 0;
        for(int i=0;i+1<s.length();i++){
            int len = Math.max(check(s,i,i), check(s,i,i+1));
            if(len > b-a+1){

                a = i - (len-1)/2;
                b = i + len/2;
                System.out.println(len+" "+a+ " "+b);
            }
        }

        return s.substring(a,b+1);
    }
    public int check(String s,int a,int b){
        while(a>=0 && b<s.length() && s.charAt(a)==s.charAt(b)){
            a--;
            b++;
        }
        // return new int[]{a,b};
        System.out.println(a+ " "+b);
        return b-a-1;
    }
```

方法四、Manacher    ( 4ms )
```java
    public String longestPalindrome4(String s) {
        /*
            Manacher
            O(n)
              a   b   c   b  
              0   1   2   3
            # a # b # c # b #
            0 1 2 3 4 5 6 7 8
            i < maxRight: m[i] = min(m[pos+(i-pos)], maxRight - i)
                
        */
        if (s == null || s.length() < 1) {
            return "";
        }
        char ss[] = new char[s.length()*2+1];
        ss[0] = '#';
        for(int i=0;i<s.length();i++){
            ss[i*2+1] = s.charAt(i);
            ss[i*2+2] = '#';
        }   
        int man[] = new int[ss.length];
        int pos = 0;
        int maxRight = 0;
        int maxPos = 0;
        int maxLen = 0;
        for(int i=0;i<man.length;i++){
            if(i<maxRight){
                man[i] = Math.min(man[pos+pos-i],maxRight-i);
            }else{
                man[i] = 0;
            }
            while(i-man[i] >= 0 && i+man[i]<ss.length && ss[i+man[i]] == ss[i-man[i]]){
                man[i]++;
            }
            if(maxRight < i+man[i]){
                maxRight = i+man[i];
                pos = i;
            }
            if(man[i] > maxLen){
                maxLen = man[i];
                maxPos = i;
            }
        }
        System.out.println(maxLen+" "+maxPos);
        if(maxPos%2==1){
            return s.substring((maxPos-1)/2-(maxLen-1)/2,(maxPos-1)/2+(maxLen-1)/2+1);
        }else{
            return s.substring((maxPos-1)/2-(maxLen-1)/2+1,(maxPos-1)/2+(maxLen-1)/2+1);
        }
    }
```
