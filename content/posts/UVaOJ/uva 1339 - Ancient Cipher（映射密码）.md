> 例题4-1 古老的密码（Ancient Cipher, NEERC 2004, UVa1339）
给定两个长度相同且不超过100的字符串，判断是否能把其中一个字符串的各个字母重
排，然后对26个字母做一个一一映射，使得两个字符串相同。例如，JWPUDJSTVP重排后可
以得到WJDUPSJPVT，然后把每个字母映射到它前一个字母（B->A, C->B, …, Z->Y, A-
>Z），得到VICTORIOUS。输入两个字符串，输出YES或者NO。
**Sample Input**
JWPUDJSTVP
VICTORIOUS
MAMA
ROME
HAHA
HEHE
AAA
AAA
NEERCISTHEBEST
SECRETMESSAGES
**Sample Output**
YES
NO
YES
YES
NO

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=832&problem=4085&mosmsg=Submission+received+with+ID+20728592


```
//1、我们只需要分别记录两个字串里出现的各个字母的数量，
//2、然后从小到大或从大到小排序，
//3、之后进行比较，如果两个字串对应，那么他们的字母数量表相同下标的元素值（代表字母的数量）也应该相同。
#include<iostream>
#include<string>
using namespace std;
void ssqrt(int *str)
{
    //cout<<str<<endl;
    for(int i=0;i<26;i++)
        for(int j=i+1;j<26;j++)
            if(str[i]>str[j])
            {
                int temp=str[i];
                str[i]=str[j];
                str[j]=temp;
            }
   //cout<<str<<endl;
}
int main()
{
    string str,guess;//输入的两个字串
    while(cin>>str>>guess)
    {
        int cha1[26]={0},cha2[26]={0};//分别记录两个字串里的各个字母的数量

        for(int i=0;i<str.size();i++)//记录str里各个字母数量
        {
            cha1[str[i]-'A']++;
            cha2[guess[i]-'A']++;
        }
        ssqrt(cha1);//对各个字母量排序
        ssqrt(cha2);
//        for(int i=0;i<26;i++)
//        cout<<cha1[i];
//        cout<<endl;
//        for(int i=0;i<26;i++)
//        cout<<cha2[i];
//        cout<<endl;


        int iff=1;

//        cout<<"str    "<<str<<endl;
//        cout<<"guess  "<<guess<<endl;

        for(int i=1;i<26;i++)//比较排序后的字母量串
        {
            if(cha1[i]!=cha2[i])
            {
                iff=0;
                cout<<"NO"<<endl;
                break;
            }
        }
        if(iff==1)
            cout<<"YES"<<endl;
    }
    return 0;
}
//AC at 2018/2/5

```

另：uva上，题目目录如果是蓝色的，代表那道题你提交过但没有Accepted。绿色则是通过，无色就是没有提交过。