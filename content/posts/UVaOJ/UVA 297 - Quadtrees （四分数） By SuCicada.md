>例题6-11 四分树（Quadtrees, UVa 297）
如图6-8所示，可以用四分树来表示一个黑白图像，方法是用根结点表示整幅图像，然
后把行列各分成两等分，按照图中的方式编号，从左到右对应4个子结点。如果某子结点对
应的区域全黑或者全白，则直接用一个黑结点或者白结点表示；如果既有黑又有白，则用一
个灰结点表示，并且为这个区域递归建树。
图6-8 四分树
给出两棵四分树的先序遍历，求二者合并之后（黑色部分合并）黑色像素的个数。p表
示中间结点，f表示黑色（full），e表示白色（empty）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728225137500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
**样例输入：**
3
ppeeefpffeefe
pefepeefe
peeef
peefe
peeef
peepefefe
**样例输出：**
There are 640 black pixels.
There are 512 black pixels.
There are 384 black pixels.

[本家](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=233)

---
构造32*32的矩阵，构建树木的时候同时给矩阵染色，由于黑色会覆盖，所以也不怕树木之间的冲突。然后最后刷一下矩阵，看看有多少个黑块块就行（即数字为1）

```cpp
#include<iostream>
#include<string>
#include<cmath>
using namespace std;


/* 共5+1层 32*32=4^5 */
int SIDE_LENGTH = 32;
int quad[100][100];
int sum = 0;
/* f > p > e */
void build(int side,int x,int y){
    char c;
    cin>>c;
    if(c=='p'){
        for(int xi=0;xi<=1;xi++){
            for(int yi=0;yi<=1;yi++){
                int nextSide = side / 2;
                int xx = x+nextSide*xi;
                int yy = y+nextSide*yi;
                build(nextSide,xx,yy);
            }                
        }
    }else if(c=='f'){
        /* fill $quad from $begin to $end */
        for(int xi=x;xi<x+side;xi++){
            for(int yi=y;yi<y+side;yi++){
                if(quad[xi][yi]==0){
                    sum++;
                    quad[xi][yi] = 1;
                }
            }   
        }            
    }
}


int main(){
    int T;
    cin>>T;
    while(T--){
        // memset(quad,0,SIDE_LENGTH*SIDE_LENGTH*sizeof(int));
        for(int i=0;i<SIDE_LENGTH;i++){
            for(int j=0;j<SIDE_LENGTH;j++){
                quad[i][j]=0;
            }
        }
        sum = 0;
        for(int i=0;i<2;i++){
            build(SIDE_LENGTH,0,0);
        }

        cout<<"There are "<<sum<<" black pixels."<<endl;
    }
    return 0;
}

// AC at 2020/07/26
```
这个题遇到了  runtime error的错误，百思不得其解，甚至都开始在oj上debug（指注释代码定位错误消失情况）。后来才发现原来是build函数，设置了int作为返回值类型在一开始，但是没有给出一个return，导致，太不谨慎了。

----
ps：This blog is to make up for yesterday before yesterday. Recently, I often feel a headache, dizziness, and sleepiness. It must be because I used to stay up late day after day.
Today I will continue to caught a AC, and then read English.
