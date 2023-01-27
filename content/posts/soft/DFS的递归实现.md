对一个例子的理解，以下是关系图的关系矩阵，求从1开始能到哪个数
```
#include<iostream>
#include<queue>
# e<cstdio>
#include<vector>
#include<stack>
using namespace std;
int const N =5;
int maze[N][N]={
    {0,1,1,0,0},
    {0,0,1,0,1},
    {0,0,1,0,0},
    {1,1,0,0,1},
    {0,0,1,0,0}
};
int visit[N+1]={0};
//循环的思路：
//用递归，一旦有更深的点就产生新的循环，当深的点操作完了，就结束了这个陷得最深的循环。
void dfs1(int start)
{
    visit[start]=1;
    for(int i=1;i<=N;i++)
    {
        if(!visit[i]&&maze[start-1][i-1]==1)
        {
            dfs1 (i);
        }
    }
    cout<<start<<" ";
}
void dfs2(int start)
{
    stack<int>s;
    s.push(start);
    visit[start]=1;
    int iff=0;
    while(!s.empty())
    {
        iff=0;
        int v=s.top();
        for(int i=1;i<=N;i++)
        {
            if(visit[i]==0&&maze[v-1][i-1]==1)
            {
                iff=1;//代表if了
                visit[i]=1;//第一层
                s.push(i);
                break;//保证当前只走一条路
            }
        }
            if(iff==0)//到头不if才执行
            {
                cout<<v<<" ";
                s.pop();
            }
    }

}
int main()
{
    dfs2(1);
//    for(int i=1;i<=N;i++)
//    {
//        if(visit[i]==1)
//            continue;
//        dfs1(i);
//
//    }
    return 0;
}

```