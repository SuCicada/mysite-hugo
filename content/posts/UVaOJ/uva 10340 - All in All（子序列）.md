> 习题3-9 子序列（All in All, UVa 10340）
输入两个字符串s和t，判断是否可以从t中删除0个或多个字符（其他字符顺序不变），
得到字符串s。例如，abcde可以得到bce，但无法得到dc。
**Sample Input**
sequence subsequence
person compression
VERDI vivaVittorioEmanueleReDiItalia
caseDoesMatter CaseDoesMatter
**Sample Output**
Yes
No
Yes
No

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=1281&mosmsg=Submission+received+with+ID+20551262
循环大的  从第一位小的开始 判断是否匹配，若是小的下标+1，若非大的+1，继续循环。
```
#include<iostream>
#include<string>
using namespace std;
int main()
{
    string s,a;
    while(cin>>s>>a)
    {
        int sn=0;
        for(int i=0;i<a.size()&&sn<s.size();i++)
        {
            if(a[i]==s[sn])
            {
                sn++;
                //cout<<sn<<"!!"<<endl;
            }
        }
        if(sn==s.size())
            cout<<"Yes"<<endl;
        else
            cout<<"No"<<endl;
    }
    return 0;
}
//AC at 2017/12/30

```
（没想到居然一下就过了，总共十分钟？这还是在同一天的）