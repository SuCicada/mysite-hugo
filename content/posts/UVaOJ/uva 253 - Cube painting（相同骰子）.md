> 习题4-4 骰子涂色（Cube painting, UVa 253）
输入两个骰子，判断二者是否等价。每个骰子用6个字母表示，如图4-7所示。
图4-7 骰子涂色
例如rbgggr和rggbgr分别表示如图4-8所示的两个骰子。二者是等价的，因为图4-8（a）
所示的骰子沿着竖直轴旋转90°之后就可以得到图4-8（b）所示的骰子。
![这里写图片描述](https://img-blog.csdn.net/20180422182216607?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VfY2ljYWRh/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
（a） （b）
图4-8 旋转前后的两个骰子
.
**Sample Input**
rbgggrrggbgr
rrrbbbrrbbbr
rbgrbgrrrrrg
**Sample Output**
TRUE
FALSE
FALSE

思路：暴力枚举，将一个骰子的所有姿态都列出来，
1、注意第一个图上的数字，那个是记录骰子面的顺序
2、先找最上面的，也就是1的位置，能排列6种（1，2，3，4，5，6）
3、然后找到了上面也就找到了与其相对的面，就是下面，就是字串中第六个元素。这个不会额外记录，因为有上就有下了。
4、然后就是中间四个的排列了，很显然，4种。
5、然后我们变换第二个骰子，看看它在这24种情况中，有没有一种的情况和第一个骰子的记录是相同的。（比较字串即可）
6、请注意骰子面的转换是否正确，虽然这个逻辑简单，但是容易写错，要好好检查，我就因为写错下标错了两次。
```
#include<iostream>
#include<string>
using namespace std;
/*
  1
3 2 4 5         1在顶上
  6

  2
3 6 4 1         2在顶上
  5

  3
5 6 2 1			3在顶上
  4

  4
2 6 5 1			4在顶上
  3

  5
4 6 3 1			5在顶上
  2

  6
3 5 4 2			6在顶上
  1


*/
string s1,s2;//记录两个骰子的字符串
int str_equal(string a,char s1,char s2,char s3,char s4,char s5,char s6)//比较两个字串相等吗
{
    string b="0000000";
    b[1]=s1;
    b[2]=s2;
    b[3]=s3;
    b[4]=s4;
    b[5]=s5;
    b[6]=s6;
    //cout<<b.substr(1)<<endl;
    if(a==b)
        return 1;
    return 0;
}
int exc(string a,char s1,char s2,char s3,char s4,char s5,char s6)//中间四个面的四种情况
{
    if(str_equal(a,s1,s2,s3,s4,s5,s6)) return 1;
    if(str_equal(a,s1,s3,s5,s2,s4,s6)) return 1;
    if(str_equal(a,s1,s5,s4,s3,s2,s6)) return 1;
    if(str_equal(a,s1,s4,s2,s5,s3,s6)) return 1;
    return 0;
}
int main()
{
    char c;
    while(cin>>c)
    {
//        cin>>s1;
//        s2=s1.substr(5);
//        s1.assign(s1,0,6);
//        cout<<s1<<" "<<s2<<endl;
        //以上注释是一种输入方法，以下是另一种

        s1=s2="0000000";
        s1[1]=c;
        for(int i=2;i<=6;i++)  //因为s1[1]已经在while中输入了
            cin>>s1[i];
        for(int i=1;i<=6;i++)
            cin>>s2[i];
        //cout<<s1.substr(1)<<"  "<<s2<<endl;

        if(exc(s1,s2[1],s2[2],s2[3],s2[4],s2[5],s2[6])){cout<<"TRUE"<<endl;continue;}
        if(exc(s1,s2[2],s2[6],s2[3],s2[4],s2[1],s2[5])){cout<<"TRUE"<<endl;continue;}
        if(exc(s1,s2[3],s2[6],s2[5],s2[2],s2[1],s2[4])){cout<<"TRUE"<<endl;continue;}
        if(exc(s1,s2[4],s2[6],s2[2],s2[5],s2[1],s2[3])){cout<<"TRUE"<<endl;continue;}
        if(exc(s1,s2[5],s2[6],s2[4],s2[3],s2[1],s2[2])){cout<<"TRUE"<<endl;continue;}
        if(exc(s1,s2[6],s2[5],s2[3],s2[4],s2[2],s2[1])){cout<<"TRUE"<<endl;continue;}

        cout<<"FALSE"<<endl;

    }
    return 0;
}
//AC at 2018/4/20

```

-----------
题外话：（
休战一个月后的第一道题，手生的很（才不），这道题的灵感还是来自蓝桥的直播课上老师演示的二阶魔方转换题（然而并并不会做）
因为最近一直看python，所以都不会用c++的string了，其中将字符连接起来就伤了我脑筋，不得已用了同样暴力的传进6+个参数的方法。（所谓一暴到底吗）
啊啊好想做后面的题，一直窝在第四章，会来不及看真正有用的算法的，本来以外蓝桥后算法会松一些，但是比赛还有（好事不是吗），我会继续努力的（乖孩子的话语，不喜欢）
）
