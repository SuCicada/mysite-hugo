> 习题3-8 循环小数（Repeating Decimals, ACM/ICPC World Finals 1990, UVa202）
输入整数a和b（0≤a≤3000，1≤b≤3000），输出a/b的循环小数表示以及循环节长度。例
如a=5，b=43，小数表示为0.(116279069767441860465)，循环节长度为21。
注意：有些即便是原题也可能没用看清的要求
（如果小数位大于50括号里显示到50个小数位即可，后面加...）
（但是输出的小数位要是确实的位数，即便几百几千）

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=138&mosmsg=Submission+received+with+ID+20551105

**Sample Input**
76 25
5 43
1 397
**Sample Output**
76/25 = 3.04(0)
1 = number of digits in repeating cycle

5/43 = 0.(116279069767441860465)
21 = number of digits in repeating cycle
 
1/397 = 0.(00251889168765743073047858942065491183879093198992...)
99 = number of digits in repeating cycle
*（ps：这个案例的99就很是误导人）*


----------
思路：从手算的除法公式下手，每一次的被除数都是 上一个被除数 -- 上一位商*除数，而我们只要找到从哪里开始的被除数和之前的某一个被除数一样，那么从这一位便开始循环；如果不懂，看下面的例子：2/3
  

```
  0.6 6  //
3|2.0    //一开始不够除，补零
  1.8    //28 = 4（商）* 7（除数）
    2 0  //20 = (30（被除数）- 4（商）* 7（除数）)*10
         //开始循环
         //同时我们发现 这里的被除数20 和第二行的被除数20 一样，  
```
如果还不懂，亲自写一下除法运算，真的可以秒懂。

------
```
#include<iostream>
#include<cstring>
using namespace std;
int const N = 3000;
int divident[N+5];//被除数
int result[N+5];//得数
int circle(int nd,int n1)
{
    for(int i=0;i<nd;i++)
    {
        if(n1==divident[i])//发现循环
        {
            return i;//返回 循环体的头部位置,
        }
    }
    return -1;
}
int main()
{
    int a,b;
    while(cin>>a>>b)
    {
        int n1=a;//被除数
        int nc=-1;
        int nd=1,nr=0;//因为result第一位是整数位，所以为了输出对上 nd=1
        memset(divident,0,sizeof(divident));
        memset(result,0,sizeof(result));
        do
        {
            result[nr++]=n1/b;//cout<<n1/b;
            //cout<<n1<<" n1  "<<result[nr-1]<<endl;
            //if(nc==1)
            n1 = (n1-n1/b*b);
            n1*=10;
            divident[nd++]=n1;//将每一阶段的被除数存下
            //cout<<n1<<" "<<result[nr-1]<<endl;
            if(n1==0)//若能整除
            {
                result[nr++]=0;
                nc=nd-1;
                nd++;
                break;
            }
         }while((nc=circle(nd-1,n1))==-1);//&&nr<=50);//nc---(nd-1)循环
//         if(nr>50)
//            nc=1;
        cout<<a<<"/"<<b<<" = ";
        cout<<result[0]<<".";
        for(int i=1;i<nc;i++)
            cout<<result[i];
        cout<<"(";
        for(int i=nc;i<nd-1&&i<=50;i++)
            cout<<result[i];
        if(nr>50)
            cout<<"...";
        cout<<")"<<endl;
        cout<<"   "<<nd-nc-1//(nr<=50?nd-nc-1:99)要求算具体的
            <<" = number of digits in repeating cycle"<<endl<<endl;
    }
    return 0;
}
//AC at 2017/12/30
```
（题外话：一开始认为很难，查询资料，琢磨用辗转还是减减，后来在（未再次找到）博客上获得灵感。总计花费2个多小时，一周前就写好初始版了，之后一直忙于课设，昨天修改后始终wa，深夜难眠。今日有幸发现uva站的参考参数和参考结果 模块，甚是大喜，风吹落叶般改好了代码，果不其然，漂亮的AC。
给吾身一言，入寒假之后，还望能发出关于两个课设的程序，与uva站的使用介绍。）