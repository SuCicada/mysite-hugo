> 例题4-3 救济金发放（The Dole Queue, UVa 133）
n(n<20)个人站成一圈，逆时针编号为1～n。有两个官员，A从1开始逆时针数，B从n开
始顺时针数。在每一轮中，官员A数k个就停下来，官员B数m个就停下来（注意有可能两个
官员停在同一个人上）。接下来被官员选中的人（1个或者2个）离开队伍。
输入n，k，m输出每轮里被选中的人的编号（如果有两个人，先输出被A选中的）。例
如，n=10，k=4，m=3，输出为4 8, 9 5, 3 1, 2 6, 10, 7。注意：输出的每个数应当恰好占3格。（即setw(3)）
**Sample Input**
10 4 3
0 0 0
**Sample Output**
␣␣4␣␣8,␣␣9␣␣5,␣␣3␣␣1,␣␣2␣␣6,␣10,␣␣7

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=832&page=show_problem&problem=69

有一个数组来表示队列里的人，然后分别用两个循环，从头和尾开始依次过，并且记录下每一轮最后的落脚点，在下一轮中就当做是起始点了。点出来的人的位置可以直接用0代替。
```
#include<iostream>
#include<iomanip>
using namespace std;

int dole[25];
    int n,a,b;

int cycle(int j)//int n,int ab)
{
    //int j=0;
    for(int i=0;i<a;i++)
    {
        do
        {
            j++;
        if(j==n)
            j=0;
            continue;
        }
        while(dole[j]==0);
        //j++;
    }
    return j;
}
int cycle2(int j)//int n,int b)
{
    //int j=n-1;
    for(int i=0;i<b;i++)
    {
        do
        {
            j--;
        if(j<0)
            j=n-1;
            continue;
        }
        while(dole[j]==0);
    }
    //cout<<"j  "<<j<<endl;
    return j;
}
int main()
{
    while(cin>>n>>a>>b&&(n!=0||a!=0||b!=0))
    {
        int num=n;//num是还存在的人
        for(int i=0;i<n;i++)
            dole[i]=i+1;

        int tempa=-1,tempb=n;//
        while(num)
        {
            tempa = cycle(tempa);//这里用引用更方便
            tempb = cycle2(tempb);
            //cout<<tempa<<" aaa  "<<tempb<<endl;
            cout<<setw(3)<<dole[tempa];
            if(tempa != tempb)
            {
                cout<<setw(3)<<dole[tempb];
                num-=2;
            }
            else
                num--;
            dole[tempa]=dole[tempb]=0;
            if(num)
                cout<<",";
            else
                cout<<endl;
            //for(int i=0;i<n;i++)cout<<dole[i];cout<<endl;
        }
    }
    return  0;
}
//AC at 2018/2/5

```