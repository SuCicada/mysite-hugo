>例题6-18 雕塑（Sculpture, ACM/ICPC NWERC 2008, UVa12171）
某雕塑由n（n≤50）个边平行于坐标轴的长方体组成。每个长方体用6个整
数x0，y0，z0，x，y，z表示（均为1～500的整数），其中x0为长方体的顶点中x坐标的最小
值，x表示长方体在x方向的总长度。其他4个值类似定义。你的任务是统计这个雕像的体积
和表面积。注意，雕塑内部可能会有密闭的空间，其体积应计算在总体积中，但从“外部”看
不见的面不应计入表面积。雕塑可能会由多个连通块组成。
**Sample Input**
2
2
1 2 3 3 4 5
6 2 3 3 4 5
7
1 1 1 5 5 1
1 1 10 5 5 1
1 1 2 1 4 8
2 1 2 4 1 8
5 2 2 1 4 8
1 5 2 4 1 8
3 3 4 1 1 1
**Sample Output**
188 120
250 250

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=3323)

---
关键点：
1. 构造三维数组space来模拟雕像涂色。
2. 我们只从空白处开始遍历联通部分，最后使用总体积和面积减去空白部分的。
3. 但是不能使用500\*500*500的数组，太大。采用离散化。即将雕像分组，因为最多只有50个方块，所以每个方向最多只会有100个分组。
4. 使用map来存储坐标值与space中的方位的对应关系。、
5. 之后使用常规bfs方式进行方块遍历。不要用dfs，栈溢出。

---


```cpp
#include<iostream>
#include<vector>
#include<set>
#include<map>
#include<algorithm>
#include<cstring>
#include<queue>
using namespace std;

class Cube{
public:
    int xyz[3];
    int sss[3];
};


vector<Cube> cubeList;
/* 
site[0] => x
site[1] => y
site[2] => z

每个map键为坐标原始值，对应的值为space中的坐标值对应的单元格的下标
map<value, index>
通过 对应的坐标点的值去map里得到 space的位置

*/
map<int,int> site[3];

/* 每个方向上的原始坐标值,用vector存是为了更快的遍历 */
vector<int> value[3];

/* 因为最多只有50个方块，即每个方向100个边，即最多每个方向会分为100个不定大小的单元格 
按照单元格（不定大小）进行分组，存储单元格信息（染色与否） */
int space[105][105][105];

queue<Cube> temp;

int getSetIndex(int xyz, int value){
    return site[xyz][value];
}


long V; // 空白的体积
long S; // 空白的表面积(带外围)
int XX,YY,ZZ; // 单元格的每个方向上的数量,即space尺寸


int check(int x,int y,int z){
    if(x>=0&&x<XX && y>=0&&y<YY && z>=0&&z<ZZ){
        if(space[x][y][z]==1){
            return 1;
        }else if(space[x][y][z]==0){
            Cube c;
            c.xyz[0]=x;
            c.xyz[1]=y;
            c.xyz[2]=z;
            temp.push(c);
        }
        return 0;
    }else{
        return -1;
    }
}
int bfs(int x,int y,int z){
    Cube c;
    c.xyz[0]=x;
    c.xyz[1]=y;
    c.xyz[2]=z;
    temp.push(c);
    while(!temp.empty()){
        c = temp.front();temp.pop();
        x = c.xyz[0];
        y = c.xyz[1];
        z = c.xyz[2];
        if(x>=0&&x<XX && y>=0&&y<YY && z>=0&&z<ZZ){
            if(space[x][y][z]==0){
                space[x][y][z] = 2; /* 走过了 */
                
                int xs = value[0][x+1] - value[0][x];
                int ys = value[1][y+1] - value[1][y];
                int zs = value[2][z+1] - value[2][z];

                V += xs * ys * zs;
                int res;

                res = check(x-1,y,z); S = S + res * ys*zs; // cout<<1<<" "<<res<<" "<<V<<" "<<S<<endl;
                res = check(x+1,y,z); S = S + res * ys*zs; // cout<<2<<" "<<res<<" "<<V<<" "<<S<<endl;
                res = check(x,y-1,z); S = S + res * xs*zs; // cout<<3<<" "<<res<<" "<<V<<" "<<S<<endl;
                res = check(x,y+1,z); S = S + res * xs*zs; // cout<<4<<" "<<res<<" "<<V<<" "<<S<<endl;
                res = check(x,y,z-1); S = S + res * xs*ys; // cout<<5<<" "<<res<<" "<<V<<" "<<S<<endl;
                res = check(x,y,z+1); S = S + res * xs*ys; // cout<<6<<" "<<res<<" "<<V<<" "<<S<<endl;
            }   
        }
    }
    return 0;
}
int run(int x,int y,int z){
    return bfs(x,y,z);
}
void clac(){
    /* 从六个面开始过 */
    for(int i=0;i<XX;i++){
        for(int j=0;j<YY;j++){
            run(i,j,0);
            run(i,j,ZZ-1);
        }
    }
    for(int i=0;i<XX;i++){
        for(int k=0;k<ZZ;k++){
            run(i,0,k);
            run(i,YY-1,k);
        }
    }   
    for(int j=0;j<YY;j++){
        for(int k=0;k<ZZ;k++){
            run(0,j,k);
            run(XX-1,j,k);
        }
    }
    
    int sumX,sumY,sumZ;
    if(XX<0) sumX = 0; else sumX = value[0][XX]-value[0][0]; 
    if(YY<0) sumY = 0; else sumY = value[1][YY]-value[1][0]; 
    if(ZZ<0) sumZ = 0; else sumZ = value[2][ZZ]-value[2][0]; 
    
    int sumS = (sumX * sumY + sumX * sumZ + sumY * sumZ) * 2;
    int sumV = sumX * sumY * sumZ;

    cout<<sumS + S<<" "<<sumV-V<<endl;
}

void init(){
    V=S=XX=YY=ZZ=0;
    cubeList.clear();
    for(int i=0;i<3;i++){
        site[i].clear();
        value[i].clear();
    }
    memset(space,0,sizeof(space));
}

int main(){
    int T;
    cin>>T;
    while(T--){
        init(); 
        int N;
        cin>>N;
        while(N-- > 0){
            Cube b;
            int x,y,z,xs,ys,zs;
            cin>>x>>y>>z>>xs>>ys>>zs;
            b.xyz[0] = x;
            b.xyz[1] = y;
            b.xyz[2] = z;
            b.sss[0] = xs;
            b.sss[1] = ys;
            b.sss[2] = zs;

            /* 存map */
            site[0][x] = 0;
            site[1][y] = 0;
            site[2][z] = 0;
            site[0][x+xs] = 0;
            site[1][y+ys] = 0;
            site[2][z+zs] = 0;

            /* 存cube list */
            cubeList.push_back(b);
        }


        /* map 赋下标, 因为先存到了map中,所以作为键的坐标值就已经是排好序的了 */
        for(int i=0;i<3;i++){
            map<int,int>::iterator iter;
            /* 第一面的方块用空白预先填充, 即用空白包裹雕塑一半 */
            int n = 0;
            for(iter=site[i].begin(); iter!=site[i].end(); iter++){
                /* index -> value */
                value[i].push_back(iter->first);
                /* value -> index */
                iter->second = n;
                n++;
            }
        }

        /* 单元格数, 比点数少一 */
        XX = value[0].size()-1;
        YY = value[1].size()-1;
        ZZ = value[2].size()-1;

        /* space 涂色*/
        for(int i=0;i<cubeList.size();i++){
            /* 记录xyz 的开始和结束 的下标 */
            int start[3];
            int end[3];
            for(int ii=0;ii<3;ii++){
                start[ii] = getSetIndex(ii, cubeList[i].xyz[ii]);
                end[ii]   = getSetIndex(ii, cubeList[i].xyz[ii] + cubeList[i].sss[ii]);
            }

            /* 涂色 */
            for(int xi=start[0]; xi<end[0]; xi++){
                for(int yi=start[1]; yi<end[1]; yi++){
                    for(int zi=start[2]; zi<end[2]; zi++){
                        space[xi][yi][zi] = 1;
                    }
                }
            }
        }
        clac();
    }
    return 0;
}

// AC at 2020/09/12
```

---
ps：这道题十分美妙。辗转波折，所幸做出，十分感谢。
