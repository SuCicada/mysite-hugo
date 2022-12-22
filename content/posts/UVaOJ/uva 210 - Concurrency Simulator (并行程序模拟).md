> 例题6-1 并行程序模拟（ Concurrency Simulator, ACM/ICPC World Finals 1991,
UVa210）
你的任务是模拟n个程序（ 按输入顺序编号为1～ n） 的并行执行。 每个程序包含不超过
25条语句， 格式一共有5种： var = constant（ 赋值） ； print var（ 打印） ； lock； unlock； end。
变量用单个小写字母表示， 初始为0， 为所有程序公有（ 因此在一个程序里对某个变量
赋值可能会影响另一个程序） 。 常数是小于100的非负整数。
每个时刻只能有一个程序处于运行态， 其他程序均处于等待态。 上述5种语句分别需
要t1、 t2、 t3、 t4、 t5单位时间。 运行态的程序每次最多运行Q个单位时间（ 称为配额） 。 当
一个程序的配额用完之后， 把当前语句（ 如果存在） 执行完之后该程序会被插入一个等待队
列中， 然后处理器从队首取出一个程序继续执行。 初始等待队列包含按输入顺序排列的各个
程序， 但由于lock/unlock语句的出现， 这个顺序可能会改变。
lock的作用是申请对所有变量的独占访问。 lock和unlock总是成对出现， 并且不会嵌套。
lock总是在unlock的前面。 当一个程序成功执行完lock指令之后， 其他程序一旦试图执行lock
指令， 就会马上被放到一个所谓的阻止队列的尾部（ 没有用完的配额就浪费了） 。 当unlock
执行完毕后， 阻止队列的第一个程序进入等待队列的首部。
输入n, t1, t2, t3, t4, t5, Q以及n个程序， 按照时间顺序输出所有print语句的程序编号和结
果。

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=146

// 当前程序执行完,配额时间有剩余: 让下一个程序继续用
// 当配额时间用完,当前程序一句代码没执行完: 继续执行到完
就按照题目思路往下写出来的，但是我不知道为什么我写了这么多行。不想缩减了，

```
#include<iostream>
#include<vector>
#include<string>
#include<deque>
#include<map>
#include<cstdio>
using namespace std;



// 当前程序执行完,配额时间有剩余: 让下一个程序继续用
// 当配额时间用完,当前程序一句代码没执行完: 继续执行到完
// 多组输入
// 每组输出之间空一行
// 所有变量初值为0
// 最后一组的输出之后不要有空行

map<char, int> vars;
vector<deque<string> > group;
deque<int> stop;
deque<int> wait;

void fun(){
    int N,t1,t2,t3,t4,t5,Q;
    cin>>N>>t1>>t2>>t3>>t4>>t5>>Q;
    cin.get(); // [注意] 接收空格
    group = vector<deque<string> >(N);
    for(int i=0;i<N;i++){
        string str;
        while(str != "end"){
            getline(cin, str);
            group[i].push_back(str); 
        }
        wait.push_back(i);
    }

    int index = 0; //当前第几个程序
    int count_live = N;
    int lock = 0;  // 作为临界资源的记录型信号量,代表阻塞程序个数
    int part_time = 0; // 单位剩余时间
    while(count_live){

        int end_flag = 0; // 0:程序没结束, 1:程序end结束 -1:程序阻塞
        index = wait[0]; // 读取等待队列首
        wait.pop_front();

        part_time = Q;
        while(part_time>0){  // 一个单位时间剩余的时间
            string code = group[index][0]; // 读取程序队列首代码

            string judge = code.substr(0,2);
            if(judge == "pr"){ // print 
                cout<<index+1<<": "<< vars[code[6]]<<endl;
                part_time -= t2;

            }else if(judge == "lo"){ // lock
                if(lock){  // 要阻塞,语句不执行,不记时
                    stop.push_back(index);
                    end_flag = -1;
                    part_time = 0; // 此次时间片废了
                    break; // 进入阻止队列,就不用进入等待队列了, 而且lock代码还需要执行
                }else{    // 正常执行
                    lock++;
                    part_time -= t3;
                }

            }else if(judge == "un"){ // unlock
                int pro = stop[0];
                if(!stop.empty()){ // 若是最后一个解锁的,那么阻塞队列就是空
                    stop.pop_front();
                    wait.push_front(pro);
                }
                lock--;
                part_time -= t4;

            }else if(judge == "en"){ // end
                count_live --;
                part_time -= t5;
                end_flag = 1;
                break;  // 结束本程序,时间片给下一个

            }else{  // var
                int n;
                sscanf(code.substr(4).c_str(), "%d",&n);
                vars[judge[0]] = n;
                part_time -= t1;
            }

            group[index].pop_front();  // 执行成功,代码出队列
        }
        if(end_flag == 0)
            wait.push_back(index);  // 时间片完,只要程序没有end, 就会入wait队列

    }
}


int main()
{
    int T;
    cin>>T;
    for(int i=0;i<T;i++){
        if(i>0)
            cout<<endl;
        vars.clear();
        group.clear();
        stop.clear();
        wait.clear();
        fun();
    }

    
    return 0;
}

// AC at 2019/2/8 00:25
// spend about 4 hours

```

