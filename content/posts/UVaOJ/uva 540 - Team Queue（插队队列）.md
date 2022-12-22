> 例题5-6 团体队列(Team Queue,UVa540)
有t个团队的人正在排一个长队。每次新来一个人时,如果他有队友在排队,那么这个
新人会插队到最后一个队友的身后。如果没有任何一个队友排队,则他会排到长队的队尾。
输入每个团队中所有队员的编号,要求支持如下3种指令(前两种指令可以穿插进
行)。
ENQUEUEx:编号为x的人进入长队。
DEQUEUE:长队的队首出队。
STOP:停止模拟。
对于每个DEQUEUE指令,输出出队的人的编号。
**Sample Input**
2
3 101 102 103
3 201 202 203
ENQUEUE 101
ENQUEUE 201
ENQUEUE 102
ENQUEUE 202
ENQUEUE 103
ENQUEUE 203
DEQUEUE
DEQUEUE
DEQUEUE
DEQUEUE
DEQUEUE
DEQUEUE
STOP
2
5 259001 259002 259003 259004 259005
6 260001 260002 260003 260004 260005 260006
ENQUEUE 259001
ENQUEUE 260001
ENQUEUE 259002
ENQUEUE 259003
ENQUEUE 259004
ENQUEUE 259005
DEQUEUE
DEQUEUE
ENQUEUE 260002
ENQUEUE 260003
DEQUEUE
DEQUEUE
DEQUEUE
DEQUEUE
STOP
0
**Sample Output**
Scenario #1
101
102
103
201
202
203
Scenario #2
259001
259002
259003
259004
259005
260001

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=835&problem=481&mosmsg=Submission+received+with+ID+21283631

用map来映射每个人与其所在的组，组号码顺序记录
用两个队列存，一个存组的顺序。
另一个是队列数组，存的是没有顺序的组以及里面的人

```
#include <iostream>
#include <queue>
#include <map>
#include <string>
using namespace std;

int main()
{
	int T=1;
	int n; //几个组
	while(cin>>n&&n!=0)
	{
		map<int,int> group;  //每个人对应其所属的组
		for(int i=0;i<n;i++)
		{
			int m;//每组几个
			cin>>m;
			for(int j=0;j<m;j++)
			{
				int name;  //每人号码
				cin>>name;
				group[name] = i; //人的号码对应于组名
			}
		}

		string str;
		queue<int> que;//存组的顺序
		queue<int> per[1003];//存所有人的顺序,per[i]是第i组
		
		cout<<"Scenario #"<<T++<<endl;
		while(cin>>str&&str!="STOP")
		{
		//cout<<"???"<<endl;
			if(str == "ENQUEUE")
			{
				int name; 
				cin>>name;
				int qn=group[name];//组名
				if(per[qn].empty())  //说明这个组还没有在队列中
					que.push(qn); //组名入组队列
				per[qn].push(name);
			}
			else
			{
				int qn=que.front(); //获取组名
				cout<<per[qn].front()<<endl; //输出出队列的
				per[qn].pop();  //人出组
				if(per[qn].empty())//如果此组空了
					que.pop();
			}
		}
		cout<<endl;
	}
	return 0;
}
//AC at 2018/5/9
```


