>例题6-19 自组合（Self-Assembly, ACM/ICPC World Finals 2013, UVa 1572）
有n（n≤40000）种边上带标号的正方形。每条边上的标号要么为一个大写字母后面跟着一个加号或减号，要么为数字00。当且仅当两条边的字母相同且符号相反时，两条边能拼在一起（00不能和任何边拼在一起，包括另一条标号为00的边）。
假设输入的每种正方形都有无穷多种，而且可以旋转和翻转，你的任务是判断能否组成一个无限大的结构。每条边要么悬空（不和任何边相邻），要么和一个上述可拼接的边相邻。如图6-17（a）所示是3个正方形，图6-17（b）所示边是它们组成的一个合法结构（但大小有限）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201009213522450.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70#pic_center)
**Sample Input**
3
A+00A+A+ 00B+D+A- B-C+00C+
1
K+K-Q+Q
**Sample Output**
bounded
unbounded

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=4364)

---
提炼一下关键点：一共53种符号。方块数目很大，构造传统二维图会过大而超时。方块内部符号已知连通性。求方块间符号连通性。

可以抽象为52个点（00去掉），已知连通性（方块内部，不互补符号），互补符号可连通，当一段方块能够重复连接，即能够无限，即在可能的通路中求是否存在环路。

所以解决方案：使用dfs的方式，从一个符号开始，然后循环找方块内部的道路，找到之后，再递归走向下一个与其互补的符号。然后记录每一次递归通过的符号，即结点，当某一次递归走到了已经走过的符号结点上，那么就存在重复道路，即环路了。

具体看注释

---
```cpp
#include<iostream>
#include<string>
#include<cstring>

using namespace std;

/*  x -> y  : inner       */
/*  y -> x  : outer( + -) */
// /*  A+B+C+D+...A-B-C-D- */
/*  A+A-B+B-C+C-D+D-... */
int table[55][55];
/*  1: can go 
    2: has gone
    用于减少计算数 
*/
int look[55];
/* 作为标记，锁一类。作为当前一次dfs中,这个点是否已经走过了
    用于判断环路存在
 */
int run[55];

int getIndex(char a,char b){
    return (a-'A')*2 + (b=='-'?1:0);
}

void show();


/* 考虑什么时候有回路
    从一个口子出去, 经过几圈之后, 从另一个口子进来, 在内部又到了这个口子, 即重复路
*/
int calc(int n){
    if(run[n]==1){
        return 1;
    }

    /* n开始了 */
    run[n]=1;

    /* n没有进过 */
    if(look[n]==1){
        for(int i=0;i<52;i++){
            /* 循环找内部路 */
            if(table[n][i]==1) { /* 能走 */
                /* 找到了, i为出口,即互补符号 */
                int nextIn = i-1*(i%2*2-1);
                if(calc(nextIn)){
                    return 1;
                }
            }
        }
        look[n] = 2;
    }
    run[n] = 0;

    return 0;
}

int main(){
    int T;
    while(cin>>T){
        memset(table,0,sizeof(table));
        memset(look,0,sizeof(look));
        memset(run,0,sizeof(run));

        string str;
        while(T--){
            cin>>str;
            for(int i=0;i<8;i+=2){
                if(str[i]!='0'){
                    int xi = getIndex(str[i],str[i+1]);
                    look[xi]=1;
                    for(int j=0;j<8;j+=2){
                        if(i!=j &&  str[j] !='0'){
                            int yi = getIndex(str[j],str[j+1]);
                            table[xi][yi]=1;
                        }        
                    }
                }
            }
        }
        int yes=0;
        for(int i=0;i<52;i++){
            if(look[i]==1){
                int res = calc(i);
                if(res==1){
                    yes = 1;
                    break;
                }
            }
        }
        
        if(yes){
            cout<<"unbounded"<<endl;
        }else{
            cout<<"bounded"<<endl;
        }
    
    }
    return 0;
}

// AC at 2020/09/13
```

---
ps：有时候写一种方法写不下去了，思路纠结住了，怎么改怎么不对，那么就抛弃这个扭曲的函数，重新开始整理思路，写一个新的同样功能的函数。
这个题也求助于刘汝佳了，这个星期的状态实在太糟糕了。
