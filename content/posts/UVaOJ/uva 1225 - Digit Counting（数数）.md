> 把前n（n≤10000）个整数顺次写在一起：123456789101112…数一数0～9各出现多少次
（输出10个整数，分别是0，1，…，9出现的次数）。
**Sample Input**
2
3
13
**Sample Output**
0 1 1 1 0 0 0 0 0 0
1 6 2 2 1 1 1 1 1 1
（注：输出的最后一个数后面没有空格）

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=3666&mosmsg=Submission+received+with+ID+20469369
```
//判断一个数各个位的数字，存于一个数组中，倒的来循环也无妨
#include<iostream>
#include<cstdio>
using namespace std;
int main()
{
    int T;
    cin>>T;
    getchar();
    while(T--)
    {
        int num[10]={0};
        int N;
        cin>>N;
        getchar();
        while(N--)
        {
            int ni=N+1;
            while(ni)
            {
                num[ni%10]++;
                ni/=10;
            }
        }
        for(int i=0;i<9;i++)
            cout<<num[i]<<" ";
        cout<<num[9]<<endl;
    }
    return 0;
}
//AC at 2017/12/10

```