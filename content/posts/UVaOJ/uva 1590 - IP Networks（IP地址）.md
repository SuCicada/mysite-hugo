> 习题4-5 IP网络（IP Networks, ACM/ICPC NEERC 2005, UVa1590）
可以用一个网络地址和一个子网掩码描述一个子网（即连续的IP地址范围）。其中子网
掩码包含32个二进制位，前32-n位为1，后n位为0，网络地址的前32-n位任意，后n位为0。
所有前32-n位和网络地址相同的IP都属于此网络。
例如，网络地址为194.85.160.176（二进制为11000010|01010101|10100000|10110000），
子网掩码为255.255.255.248（二进制为11111111|11111111|11111111|11111000），则该子网
的IP地址范围是194.85.160.176～194.85.160.183。输入一些IP地址，求最小的网络（即包含IP
地址最少的网络），包含所有这些输入地址。
例如，若输入3个IP地址：194.85.160.177、194.85.160.183和194.85.160.178，包含上述3
个地址的最小网络的网络地址为194.85.160.176，子网掩码为255.255.255.248。
**Sample Input**
3
194.85.160.177
194.85.160.183
194.85.160.178
**Sample Output**
194.85.160.176
255.255.255.248
【注意：他可能有很多组输入，而每组输出之间没有空行】

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=833&problem=4465&mosmsg=Submission+received+with+ID+21184770

思路：
1、先将所有ip存起来，用数组或容器什么的。
2、转换二进制
3、从第一位开始，诸位比较所有的ip在这一位上的数字一样否
4、判断出最小网络位数，即掩码为1的位数。
5、转换十进制
（我用来存二进制的ip用的是string）
（用了下vector，当然也可以用数组存，一个32*1000的数组）

```

/*
110000100101010110100000_10110001
110000100101010110100000_10110111
110000100101010110100000_10110010

11000010010101011010000010110000
11111111111111111111111111111000
*/
//特殊情况：只输入一个IP地址，这时掩码应该32位1
#include<iostream>
#include<stack>
#include<string>
#include<vector>
#include<cmath>
#include<cstdio>
using namespace std;
string binary(int dec)
{
    string str="00000000";
    stack<int> s;
    int bin=0;
    for(int i=7;i>=0;i--)
    {
        //s.push(dec%2);
        str[i]=dec%2+'0';
        dec /= 2;
    }
//    while(!s.empty())
//    {
//        bin = bin*10 + s.top();
//        s.pop();
//    }
//    return bin;
    return str;
}
int decimal(string bin)
{
    int dec =0;
    for(int i=0;i<8;i++) //从高位开始
    {
        dec += (int)pow(2,8-i-1)* (bin[i]-'0');
    }
    return dec;
}
int main()
{
    int T;
    while(cin>>T)
    {
        vector<string>ip; //  存储输入的所有ip
        int num=0;
        if(T==1)  //防止只输入一个地址的特殊情况
            num=32;
        while(T--)    //输入
        {
            int a[4];
            string str;
            scanf("%d.%d.%d.%d",&a[0],&a[1],&a[2],&a[3]);
            for(int i=0;i<4;i++)
            {
                str += binary(a[i]);
            }
            ip.push_back(str);
        }
        for(int j=0;j<32;j++)    //判断相等的位数
        {
            int iff=0;
            for(int i=1;i<ip.size();i++)
            {
                //cout<<ip.at(i)<<endl;
                if(ip.at(0)[j] == ip.at(i)[j])  //这一位相等
                {
                    iff=1;
                }
                else
                {
                    iff=0;
                    break;
                }
            }
            if(iff)         //这一位相等，掩码位数加一
                num++;
            else
                break;
        }
        string zero="00000000"; //备用0
        string mask,minip;
        int score[4];//存储最小网络的四个十进制ip地址段
        int score2[4];//存储mask的四个十进制的ip地址段

        for(int i=1;i<=32-num;i++)  //先从最后开始补0
        {
            minip='0'+minip;
            mask='0'+mask;
        }
        for(int i=num-1;i>=0;i--)   //倒的补
        {
            minip=ip[0].at(i)+minip;
            mask='1'+mask;
        }

//        cout<<num<<endl;
//        for(int i=0;i<ip.size();i++)
//            cout<<ip[i]<<endl;
//        cout<<endl;
//        cout<<mask<<endl;

        for(int i=0;i<4;i++)  //运算最小网络
        {
            score[i] =decimal(minip.substr(i*8,8));
            score2[i] =decimal(mask.substr(i*8,8));
            //以下淘汰的方法是：边变换，边比对是否位数到了最小网络位。
            //比较上面的先做好一个最小网络的二进制地址,然后在直接变换。要更复杂
//            if(i*8+8>num)
//            {
//                //cout<<i*8<<" "<<num<<endl;
//                //cout<<ip[0].substr(i*8,num-i*8)<<"|"<<zero.substr(0,32-num)<<endl;
//                score[i]=decimal(ip[0].substr(i*8,num-i*8)+zero.substr(0,32-num));
//            }
//            else
//            {
//                //cout<<ip[0].substr(i*8,8)<<endl;
//                score[i]=decimal(ip[0].substr(i*8,8));
//            }
        }

        printf("%d.%d.%d.%d\n",score[0],score[1],score[2],score[3]);
        printf("%d.%d.%d.%d\n",score2[0],score2[1],score2[2],score2[3]);
    }
    return 0;
}
//AC at 2018/4/22

```
（题外话：好在上学期的网络课设就是算ip地址，所以这道题我才想了半个多小时就有思路了（哭））