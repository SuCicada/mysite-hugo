> 例题5-5 
集合栈计算机(The
Set
Stack
Computer,ACM/ICPC
NWERC
2006,UVa12096)
有一个专门为了集合运算而设计的“集合栈”计算机。该机器有一个初始为空的栈,并且
支持以下操作。
PUSH:空集“{}”入栈。
DUP:把当前栈顶元素复制一份后再入栈。
UNION:出栈两个集合,然后把二者的并集入栈。
INTERSECT:出栈两个集合,然后把二者的交集入栈。ADD:出栈两个集合,然后把先出栈的集合加入到后出栈的集合中,把结果入栈。
每次操作后,输出栈顶集合的大小(即元素个数)。例如,栈顶元素是A={{},
{{}}},下一个元素是B={{},{{{}}}},则:
UNION操作将得到{{},{{}},{{{}}}},输出3。
INTERSECT操作将得到{{}},输出1。
ADD操作将得到{{},{{{}}},{{},{{}}}},输出3。
输入不超过2000个操作,并且保证操作均能顺利进行(不需要对空栈执行出栈操作)。
**Sample Input**
2
9
PUSH
DUP
ADD
PUSH
ADD
DUP
ADD
DUP
UNION
5
PUSH
PUSH
ADD
PUSH
INTERSECT
**Sample Output**
0
0
1
0
1
1
2
2
2
\***
0
0
1
0
0
\***

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=835&problem=3248&mosmsg=Submission+received+with+ID+21273791

性质：
１、set< int >　　代表是集合
２、idcache 是集合与其编号的映射，每一个集合都有唯一的编号。
３、setcache是向量，即下标与其存的是集合的元素相映射。
４、每一个集合都只在以上容器中存在一个。
５、栈里存的只是集合的编号。



```
#include <iostream>
#include <set>
#include <map>
#include <stack>
#include <vector>
using namespace std;

map< set<int> ,int> idcache;   //每一个集合对应一个编号
vector< set<int> > setcache;   //每一个编号对应一个集合

int getid(set<int> s)
{
   	if(idcache.count(s))
        return idcache[s];
	else 
		setcache.push_back(s);      //将集合存入vector
	idcache[s]=setcache.size()-1;  //将集合存入map，从0开始，为了让map中的键与值与vector元素与下标相对应
	return idcache[s];
}

int main()
{   
   	int T;	
	cin>>T;
	while(T--)
	{
		stack<int> s;	//只是存的集合的编号
		idcache.clear();
		setcache.clear();
		int n;
		cin>>n;
		while(n--)
		{
			

			// cout<<"n"<<n<<endl;
			string str;
			cin>>str;
			if(str=="PUSH")
			{
				set<int> temps;
				int temp=getid(temps);
				s.push(temp);
			}
			else if(str=="DUP")
			{
				s.push(s.top());
			}
			else 
			{
				set<int> first =setcache[s.top()]; //这时要取的是vetor中的元素，是set<int>类型，而s里元素是int类型
				s.pop();  
				set<int> second=setcache[s.top()]; 
				s.pop(); 
				set<int> ans;
				if(str=="INTERSECT")  
				{
					set<int>::iterator i=first.begin();
					for(;i!=first.end();i++)
					{
						set<int>::iterator j=second.begin();
						for(;j!=second.end();j++)
						{
							if(*i==*j)
								ans.insert(*i);
						}
					}
				}
				if(str=="UNION")
				{
					ans=second;    //直接赋值set<int>
					set<int>::iterator i=first.begin();
					for(;i!=first.end();i++)				
					{
						ans.insert(*i);
					}
				}
				if(str=="ADD")
				{
					ans=second;    //直接赋值set<int>
					ans.insert(getid(first)); //插入int型，是first对应的编号，将first作为了ans的一个元素
				}
				s.push(getid(ans));
			}	
			cout<<setcache[s.top()].size()<<endl;

			// cout<<str<<":"<<endl;
			// stack<int> t=s;
			// for(int i=0;i<s.size();i++)
			// {	int ii=t.top();
			// 	t.pop();
			// 	set<int>::iterator it=setcache[ii].begin();
			// 	for(;it!=setcache[ii].end();it++)
			// 		cout<< *it<<" ";
			// 	cout<<"|"<<endl;
			// }
			// cout<<endl;

		}	
		cout<<"***"<<endl;
	}
	return 0;
}
//AC at 2018/5/7


```

