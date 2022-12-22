> 实现一个经典"猜数字"游戏。给定答案序列和用户猜的序列，统计有多少数字位置正确
（A），有多少数字在两个序列都出现过但位置不对（B）。
输入包含多组数据。每组输入第一行为序列长度n，第二行是答案序列，接下来是若干
猜测序列。猜测序列全0时该组数据结束。n=0时输入结束。
样例输入：
4
1 3 5 5
1 1 2 3
4 3 3 5
6 5 5 1
6 1 3 5
1 3 5 5
0 0 0 0
10
1 2 2 2 4 5 6 6 6 9
1 2 3 4 5 6 7 8 9 1
1 1 2 2 3 3 4 4 5 5
1 2 1 3 1 5 1 6 1 9
1 2 2 5 5 5 6 6 6 7
0 0 0 0 0 0 0 0 0 0
0
样例输出：
Game 1:
(1,1)
(2,0)
(1,2)
(1,2)
(4,0)
Game 2:
(2,4)
(3,2)
(5,0)
(7,0)

题目来自刘汝佳书，以下是原网址:
https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=829&page=show_problem&problem=276
```
//比较原数串和所猜数串,先将位置一样的找出,再对位置非-1(未匹配)的所猜数串元素和原数串诸位对比.
//所有比较过的位上的元素都替换为-1,防止二次匹配。
#include<iostream>
#include<cstdio>
#include<string.h>
using namespace std;
void output(int *a);
int input(int *a,int n)
{
    int iff=0;
    int i;
    for(i=0;i<n;i++)
    {
        cin>>a[i];
        if(a[i]==0)
            iff++;
    }
    a[i+1]='\0';
    if(iff==n)
        return 0;
    return 1;
}
void cop(int *coped,int *copee,int n)
{
    for(int i=0;i<n;i++)
    {
        copee[i]=coped[i];
    }
}
//void output(int a[])
//{
//    //cout<<"!"<<sizeof(a)<<endl;//之所以是4，是因为a只是一个指针
//    for(int i=0;a[i]!='\0';i++)
//        cout<<a[i]<<" ";
//    cout<<'\n';
//}
void guess(int *s,int *a,int n)
{
    int r=0,b=0;
    int *bb=new int[n];
    cop(s,bb,n);
    for(int j=0;j<n;j++)
    {
        if(a[j]==bb[j])
        {
            a[j]=bb[j]=-1;
            r++;
        }
    }
    for(int j=0;j<n;j++)
    {
        if(a[j]==-1)
            continue;
        for(int i=0;i<n;i++)
        {
            //cout<<"!!"<<i<<endl;
            if(a[j]==bb[i])
            {
                a[j]=bb[i]=-1;
    //output(bb);
   // output(a);
                b++;
                break;
            }
        }
    }
    printf("    (%d,%d)\n",r,b);
    delete []bb;
}
int main()
{
    int n,gn=1;
    int ori[1005]={0};
    int gus[1005]={0};
    while(scanf("%d",&n)==1&&n!=0)
    {
        cout<<"Game "<<gn++<<":"<<endl;
        input(ori,n);
        //cout<<"!"<<sizeof(ori)<<endl;
        //output(ori);
        while(input(gus,n)==1)
        {
            //output(gus);
            guess(ori,gus,n);
        }
    }
    return 0;
}

```

和书上的参考代码相比，这个代码太冗长。反思：
1.不能拘泥于题目的逻辑和固定的平常思维。
2.要的是巧解而不是解出。