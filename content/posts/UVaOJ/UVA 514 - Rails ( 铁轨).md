> 例题6-2 铁轨（Rails, ACM/ICPC CERC 1997, UVa 514）
某城市有一个火车站，铁轨铺设如图6-1所示。 有n节车厢从A方向驶入车站，按进站顺
序编号为1～n。 你的任务是判断是否能让它们按照某种特定的顺序进入B方向的铁轨并驶出
车站。 例如，出栈顺序(5 4 1 2 3)是不可能的，但(5 4 3 2 1)是可能的。
为了重组车厢，你可以借助中转站C。 这是一个可以停放任意多节车厢的车站，但由于
末端封顶，驶入C的车厢必须按照相反的顺序驶出C。 对于每个车厢，一旦从A移入C，就不
能再回到A了；一旦从C移入B，就不能回到C了。 换句话说，在任意时刻，只有两种选择：
A→C和C→B。
**Sample Input**
5
1 2 3 4 5
5 4 1 2 3
0
6
6 5 4 3 2 1
0
0
**Sample Output**
Yes
No
\
Yes

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=455

和书上的思路一样
车进入是先从1到n
我们每进入一辆,就判断一下是不是要出去的那一辆, 如果是,这辆车就走了,然后判断一下下一辆
如果不是就进入中转轨道中等的
```
#include<iostream>
#include<stack>
using namespace std;


// [注意] 最后一组输出之后要有一个空行


int fun(int N){
    stack<int> wait;  // 等待通过的火车
    stack<int> temp;  // 中转轨道
    for(int i=N;i>=1;i--){
        wait.push(i);
    }
    
    int train;
    int no = 0; // 不能吗
    for(int i=1;i<=N;i++){
        cin>>train;
        if(train == 0)
            return 0; // 该退出了
        if(no == 1)
            continue;
    // cout<<train<<" ??????????????"<<endl;
    // cout<<"wait: "; show(wait);cout<<"temp: "; show(temp);cout<<"---------------"<<endl;
        while(temp.empty() || temp.top() != train){

            if(wait.empty()){
                no = 1; // wait空了,中转的第一个却不能走,所以不行
                break;
            }

            int out = wait.top(); // 车厢出等待区
            wait.pop();

            temp.push(out);  // 车厢 进入中转区
        }
        if(temp.top() == train)
            temp.pop();  // 中转的车走了
    }
    if(temp.empty() && wait.empty()){
        cout<<"Yes"<<endl;
    }else{
        cout<<"No"<<endl;
    }
    return 1;
}

int main()
{
    int N;
    int index = 0;
    while(cin>>N && N != 0){
        // if(index != 0)
        index = 1;
        while(fun(N)!=0);
        cout<<endl;
    }
    return 0;
}

// AC at 2019/2/8 13:41
// spend about 1 hours
```
---------------------------
已经过了5天了有什么感想我也想不起来了,见鬼
