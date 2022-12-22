>例题5-9 数据库（Database，ACM/ICPC NEERC 2009，UVa1592）
输入一个n行m列的数据库（1≤n≤10000，1≤i≤10），是否存在两个不同行r1，r2和两个
不同列c1，c2，使得这两行和这两列相同（即（r1，c1）和（r2，c1）相同，（r1，c2）和
（r2，c2）相同）。例如，对于如图5-3所示的数据库，第2、3行和第2、3列满足要求。
**Sample Input**
3 3
How to compete in ACM ICPC,Peter,peter@neerc.ifmo.ru
How to win ACM ICPC,Michael,michael@neerc.ifmo.ru
Notes from ACM ICPC champion,Michael,michael@neerc.ifmo.ru
2 3
1,Peter,peter@neerc.ifmo.ru
2,Michael,michael@neerc.ifmo.ru
**Sample Output**
NO
2 3
2 3
YES

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=835&page=show_problem&problem=4467)

----
设计存储的结构
```
[
    {
        (c1.value, c2.value) -> r0.index
    }
]
```
列表中存储每一行中列列的组合（map存储）。列表的大小会是数据库的列*(列-1)/2。map的键是一个存有2个元素的列表（列表中存的是列值），map的值是对应的行下标。map的大小为总行数。

存储：
我们在存储时，针对每一行进行列的两两组合，然后对每一个组合在当前列组合下标中进行map查找，如果总排列数过完之后，有2个map匹配项，那我们就找到了。

在这里也用到了字符串索引存储的方式。类比指针，我们对每一个字符串给定一个编号，然后存在数据库中的就只是这个编号，而不用存字符串，节省了很大空间。

---
```cpp
#include<iostream>
#include<map>
#include<string>
#include<vector>
#include<cstring>
#include<cstdio>
#include<set>
using namespace std;

/* 
[
    {
        (c1.value, c2.value) -> r0.index
    }
]
*/
vector< map< vector<int>,int> > parquet;
map<string, int> dict;

/* temp of one row */
vector<int> rowTemp;

int X,Y;

char tmp[99];

int getIndexOfStr(string str){
    if(!dict.count(str)){
        dict[str] = dict.size();
    }
    return dict[str];    
}

int calc(int row,int* col1,int* col2){
    vector<int> tuple2;
    int tuple2_index = 0;
    for(int i=0;i<Y;i++){
        for(int j=i+1;j<Y;j++){

            tuple2.clear();
            tuple2.push_back(rowTemp[i]);
            tuple2.push_back(rowTemp[j]);

            // cout<<i<<" "<<j<<" "<<tuple2_index<<endl;
            if(parquet[tuple2_index].count(tuple2)){
                int row1 = parquet[tuple2_index][tuple2];
                *col1 = i;
                *col2 = j;
                return row1;
            }else{
                parquet[tuple2_index][tuple2] = row;
            }
            tuple2_index++;
        }
    }
    return -1;
}

int main(){
    int a,b;
    while(cin>>a>>b){
        getchar();
        X=a,Y=b;
        /* 新的一组 */
        /* init vector */
        parquet.clear();
        for(int i=0;i<Y*(Y-1)/2;i++){
            map<vector<int>, int> tempMap;
            parquet.push_back(tempMap);
        }
        
        /* 过一行中的一列 */
        int right=0;
        int row1,row2;
        int col1,col2;
        for(int row=0;row<a;row++){
            /* 新的一行开始了 */
            rowTemp.clear();
            char c;
            int i=0,col=0;
            while((c=getchar()) &&c !='\n' && c!=EOF){
                if(c==','){
                    string s(tmp,0,i);
                    rowTemp.push_back(getIndexOfStr(s));
                    
                    col++;
                    i=0;
                }else{
                    tmp[i]=c;
                    i++;
                }
            }
            string s(tmp,0,i);
            rowTemp.push_back(getIndexOfStr(s));

            /* 存入新的一行中的2列组合 */
            if(right==0){
                int ccol1,ccol2;
                int rrow1 = calc(row,&ccol1,&ccol2);
                if(rrow1 != -1){
                    right = 1;
                    col1 = ccol1+1;
                    col2 = ccol2+1;
                    row1 = rrow1+1;
                    row2 = row+1;
                }
            }
            /* 这一行过完了 */
        }
        /* 这一组过完了 */
        if(right){
            cout<<"NO"<<endl;
            cout<<row1<<" "<<row2<<endl;
            cout<<col1<<" "<<col2<<endl;
        }else{
            cout<<"YES"<<endl;
        }
    }
    return 0;
}

// AC at 2020/09/08
```

---
ps：这一道题重点在于设计数据库存储方式上，我居然最终求助于刘汝佳，真是太逊了。
