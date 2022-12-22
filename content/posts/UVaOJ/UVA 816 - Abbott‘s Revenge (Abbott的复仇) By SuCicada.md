>例题6-14 Abbott的复仇（Abbott's Revenge, ACM/ICPC World Finals 2000, UVa 816）
有一个最多包含9*9个交叉点的迷宫。输入起点、离开起点时的朝向和终点，求一条最
短路（多解时任意输出一个即可）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803232356961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
图6-14 迷宫及走向
这个迷宫的特殊之处在于：进入一个交叉点
的方向（用NEWS这4个字母分别表示北东西
南，即上右左下）不同，允许出去的方向也不
同。例如，1 2 WLF NR ER *表示交叉点(1,2)
（上数第1行，左数第2列）有3个路标（字
符“*”只是结束标志），如果进入该交叉点时的
朝向为W（即朝左），则可以左转（L）或者直
行（F）；如果进入时朝向为N或者E则只能右转
（R），如图6-14所示。
注意：初始状态是“刚刚离开入口”，所以即
使出口和入口重合，最短路也不为空。例如，图
6-14中的一条最短路为(3,1) (2,1) (1,1) (1,2)
(2,2) (2,3) (1,3) (1,2) (1,1) (2,1) (2,2) (1,2) (1,3) (2,3) (3,3)。
**Sample Input**
SAMPLE
3 1 N 3 3
1 1 WL NR *
1 2 WLF NR ER *
1 3 NL ER *
2 1 SL WR NF *
2 2 SL WF ELF *
2 3 SFR EL *
0
NOSOLUTION
3 1 N 3 2
1 1 WL NR *
1 2 NL ER *
2 1 SL WR NFR *
2 2 SR EL *
0
END
**Sample Output**
SAMPLE
(3,1) (2,1) (1,1) (1,2) (2,2) (2,3) (1,3) (1,2) (1,1) (2,1)
(2,2) (1,2) (1,3) (2,3) (3,3)
NOSOLUTION
No Solution Possible

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=757)

---
思路和刘汝佳的如出一辙。


```cpp
#include<iostream>
#include<string>    
#include<cstdio>
#include<cstring>
#include<sstream>
#include<queue>
#include<vector>
using namespace std;

class Site{
public: 
    int x,y,inDir;
    Site(){}
    Site(int x,int y,int i):x(x),y(y),inDir(i){}
    friend ostream & operator<<(ostream &out, Site &site){
        out<<site.x<<" "<<site.y<<" "<<site.inDir;
        return out;
    }

};
/* 9*9*4(direction)*4(direction) */
/*         |             ^       */
/*         |-------------|       */
int maz[11][11][6][6]; 

/* 9*9*4(direction)*3(x,y,dir) */
/* log[i][j][k] 值 site, 记录 i,y, dir(进入方向) 点由site*/
Site log[11][11][6];
int dis[11][11][6];

/* up, right, down, left */
char dirMap[4] = {'N','E','S','W'};
int getInDirMapIndex(char c){
    /* 进入方向  */
    for(int i=0;i<4;i++){
        if(dirMap[i] == c){
            return i;
        }
    }
    return -1;
}

/*
in L  R
0  3  1
1  0  2
2  1  3
3  2  0
*/
int getOutDirMapIndex(int inIndex,char dir){
    /* 出向方向 */
    if(dir == 'F'){
        return inIndex;
    }else if(dir == 'L'){
        return (inIndex-1+4)%4;
    }else if(dir == 'R'){
        return (inIndex +1)%4;
    }
    return -1;
}
void show(){
    cout<<" N \nW E\n S "<<endl;
    cout<<" ";
    for(int i=1;i<=9;i++){
        cout<<" NESW";
    }
    cout<<endl;
    for(int i=1;i<=9;i++){
        for(int k=0;k<4;k++){     
            cout<<dirMap[k];          
            for(int j=1;j<=9;j++){
                cout<<" ";
                for(int p=0;p<4;p++){
                    cout<<maz[i][j][k][p];
                }
            }
            cout<<endl;    
        }
        cout<<endl; 
    }
}

void addRule(int x,int y,string rule){
    for(int i=1;i<rule.size();i++){
        /* x,y 位置的 rule[0] -> rule[i] 走向的规则建立了  */
        int inIndex = getInDirMapIndex(rule[0]); 
        int outIndex = getOutDirMapIndex(inIndex, rule[i]);
        maz[x][y][inIndex][outIndex] = 1; 
    }
}
int beginX,beginY;
char beginDir;
int endX,endY;


void showLog(){
    for(int i=1;i<=3;i++){
        for(int k=0;k<4;k++){
            for(int j=1;j<=3;j++){
                Site site = log[i][j][k];
                cout<<site.x<<","<<site.y<<" "<<site.inDir<<" | ";
            }cout<<endl;
        }cout<<endl;
    }cout<<endl;
}
void showDis(){
    for(int i=1;i<=3;i++){
        for(int j=1;j<=3;j++){
            for(int k=0;k<4;k++){
                int n = dis[i][j][k];
                cout<<n<<" ";
            }cout<<" | ";
        }cout<<endl;
    }cout<<endl;
}
Site getNextSite(int x,int y,int outDir){
    /*   x  y 
    n 0 -1  0 
    e 1  0  1 
    s 2  1  0 
    w 3  0 -1 
    */
    return Site(
        x+(outDir%2?0:outDir-1),
        y+(outDir%2?2-outDir:0),
        outDir);
}
void bfs(){
    queue<Site> que;
    int beginDirN = getInDirMapIndex(beginDir);
    Site beginSite(beginX,beginY,beginDirN);
    Site nextSite = getNextSite(beginX,beginY,beginDirN);
    que.push(nextSite);
    log[nextSite.x][nextSite.y][nextSite.inDir] = beginSite;
    dis[nextSite.x][nextSite.y][nextSite.inDir] = 0; /* 一步跨到 */

    int isSolu=0;
    int overDir;
    while(!que.empty()){
        Site site = que.front();
        que.pop();/* 当前块块 */
        int x = site.x;
        int y = site.y;
        int inDir = site.inDir;
        if(x == endX && y==endY){
            isSolu = 1;
            overDir = inDir;
            break;
        }

        for(int i=0;i<4;i++){
            if(maz[x][y][inDir][i]==1){
                /* 这个出方向 能走 */
                Site newSite = getNextSite(x,y,i);

                /* 下一步必须是一个新步才行 */
                Site newLogSite = log[newSite.x][newSite.y][newSite.inDir];
                if(newSite.x >=1 && newSite.x <=9 && newSite.y >= 1 && newSite.y <=9 &&
                dis[newSite.x][newSite.y][newSite.inDir] == -1 /* 未被走过 */
                    ){

                    /* 记录从哪里过来的 */
                    log[newSite.x][newSite.y][newSite.inDir] = site; 

                    /* 记录距离 */
                    dis[newSite.x][newSite.y][newSite.inDir] = dis[x][y][inDir] + 1;

                    que.push(newSite);
                }
            }
        }
    }
    if(isSolu){
        vector<Site> res;
        Site lastSite = Site(endX,endY,overDir);
        while(1){
            res.push_back(lastSite);
            if(!dis[lastSite.x][lastSite.y][lastSite.inDir]){
                break;
            }
            lastSite = log[lastSite.x][lastSite.y][lastSite.inDir];
        }
        res.push_back(Site(beginX,beginY,beginDirN));
        int sum = 0;
        for(int i=res.size()-1;i>=0;i--){
            if(sum % 10 ==0){
                if(sum){
                    cout<<endl;
                }
                cout<<" ";
            }
            printf(" (%d,%d)",res[i].x,res[i].y);
            sum++;
        }
        cout<<endl;
    }else{
        cout<<"  No Solution Possible"<<endl;;
    }
}

int main(){
    while(1){
        memset(maz,0,sizeof(maz));
        memset(log,0,sizeof(log));
        memset(dis,-1,sizeof(dis));
        beginX = beginY = endX = endY = 0;
        string name;
        cin>>name;
        if(name == "END"){
            break;
        }
        cin>>beginX>>beginY>>beginDir>>endX>>endY;
        while(1){
            /* 一个点 */
            int x,y;
            cin>>x;
            if(x==0){
                break;
            }
            cin>>y;
            while(1){
                /* 一个规则 */
                string rule;
                cin>>rule;
                if(rule=="*"){
                    break;
                }
                addRule(x,y,rule);
            }
        }
        cout<<name<<endl;
        bfs();
    }
    return 0;
}

// AC at 2020/08/02
```
--------
ps：太傻了，居然想要手动实现队列和栈，然后还给写错了，本地调试没有任何问题。但是一提交就是WA。昨天的一个下午，要疯了。
最近台风要来了，云都被吹散了在晚上，月亮特别圆特别亮特别好看。
