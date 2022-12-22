> 习题3-10 盒子（Box, ACM/ICPC NEERC 2004, UVa1587）
给定6个矩形的长和宽wi和hi（1≤wi，hi≤1000），判断它们能否构成长方体的6个面。
**Sample Input**
1345 2584
2584 683
2584 1345
683 1345
683 1345
2584 683
1234 4567
1234 4567
4567 4321
4322 4567
4321 1234
4321 1234
**Sample Output**
POSSIBLE
IMPOSSIBLE

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=4462&mosmsg=Submission+received+with+ID+20554313

1、因为长方体都有三组含四条的边，我们先将三条不同的边长度用一个数组存起，在用一个数组存各个边在六组数据中分别出现的次数。
2、输入的同时来进行存边，并记录下出现的正方形的个数（即输入的两条边相等）。
3、六组输入完成后，判断是否属于以下情况
（1）a 4   , b 4 ,c 4 ,正方面 0
（2）a 8   , b 4 ,c 0 ,正方面 2
（3）a 12 , b 0 ,c 0 ,正方面 6
（a 4：a长度的边有4条）
属于的则是长方形，这里就要注意长方形有三种类型
```
#include<iostream>
using namespace std;
int main()
{
    int a[2];//每组的两条边
    while(cin>>a[0]>>a[1])
    {
        int s[3]={0},n[3]={0};//边长，边数目
        int ns6=6;
        int same=0;
        while(ns6--)//六个面
        {
            if(ns6<5)
                cin>>a[0]>>a[1];
            if(a[0]==a[1])
                same++;//输入的正方形的个数
            for(int j=0;j<2;j++)
                for(int i=0;i<3;i++)
                {
                    if(s[i]==a[j])//如果长度和i边一样，则第i边出现的个数+1
                    {
                        n[i]++;
                        break;
                    }
                    else if(s[i]==0)//存入新边
                    {
                        s[i]=a[j];
                        n[i]++;
                        break;
                    }
                }
        }
//        for(int i=0;i<3;i++)
//            cout<<"s "<<i<<" "<<s[i]<<" |n "<<i<<"  = "<<n[i]<<" same "<<same<<endl;
        if((n[0]==4&&n[1]==4&&n[2]==4&&same==0)||
           ((n[0]==4||n[0]==8)&&n[2]==0&&same==2)||//8并不一定是在n[0]
           (n[0]==12&&same==6))
           cout<<"POSSIBLE"<<endl;
        else
            cout<<"IMPOSSIBLE"<<endl;
    }
    return 0;
}
//AC at 2017/12/31

```
（题外话：题不难，多亏uva用户提供的样例，不然就难以发现最后一个代码注释所说的错误。）