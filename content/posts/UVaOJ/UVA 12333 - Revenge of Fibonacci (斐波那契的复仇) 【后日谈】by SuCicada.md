>### 正篇以及正确解题思路和代码参见
>### [UVA 12333 - Revenge of Fibonacci (斐波那契的复仇) by SuCicada](https://blog.csdn.net/su_cicada/article/details/104714087)

## **此篇为后日谈**

---

要说为什么专门开一篇来记录想法呢，主要是因为想说的太多了。首先从UVA的提交记录上来看，上一次答题是在足足1年之前了。
这么久以来都没有再好好做算法，感觉快要忘本了。而且出来之后脑子也不是那么灵光了，虽然更理性，但是却少了些抽象的想象力。

再说这道题，一开始我根本不知道什么字典序，完全是打表暴力比对的。把前40位算出来那里都是没错的。
但是之后我想的是：用map排个序，然后二分查找。但是这样会有问题，因为二分找到的可能并不是最小的，所以找到之后还需要分别找到同样匹配前缀的那一区域的数字，因为是map排过序的，所以他们都是挨着的。

但是之后在极端痛苦了两个晚上之后，我放弃了这种做法，经过测试计算，发现我电脑中g++在一秒钟可以进行4亿次运算，假设有5万个40位数字需要比对，4亿除以5万除以40 有20万呢，或者保险点我们取个1000。（一直到当时我都以为这道题的时间限制是1秒）

然后我就在死灰复燃的状态下在下2天晚上把位数比对写出了（暴力）。为了减少比对量，我打表了前3位，也就是记录下前三位的数字第一次出现的序数，而且这个序数是map中的序数，map中的序数和fibo的序数可是不同的。
而且map不能随机读取，怎么办，我又创造了两个数组专门存放键和值。

后来我还发现个位数的搜索起来比较慢，因为他们要暴力比对的数是最多的，所以我又用一个map来记录已经查找过的结果。

我真的是要疯，一直到昨天晚上我把这套模型整通。本地一跑，通过，time测下来 1.2秒，我巨喜。提交，time limit exceeded。把udebug上的样例全部跑了一遍，没有超过1.5秒的。我随机生成各种5万个数的组合，2秒之内绝对能过。

但是 time limit exceeded， time limit exceeded，time limit exceeded，time limit exceeded，time limit exceeded。
”难道是表打的太小了？“我又将打表位数升到4，升到5。然而为什么更慢了。

那会已经3点钟了，我感到生命力的衰减，在极度无望的情况下，我打开百度，UVA 12333，我真特么被这道题23333了，
>这道题考大数+字典树

我惊住，一个新的窗子仿佛打开了，一查，全懂了，完全懂了。我缩在床上颤抖不已，脑子瞬间就明白了我该怎么做，但是身体已经无法支撑下去，为了能保持住自己的生命力。我艰难的进入睡眠。

第二天我写，真的很快，因为我已经知道了，虽然遇到了oj离奇而严厉的 runtime error错误。但是我很冷静，因为我知道这个方法肯定是没错的，这条路一定是对的。就是这种知道目的的坚定。

当Accepted出现在屏幕上，我想哭了。太痛苦了，太刺激了，太疯狂了。再加上17个小时没有进食带来的虚弱感让我恍惚，感觉我活下来了。

劫后余生的感激之情。

而现在已经5点怅然若失感时不时在席卷我，跨时两个星期，4个夜晚的崩溃，20余小时的挣扎，20多次的错误。我太弱了，我为我的弱小感到伤心，但是我又为我的成功感到高兴。

算法拯救我，算法伤害我，算法摧残我，算法安慰我。

我时尝为我的自艾感到痛苦。

但是我害怕痛苦吗，不如说我正喜欢痛苦。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200307173321168.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
---
附赠更快的却更错的代码：
```cpp
#include<iostream>
#include<algorithm>
#include<iterator>
#include<vector>
#include<string>
#include<cstdio>
#include<iomanip>
#include<map>
#include<cmath>
#include<cstring>
using namespace std;

vector<string> fibonacci(100005);
map<string,int> fiboSorted;
vector<string> fiboName(100005);
vector<int> fiboIndex(100005);
int limitSite = 52;

string add(string a,string b){
    int aLen = a.size()-1;
    int bLen = b.size()-1;
    int aSiteNum = a[limitSite>aLen?aLen:limitSite];
    int bSiteNum = b[limitSite>bLen?bLen:limitSite];
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
        // cout<<r<<" "<<ten<<" "<<res<<endl;
    }
    // if(plusSite){
    res[0] = ten+'0';
    // }
    if(ten==0){ //不会进位
        string result = res.substr(1,limitSite+1);
        result[result.size()-1] = bSiteNum;
        return result;
    }else{
    // if(aSiteNum!=bSiteNum){ // 位数不等
        string result = res.substr(0,limitSite+1);
        result[result.size()-1] = !(bSiteNum-'0') +'0';
        return result;
    }
}
vector<string> fibo(100005);
// int find(string str,int fbegin,int fend);

// map<int,map<int,map<int,int> > > dict;
int dict[1004];

int presitename = 3;    

int find(string str){
    int presitenum10 = pow(10,presitename);
    int threenum = 0;// = (name[0]-'0')*100 + (name[1]-'0')*10 + (name[2]-'0');
    int len = str.size(); // 这个长度很重要
    int nowSiteName = presitename; // min(len,presitename); // 考虑数字要更小的情况, 比如12 我们要存12的位置, 不能是120
    for(int j=0;j<nowSiteName;j++){
        if(len >= j+1){ // 加1是因为要比对数字字符长度
            threenum += (str[j]-'0') * pow(10,nowSiteName-1-j);
        }
    }

    int newSite = pow(10,presitename-min(len,presitename));  // 这个是为了确定查找的区间, 要使用的数量级和长度相关



    // cout<<threenum<<endl;
    int index = dict[threenum];
    int end = -1;
    int nextSite = threenum+newSite;
    if(dict[nextSite] == -1){
        nextSite += 5* newSite;
    }
    // while((end == -1) && (nextSite <= presitenum10)){  // 防止 dict下一位没有东西
    //     end = dict[nextSite];
    //     cout<<nextSite<<" "<<dict[nextSite]<<" "<<end<<endl;
    //     nextSite += newSite;
    // }

    if(nextSite > presitenum10){
        end = 100001;
    }else{
        end = dict[nextSite]; 
    }

    // cout<<index<<" "<<end<<endl;
    // 要找合适的,先从index开始一个一个数比较,
    // 一位一位的比,如果相同,就记录fibo序数,取小,一直比到不相等,跳出
    int startSame = 0;
    int minFiboIndex = 100001;
    for(int i=index;i<end;i++){
        if( str.size() <= fiboName[i].size()){
            string fs = fiboName[i].substr(0,str.size()); 
            // cout<<i<<" "<<str<<" "<<fs<<endl;
            if(str == fs){
                minFiboIndex = min(fiboIndex[i],minFiboIndex);
                startSame = 1;
            }else{
                if(startSame == 1){
                    break;
                }
            }
        }
    }
    if(minFiboIndex == 100001){
        minFiboIndex = -1;
    }
    return minFiboIndex;
}   

map<string,int> answer;
int run(){

    fibonacci[0] = fibonacci[1] = "11";
    fiboSorted["1"] = 0;
    for(int i=2;i<=100000;i++){
        fibonacci[i] = add(fibonacci[i-2],fibonacci[i-1]);
        string s = fibonacci[i].substr(0,fibonacci[i].size()-1);
        fiboSorted[s] = i;
        // cout<<fibonacci[i]<<" "<<i<<" "<<fiboSorted[fibonacci[i]]<<endl;
        // cout<<setw(2)<<i<<" ";
        // cout<<" "<<setw(limitSite+1)<<fibonacci[i].substr(0,fibonacci[i].size()-1)<<" ";
        // cout<<setw(4)<<fibonacci[i].substr(fibonacci[i].size()-1);
        // cout<<endl;
        // stirng aa =
        // cout<<" "<<setw(4)<<
        // cin.get();
    }

    // copy(fibonacci.begin(),fibonacci.end(),ostream_iterator<string>(cout," |\n"));
    // cout<<fibonacci[99999]<<endl;
    // cout<<endl;
    memset(dict,-1,sizeof(dict));

    int i=0;
    for(map<string,int>::iterator m = fiboSorted.begin();m!=fiboSorted.end();m++,i++){
        // cout<<m->first<<" "<<m->second<<endl;
        fiboName[i] = m->first;
        fiboIndex[i] = m->second;

        string name = m->first;
        int index = i;
        int threenum = 0;// = (name[0]-'0')*100 + (name[1]-'0')*10 + (name[2]-'0');
        int len = name.size(); // 这个长度很重要
        int nowSiteName = presitename; // min(len,presitename); // 考虑数字要更小的情况, 比如12 我们要存12的位置, 不能是120
        for(int j=0;j<nowSiteName;j++){
            if(len >= j+1){ // 加1是因为要比对数字字符长度
                threenum += (name[j]-'0') * pow(10,nowSiteName-1-j);
            }
        }

        // cout<<threenum<<" "<<dict[threenum]<<" "<<index<<endl;   
        if(dict[threenum] == -1){ // 未记录
            dict[threenum] = index; // 因为递增遍历 map 数列, 所以第一次填入的 map 序号肯定是最小的
        }

    }

    // for(int i=0;i<sizeof(dict)/sizeof(int);i++){
    //     printf("%03d %6d %s\n ",i,dict[i],fiboName[dict[i]].c_str());
    //         // cout<<i<<dict[i]<<endl;
    // }
    // makedict();


// return 0;
    // int mapLen = i;
    int T;
    cin>>T;
    for(int i=0;i<T;i++){
        string n;
        cin>>n;
        int res;
        if(answer.count(n)){
            res = answer[n];
        }else{
            res = find(n);
            answer[n] = res;
        }
        printf("Case #%d: %d\n", i+1, res);
        // cout<<"find(n)<<endl;

        // int res = find(n,0,mapLen);
        // cout<<fiboIndex[res]<<endl;
    }
    // cout<<fiboSorted.size()<<endl;
    // cout<<(fiboSorted.begin()+1)->first<<endl;
}



int main()
{
    run();
    return 0;
}
```
