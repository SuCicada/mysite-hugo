> 习题3-5 谜题（Puzzle, ACM/ICPC World Finals 1993, UVa227）
有一个5*5的网格，其中恰好有一个格子是空的，其他格子各有一个字母。一共有4种指
令：A, B, L, R，分别表示把空格上、下、左、右的相邻字母移到空格中。输入初始网格和指
令序列（以数字0结束），输出指令执行完毕后的网格。如果有非法指令，应输出“This
puzzle has no final configuration.
还有：输入的迷宫以大写 Z 结束。输出的行与行间要有一行空行
**Sample Input**
TRGSJ
XDOKI
M VLN
WPABE
UQHCF
ARRBBL0
ABCDE
FGHIJ
KLMNO
PQRS 
TUVWX
AAA
LLLL0
ABCDE
FGHIJ
KLMNO
PQRS 
TUVWX
AAAAABBRRRLL0
Z

**Sample Output**
Puzzle #1:
T R G S J
X O K L I
M D V B N
W P A E
U Q H C F

Puzzle #2:
A B C D
F G H I E
K L M N J
P Q R S O
T U V W X

Puzzle #3:
This puzzle has no final configuration.
（注意：例子中有PQRS的那一行后是有空格的，在用样例测试时注意不要丢失）
https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=163&mosmsg=Submission+received+with+ID+20487026

```
//思路没什么难的，通过判断指令的字符是什么，来替换空格用它上下左右的元素。
//主要是逻辑上容易出错，还有就是一些容易忽视的小陷阱。
//要注意的都写在了注释里
```


```
#include<iostream>
#include<cstdio>
using namespace std;
void dis(char puzz[5][5])//为了在测试时方便观看过程写了函数
{
    for(int i=0;i<5;i++)
    {
        for(int j=0;j<4;j++)
            cout<<puzz[i][j]<<" ";//注意每行最后一个字符后不能有空格
        cout<<puzz[i][4];
        cout<<endl;
    }

}
int main()
{
    char puzz[5][5];
    char mov;
    int N=1;//次数
    int si,sj;//空格坐标
    while(gets(puzz[0])&&puzz[0][0]!='Z')//先接收第一行，以便好判断Z
    {
        //cout<<puzz[0][0]<<endl;
        int iff=1;//0为越出数组网格
//        gets(puzz[0]);
//        if(puzz[0][0]=='Z')//判断可以写在里面
//            break;
        for(int i=0;i<5;i++)
        {
            if(i>0)
                gets(puzz[i]);
            //scanf("%[^\n]",puzz[i]);
            for(int j=0;j<5;j++)
            {
                //cout<<puzz[i][j];
                if(puzz[i][j]==' ')
                {
                    si=i;
                    sj=j;
                }
            }
            //cout<<endl;
        }
        //dis(puzz);cout<<si<<"  "<<sj<<endl;
        while((mov=getchar())!='0')
        {
            switch(mov)
            {
                case 'L':
                    if(sj-1<0){iff=0;break;}
                    puzz[si][sj]=puzz[si][sj-1];//让移动位代替空格即可，不必将移动位换成空格
                    sj--;
                    break;
                case 'R':
                    if(sj+1>4){iff=0;break;}
                    puzz[si][sj]=puzz[si][sj+1];
                    sj++;
                    break;
                case 'A':
                    if(si-1<0){iff=0;break;}
            //cout<<"___A"<<endl;
                    puzz[si][sj]=puzz[si-1][sj];
                    si--;
                    break;
                case 'B':
                    if(si+1>4){iff=0;break;}
                    puzz[si][sj]=puzz[si+1][sj];
                    si++;
                    break;
            }
            //if(iff==0) break;
        }
        if(N!=1)//采用先换行再输出，所以第一次输出前不能有空行
            cout<<endl;
        printf("Puzzle #%d:\n",N++);
        if(iff==0)
        {
            cout<<"This puzzle has no final configuration."<<endl;
        }
        else
        {
            puzz[si][sj]=' ';
            dis(puzz);
        }
        getchar();//回收0后的换行符
        //cout<<"end"<<endl;
    }
    return 0;
}
//AC at 2017/12/14


```
ps:写一篇文章平均要花去20+分钟时间