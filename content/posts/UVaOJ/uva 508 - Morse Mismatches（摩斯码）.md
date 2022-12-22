> 习题4-6 莫尔斯电码（Morse Mismatches, ACM/ICPC World Finals 1997, UVa508）
输入每个字母的Morse编码，一个词典以及若干个编码。对于每个编码，判断它可能是
哪个单词。如果有多个单词精确匹配，**选取字典序第一个**再加上“!”；如果无法精确匹
配，可以在编码尾部增加或删除一些字符以后匹配某个单词（增加或删除的字符应尽量少）。如果有多个单词可以这样匹配上，**选取字典序第一个**输出并且在后面加上“?”。
。
提供一个样例
**Sample Input**
A .-
B -...
C -.-.
D -..
E .
F ..-.
G --.
H ....
I ..
J .---
K -.-
L .-..
M --
N -.
O ---
P .--.
Q --.-
R .-.
S ...
T -
U ..-
V ...-
W .--
X -..-
Y -.--
Z --..
0 ------
1 .-----
2 ..---
3 ...--
4 ....-
5 .....
6 -....
7 --...
8 ---..
9 ----.
*
AN
EARTHQUAKE
EAT
GOD
HATH
IM
READY
TO
WHAT
WROTH
*
.--.....--         .....--....
--.----..        .--.-.----..
.--.....--          .--.
..-.-.-....--.-..-.--.-.
..--            .-...--..-.--
----            ..--
*
**Sample Output**
WHAT
HATH
GOD
WROTH?
WHAT
AN
EARTHQUAKE
EAT!
READY
TO
EAT!

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=833&problem=449&mosmsg=Submission+received+with+ID+21190426

解释一下阅读题：输入：
先输入字符与摩斯码的对应，以 * 结束 ，
再输入字典（字典的意思就是你解密的词语只能从这里选取），以 * 结束，
在输入需要解密的摩斯码 ，以 * 结束。
几个注意点：
1、经过多方求证，第二个输入的字典，需要以字典序排列好，这样在查找词组时才能按照字典序查找。
2、刘汝佳书中写的书错的，不是任意选取一个输出，是按照字典序选取
3、udebug中有一个样例输出结果有问题
4、只要是模糊匹配，就加上 ？ ，无论匹配到几个合适的

思路：
1、使用两个map映射，或者四个两对vector，来分别存储‘真实字符’和‘每个真实字符的摩斯码’，‘字典里的每个词组’和‘字典里每个词组对应的摩斯码’（这个需要经过加密运算后进行存储）
（因为是要存的数组中每个元素都是字符串型，所以我选取了vector string>来进行存储）
（可以使用map来进行存储，而且map默认字典序存储）
2、一个函数加密，一个函数解密，通过遍历边比来进行比对出模糊匹配中最小的增删字符长度

附赠样例对照表一份

```
/*
AN           .--.
EARTHQUAKE   ..-.-.-....--.-..-.--.-.
EAT          ..--
GOD          --.----..
HATH         .....--....
IM           ..--
READY        .-...--..-.--
TO           ----
WHAT         .--.....--
WROTH        .--.-.----....
*/
```

```
//在匹配到的中，应选择字符最少的词组
//比如：A . B - C.-  那么.-xx就应该是C
#include<iostream>
#include<string>
#include<vector>
#include<cstdio>
#include<cmath>
#include<algorithm>
using namespace std;

char word[50];           //存真实字符
vector<string> morse;    //存字符的摩斯码
vector<string> dict;     //存词组的字典
vector<string> puzzle;   //存字典对应的摩斯码
vector<string> question; //存要解的摩斯码

string zip(string str)   //词组加密成摩斯码
{
    string ans;
    for(int i=0;i<str.size();i++)
    {
        for(int j=0;j<26+10;j++)
        {
            if(str[i]==word[j])
            {
                ans = ans +morse[j];
                break;
            }
        }
    }
    return ans;
}

string solove(string mos)  //从摩斯解密为词组
{
    int jingzhun_n=-1,jingzhun=0;  //精准匹配的下标，个数
    int mohu_n=-1,mohu=0,mohu_len=999; //模糊匹配的下标，个数，模糊字符长度
    int yes;//匹配位数
    int length=mos.size();
    int puzz_len; //遍历中的词组的长度

    for(int i=0;i<puzzle.size();i++)   //遍历所有的词语
    {
        puzz_len=puzzle[i].size();
        yes =0;   //匹配的字符数量
        //cout<<length<<"   "<<puzz_len<<endl;
        for(int j=0;j<puzz_len&&j<length;j++)  //遍历每个词语摩斯码的每一位
        {
            if(mos[j]==puzzle[i].at(j))
            {
                yes++;
                //cout<<yes<<"||||"<<endl;
            }
            else
                break;
        }//for(j
        if(yes==puzz_len&&length==puzz_len) //精准匹配
        {
            if(jingzhun<1)
                jingzhun_n=i;
            jingzhun++;
        }
        else if(yes==puzz_len&&length>yes|| //模糊匹配，mose长
                yes<puzz_len&&yes==length)  //当前词组长
        {
            if(mohu_len>abs(puzz_len-length))
            {
                mohu_len= abs(puzz_len-length);
//    cout<<"\||||"<<endl<<mohu<<"  "<<dict[mohu_n]<<endl<<endl;
                    mohu_n= i;
                mohu++;
            }
        }
    }//for(i

    if(jingzhun>0)
    {
        if(jingzhun>1)
            return dict[jingzhun_n]+"!";
        return dict[jingzhun_n];
    }
    else if(mohu>0)
    {
        return dict[mohu_n]+"?";
    }
    else
        return dict[0];
}
int main()
{
    //输入字符和对应的摩斯码
    for(int i=0;;i++)
    {
        string temp;
        char c;
        cin>>c;
        if(c=='*')
            break;
        word[i]=c;
        cin>>temp;
        morse.push_back(temp);
    }

    //输入词组的字典
    string temp;
    while(cin>>temp&&temp!="*")
    {
        dict.push_back(temp);
    }
    sort(dict.begin(),dict.end());

    //输入要解密的摩斯码
    while(cin>>temp&&temp!="*")
    {
        question.push_back(temp);
    }

    //输入结束

    //计算加密后的字典中的词组
    for(int i=0;i<dict.size();i++)
    {
        string temp2 =zip(dict[i]);
        puzzle.push_back(temp2);
    }


    //开始解密
    for(int i=0;i<question.size();i++)
    {
        //cout<<question[i]<<"????"<<endl;
        cout<<solove(question[i])<<endl;
    }


//    cout<<dict.size()<<"   "<<puzzle.size()<<endl;
//    for(int i=0;i<puzzle.size();i++)
//    {
//        cout<<dict[i]<<"   ";
//        cout<<puzzle[i]<<endl;
//    }
    return 0;
}
//AC at 2018/4/23



```
--------
（题外话：解出题目的时候是很高兴的，写出程序是很高兴的，不能写程序的话我就要死了，这道题我早上开始做，不知不觉占用了所有的空闲时间，导致别人分配给我的任务都没有做，我真的是。）


