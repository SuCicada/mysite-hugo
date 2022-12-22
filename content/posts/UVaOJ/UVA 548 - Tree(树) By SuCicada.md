>例题6-8 树（Tree, UVa 548）
给一棵点带权（权值各不相同，都是小于10000的正整数）的二叉树的中序和后序遍
历，找一个叶子使得它到根的路径上的权和最小。如果有多解，该叶子本身的权应尽量小。
输入中每两行表示一棵树，其中第一行为中序遍历，第二行为后序遍历。
**样例输入：**
3 2 1 4 5 7 6
3 1 2 5 6 7 4
7 8 11 3 5 16 12 18
8 3 11 7 16 18 12 5
255
255
**样例输出：**
1
3
255

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=489)

----
在根据中序和后序遍历进行构造的时候，同时计算叶子的权值和，比较选择最小的权值的叶子，记录最小权值和的叶子和权值和。


```cpp
#include<iostream>
#include<cstdio>
#include<sstream>
#include<cstring>
using namespace std;

int inorder[10004];
int postorder[10004];
int inindex[10004];

int min_top_value = 100000004; // 最小的权和
int loft = -1;  // 叶子本身的值,
/* 参数: 当前子树的 中序前,后 后序前,后 */
int calcul(int in_begin, int in_end, int post_begin, int post_end, int alculate_root_value){
    // in_end - in_begin == post_end - post_begin
    int root_value = postorder[post_end]; // 根, 从后序
    int root_in_index = inindex[root_value];//fin_inorder(in_begin, in_end, root_value);

    alculate_root_value = alculate_root_value + root_value; // 新的累计根值, 加上自己身上的

    if(in_end <= in_begin){ // 叶子了
        if(loft == -1 ||  // 第一个叶子
            (alculate_root_value < min_top_value) ||  
            (alculate_root_value == min_top_value && inorder[in_begin] < loft) ) {
                min_top_value = alculate_root_value;
                loft = inorder[in_begin];
            }
        return 0;
    }

    int len = root_in_index-1-in_begin;

    if(root_in_index > in_begin){
        calcul(in_begin, root_in_index-1,
                post_begin, post_begin + len, 
                alculate_root_value); // 左枝
    }

    len = in_end - root_in_index-1;
    if(root_in_index < in_end){
        calcul(root_in_index+1, in_end, 
                post_end-1-len , post_end-1, 
                alculate_root_value); // 右枝
    }

    return 0;
}

void init(){
    memset(inorder,0,sizeof(inorder));
    memset(postorder,0,sizeof(postorder));
    memset(inindex,0,sizeof(inindex));
    min_top_value = 100000004; // 最小的权和
    loft = -1;  // 叶子本身的值
}

int main()
{
    int row = 0;
    string line;
    while(getline(cin,line)){
        // cout<<n<<" ";
        stringstream ss(line);
        int i=0;
        int n;
        
        if(row==0){
            // 新的一组, 要初始化
            // 代表是一组中的第一行,即中序遍历
            init();
            while(ss>>n){
                inorder[i] = n;
                inindex[n] = i;
                i++;
            }
            row = 1;
        }else{
            // 一组中的第二行, 即后序遍历
            while(ss>>n){
                postorder[i++] = n;
            }
            row = 0;
            calcul(0,i-1,0,i-1,0);
            cout<<loft<<endl;
        }
    return 0; 
}
// AC at 2020/03/21
```
---
ps：债还清了，这个代码提交了12次数，uva因为赞助商跑路的关系（不知道也没有关系），感觉现在更加慢了。网站首页筹集代码贡献。也想做点贡献，但是C++的网站。。。
