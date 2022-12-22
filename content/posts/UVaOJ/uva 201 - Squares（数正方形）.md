> 习题4-2 正方形（Squares, ACM/ICPC World Finals 1990, UVa201）
有n行n列（2≤n≤9）的小黑点，还有m条线段连接其中的一些黑点。统计这些线段连成
了多少个正方形（每种边长分别统计）。
行从上到下编号为1～n，列从左到右编号为1～n。边用H i j和V i j表示，分别代表边
(i,j)-(i,j+1)和(i,j)-(i+1,j)。如图4-5所示最左边的线段用V 1 1表示。图中包含两个边长为1的正
方形和一个边长为2的正方形。
**Sample Input**
4
16
H 1 1
H 1 3
H 2 1
H 2 2
H 2 3
H 3 2
H 4 2
H 4 3
V 1 1
V 2 1
V 2 2
V 2 3
V 3 2
V 4 1
V 4 2
V 4 3
2
3
H 1 1
H 2 1
V 2 1
**Sample Output**
Problem #1
>
2 square (s) of size 1
1 square (s) of size 2
>
\**********************************
>
Problem #2
>
No completed squares can be found.

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=833&problem=137&mosmsg=Submission+received+with+ID+20866282
（注意最后一次输出后面没有空行）
![这里写图片描述](https://img-blog.csdn.net/20180304114710953?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VfY2ljYWRh/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

思路：
先看输入的数据：
H输入的是**行列**的点往**右**连的一条边
V输入的是**列行**的点往**下**连的一条边
1、我们先用两个二维数组记录下输入的点的坐标，一个H（水平），一个V（垂直）。比如右边的点就记为1。
2、写个函数，传进参数为查找的方块的边长。
3、然后从第一个点开始遍历。判断以这个点作为左上顶点的正方形，的四条边是不是存在，即标志四条边的点的值是不是1。
4、关于大于1的边长来说，我们可以在判断时用个循环，每次循环判断的是边长中的一部分（一个单位长度）的边是不是存在。即可。
5、具体看代码吧。

```
#include<iostream>
#include<cstring>
using namespace std;
int hor[10][10],ver[10][10];
    int N;
int square(int sq_n)
{
    //cout<<sq_n<<"sq_n"<<endl;
    int sqnum=0,iff=0;
    for(int x=1;x<=N-sq_n;x++)
    {
        for(int y=1;y<=N-sq_n;y++)
        {
            //cout<<hor[x][y]<<"  "<<ver[x][y]<<endl;
            for(int i=0;i<sq_n;i++)//判断大于1边长正方形
                if(hor[x][y+i]&&hor[x+sq_n][y+i]&&
                    ver[x+i][y]&&ver[x+i][y+sq_n])
                    iff=1;//成方块

                else
                {
                    iff=0;//不成方块
                    break;
                }
            if(iff)
                sqnum++;//方块的个数
        }
    }
    return sqnum;
}
int main()
{
    int T,TN=1;
    while(cin>>N>>T)
    {
        if(TN>1)
            cout<<endl<<"**********************************"<<endl<<endl;
        cout<<"Problem #"<<TN++<<endl<<endl;

        memset(hor,0,sizeof(hor));
        memset(ver,0,sizeof(ver));
        while(T--)//记录点的信息
        {
            char hv;
            int x,y;
            cin>>hv>>x>>y;
            if(hv=='H')
                hor[x][y]=1;
            else
                ver[y][x]=1;//注意ver的是反的
        }

        int num;
        int ifn=0;
        for(int i=1;i<=N;i++)
        {
            num = square(i);
            if(num)
            {
                cout<<num<<" square (s) of size "<<i<<endl;
                ifn++;
            }
        }
        if(!ifn)
            cout<<"No completed squares can be found."<<endl;
    }
    return 0;
}
//AC at 2018/3/4

```

（题外话：到校第一道题还算顺利。祝福接下来的旅程吧。算法真害人。）