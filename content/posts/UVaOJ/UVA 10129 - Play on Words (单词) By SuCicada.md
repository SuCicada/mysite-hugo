>例题6-16 单词（Play On Words, UVa 10129）
输入n（n≤100000）个单词，是否可以把所有这些单词排成一个序列，使得每个单词的
第一个字母和上一个单词的最后一个字母相同（例如acm、malform、mouse）。每个单词最多包含1000个小写字母。输入中可以有重复单词。
**Sample Input**
3
2
acm
ibm
3
acm
malform
mouse
2
ok
ok
**Sample Output**
The door cannot be opened.
Ordering is possible.
The door cannot be opened.

[本家链接](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=1070)

----------------------------
将一个单词看做是首字母到尾字母的一条路，所以这条路我们只关心头尾，即首尾两个字母，不关心中间字母。
所以我们可以构造一个图，26*26的图，有向的。其中的边即是一个单词。由此我们便无需关心重复以及首尾字母相同的具体单词了，只关心具有同样首尾的单词的数量。
所以这道题就成为了：构造了一个图，是否存在欧拉回路。

存在欧拉回路条件：
1. 图连通
2. 除了起点和终点，其余每个点的出入度都必须一样。
3. 起点和终点的出入度之差为0或各为1。	

用dfs判断连通，用数组记录各个点的出入度。

-----
```cpp
#include<iostream>
#include<string>
#include<cstring>
using namespace std;

/* x --> y */
int dict[33][33];
int inDegree[33];
int outDegree[33];
int N;

int SUM = 26;

int dfs(int w){
    /* start at w */
    int road = 0;
    for(int i=0;i<SUM;i++){
        /* 找到了对应的尾字母，即存在一个单词，一条路 */
        if(dict[w][i]==1){
            dict[w][i] = 2; /* 走过了 */
            road = 1;       /* 路标记 */
            dfs(i);         /* 找下一个 */
    }
    return road;
}

int main(){
    int T;
    cin>>T;
    while(T--){
        cin>>N;
        int n=N;
        memset(dict,0,sizeof(dict));
        memset(inDegree,0,sizeof(inDegree));
        memset(outDegree,0,sizeof(outDegree));
        string s;   
        while(n--){
            cin>>s;
            int x = s[0]-'a';
            int y = s[s.size()-1]-'a';
            dict[x][y] = dict[y][x] = 1;

            outDegree[x] ++; 
            inDegree[y] ++; 
        }

        int res = 0;
        int odd = 0;
        int inOdd = 0;
        int outOdd = 0;
        int ok = 1;
        for(int i=0;i<SUM;i++){
            res += dfs('a'+i-'a');   
            if(outDegree[i]!=inDegree[i]){
                if((outDegree[i] - inDegree[i])==1){
                    outOdd ++;
                }else if((outDegree[i] - inDegree[i])==-1){
                    inOdd ++;
                }else{
                    ok = 0;
                }
            }
        }
        if(res == 1 && 
            ok == 1 &&
            ((inOdd+outOdd==0) || (inOdd==1 && outOdd==1))){
            cout<<"Ordering is possible."<<endl;
        }else{
            cout<<"The door cannot be opened."<<endl;
        }
    }
    return 0;
}

// AC at 2020/08/12
```
---

（ps：拖了实在太久了）
