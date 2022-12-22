>例题6-17 看图写树（Undraw the Trees, UVa 10562）
你的任务是将多叉树转化为括号表示法。如图6-16所示，每个结点用除了“-”、“|”和空格
的其他字符表示，每个非叶结点的正下方总会有一个“|”字符，然后下方是一排“-”字符，恰
好覆盖所有子结点的上方。单独的一行“#”为数据结束标记。

**Sample Input**
<pre>

2
    A
    |
--------
B   C  D
    |  |
 ----- -
 E   F G
#
e
|
----
f g
#
</pre>
**Sample Output**
(A(B()C(E()F())D(G())))
(e(f()g()))

---
[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=1503)

---

先记录树，然后从头开始递归遍历树，并且构造先序遍历字串。
非叶子结点字母下方必有竖线。竖线下方必有若干横线，横线下方必有若干字母，即子节点。

----
```cpp
#include<iostream>
#include<string>
#include<cstdio>
#include<cstring>
using namespace std;

char tree[222][222];


int isRightChar(char c){
    return c && c!='-' && c!='|' && c!=' ' && c!='#';
}

int loop(int x,int y){
    cout<<tree[x][y];
    cout<<"(";
    if(tree[x+1][y]!='|'){
        return 0;
    }else{
        int a;
        for(a=y;tree[x+2][a]=='-';a--){}
        a++;    
        int b;  
        for(b=y;tree[x+2][b]=='-';b++){}
        b--;

        for(int i=a;i<=b;i++){
            if(isRightChar(tree[x+3][i])){
                loop(x+3,i);
                cout<<")";
            }
        }
    }
    return 1;
}


int main(){
    int T;
    cin>>T;
    getchar();
    while(T--){
        memset(tree,0,sizeof(tree));
        int line=0;
        int row=0;
        while(1){
            string s;
            getline(cin,s);
            row=0;
            if(s.size()==1 && s[0]=='#'){
                break;
            }else{
                for(int i=0;i<s.size();i++){
                    tree[line][row++] = s[i];
                }
                line++;
            }
        }

        cout<<"(";
        for(int i=0;i<200;i++){
            if(isRightChar(tree[0][i])){
                loop(0,i);
                cout<<")";
                break;
            }
        }
        cout<<")"<<endl;


    }
    return 0;
}

// AC at 2020/08/16
```
