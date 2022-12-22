> 习题3-11 换低挡装置（Kickdown, ACM/ICPC NEERC 2006, UVa1588）
给出两个长度分别为n1，n2（n1，n2≤100）且每列高度只为1或2的长条。需要将它们放
入一个高度为3的容器（如图3-8所示），问能够容纳它们的最短容器长度。![这里写图片描述](https://img-blog.csdn.net/20180103205113155?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VfY2ljYWRh/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
**Sample Input**
2112112112
2212112
12121212
21212121
2211221122
21212
**Sample Output**
10
8
15

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=4463&mosmsg=Submission+received+with+ID+20560401
用两个字符串数组分别存这两个块，一块作为不动的，另一块在其上移动，判断当前移动到的位置是不是和下块契合（用循环依次比较各个位置），若不是则继续移动到下一位。
需要注意的是，最短的契合方案有可能你漏想了，下面一共是三种可能的情况。所以我用一个函数来将两个块换了位置后又移动了一次。
```
//三种情况：bbbbb aa (|a|:ab重叠
//1.短块在长块里(bb|aa|b)
//2.短块头在长块里（外），短块尾巴超出长块尾(bbbb|a|a)
//3.长块头在短块尾（外），长块尾巴超出短块尾(a|a|bbbb)
//不要忘记第三种情况，有时最短空间就是出自3
#include<iostream>
#include<cstring>
using namespace std;
const int N = 100;
//int kick1[N+5],kick2[N+5];
int kickdown(string k1,string k2)//定k1，移k2
{
        //cout<<k1.size()<<"  "<<k2.size()<<endl;
    for(int i=0;i<k1.size();i++)
    {
        int ii=i,j=0;//i就是大小块契合的那一位
        //cout<<ii<<" "<<j<<endl;
        while((k1[ii]-'0'+k2[j]-'0')<=3&&(j<k2.size()&&ii<k1.size()))//若当前位不匹配则进行for到下一位
        {
            //cout<<"k1 ii "<<ii<<"  "<<k1[ii]<<" | k2 j"<<j<<"  "<<k2[j]<<endl;
            ii++;
            j++;
        }
        if(j==k2.size()||ii==k1.size())//若是寿终正寝（即循环到块尾了）
        {
            //cout<<j<<"  i "<<i<<endl;
            return (i+k2.size())>=k1.size()?(i+k2.size()):k1.size();
            //若i+k2.size()比k1（长块）的长度还短，那么所需长度就直接是k1长度了
            /*下面是通俗代码*/
            //space=i+k2.size();
            //if(space<k1.size())
            //    space=k1.size();
            //break;
        }
    }
    return k1.size()+k2.size();//若没有契合处，只能接到后面了
}

int main()
{
    string k1,k2;//咱们要k1>k2
    while(cin>>k1>>k2)
    {
        int space1=0,space2=0;
        space1=kickdown(k1,k2);
        space2=kickdown(k2,k1);
        //cout<<"s1   "<<space1<<"  s2 "<<space2<<endl;
        cout<<(space1<space2?space1:space2)<<endl;
    }
    return 0;
}
//AC at 2018/1/2
```
一开始我被题目中的例子所蒙蔽，想的是固定长的块，移动短的块，最后算下来和网友给的样例比总是有一些要大上几个数。那人一口气给了500+对数十上百个元素的12字符串，想用cout断点的方式看出一些端倪是十分困难的。这道题花了两天吧，太糟。