>例题6-12 油田（Oil Deposits, UVa 572）
输入一个m行n列的字符矩阵，统计字符“@”组成多少个八连块。如果两个字符“@”所在
的格子相邻（横、竖或者对角线方向），就说它们属于同一个八连块。例如，图6-9中有两
个八连块。
**Sample Input**
1 1
\*
3 5
*@*@*
**@**
*@*@*
1 8
@@****@*
5 5
****@
*@@*@
*@**@
@@@*@
@@**@
0 0
**Sample Output**
0
1
2
2

[本家连接](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=513)

----
 恩，嘛，遍历跑呗。反正到处都有。

```cpp
#include<iostream>
using namespace std;

char oil[110][110];

int sum=0;
int length,width;
int isOil=0;

void show(){
    for(int i=0;i<length;i++){
        for(int j=0;j<width;j++)
            cout<<oil[i][j];
        cout<<endl;
    }
    // cout<<"========================"<<endl;
    cout<<endl;
}
int dfs(int x,int y){
    char plot = oil[x][y];
    if(x<0 || x>=length || y<0 || y>=width || plot!='@'){
        return 0;
    }else if(plot=='@'){
    // cout<<x<<" -------------------- "<<y<<endl;
    // show();
        oil[x][y] = '-';
        if(!isOil){
            sum++;
            isOil = 1;
        }
        dfs(x-1,y-1);
        dfs(x-1,y);
        dfs(x-1,y+1);
        dfs(x,y-1);
        dfs(x,y+1);
        dfs(x+1,y-1);
        dfs(x+1,y);
        dfs(x+1,y+1);
    // show();
    //     cout<<x<<"  "<<y<<" res "<<res<<endl;

        return 1;
    }
}

int main(){
    while(1){
        sum = 0;
        isOil = 0;
        cin>>length>>width;
        // cout<<length<<" "<<width<<endl;
        if(length==0 && width==0){
            break;
        }

        for(int i=0;i<length;i++){
            for(int j=0;j<width;j++){
                cin>>oil[i][j];
            }
        }

        for(int i=0;i<length;i++){
            for(int j=0;j<width;j++){
                dfs(i,j);
                isOil = 0;
            }
        }


        cout<<sum<<endl;

    }
    return 0;
}

// AC at 2020/07/28
```
