> 给出一个由O和X组成的串（长度为1～80），统计得分。每个O的得分为目前连续出现
的O的个数，X的得分为0。例如，OOXXOXXOOO的得分为1+2+0+0+1+0+0+1+2+3。
**Sample Input**
5
OOXXOXXOOO
OOXXOOXXOO
OXOXOXOXOXOXOX
OOOOOOOOOO
OOOOXOOOOXOOOOX
**Sample Output**
10
9
7
55
30

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=4460&mosmsg=Submission+received+with+ID+20465648
```
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
        int sum=0;
        int o=1;
        int c;
        while((c=getchar())!='\n')
        {
            if(c=='O')
            {
                sum+=o;
                o++;
            }
            if(c=='X')
            {
                o=1;
            }
        }
        cout<<sum<<endl;
    }
    return 0;
}
//AC at 2017/12/9

```