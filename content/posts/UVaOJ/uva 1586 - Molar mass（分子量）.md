> 给出一种物质的分子式（不带括号），求分子量。本题中的分子式只包含4种原子，分
别为C, H, O, N，原子量分别为12.01, 1.008, 16.00, 14.01（单位：g/mol）。例如，C6H5OH的
分子量为94.108g/mol。
**Sample Input**
4
C
C6H5OH
NH2CH2COOH
C12H22O11
**Sample Output**
12.010
94.108
75.070
342.296

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=4461&mosmsg=Submission+received+with+ID+20466109
```
//通过判断下一个字符的类型来执行上一个原子的量计算
#include<iostream>
#include<cstdio>
#include<cctype>
using namespace std;
int main()//两个变量，一个存原子量，一个存后面的数字
{
    int T;
    cin>>T;
    getchar();
    while(T--)
    {
        int c;
        double sum=0,mol=0;
        int num=0;
        while((c=getchar())!='\n')
        {
            if(isdigit(c))
            {
                num=num*10+c-'0';
                //sum+=mol*(c-'0');
                //num=1;
            }
            else
            {
                if(num)
                {
                    sum+=mol*num;//加前一个原子*数量
                    mol=num=0;
                }
                else
                {
                    sum+=mol;//加前一个原子
                }
                switch(c)
                {
                    case 'C':mol=12.01;break;
                    case 'H':mol=1.008;break;
                    case 'O':mol=16.00;break;
                    case 'N':mol=14.01;break;
                }
                //cout<<"mol  "<<mol<<"  num  "<<num<<"  sum  "<<sum<<endl;
            }

        }
        if(num==0)
            sum+=mol;
        else
            sum+=mol*num;
        printf("%.3lf\n",sum);
    }
    return 0;
}
//AC at 2017/12/9
// hh23h
// c   mol    num    sum
// h   1      0      0
// h   1      0      1
// 2   1      2      1
// 3   1      2+3*10 1
// h   1      0      1+23+1
// \n  1      0      24+1



```