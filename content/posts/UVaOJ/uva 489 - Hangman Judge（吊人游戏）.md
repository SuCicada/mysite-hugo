> 例题4-2 刽子手游戏（Hangman Judge, UVa 489）
刽子手游戏其实是一款猜单词游戏，如图4-
1所示。游戏规则是这样的：计算机想一个单词
让你猜，你每次可以猜一个字母。如果单词里有
那个字母，所有该字母会显示出来；如果没有那
个字母，则计算机会在一幅“刽子手”画上填一
笔。这幅画一共需要7笔就能完成，因此你最多
只能错6次。注意，猜一个已经猜过的字母不！算
错。
在本题中，你的任务是编写一个“裁判”程
序，输入单词和玩家的猜测，判断玩家赢了
（You win.）、输了（You lose.）还是放弃了
（You chickened out.）。每组数据包含3行，第1
行是游戏编号（-1为输入结束标记），第2行是
计算机想的单词，第3行是玩家的猜测。后两行
保证只含小写字母。
注意，猜一个已经猜过的字母不算
错。！
**Sample Input**
1
cheese
chese
2
cheese
abcdefg
3
cheese
abcdefgij
-1
**Sample Output**
Round 1
You win.
Round 2
You chickened out.
Round 3
You lose.

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=832&page=show_problem&problem=430

用两个整型来记录当前还允许错的次数，和原始字符串的总字符数
1，因为重复猜测不算错，所以我们先将guess字符串中的重复字符消去，用循环和string::erase()
2，外循环猜测字符串，内循环原始字符串，如果相同，就将原始字符串的此元素变成‘ * ’。
3，没猜中一次就记录，内循环结束后来更新记录。若果win或lose就return函数，否则循环都结束之后就说明是‘弃权’。
______
```
#include<iostream>
#include<string>
using namespace std;
    string orig,guess;
int hang(int num,int right)
{
    for(int i=0;i<guess.size();i++)
    {
        int iff=0;
        for(int j=0;j<orig.size();j++)
        {
            if(orig[j]==guess[i])
            {
                orig[j]='*';
                iff=1;
                num--;
            }
            if(num==0)
            {
                cout<<"You win."<<endl;
                return 1;//break;
            }
        }
        if(iff==0)
        {
            right--;
        }
        if(right<0)
        {
            cout<<"You lose."<<endl;
            return -1;
        }
//        if(num==0)
//        {
//            cout<<"win"<<endl;
//            break;
//        }
    }
    cout<<"You chickened out."<<endl;
}
int main()
{
    int T;
    while(cin>>T&&T!=-1)
    {
        cin>>orig>>guess;
        for(int i=0;i<guess.size();i++)
        {
            for(int j=i+1;j<guess.size();j++)
            {
                if(guess[j]==guess[i])
                    guess.erase(guess.begin()+j);
            }
        }
        //cout<<guess<<endl;
        int right=6;//错误次数
        int num=orig.size();//要猜的个数
        cout<<"Round "<<T<<endl;
        hang(num,right);
    }
    return 0;
}
//AC at 2018/2/7
```