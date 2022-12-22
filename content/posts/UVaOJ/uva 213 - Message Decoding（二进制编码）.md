> 例题4-4 信息解码（Message Decoding, ACM/ICPC World Finals 1991, UVa 213）
考虑下面的01串序列：
0, 00, 01, 10, 000, 001, 010, 011, 100, 101, 110, 0000, 0001, …, 1101, 1110, 00000, …
首先是长度为1的串，然后是长度为2的串，依此类推。如果看成二进制，相同长度的后
一个串等于前一个串加1。注意上述序列中不存在全为1的串。
你的任务是编写一个解码程序。首先输入一个编码头（例如AB#TANCnrtXc），则上述
序列的每个串依次对应编码头的每个字符。例如，0对应A，00对应B，01对应#，…，110对
应X，0000对应c。接下来是编码文本（可能由多行组成，你应当把它们拼成一个长长的01
串）。编码文本由多个小节组成，每个小节的前3个数字代表小节中每个编码的长度（用二
进制表示，例如010代表长度为2），然后是各个字符的编码，以全1结束（例如，编码长度
为2的小节以11结束）。编码文本以编码长度为000的小节结束。
例如，编码头为$#**\，编码文本为0100000101101100011100101000，应这样解码：
010(编码长度为2)00(#)00(#)10(*)11(小节结束)011(编码长度为3)000(\)111(小节结束)001(编码
长度为1)0($)1(小节结束)000(编码结束)。
**Sample input**
TNM AEIOU
0010101100011
1010001001110110011
11000
$#**\
0100000101101100011100101000
**Sample output**
TAN ME
\##*\$

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=832&problem=149&mosmsg=Submission+received+with+ID+20759428

```
/*
1、先接收字符串，因为有空格，所以我用了getline，因为我用string
2、然后我们将输入的字符串存入code这个二维数组里，code[i][j]中i代表二进制码的位数，j代表当前01码的十进制数。以此我们每次循环的j小于2^i-i即可。
3、然后我们用循环，先将三位的二进制数传入bin_dec()函数，来得出接下来代表一个字符的二进制码的位数。
4、contra()函数是用来进行每一组相同码长的字符的输出。
（1）其思路是用一个变量记录当前bcode（01码字符串）读到哪个位了。
（2）然后循环将之后的len位二进制变成一个整数，传入bin_dec()函数得出其代表的十进制数，而这个十进制数也是code数组的第二维的下标。
5、还有一点，因为二进制码在输入的过程中并不一定是在同一行,所以我用了：当前下标+要接受的码长 和 当前的01码字符串长度来进行比较，若大则接收新的一行字符串，再加到原先01码字符串后面。（！注意这里用的是while循环而不是单一次的if判断，是因为可能接收一次字串后也依然不够码长，所以要一直接收到足够长为止。）*/
#include<iostream>
#include<cmath>
#include<string>
#include<cstring>
using namespace std;
char code[7][1<<7];//长度，值，来存字符

int bin_dec(int b)//二进制转十进制
{
    int dec=0;
    for(int i=0;b!=0;i++)
    {
        dec += ((b%10)<<i);
        b/=10;
    }
    return dec;
}
int dec_bin(int dec)//十进制转二进制
{
    int b=0;
    for(int i=0;dec!=0;i++)
    {
        b += (dec%2)*(int)(pow(10,i)+0.1);
        dec=dec>>1;
    }
    return b;
}

string s;//字符串
string bcode;//01码
//此函数用于输出当前码长为len的一组01码所代表的的字符
void contra(int &n,int len)//下一元素的下标，码长
{
    int dec;
    while(1)
    {
        int bin=0;
        while(n+len>=bcode.size())//用于接收断裂处之后的01码
        {
            string temp;
            cin>>temp;
            bcode += temp;
        }
        for(int i=0;i<len;i++)
        {
            bin+=(bcode[n++]-'0')*(int)(pow(10,len-i-1)+0.1);
        }
        dec=bin_dec(bin);
        if(dec==(1<<len)-1) break;
        cout<<code[len-1][dec];

    }
}
int main()
{
    while(getline(cin,s)&&cin>>bcode)
    {
        memset(code,0,sizeof(code));//清空数组
        int si=0;
        for(int i=0;i<7&&si<s.size();i++)//存表
        {
            for(int j=0;j<(1<<(i+1))-1&&si<s.size();j++)
            {
                code[i][j]=s[si++];
            }
        }

        int n=0;//下一个元素下标
        while(1)
        {
            while(n+3>bcode.size())//用于接收断裂处之后的01码
            {
                string temp;
                cin>>temp;
                bcode += temp;
            }

            int s1=bcode[n]*100+bcode[n+1]*10+bcode[n+2]-111*'0';//三位01码
            if(s1==0) break;
            n+=3;
            s1=bin_dec(s1);//判断前三位 分析出码长
            contra(n,s1);
        }
        //cout<<"!!!!!end"<<endl<<endl;
        cout<<endl;
        cin.get();
    }
    return 0;
}
//AC at 2018/2/11
```

另外在使用pow函数时遇到了一个问题，那就是浮点型的精度问题，
在计算pow(10,2)算下来的double是99.9999多，我在强制转换int后就只剩下了99，所以当时在十转二进制十出错了。
虽然我以前没出现过此错误，但是我们可以采用(int)(pow(10,i)+0.1)这个办法。

------------------------------------
（然后就是题外话：这道题上午看没有头绪，睡起来花了累计3小时才做出来，真的累。快过年了，真的是）