>例题6-15 给任务排序（Ordering Tasks, UVa 10305）
假设有n个变量，还有m个二元组(u, v)，分别表示变量u小于v。那么，所有变量从小到
大排列起来应该是什么样子的呢？例如，有4个变量a, b, c, d，若已知a < b，c < b，d < c，则
这4个变量的排序可能是a < d < c < b。尽管还有其他可能（如d < a < c < b），你只需找出其
中一个即可。
**Sample Input**
5 4
1 2
2 3
1 3
1 5
0 0
**Sample Output**
1 4 2 5 3

[本家连接](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=1246)

----

dfs(n) 从n开始找上级
循环找上级
    如果没有上级，break
    如果有上级，
        if 上级 is execute: continue
        else if 上级 not execute. then dfs(上级),
end loop
exec n

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;

/* x --> y */
int table[111][111];
int book[111];
int N,M;
int space;
int loop(int n){
    // cout<<"loop "<<n<<" "<<book[n]<<endl;
    if(!book[n]){
        /* n has not executed */
        for(int i=1;i<=N;i++){
            /* 循环找上级 */
            if(table[i][n]){
                /* 有上级 */
                if(book[i]){
                    /* 上级运行过了 */
                    continue;
                }else{
                    /*　上级没过　*/
                    loop(i);
                }
            }
        }

        if(space){
            cout<<" ";
        }
        space = 1;
        cout<<n;
        book[n] = 1;
    }
    return 0;
}

int main(){
    while(1){
        // if(scanf("%d%d",&a,&b)
        cin>>N>>M;
        space = 0;
        if(N==0 && M==0){
            break;
        }
        memset(table,0,sizeof(table));
        memset(book,0,sizeof(book));
        while(M--){
            int a,b;
            cin>>a>>b;
            table[a][b] = 1;
        }

        // for(int i=1;i<=N;i++){
        //     for(int j=1;j<=N;j++){
        //         cout<<table[i][j]<<" ";
        //     }
        //     cout<<endl;
        // }

        for(int i=1;i<=N;i++){
            loop(i);
        }
        cout<<endl;

    }
    return 0;
}

// AC at 2020/08/10
```

----
ps：想记一点日记，就来写道题吧。
周日去参加miku线下应援，太(｡･∀･)ﾉﾞ嗨，太感动了，这等经历定会回味良久。当晚与同行之人会餐，上海的学生，大城市的还是不一样的。
积攒半月之久的压力与烦恼一扫而光。非常非常非常感动。谢谢谢谢谢谢谢谢。
想组装一台台式机。
