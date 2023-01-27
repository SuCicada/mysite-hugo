简单做法, 左边哪一些, 右边拿一下, 看谁小拿谁.
但是时间复杂度达不到要求, 虽然在leetcode上也是很快
```java
public double findMedianSortedArrays1(int[] A,int[] B){
        /*
            time complexity: O((m+n)/2+1)
        */
        int m = A.length;
        int n = B.length;
        int index1 = 0;
        int index2 = 0;
        int num1=0;
        int num2=0;
        for(int i=0;i<(m+n)/2+1;i++){
            num2 = num1;
            if(index1 == m){
                num1 = B[index2];
                index2 ++;
            }else if(index2 == n){
                num1 = A[index1];
                index1++;
            }else if(A[index1] < B[index2]){
                num1 = A[index1];
                index1++;
            }else{
                num1 = B[index2];
                index2++;
            }
        }
        System.out.println(num1+" "+num2+" "+index1+" "+index2);
        if((n+m)%2==1){
            return num1;
        }else{
            return (num1+num2)/2.0;
        }        
    }

```

官方正解
```java
 public double findMedianSortedArrays(int[] A, int[] B) {
        /*
         time complexity O(log(m+n))
         0 -- i-1 | i -- m-1
         0 -- j-1 | j -- n-1
         1. A[i-1] <= B[j]
         2. B[j-1] <= A[i]
         len(0,i-1)+len(0,j-1) = len(i,m-1)+len(j,n-1)
         i-1-0+1 + j-1-0+1 = m-1-i+1 + n-1-j+1
         2*i+2*j = n+m  or n+m+1 (if n+m is odd)
         3. j = (n+m+1)/2 - i  (this '+1' don't refuse result)
         4. max(A[i-1],B[j-1]) <= min(A[i],B[j])
        */
        if(A.length>B.length){
            int []t = A;
            A = B;
            B = t;
        }
        int m = A.length;
        int n = B.length;
        int i;
        int j;
        int begin=0;
        int end=m;

        // i=0;
        while(begin<=end){
            i = (end + begin)/2;
            j = (n+m+1)/2-i;
            System.out.println(i+" "+j+" "+m+" "+n);
            if(i<end && B[j-1] > A[i]){  // i is small 
                begin = i+1;
                // i++;
            }else if(i>begin && A[i-1] > B[j]){   // i is big
                end = i-1;
                // i--;
            }else{
                System.out.println(i+" "+j);
                int maxLeft;
                if(i==0){
                    maxLeft = B[j-1];
                }else if(j==0){
                    maxLeft = A[i-1];
                }else{
                    maxLeft = Math.max(A[i-1],B[j-1]);
                }

                if((m+n)%2==1){
                    return maxLeft;
                }

                int minRight;
                if(i==m){
                    minRight = B[j];
                }else if(j==n){
                    minRight = A[i];
                }else{
                    minRight = Math.min(A[i],B[j]);
                }
                return (maxLeft + minRight)/2.0;
            }
        }
        return 0;
    }
```
