> 长度为n的环状串有n种表示法，分别为从某
个位置开始顺时针得到。例如，图3-4的环状串
有10种表示：
CGAGTCAGCT，GAGTCAGCTC，AGTCAGCTCG等。在这些表示法中，字典序最小的称
为"最小表示"。
输入一个长度为n（n≤100）的环状DNA串（只包含A、C、G、T这4种字符）的一种表
示法，你的任务是输出该环状串的最小表示。例如，CTCC的最小表示是
CCCT，CGAGTCAGCT的最小表示为AGCTCGAGTC。
**Sample Input**
2
CGAGTCAGCT
CTCC
**Sample Output**
AGCTCGAGTC
CCCT

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=829&page=show_problem&problem=4459
```
#include<iostream>
#include<string>
#include<cstdio>
#include<string.h>
using namespace std;
int const N = 100;
//比较从be1,和从be2开始的字串哪一个更小
int com(char *s,int be1,int be2)
{
    int sl=strlen(s);
    for(int i=0;i<sl;i++)
    {
        //cout<<endl<<(be1+i)%N<<endl;
    //cout<<s[(be1+i)%N]<<"_"<<s[(be2+i)%N]<<endl;
        if(s[(be1+i)%sl]>s[(be2+i)%sl])
            return be2;
        if(s[(be1+i)%sl]<s[(be2+i)%sl])
            return be1;
    }
    return be1;
}
    char s[N+5];
int main()
{
//    char acgt[4]="ACGT";
    int T=1;
    cin>>T;
    while(T--)
    {
        scanf("%s",s);
        int j=0;
        int be1=0,be2;

        for(int i=0;i<strlen(s);i++)//找最小的首字符给be1
        {
            if(s[i]<s[be1])
            {
                be1=i;
            }
        }
        //cout<<be1<<endl;
        for(int i=0;i<strlen(s);i++)//在最小的首字符中 找最小的字串
        {
            if(s[i]==s[be1])
            {
                if(com(s,be1,i)==i)
                   be1=i;
            }
        }
        for(int i=0;i<strlen(s);i++)
            cout<<s[(be1+i)%strlen(s)];
        cout<<endl;
    }
    return 0;
}
ACat2017/12/9
```