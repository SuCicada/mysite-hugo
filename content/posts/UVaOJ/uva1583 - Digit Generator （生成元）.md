> 
如果x加上x的各个数字之和得到y，就说x是y的生成元。给出n（1≤n≤100000），求最小
生成元。无解输出0。例如，n=216，121，2005时的解分别为198，0，1979。
**Sample Input**
3
216
121
2005
**Sample Output**
198
0
1979

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=829&page=show_problem&problem=4458
```
//先算出所有的生成元x的原数y，以s[y]=x的形式将其存起，在求y的解x的时候，直接输出是s[y]即可
//所以为了求最小生成元也就是x，就要从尾到头的循环算y。
#include<iostream>
#include<cstdio>
#include<string.h>
using namespace std;
int const N = 100000;
int sn[N+5]={0};
int main()
{
    memset(sn,0,sizeof(sn)); 
    for(int i=N;i>=0;i--)
    {
        int n=i;
        int s=i;
        while(n>0)
        {
            s+=n%10;
            n/=10;
        }
        sn[s]=i;
    }
    //for(int i=0;i<N;i++)
    //    cout<<sn[i]<<"  ";
   // cout<<endl;
    int n,T;
    cin>>T;
    while(T--)
    {
        int i;
        cin>>n;
        cout<<sn[n]<<endl;
    }
    return 0;
}
//AC at 2017/12/7

```
---
以下算法虽然一样可以算出正确答案，但是超时，oj不通过。
```

//Time limit exceeded
//先算出所有的原数y，将其以s[x]=y;的形式存起，然后循环对比：要求的数和s中哪个元素相等，输出此元素的下标即可
#include<iostream>
#include<cstdio>
using namespace std;
int const N = 100000;
int main()
{
    int sn[N+5]={0};
    for(int i=1;i<=N;i++)
    {
        int n=i;
        sn[i]=i;
        while(n>0)
        {
            sn[i]+=n%10;
            n/=10;
        }
    }
    //for(int i=0;i<N;i++)
    //    cout<<sn[i]<<"  ";
   // cout<<endl;
    int n,T;
    cin>>T;
    while(T--)
    {
        int i;
        cin>>n;
        for(i=1;i<=N;i++)
        {
            if(n==sn[i])
            {
                cout<<i<<endl;
                break;
            }
        }
        if(i>N)
            cout<<0<<endl;
    }
    return 0;
}

```
总结：
第一种还是刘汝佳给的思路，只循环了N次，而后者循环2*N次。思维不能太死板，就如同那句警言：
KEEP IT SIMPLE AND STUPID