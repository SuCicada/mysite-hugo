> 习题3-4 周期串（Periodic Strings, UVa455）
如果一个字符串可以由某个长度为k的字符串重复多次得到，则称该串以k为周期。例
如，abcabcabcabc以3为周期（注意，它也以6和12为周期）。
输入一个长度不超过80的字符串，输出其最小周期。

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=396&mosmsg=Submission+received+with+ID+20473859
```
//先找到和第一个元素相等的下一个元素，然后第一个元素位和找到的元素位依次往后过，
//一直到结束，若一直一样则周期为找到的元素位。
//比如ababcababc，
//1.s[0]先找到了和s[1]相等,然后分析，
//2.s[0+1]和s[1+1]比，相等。
//3.s[0+1+1]再和s[1+1+1],不等，跳出
//4.s[0]再和s[2],s[3],s[4]比。。。

```
---
```

#include<iostream>
#include<cstdio>
#include<string.h>
using namespace std;
const int N = 80;
char s[N+5];
int cir(int i)
{
    int n=strlen(s);
    if(n%i!=0)//防止aba或abca输出2或3而不是3或4的情况
        return 0;
    for(int j=i;j<strlen(s);j++)//顺次比较前后字串相等否
    {
        if(s[j-i]!=s[j])
        {
            return 0;
        }
    }
    return i;
}
int main ()
{
    int tt;
    scanf("%d",&tt);
    while(tt--)
    {
        scanf("\n%s",s);
        int T=0;//周期
        int i,n=strlen(s);
        for(i=1;i<n;i++)
        {
            if(s[0]==s[i])
            {
                if((T=cir(i))==i)
                {
                    break;
                }
            }
        }
        cout<<i<<endl;
        if(tt!=0)
            cout<<endl;
    }
    return 0;
}
//AC at 2017/12/11

```
---
几周前就看见了这道题，寻思良久不得要领，本以为很难，今日侥幸做出。
一开始一直WA。感谢http://bbs.csdn.net/topics/391840422的一楼小子。原来这种做法有陷阱。
所谓缘分做题不过与此。