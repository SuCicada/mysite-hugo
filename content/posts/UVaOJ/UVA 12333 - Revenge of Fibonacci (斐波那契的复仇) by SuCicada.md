---
date: 2020/3/7 13:00
---
- [合适的思路](#合适的思路)
	- [大数加法](#大数加法)
	- [字典树](#字典树)
- [代码](#代码)
- [注意点](#注意点)
- [附录](#附录)
- [后日谈](#后日谈)

>习题5-15 Fibonacci的复仇（Revenge of Fibonacci, ACM/ICPC Shanghai 2011,
UVa12333）
Fibonacci数的定义为：F(0)=F(1)=1，然后从F(2)开始，F(i)=F(i-1)+F(i-2)。例如，前10
项Fibonacci数分别为1, 1, 2, 3, 5, 8, 13, 21, 34, 55……
有一天晚上，你梦到了Fibonacci，它告诉你一个有趣的Fibonacci数。醒来以后，你只记
得了它的开头几个数字。你的任务是找出以它开头的最小Fibonacci数的序号。例如以12开头
的最小Fibonacci数是F(25)。输入不超过40个数字，输出满足条件的序号。
如果序号小于100000的Fibonacci数均不满足条件，输出-1。
提示：本题有一定效率要求。如果高精度代码比较慢，可能会超时。

## [原题链接](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=836&page=show_problem&problem=3755)

<div id="合适的思路"/>

## 合适的思路

首先想我们该怎么样能匹配前缀，首先我们不知道完整的数字是什么，所以我们要先得到符合要求的数字数据集。
然后将这些数据集放入**字典树**中
所以我们要做的就是
1. 使用大数加法技巧，计算前100000个fibonacci
2. 将每一个fibo数存入字典树中

<div id="大数加法"/>

### 大数加法
首先先做大数加法，使用字符串存储数字，两个字符串（数字）从最后一位即个位开始一位一位加。
**问题在于：**
1. 100000个fibo数字，到后期位数是相当恐怖的，测试时发现第10万个数字有足足2万多位。这会造成在运算时时间和空间的非常糟糕的消耗。
	    所以我们就可以遵从题意，只关心前40位，我们只截取每个数字的前面一部分做计算。这就要引出第2个和第3个问题。
2. 如果我们使用40位，不论后面多少位，就把第40位当作个位。这会产生一个误差的问题。   比如两个数字 11001 和88999，如果我们只取他们的前2位算出来下一个数的前2位是99，但是实际上下一个数的前2位是10。这就是误差。
3. 在解决第1个问题的时候，我们需要考虑数字进位的情况，因为我们只能看到数字的前一部分，不知道两个相加的数字是否位数相等，比如1234和345相加，假设取前2位，那么我们只能看到12和34，这样直接相加是不对的。

**解决**
1. 问题:3：先解决位数问题，我们可以将位数记录在字符串中，比如加入一个最后一位专门用来放位数。但是这样我们只能放256个无符号数字（因为使用char型元素存储）。那么换个思路，因为我们的问题在于两个数字位数不等，而不是位数究竟多少，所以可以只记录位数的奇偶即可。
（另：其他办法，因为是Fibo数，位数不等的情况下，后一个数是大的数字，位数肯定多，而且第一位肯定是1，可以用这些关键点来解决）
2. 问题:2：为了解决误差问题，我们就不能只选取前40位，那么应该选几位呢，经过测试发现最小能保证前40位没有问题的位数是52，测试方法见[附录](#附录)。
不过要是不能测试的时候可以以10位为单位扩大范围，反正50和60位的差距远比50和2万的差距小。

<div id="字典树"/>

### 字典树
每一个结点中存有
+  从根节点到此结点的最小的Fibo序数，即最小前缀数字。
+ 一个map，存有下一位的数字到下一位所在的结点的位置映射关系。

结点之间的关系如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200307140459608.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
在存储的时候，存入的字符串在经过每一个结点的时候，都要比一下这个结点中存储的minIndex（FIbo最小前缀序号），并将其替换成两者中更小的。
这样就能保证按照字典树的脉络寻找，寻找到的结点中的序号，一定是前缀数所在的最小的Fibo序号。

<div id="注意点"/>

### 注意点
不知道现在的UVA 编译器是怎么回事，我定义的 int型返回值函数没有给返回值，返回判我 Runtime error。我一开始压根不明觉厉，一行一行注释，然后一次次提交看看哪部分代码没有才会不报 re，以此来判断问题出在哪部分。光是这样就提交了十几次。
我愿称之为绝活。
在 线 O J ，现 场 调 试。

<div id="代码"/>

## 代码
```cpp
#include<iostream>
#include<map>
#include<cmath>
#include<vector>
#include<cstdio>
using namespace std;

class Site{
public:
    int minIndex; // 到目前这个结点为止的最小序数
    map<char,Site*> next; // 下一个位们的 位对应结点
};

Site root;
int limitSite = 52; // 选取的Fibo数的前置位数数，这个数能保证前40位的准确

void putTree(string a,int index){
    int len = a.size();
    Site* front = &root; 
    for(int i=0;i<len;i++){
        // 开始一位一位找, 每一位都需要
        // 1. 比较树中有没有结点, 没有就new一个,把自己加上(还有序数),然后记录下一个指针,并把自己给了上一个指针, 
        //    有则比较序数, 换成小的
        char c = a[i];
        if(!(front->next).count(c)){  // 如果没有
            Site* newsite = new Site();  // 手动申请空间, 不然函数结束后就会被清理
            newsite->minIndex = index; 
            (front->next)[c] = newsite;
        }else{
            (front->next)[c]->minIndex = min((front->next)[c]->minIndex, index);
        }
        front = (front->next)[c]; // 跳到下一个结点(位)
    }
}

int find(string str){
    int len = str.size();
    Site* front = &root;
    for(int i=0;i<len;i++){
        // 一位一位找, 如果有一位没有找到, 那说明树中没有这个前缀, 返回-1
        // 一直找到最后一位, 这最后一位所在的结点上的值就是要的
        char c = str[i];
        if(!(front->next).count(c)){  // 如果没有
            return -1;
        }
        front = (front->next)[c]; // 跳到下一个结点(位)
    }
    // 没有位了,所以上一个结点就是最后一位所在结点
    return front->minIndex;
}

string add(string a,string b){
    int aLen = a.size()-1; // 数长
    int bLen = b.size()-1;
    int alastSite = min(limitSite, aLen);
    int blastSite = min(limitSite, bLen);
    int aSiteNum = a[alastSite]; // 位数奇偶,从最后一位取得
    int bSiteNum = b[blastSite];
    int ten = 0;
    int newLen = bLen+1; // 新数长度,一位放头放0

    int i = aLen-1;
    int j = bLen-1;
    if(aLen == bLen && aSiteNum!=bSiteNum){ // 位数不等,并且数字已经取了部分了
        i--; // 小的数扔一位
    }

    string res(newLen+1,'0');  // 最后一位放位数(奇偶)
    int resSite = newLen-1;  // 记录结果的当前位数
    for(; j>=0; i--,j--){
        int aa='0';
        if(i>=0){
            aa = a[i];
        }
        int r = aa-'0' + b[j]-'0' + ten;
        ten = r/10;
        res[resSite--] = r%10+'0';
    }
    res[0] = ten+'0';

    if(ten==0){ //不会进位
        string result = res.substr(1,limitSite+1);
        result[result.size()-1] = bSiteNum;
        return result;
    }else{
        string result = res.substr(0,limitSite);
        result[result.size()-1] = !(bSiteNum-'0') +'0';
        return result;
    }
}

int main(){
    string a,b,c;
    a = "11"; 
    b = "11";
    c = "";
    putTree("1",0);
    for(int i=2;i<100000;i++){  // 100000以内,不包括
        c = add(a,b);
        a = b;
        b = c;
        string s = c.substr(0, min(40,(int)c.size()-1) );
        putTree(s,i);
    }

    int T;
    cin>>T;
    for(int i=0;i<T;i++){
        string n;
        cin>>n;
        int res;
        printf("Case #%d: %d\n", i+1, find(n));
    }
    return 0;
}

// AC at 2020/3/7 13:00
```

<div id="附录"/>

## 附录
- 解决位数选取部分导致的误差使用的测试方法
	先生成完整的Fibo数，10万位，数据文件有100MB了。然后设置limitSite，将结果输出到文件中。然后使用脚本将两个文件一行一行即一个数一个数进行对比可知。

<div id="后日谈"/>

## 后日谈

[UVA 12333 - Revenge of Fibonacci (斐波那契的复仇) 【后日谈】by SuCicada](https://blog.csdn.net/su_cicada/article/details/104716628)

<br><br><br><br>

----
**唠叨几句**

居然写了120行，说明还不够精炼，我见有人80行。而且通过这次，我了解了一个事，

我不是那些天才，可以自己发现发明算法。
我只能积累前人创造的知识，
有时一个知识点，一个关键词，一个方法，真的胜过自己乱搞。

但是我明明在期间也想到了map套map的形如字典树的形式，为什么没有采用那种方法呢。我回忆，是因为不确定的方法，以及思维上的疏忽，我当时认为每一位都会有10个数字的分支，算下来就是10^40个分支了，这个数字吓到了我。导致我没有意识到，很多分支不会存在，最多的情况只会有10万个，而且实际上只有 3567669 个。

为什么会这样呢，是对自己的不信任，是没有否定之否定。思维深度不够，不够严谨，不够周密，不够勇敢。如果能够敢于将错误的方法，思路也进行思考、分析。可能就不会这么痛苦了。

但是我害怕痛苦吗。
