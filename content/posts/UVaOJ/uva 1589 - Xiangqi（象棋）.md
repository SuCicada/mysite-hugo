> 习题4-1 象棋（Xiangqi, ACM/ICPC Fuzhou 2011, UVa1589）
考虑一个象棋残局，其中红方有n（2≤n≤7）个棋子，黑方只有一个将。红方除了有一个
帅（G）之外还有3种可能的棋子：车（R），马（H），炮（C），并且需要考虑“蹩马
腿”（如图4-4所示）与将和帅不能照面（将、帅如果同在一条直线上，中间又不隔着任何棋
子的情况下，走子的一方获胜）的规则。
输入所有棋子的位置，保证局面合法并且红方已经将军。你的任务是判断红方是否已经
把黑方将死。
**Sample Input**
2 1 4
G 10 5
R 6 4
> 
3 1 5
H 4 5
G 10 5
C 7 5
>
0 0 0
**Sample Output**
YES
NO

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=833&problem=4464&mosmsg=Submission+received+with+ID+20833399
注意：
输入的第一行第一个为红子的数量，后两个是黑将的位置。接下来的是红子类型和位置。

思路：
1、黑将有四种走法，即上下左右。我们只要判断 是否这四种走法中合理的都是死路 即可判断将是否被将死 。
2、先判断是否当前的黑将走子的位置是否合理，即当前的将子有没有超出九宫格。
3、对于车炮帅的将军，我们可以一起判断，先从将的位置开始，依次往一条路过，比如从（1，4）向（10，4）竖的过，
（1）如果路上遇到车或帅，那么就是将军了。
（2）若是非车帅的子，就计数加一。（比如我用c_iff变量记录目前非车帅的子数量）。
（3）若是炮，判断炮前是否有一个子（c_iff的值是不是一），若是，将军 。
（4）关于如何将“顺次从黑将开始分别左一行，右一行，上一列，下一列遍历格子上的子”放在一个循环条件里，见代码，我是将本应四个循环的条件写在一个循环体里用 if 处理了
4、关于马的将军，我们可以单独判断。看图,(x,y)位置为黑将，黑框位置为蹩马腿，如果此处没有子那么与其相邻的两个位置上有马的话就可以将军了。
![这里写图片描述](https://img-blog.csdn.net/20180226223117553?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VfY2ljYWRh/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
这里的技巧1：两层循环过完四个方向，见代码
技巧2：黑框位置：（x+i，y+j），与其相邻的马（x+i+i，y+j）和（x+i，y+j+j）
5、注意：还有一点陷阱，我们还要考虑一开始黑将就和红帅对面，那样的话黑将就可以直接击杀红帅。样例：
2 1 5
G 10 5
R 1 1

```
#include<iostream>
#include<cstring>
using namespace std;
char board[14][15];//9*10的棋盘
int checkmate(int rx,int ry)
{
    //cout<<"rx "<<rx<<"  ry  "<<ry<<endl;
    if((rx<1||rx>3)||(ry<4||ry>6))//出逃即越界，不合理走法
    {
        //cout<<"escape"<<endl;
        return 1;
    }

    int c_iff=0;//炮前有子加1
    int ix=-1,iy=0;
    int x=rx+ix;//!注意：一开始的位置就是将的位置，所以要先避开，同时模拟了吃子
    int y=ry;
    //int xy,rc_n=10;
//    for(;y<=9;y+=ic)
//    for(;/*x>=1&&*/x<=10;x+=ic)
    for(;y<=9;x+=ix,y+=iy)//3.(4)的技巧见33--54行
    {
        //cout<<"x "<<x<<" y "<<y<<endl;
        if(x<1)//上边过完了，就该过下边的列
            {ix=1;x=rx;c_iff=0;continue;}
        
		if(x>10)//该遍历竖列了，先左行
        {
            ix=0;//注意：接下来x就不能动了
            iy=-1;
            x=rx;
            c_iff=0;
            continue;
        }
        if(y<1)//左行遍历结束，该右行
            {iy=1;y=ry;c_iff=0;continue;}

        if(board[x][y]=='0')//没有子
            continue;
			
        //如果当前是炮或将且之前没有过 子
        if((c_iff==0&&(board[x][y]=='R'||board[x][y]=='G'))||//车决
           (board[x][y]=='C'&&c_iff==1))//炮击身亡
        {
            //cout<<board[x][y]<<"  "<<x<<" "<<y<<" dead"<<endl;
            return 1;//dead
        }
        else //若果不是炮或将，就加一
            c_iff++;
    }
//    return 0;
//}
//int horse(int rx,int ry)
//{
	//判断马子是否将军
    for(int ix=1;ix>=-1;ix-=2)
        for(int iy=1;iy>=-1;iy-=2)//用于判断四个方位
            if(((rx+ix)<1||(rx+ix)>10||(ry+iy)<1||(ry+iy)>9)==0)//不越界
                if(board[rx+ix][ry+iy]=='0')//不蹩马腿
                    if(board[rx+ix*2][ry+iy]=='H'||board[rx+ix][ry+iy*2]=='H')//那个致死位置上有马
                        return 1;//马杀
    //cout<<"safe"<<endl;
    return 0;
}
int main()
{
    int T,rx,ry;
    int cx,cy;
    char chess;
    while(cin>>T>>rx>>ry&&!(!T||!rx||!ry))
    {
        memset(board,'0',sizeof(board));
        while(T--)//摆棋子
        {
            cin>>chess;
            cin>>cx>>cy;
            board[cx][cy] = chess;
        }

        int i;
        for(i=rx+1;rx<=10&&board[i][ry]=='0';i++);//判断一开始是否将对帅，是就NO，即思路5
        if(board[i][ry]=='G'){cout<<"NO"<<endl;continue;}

        int dead_sum=0;//记录死亡路线，为4就是将死
        //cout<<"rx "<<rx<<"  ry  "<<ry<<endl;
        dead_sum += checkmate(rx+1,ry);
        dead_sum += checkmate(rx-1,ry);
        dead_sum += checkmate(rx,ry+1);
        dead_sum += checkmate(rx,ry-1);
        //cout<<"dead_sum  "<<dead_sum<<endl;
        if(dead_sum==4)
            cout<<"YES"<<endl;
        else
            cout<<"NO"<<endl;
    }
    return 0;
}
//AC at 2018/2/26

```

（题外话：终于病好了，赶紧交题，这个题比上一道用友好多了，感谢udebug，博客写的更累，头晕目眩不能再写了）




