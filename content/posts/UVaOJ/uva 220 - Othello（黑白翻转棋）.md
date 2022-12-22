> 习题4-3 黑白棋（Othello, ACM/ICPC World Finals 1992, UVa220）
你的任务是模拟黑白棋游戏的进程。黑白棋的规则为：黑白双方轮流放棋子，每次必须
让新放的棋子“夹住”至少一枚对方棋子，然后把所有被新放棋子“夹住”的对方棋子替换成己
方棋子。一段连续（横、竖或者斜向）的同色棋子被“夹住”的条件是两端都是对方棋子（不
能是空位）。如图4-6（a）所示，白棋有6个合法操作，分别为(2,3),(3,3),(3,5), (6,2),(7,3),
(7,4)。选择在(7,3)放白棋后变成如图4-6（b）所示效果（注意有竖向和斜向的共两枚黑棋变
白）。注意(4,6)的黑色棋子虽然被夹住，但不是被新放的棋子夹住，因此不变白。
![这里写图片描述](https://img-blog.csdn.net/20180415191510220?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VfY2ljYWRh/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
（a） （b）
图4-6 黑白棋
输入一个8*8的棋盘以及当前下一次操作的游戏者，处理3种指令：
**L**指令打印所有合法操作，按照从上到下，从左到右的顺序排列（没有合法操作时输出No legal move）。
**Mrc**指令放一枚棋子在(r,c)。如果当前游戏者没有合法操作，则是先切换游戏者再操作。输入保证这个操作是合法的。输出操作完毕后黑白方的棋子总数。
**Q**指令退出游戏，并打印当前棋盘（格式同输入）。
**Sample Input**
2
\--------
\--------
\--------
---WB---
\---BW---
\--------
\--------
\--------
W
L
M35
L
Q
WWWWB---
WWWB----
WWB-----
WB------
\--------
\--------
\--------
\--------
B
L
M25
L
Q
**Sample Output**
(3,5) (4,6) (5,3) (6,4)
Black - 1 White - 4
(3,4) (3,6) (5,6)
\--------
\--------
----W---
---WW---
---BW---
\--------
\--------
\--------
No legal move.
Black - 3 White - 12
(3,5)
WWWWB---
WWWWW---
WWB-----
WB------
\--------
\--------
\--------
\--------

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=833&problem=156&mosmsg=Submission+received+with+ID+20901787

挺复杂的题，题目就比较难弄懂，要不是我会玩黑白翻转棋（笑），可能就很难办了吧。
思路：
1、用一个二维数组存储棋盘的状态，
2、通过一个函数来翻转棋子，传进去的两个参数是落子的坐标

```
#include<iostream>
#include<iomanip>
using namespace std;
char board[10][10];
int yesnum=0;//输出空格的格式
char disk,nodisk;//要走的棋，被翻的棋
//int exceed(int i,int j)//判断越界
//{
//    if(i>=1&&i<=8&&j>=1&&j<=8)
//        return 1;
//    else
//        return 0;
//}
int reverse(int i,int j,int con)//(i,j)上应该是空的
{
    for(int in=-1;in<=1;in++)//八个方位的循环
    {
        for(int jn=-1;jn<=1;jn++)//八个方位的循环用于翻棋子
        {
            if(in==0&&jn==0) continue;//中间的棋不用管它先
//            if(i+in>=1&&i+in<=8&&j+jn>=1&&j+jn<=8&&/*exceed(i+in,j+jn)&&*/
//               board[i+in][j+jn]==nodisk)//可翻  //board[i+in][j+jn]=='-')//有空位能放子
//            {
                //cout<<"board  "<<board[i+in][j+jn]<<endl;
            int n=1;//用于能翻的子的包的界限标志（说不懂，见下面第一个if）
            while((i+in*n>=1&&i+in*n<=8&&j+jn*n>=1&&j+jn*n<=8)&&/*exceed(i-in*n,j-jn*n)*///判断越界
                  board[i+in*n][j+jn*n]==nodisk)//还能继续翻
            {
                n++;
            }
            if(board[i+in*n][j+jn*n]==disk&&n>1)//包住了，就翻这么多
            {
                //cout<<i+in*n<<"_"<<j+jn*n<<"_"<<board[i+in*n][j+jn*n]<<" "<<endl;
                if(con=='L')
                {
                    if(yesnum>0) cout<<" ";
                    cout<<"("<<i<<","<<j<<")";
                    yesnum++;
                    return 0;
                }
                else
                {
                    for(int n2=0;n2<n;n2++)//翻转操作，--n为了不bang掉最后的disk
                    {
                        board[i+in*n2][j+jn*n2]=disk;
                    }
                }
                //break;
            }
            //}
        }
    }
}

int main()
{
    int T;
    cin>>T;
    while(T--)
    {
        for(int i=1;i<=8;i++)//存表
            for(int j=1;j<=8;j++)
                cin>>board[i][j];
        char con;//下一个子，操作
        cin>>disk;
        int Wnum=0,Bnum=0;
        while(cin>>con&&con!='Q')
        {
            if(disk=='W') nodisk = 'B';
            else nodisk = 'W';
            //cout<<"disk "<<disk<<endl;
            if(con=='L')//显示下一步可走子
            {
                yesnum=0;
                for(int i=1;i<=8;i++)//全地图遍历
                {
                    for(int j=1;j<=8;j++)//全地图遍历
                    {
                        if(board[i][j]=='-')//如果可以翻
                        {
                            //cout<<disk<<"  "<<i<<" ij "<<j<<endl;
                            reverse(i,j,'L');
                        }
                    }
                }
                if(!yesnum)
                {
                    cout<<"No legal move.";
                    if(disk=='W') disk='B';
                    else disk = 'W';
                }
                cout<<endl;
            }
            else
            {
                int x;
                cin>>x;
                reverse(x/10,x%10,'M');
                Bnum=Wnum=0;
                for(int i=1;i<=8;i++)
                    for(int j=1;j<=8;j++)
                        if(board[i][j]=='B')
                            Bnum++;
                        else if(board[i][j]=='W')
                            Wnum++;
                cout<<"Black -"<<setw(3)<<Bnum<<" White -"<<setw(3)<<Wnum<<endl;

                if(disk=='W') disk='B';
                else disk = 'W';
            }

        }
        for(int i=1;i<=8;i++)
        {
            for(int j=1;j<=8;j++)
                cout<<board[i][j];
            cout<<endl;
        }
        if(T>=1)
            cout<<endl;
    }
    return 0;
}
//AC at 2018/3/9

```
-------
（题外话：这个题一个月多前就通过了，但是一直拖到现在才写博客，我都忘了当时遇到的困难和当时的思路了。那时是因为要准备蓝桥杯，所以就没再做这本书上的题了，这一个月里发生了不少事，团队天梯赛，蓝桥杯，清明，英语活动。
又去了两趟医院去看眼睛，对于要靠电脑吃饭的人来说，眼睛是绝对不能出问题的，但是啊哎，好希望若要研究程序的话，能抛开肉体的限制就好了。存在于网络上的文化乐园也岌岌可危，我无能为力，我要争取机会去往乐园能深深扎根的地方）