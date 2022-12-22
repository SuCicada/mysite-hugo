![在这里插入图片描述](http://acm.hdu.edu.cn/data/images/1030-1.jpg)
两格子之间的距离
http://acm.hdu.edu.cn/showproblem.php?pid=1030

走一步判断一下 下一步走哪里
是走下面呢（前提要能走）
还是走左边
还是走右边

设定一个格子，代表当前走到的地方
如果能直接向下走，就向下走
不能的话：
如果终点在当前格子的哪一边（只看左右方向，不看上下），就走哪一边

```
#include<iostream>
#include<cmath>
using namespace std;

// 同一行 相邻的能通过
// 上下行  上行的第奇数个能和下行中第偶数个过

//1 3 5 7
//1 2*1-1 2*n-1
//(1+ 2*n-1)*n/2 = int(a)

//第 sqrt(a) +1 行
//第 a - sqrt(a)^2 个


// 向左走，向右走？

int which(int a){
    int n = sqrt(a-1);
    int ge = a-n*n;
    n ++;
    return n;
}
int getmid(int a){
    int n = which(a);
    return pow(n-1,2) + n;
}
int main(){
    int a,b;
    while(cin>>a>>b){
        if(a>b)
            a^=b^=a^=b;

        int go = a;
        int b_n = which(b);// 行
        int b_mid = getmid(b);
        int go_mid;
        int go_n;
        int sum = 0;
        while(go != b){
            go_n = which(go);
            if(go_n == b_n){
                sum += abs(go - b);
                break;
            }
            else if((go_n+go) % 2 == 0){
                // 直接下去
                go = go + go_n*2;
                sum++;
                continue;
            }

            go_mid = getmid(go);
            int go_distance = go - go_mid; //go距离行中线的距离
            int b_distance = b - b_mid;    // b 距离中线的距离
            int hang_distance = go_distance - b_distance;  // go 到 b的距离

            if(hang_distance<0){  // b 在 go 的右边
                // right
                go ++;
            }else // b 在 go 右边 或 b和go在同一列
                go--;
            }
//            else{
////                if(b - b_mid < 0)
//                    go--;
////                else
////                    go++;
//            }
            sum++;
        }
        cout<<sum<<endl;
    }
    return 0;
}

// AC at 2019/3/9 23:09
```

-----
我只能想到这个笨办法

