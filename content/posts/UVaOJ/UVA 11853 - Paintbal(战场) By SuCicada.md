>例题6-22 战场（Paintball, UVa 11853）
有一个1000×1000的正方形战场，战场西南角的坐标为(0,0)，西北角的坐标为(0,1000)。
战场上有n（0≤n≤1000）个敌人，第i个敌人的坐标为(xi
,yi
)，攻击范围为ri。为了避开敌人的
攻击，在任意时刻，你与每个敌人的距离都必须严格大于它的攻击范围。你的任务是从战场
的西边（x=0的某个点）进入，东边（x=1000的某个点）离开。如果有多个位置可以进/出，
你应当求出最靠北的位置。输入每个敌人的xi、yi、ri，输出进入战场和离开战场的坐标。
**Sample Input**
3
500 500 499
0 0 999
1000 1000 200
**Sample Output**
0.00 1000.00 1000.00 800.00

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=2953)

---

关键在于抓住问题的本质，即问题的核心，即问题去除了表面保护层之下的那个最纯粹的那个命脉。
初看，我们发现，这是要在坐标图上画圆啊。过分可怕。
但是我们鼓起勇气再细看问题：
    求最靠上的进入战场和离开战场的坐标。

解剖一下：
1. 能否离开
2. 若能，最上
3. 最左边进，最后边出。

然后我们随便画几个图来看看。

怎么样就不能离开：
    当有一条敌人从最上一直连通到最下。
若能，最靠上的地方是哪里：
    如果我们最左边进入的地方的下方有一条敌线一直连通到最上边。
    那么这条敌线就把我们包住了。
    所以 敌线（从上方延伸 and 延伸到左边），就不能被我们在其上方进入。

所以我们只要求敌线的连通性和敌线范围（从上方开始的）在最左边和最右边的最下方坐标即可。




```cpp
#include<iostream>
#include<map>
#include<cmath>
#include<cstring>
#include<vector>
#include<queue>
#include<cstdio>
using namespace std;

/*
关键在于抓住问题的本质，即问题的核心，即问题去除了表面保护层之下的那个最纯粹的那个命脉。
初看，我们发现，这是要在坐标图上画圆啊。过分可怕。
但是我们鼓起勇气再细看问题：
    求最靠上的进入战场和离开战场的坐标。

解剖一下：
1. 能否离开
2. 若能，最上
3. 最左边进，最后边出。

然后我们随便画几个图来看看。

怎么样就不能离开：
    当有一条敌人从最上一直连通到最下。
若能，最靠上的地方是哪里：
    如果我们最左边进入的地方的下方有一条敌线一直连通到最上边。
    那么这条敌线就把我们包住了。
    所以 敌线（从上方延伸 and 延伸到左边），就不能被我们在其上方进入。

所以我们只要求敌线的连通性和敌线范围（从上方开始的）在最左边和最右边的最下方坐标即可。



*/


/* 地图 */
int site[1003][1003];

/* 下标就是这个敌人的标号 */
vector<int> xList;
vector<int> yList;
vector<int> range;

/* 走过的标记 */
int book[1003];

/* 敌人的关系图 */
int table[1003][1003];

/* 
    map[x][y] 记录 敌人标号
    table[x][y] 标号对应

    从上边找起, bfs, 若能达到最下边 -> 不可达, break;
                   若不能达到最下边 -> continue
                                    -> 若全部不能到下边 -> 左可达右
                                    -> 在bfs中记录达到的左边的最下部, 以及右边的最下部, 获取走

*/
int N;
int cover(int a, int b){
    return pow(xList[a]-xList[b],2) + pow(yList[a]-yList[b],2) <= pow(range[a]+range[b],2);
}

double getlen(int index,int lr){
    int x;
    if(lr==0){
        x = xList[index];
    }else if(lr == 1){
        x = 1000-xList[index];
    }
    return sqrt(pow(range[index],2) - pow(x,2));
}
/* 左边最低 */
double left_lowest = 1000;
/* 右边最低 */
double right_lowest = 1000;

int bfs(int index){


    /* 到下边 */
    int pass = 1;

    queue<int> temp;
    temp.push(index);
    while(!temp.empty()){
        int n = temp.front();temp.pop();

        book[n] = 1;

        for(int i=0;i<N;i++){
            /* 圆接触的 , 同时没走过的 */
            if(table[n][i] && book[i]==0){
                temp.push(i);
            }
        }

        /* if 接触下边 */
        if(yList[n]-range[n]<=0){
            pass = 0;
            break;
        }
        /* if 接触左边 */
        if(xList[n]-range[n]<=0){
            left_lowest = min(left_lowest, yList[n]-getlen(n,0));
        }
        /* if 接触右边 */
        if(xList[n]+range[n]>=1000){
            right_lowest = min(right_lowest, yList[n]-getlen(n,1));
        }
    }
    return pass;
    
}

int main(){
    int T;
    while(cin>>T){
        memset(site,-1,sizeof(site));
        memset(book,0,sizeof(book));
        memset(table,0,sizeof(table));
        xList.clear();
        yList.clear();
        range.clear();
        /* 左边最低 */
        left_lowest = 1000;
        /* 右边最低 */
        right_lowest = 1000;

        N = T;
        while(T--){
            int a,b,c;
            cin>>a>>b>>c;
            int index = xList.size();
            xList.push_back(a);
            yList.push_back(b);
            range.push_back(c);
            site[a][b] = index;   
        }

        for(int i=0;i<N;i++){
            for(int j=i+1;j<N;j++){
                if(cover(i,j)){
                    table[i][j] = 1;
                    table[j][i] = 1;
                }
            }
        }

        /* 从最上面开始, (0,1000) -- (1000,1000)  */
        int pass = 1;
        for(int i=0;i<N;i++){
            if(yList[i]+range[i]>=1000){
                pass = bfs(i);
                if(pass==0){
                    break;
                }
            }
        }

        if(pass==0){
            cout<<"IMPOSSIBLE"<<endl;
        }else{
            printf("0.00 %.2lf 1000.00 %.2lf\n",left_lowest,right_lowest);
        }
    return 0;
}

// AC at 2020/09/26 
```

---
---
ps：当然，这篇也求助了刘汝佳，我真是没用。
至此第六章的例题就结束了（终于）。可以开始下一章的例题了（苦笑）
暴力求解，应该能简单屑吧（并不）。
只要够暴力就行了。

至此拖更了足足40多天的题目blog就都更完了。皆大欢喜。然而今天已经10.11了。拖延症已经到了如此地步。

这两天状态很不好，喜怒无常。难以调动自己的积极情绪。
我想我应该换一个平台来记。我应该制定规划。
感觉不经意之间又被现实裹挟了。我应该充分拥抱我所喜爱之物，我应该充满活力。
室友不在的几天，在黄昏，在夜晚，感到了空廖。就像许久之前。
但我翻开那时的记录，泥泞之至让现在的我感到窒息。所以我就在想，我好不容易脱离那泥潭，我又为何要再入，亦或是说我此刻又陷入了另一个泥潭。
人的本质还是想要群居的。一个人久就会开始恐慌。所以我们需要陪伴之物，不会抛弃不会背叛我们之物。之前我选择的是算法和程序。除此之外还需要一些其他的，但是我一直抓不住这其他的是什么。
有人说，这最好的陪伴之物是人。但那是最终极的成就，我等尚仅可奢望。
`
