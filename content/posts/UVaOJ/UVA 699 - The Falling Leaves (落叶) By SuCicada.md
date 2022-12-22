>例题6-10 下落的树叶（The Falling Leaves, UVa 699）
给一棵二叉树，每个结点都有一个水平位
置：左子结点在它左边1个单位，右子结点在右
边1个单位。从左向右输出每个水平位置的所有
结点的权值之和。如图6-7所示，从左到右的3个
位置的权和分别为7，11，3。按照递归（先序）
方式输入，用-1表示空树。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072623534815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)

**样例输入：**
5 7 -1 6 -1 -1 3 -1 -1
8 2 9 -1 -1 6 5 -1 -1 12 -1
-1 3 7 -1 -1 -1
-1
**样例输出：**
Case 1:
7 11 3
Case 2:
9 7 21 15
**【注意】** 一棵树的输[入可能分为多行。输出最多一行80个（代表树最多80列）
[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=640)

----
一边输入一边记录左右结点的值，放在数组中叠加。唯一的问题就是存在左右子树，无法确定左右范围具体为多少，所以可以采用两种方法：
1. 设定一个最大的范围，80*2-1 范围，选择最中点为root结点，然后左右放置子节点。
2. 将左右子树分开存放，右子树包含根节点，放置一个数组，下标递增代表结点向右扩增。将左子树放置另一个数组，下标递增代表结点向左扩增。如果以根节点为坐标轴0，那么左子树放置的坐标中就是反坐标轴，只是方向相反。

我采用第2种，因为一开始不知道范围（貌似）

```cpp
#include<iostream>
#include<cmath>
#include<cstdio>
#include<cstring>
using namespace std;

int leftTree[100] = {0};
int rightTree[100] = {0}; /* include root */
int leftIndex = 0;
int rightIndex = 0;


char c;
int tree(int index){
    int n;
    cin>>n;
    if(n!=-1){
        if(index<0){
            leftTree[-1-index] += n;
            leftIndex = max(leftIndex,-1-index);
        }else{
            rightTree[index] += n;
            rightIndex = max(rightIndex,index);
        }
        tree(index-1);
        tree(index+1);
    }
    return n;
}

int main(){
    int T=1;
    int n;
    do{
        leftIndex=0;
        rightIndex=0;
        memset(leftTree,0,sizeof(leftTree));
        memset(rightTree,0,sizeof(rightTree));

        n = tree(0);
        if(n!=-1){
            cout<<"Case "<<T++<<":"<<endl;
            int first = 1;
            for(int i=leftIndex;i>=0;i--){
                if(leftTree[i]){
                    if(!first){
                        cout<<" ";
                    }else{
                        first = 0;
                    }
                    cout<<leftTree[i];
                }
            }
            for(int i=0;i<=rightIndex;i++){
                if(rightTree[i]){
                    if(!first){
                        cout<<" ";
                    }else{
                        first = 0;
                    }
                    cout<<rightTree[i];
                }
            }
            cout<<endl;
            cout<<endl;
        }
    }while(n!=-1);

    return 0;
}
// AC at 2020/07/25 
```
----
ps：一开始以为输入是每一行一个树，导致耽误了很久，还一致以为刘汝佳写错了（指代码）。
