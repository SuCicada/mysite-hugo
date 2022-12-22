> 习题3-6 纵横字谜的答案（Crossword Answers, ACM/ICPC World Finals 1994,
UVa232）
输入一个r行c列（1≤r，c≤10）的网格，黑格用“*”表示，每个白格都填有一个字母。如
果一个白格的左边相邻位置或者上边相邻位置没有白格（可能是黑格，也可能出了网格边
界），则称这个白格是一个起始格。
首先把所有起始格按照从上到下、从左到右的顺序编号为1, 2, 3,…，如图所示。
![这里写图片描述](https://img-blog.csdn.net/20171218211353308?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VfY2ljYWRh/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
接下来要找出所有横向单词（Across）。这些单词必须从一个起始格开始，向右延伸到
一个黑格的左边或者整个网格的最右列。最后找出所有竖向单词（Down）。这些单词必须
从一个起始格开始，向下延伸到一个黑格的上边或者整个网格的最下行。
输出时每两行之间有空行

**Sample Input**
2 2
AT
*O
6 7
AIM*DEN
*ME*ONE
UPON*TO
SO*ERIN
*SA*OR*
IES*DEA
0
**Sample Output**
puzzle #1:
Across
1.AT
3.O
Down
1.A
2.TO

puzzle #2:
Across
1.AIM
4.DEN
7.ME
8.ONE
9.UPON
11.TO
12.SO
13.ERIN
15.SA
17.OR
18.IES
19.DEA
Down
1.A
2.IMPOSE
3.MEO
4.DO
5.ENTIRE
6.NEON
9.US
10.NE
14.ROD
16.AS
18.I
20.A

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=168&mosmsg=Submission+received+with+ID+20505776

```
关于char数组和动态指针做的数组
1.前者下标可以越界，后者一旦越界即便没有操作那个地方的元素，也会程序出错。
2.后者好在大小可以自定义。而却因为下标越界问题导致有些算法判断的地方需要多些代码。
(其实即便是动态数组，只要在创建时将行列多建几个就行了)
```
```
//以汉字书写顺序，先左到右，在上到下依次边遍历，边输出，
//凡是输出的格子以@代替，之所以不用*代替是为了统计起始格的个数。
//本题因为说了行列的范围，所以直接用char型的二维数组即可。
//而我没注意以为是任意行列的数组，所以用了动态内存分配生成二维数组。（代码数多在这了）
//其他注意点在代码里注释了。
#include<iostream>
#include<iomanip>
using namespace std;

int hang,lie;
//测试用的显示函数
//void dis(char **puzz)
//{
//    for(int i=0;i<hang;i++)
//    {
//        for(int j=0;j<lie;j++)
//            cout<<puzz[i][j];
//        cout<<endl;
//    }
//}
char** creat()
{
    char **puzz;
    puzz=new char*[hang];
    for(int i=0;i<hang;i++)
        puzz[i]=new char[lie];
    return puzz;
}

void print(char **puzz,char AD)
{
    int i,j,pi,pj,*pij;//p系列是为打印一组字符的
    if(AD=='A')
    {
        cout<<"Across";
        pij=&pj;
    }
    if(AD=='D')
    {
        cout<<"Down";
        pij=&pi;
    }
    int qsg=0;
    for(i=0;i<hang;i++)
        for(j=0;j<lie;j++)
        {
            //cout<<i<<"  "<<j<<"  "<<puzz[i][j]<<"  ????"<<endl;
            if(puzz[i][j]=='*')//把*当作起始格就不好了
                continue;
            bool iff=false;
            if(j==0||i==0)//找起始格
            {
                iff=true;
                qsg++;
            }
            else if(puzz[i][j-1]=='*'||puzz[i-1][j]=='*')//为了防止j-1和i-1小于0的情况
            {
                iff=true;
                qsg++;
            }
            if(iff)
            {
                if(puzz[i][j]=='@')//虽然此元素也在起始格，但是因为已经被走过了，所以不允许其捣乱
                        continue;//不能break
                cout<<endl//开始一组才换行，防止多余空行
                    <<setw(3)//注意是3格的右对齐
                    <<qsg<<".";
                pi=i;
                pj=j;
                while(puzz[pi][pj]!='*')//输出across的字符
                {
                    cout<<puzz[pi][pj];
                    puzz[pi][pj]='@';
                    (*pij)++;
                    if(!(pj<lie&&pi<hang))//一旦下标越界，就会出错
                        break;
//                    if(AD=='A')
//                        j++;
//                    if(AD=='D')
//                        i++;
                }
            }
            //if(pj<lie&&pi<hang)//还有更好的办法吗//yes
                //cout<<endl;
        }
    cout<<endl;
}


int main()
{
    int num=1;
    while(cin>>hang&&hang!=0)
    {
        cin>>lie;
        if(num>1)
            cout<<endl;
        cout<<"puzzle #"<<num++<<":"<<endl;
        char **puzz;
        puzz=creat();//必须把指针传回来
        for(int i=0;i<hang;i++)//原始迷宫
            for(int j=0;j<lie;j++)
            {
                //puzz[i][j]='o';
                cin>>puzz[i][j];
            }
        //dis(puzz);
        char **puzz2;
        puzz2=creat();
        for(int i=0;i<hang;i++)//克隆的迷宫
            for(int j=0;j<lie;j++)
                puzz2[i][j]=puzz[i][j];
        print(puzz,'A');
        //cout<<endl;
        print(puzz2,'D');
        //    cout<<endl;
      //  dis(puzz2);
    }
    return 0;
}
//AC at 2017/12/18

```


（题外话：一开始想出大体逻辑花了30+分钟，而一直到写成并通过花了3个晚上的算法题时间+，累计是3+小时吧，写的时候逻辑混乱，代码一改再改。 每次写都会查很多东西。）