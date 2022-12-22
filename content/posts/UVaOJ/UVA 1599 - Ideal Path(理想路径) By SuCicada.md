>例题6-20 理想路径（Ideal Path, NEERC 2010, UVa1599）
给一个n个点m条边（2≤n≤100000，1≤m≤200000）的无向图，每条边上都涂有一种颜
色。求从结点1到结点n的一条路径，使得经过的边数尽量少，在此前提下，经过边的颜色序
列的字典序最小。一对结点间可能有多条边，一条边可能连接两个相同结点。输入保证结点
1可以达到结点n。颜色为1～10
9的整数。
**Sample Input**
4 6
1 2 1
1 3 2
3 4 3
2 3 1
2 4 4
3 1 1
**Sample Output**
2
1 3

[本家链接](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=4474)

---

   先找最短路：倒序从终点bfs找。
        - 在此认为倒序和正序效果一样。只是某些处理逻辑相反。
    bfs之后，可能存在多条最短路，所以再需要一次bfs，来选出颜色最小的路。

降低时间的注意点：
1. 选择合适的数据结构存储图。   一开始使用了map作为存储图的结构，后来在遍历过程中实在比vector慢了太多。
2. 使用标记记录走过的结点，减少bfs重复计算。

对了2:00 - 2:30 期间uva oj 判题特别慢特别慢。尽量避开。
```cpp
#include<iostream>
#include<vector>
#include<map>
#include<queue>
#include<cmath>
#include<cstring>
#include<set>
using namespace std;

#define mapiter map<int,int>::iterator 
const int MAX_N = 100005;


/* 
    先找最短路：倒序从终点bfs找。
        - 在此认为倒序和正序效果一样。只是某些处理逻辑相反。
    bfs之后，可能存在多条最短路，所以再需要一次bfs，来选出颜色最小的路。

    一开始使用了map作为存储图的结构，后来在遍历过程中实在比vector慢了太多。
        
*/

/* a->b  
    map[a][b]  == color
*/
class Node{
public:
    int next;
    int color;
};
// map<int,int>  door[MAX_N];
vector<Node> door[MAX_N]; /* 存储无向图，因为结点太多了 */
int book[MAX_N];          /* 记录每个点距离终点`的最小距离 */
int visit[MAX_N];         /* 记录走过的点 */
int big = (1L<<31)-1;
int res[MAX_N];
int N,M;

/* n下各点 到n 的距离取最短+1 
    door[a][b] a -> b
    log[b] = min(log[b], log[a]+1)

*/
void bfs(){
    queue<int> tmp;
    tmp.push(N);
    visit[N] = 1;
    while(!tmp.empty()){
        int a = tmp.front();
        tmp.pop();
        if(a==1){ /* 到头了 */
            break;
        }

        for(int i=0;i<door[a].size();i++){
            int b = door[a][i].next;
            if(book[b] > book[a]+1){
                book[b] = book[a]+1;
            }
            if(!visit[b]){
                visit[b] = 1;
                tmp.push(b);
            }
        }
    }
}
void calc(){
    memset(visit, 0, sizeof(visit));
    set<int> tmp;
    int resN = book[1];
    tmp.insert(1);
    while(!tmp.empty()){
        vector<int> tmpV;
        int a;
        int min_color = big;
        int min_b = big;
        int n;
        /* 同color同距离的 一层 , 不同的位置即a, 
            要找出最小的 color和对应的 位置
        */
        for(set<int>::iterator it = tmp.begin();it!=tmp.end();it++){
            a = *it; // 这个的位置
            n = book[a]; // 这一个的距离到n
            /* 扫相连, 找下一跳, && 字典序小 */
            for(int i=0;i<door[a].size();i++){
                int b = door[a][i].next;
                int color = door[a][i].color;
                if(book[b]==n-1){
                    min_b = b;
                    if( min_color > color){
                        min_color = color;
                        tmpV.clear();
                        tmpV.push_back(b);
                    }else if(min_color == color){
                        tmpV.push_back(b);// 相同的color位置存
                    }
                }
            }
        }
        // 得到了这一层最小的color对应的位置: tmp
        if(n==0) break;
        // 这个位置虽然下一跳不确定,但是color确定了, 存下
        res[n] = min(res[n],min_color); 
        tmp.clear();
        // 开始遍历color相同的位置
        for(int i=0;i<tmpV.size();i++){
            // 不走重复的愿望路是非常重要的
            if(visit[index]==0){
                visit[index]=1;
                tmp.insert(index);
            }
        }
    }
}

int main(){
    int n,m;
    while(cin>>n>>m){
        for(int i=1;i<=n;i++){
            door[i].clear();
            book[i]= big;
            res[i] = big;
            visit[i] = 0;
        }        
        N = n;
        M = m;
        for(int i=0;i<m;i++){
            int a,b,c;
            cin>>a>>b>>c;
            Node n1;
            n1.next = b;
            n1.color = c;
            Node n2;
            n2.next = a;
            n2.color = c;
            door[a].push_back(n1);
            door[b].push_back(n2);
        }
        book[n] = 0;
        bfs();
        calc();

        cout<<book[1]<<endl;
        for(int i=book[1];i>=1;i--){
            cout<<res[i];
            if(i!=1){
                cout<<" ";
            }
        }
        cout<<endl;
    }
    return 0;
}

// AC at 2020/09/16
```
---
ps：这道题提交了太多次，做了很多天，一度绝望，一直超时。熬夜一天废一周。所以要保证良好的精神状态和心态再来做题。警惕陷入负面漩涡之中。
