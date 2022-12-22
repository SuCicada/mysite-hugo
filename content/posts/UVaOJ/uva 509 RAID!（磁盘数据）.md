> 习题4-7 RAID技术（RAID!, ACM/ICPC World Finals 1997, UVa509）
RAID技术用多个磁盘保存数据。每份数据在不止一个磁盘上保存，因此在某个磁盘损
坏时能通过其他磁盘恢复数据。本题讨论其中一种RAID技术。数据被划分成大小
为s（1≤s≤64）比特的数据块保存在d（2≤d≤6）个磁盘上，如图4-9所示，每d-1个数据块都
有一个校验块，使得每d个数据块的异或结果为全0（偶校验）或者全1（奇校验）。
![这里写图片描述](//img-blog.csdn.net/20180425195940629?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
图4-9 数据保存情况
例如，d=5，s=2，偶校验，数据6C7A79EDFC（二进制01101100 01111010 01111001
11101101 11111100）的保存方式如图4-10所示。
![这里写图片描述](//img-blog.csdn.net/20180425195957465?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
图4-10 数据6C7A79EDPC的保存方式
其中加粗块是校验块。输入d、s、b、校验的种类（E表示偶校验，O表示奇校验）以
及b（1≤b≤100）个数据块（其中“x”表示损坏的数据），你的任务是恢复并输出完整的数
据。如果校验错或者由于损坏数据过多无法恢复，应报告磁盘非法。
**Sample Input**
5 2 5
E
0001011111
0110111011
1011011111
1110101100
0010010111
3 2 5
E
0001111111
0111111011
xx11011111
3 5 1
O
11111
11xxx
x1111
0
**Sample Output**
Disk set 1 is valid, contents are: 6C7A79EDFC
Disk set 2 is invalid.
Disk set 3 is valid, contents are: FFC

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=833&problem=450&mosmsg=Submission+received+with+ID+21205211

注意点：
（行列块的顺序就按照图中的，一个磁盘道是一列）
1、校验块是不加入十六进制运算的
2、校验块的顺序是第一行的第一块，第二行的第二个，到了某行最后一个时，下一行就有从第一个开始算做校验块
3、十六进制转换是 一个十六进制数字需要四个二进制数字，所以每四位二进制就是一位十六进制
4、校验进行是一行中每个块的相同位进行校验
5、奇校验就是每个数互相异或下来是1，偶校验就是0
6、磁盘不合理有两种可能性：一是已知的位校验不符合，二是未知位有多位，无法判断其内容

7、关于输入的内容：
第一个数是磁盘道数，即列数，
第二个数是每个数据块中包含的二进制位个数
第三个数是有多少行数据，就是行数
8、题输入的数据流的分段没有意义，需要后期排列
9、数据存储是**先存完一个磁道再存下一个，即按列的顺序竖的存**（在这里栽过）

思路：
1、存储数据：使用三维数组，或二维向量vector（元素是string或int），我用的三维数组，可以想象成每个数据块就是在行和列组成的平面上拔地而起向上的01列
2、整体分两部分：补全缺的位，以及转换十六进制
3、补全：先循环行，再循环块里的位，用每一位循环列，每一列的当前位进行异或处理，结果和校验奇偶性即0，1一样即可，不一样就是错的，如果有 x ，那先记录下来 x 的下标，最后算完后的结果就是 x 的值。如果有多个 x ，就是错的。
4、二进制转十六进制：可以十六行暴力解，也可以转成十进制，大于9的六个数罗列输出
5、注意输入开始三个数的顺序
6、可以使用一个函数来看存储的数据。
没了

这个代码我使用了动态分配的数组，网上查的，挺有意思，C语言的三重指针真的是很奇妙，指针这种东西本身也是个变量，他特别的就是里面存的值是别的变量的地址罢了，仅此而已，无论几重指针都是如此
```
//uva 509 RAID（磁盘数据）
#include<iostream>
#include<string>
using namespace std;
/*
1 0 1
1 1 0
0 0 0
0 1 1
*/
//列个行 bca
int a,b,c; //数据块（几行），磁盘（一行几个，列）,一单位数据几位

//一个显示三维表的函数，辅助，不用也可以
void show(char ***disk)
{
    for(int i=0;i<a;i++)
    {
        for(int j=0;j<b;j++)
        {
            for(int k=0;k<c;k++)
                cout<<disk[i][j][k];
            cout<<"_";
        }
        cout<<endl;
    }
    cout<<endl;
}

int repair(char ***disk,int od_ev)//od_ev直接传来0或1
{
    //应该先循环a，再循环c，后循环b
    //找到一位然后 循环一行中所有的这个位
    //cout<<a<<"  "<<b<<"  "<<c<<endl;
    for(int i=0;i<a;i++)
    {
        for(int k=0;k<c;k++)
        {
            int yihuo=0;
            int x_num=-1;
            for(int j=0;j<b;j++)
            {
//        cout<<i<<" "<<j<<"  "<<k<<endl;
//        show(disk);
                if(disk[i][j][k]=='x')
                {
                    if(x_num!=-1)
                        return 0;//disk invalid
                    x_num=j;
                }
                else
                    yihuo=yihuo^(disk[i][j][k]-'0');
            }
            if(x_num!=-1)
                disk[i][x_num][k] = yihuo^od_ev+'0';
            else if(yihuo!=od_ev)
                return -1;   //数据错误
        }
    }
    return 1;
}

char hex(string bin)//char b1,char b2,char b3,char b4)
{
    //cout<<bin<<"----"<<endl;
    int n=(bin.at(0)-'0')*2*2*2+ (bin.at(1)-'0')*2*2 + (bin[2]-'0')*2+ (bin[3]-'0');
    if(n<10) return (n+'0');
    switch(n)
    {
        case 10:return 'A';
        case 11:return 'B';
        case 12:return 'C';
        case 13:return 'D';
        case 14:return 'E';
        case 15:return 'F';
    }
//    以下是暴力解233
//    string h="0000";
//    h[0]=b1;
//    h[1]=b2;
//    h[2]=b3;
//    h[3]=b4;
//    if(h=="0000") return "0";
//    if(h=="0001") return "1";
//    if(h=="0010") return "2";
//    if(h=="0011") return "3";
//    if(h=="0100") return "4";
//    if(h=="0101") return "5";
//    if(h=="0110") return "6";
//    if(h=="0111") return "7";
//    if(h=="1000") return "8";
//    if(h=="1001") return "9";
//    if(h=="1010") return "A";
//    if(h=="1011") return "B";
//    if(h=="1100") return "C";
//    if(h=="1101") return "D";
//    if(h=="1110") return "E";
//    if(h=="1111") return "F";

}
int main()
{
    int NUM =0;
    while(cin>>b>>c>>a&&b!=0)  //注意输入的顺序
    {
        char ***disk;
        char od_ev;
        cin>>od_ev;

        disk = new char**[a]; //disk指向存着a个char**型的空间
        //char **disk[a]
        for(int i=0;i<a;i++)
            disk[i] = new char*[b];   //disk里的每个元素指向存着b个char*型空间
            //char *disk[i][b]
        for(int i=0;i<a;i++)
            for(int j=0;j<b;j++)
                disk[i][j] = new char[c];

            for(int j=0;j<b;j++)
        for(int i=0;i<a;i++)
                for(int k=0;k<c;k++)
                    cin>>disk[i][j][k];

        NUM++;
        cout<<"Disk set "<<NUM<<" is ";

        int retu = repair(disk,od_ev=='E'?0:1);
        if(retu!=1)
        {
            cout<<"invalid."<<endl;
            continue;   //进行下一次大循环
        }
        //cout<<retu<<endl;

        //show(disk);


        string result;
        string bin;     //要转十六进制的四位二进制
        int num=0;      //间隔是4,代表四位二进制
        int sum=a*b*c-a*c;  //一共要过的字符量，除去校验位
        for(int i=0;i<a;i++)
            for(int j=0;j<b;j++)
            {
                if((i+1)%b==(j+1)%b)  //校验码
                {
                    //sum-=c;  //将查过的这c个也要记录
                    continue;
                }
                for(int k=0;k<c;k++)
                {
//                    cout<<"disk"<<i<<" "<<j<<" "<<k<<" "<<(disk[i][j][k])<<endl;;
                    bin.append(1,disk[i][j][k]);   //这里的1代表加入1个处于第二个元素的字符，这里没处理好。
                    num++;
//                    cout<<bin<<"__"<<sum<<" "<<num<<endl;
                    if(num%4==0)
                    {
                        result.append(1,hex(bin));
                        bin.erase();
                    }
                    else if(sum%4!=0&&sum==num)
                        result.append(1,hex(bin+"0000"));
                }
            }

        cout<<"valid, contents are: ";
        cout<<result<<endl;

    }//while

    return 0;
}
//AC at 2018/4/25

```


-----------
（题外话：这个一不小心又花掉了一个晚上，过了样例后，第二天一次过，爽哈哈哈，我只能写写程序来找回点信心的意义了，每天写代码的时间也并不多啊，感觉还是写程序好，每天都能获得一点成就感。每天真的好麻烦的，我可能也仅限于此了）