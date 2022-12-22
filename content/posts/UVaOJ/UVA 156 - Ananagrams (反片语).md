> 例题5-4 反片语（Ananagrams，Uva 156）
输入一些单词，找出所有满足如下条件的单词：该单词不能通过字母重排，得到输入文
本中的另外一个单词。 在判断是否满足条件时，字母不分大小写，但在输出时应保留输入中
的大小写，按字典序进行排列（所有大写字母在所有小写字母的前面）。
**Sample Input**
ladder came tape soon leader acme RIDE lone Dreis peat
ScAlE orb eye Rides dealer NotE derail LaCeS drIed
noel dire Disk mace Rob dries
\#
**Sample Output**
Disk
NotE
derail
drIed
eye
ladder
soon

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=835&problem=92&mosmsg=Submission+received+with+ID+22713249

思路和书上的一样, 先把每次的单词变成小写, 然后将其存入map的键,  值就是**原单词**.
如果是重复的单词, 那么就把值变成空的.  
最后判断 map 里每一个迭代对象的值是不是空就行了
将不是空的结果存入一个set,   set自动排序, 然后再将set遍历输出即可

```
#include<iostream>
#include<map>
#include<string>
#include<set>
#include<algorithm>
using namespace std;

string lower(string s){
    string re;
    for(int i=0;i<s.size();i++){
        re += s[i]>='a' && s[i]<='z' ? s[i] : s[i] - 'A' + 'a'; 
    }
    return re;
}

int main()
{
    map<string, string> dict;
    string word,temp;

    while(cin>>word && word != "#"){
        // cout<<word<<endl;
        temp = lower(word);
        // cout<<"lower "<<temp<<endl;
        sort(temp.begin(), temp.end());
        // cout<<"--"<<word<<endl;
        // cout<<"dict "<<dict[temp]<<endl;
        if(dict.count(temp) == 0){  //说明没有
            dict[temp] = word;
        }else{ //有了我们就不要了
            dict[temp] = "";
        }
    }
    
    set<string> res;
    map<string, string>::iterator i = dict.begin();
    for(;i!=dict.end();i++){
        if((*i).second != ""){
            // cout<<(*i).second<<endl;
            res.insert((*i).second);
        }
    }
    for(set<string>::iterator i = res.begin(); i != res.end(); i++){
        cout<<(*i)<<endl;
    }

    return 0;
}
// AC at 2019/1/30
```

---
2019年第一道题, 还是例题, 距离上次做题已经有8个月以上了.
和去年相比实在是太散漫了,  是因为要学的东西多了吗,是因为要做的事多了吗,这不是借口,是我太废物了. 好不容易活到这个等级, 不能gameover啊啊.

