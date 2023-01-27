控制台运行输入数据格式  ： xxx（程序名） 3（汉诺塔层数）
会显示出每一步的移动步骤，以及每一柱上留有的盘数
```
#include<iostream>
#include<sstream>
using namespace std;
int a=0,b=0,c=0;
void hno(int n,char from,char mid,char to)
{
    if(n>1)
    {
        hno(n-1,from,to,mid);
        hno(1,from,mid,to);
        hno(n-1,mid,from,to);
    }
    else
    {
        switch(from)
        {
            case 'A':a--;break;
            case 'B':b--;break;
            case 'C':c--;break;
        }
        switch(to)
        {
            case 'A':a++;break;
            case 'B':b++;break;
            case 'C':c++;break;
        }
        cout<<from<<"-->"<<to<<"   A:"<<a<<" B:"<<b<<" C:"<<c<<endl;
    }
}
int main(int argc,char *argv[])
{
//    cout<<argc<<endl;
//    cout<<"_"<<argv[1]<<"_"<<endl;
//    return 0;
    stringstream s;
    int n;//=*argv[1];
    s<<argv[1];
    s>>n;
    //cin>>n;
    a=n;
    //hno(n,)
    hno(n,'A','B','C');
    return 0;
}

```