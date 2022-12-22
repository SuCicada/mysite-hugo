>例题6-9 天平（Not so Mobile, UVa 839）
输入一个树状天平，根据力矩相等原则判断是否平衡。如图6-5所示，所谓力矩相等，
就是WlDl=WrDr，其中Wl和Wr分别为左右两边砝码的重量，D为距离。
采用递归（先序）方式输入：每个天平的格式为Wl，Dl，Wr，Dr，当Wl或Wr为0时，表
示该“砝码”实际是一个子天平，接下来会描述这个子天平。当Wl=Wr=0时，会先描述左子天
平，然后是右子天平。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200725223242718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
**样例输入：**
1
0 2 0 4
0 3 0 1
1 1 1 1
2 4 4 2
1 6 3 2
**Sample Output**
YES
【注意】 输出结果之间空一行

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=780)

----
递归走，一边输入每一结点，一边递归判断这个结点的左结点结果（左子树重量总和）和右结点结果（右子树重量总和）是否满足要求。
这样的话在建立树的时候也就直接计算结束了。

```cpp
#include<iostream>
using namespace std;


int lair(){
    int wl,dl,wr,dr;
    cin>>wl>>dl>>wr>>dr;
    if(wl == 0){
        /* have left branch */
        wl = lair();
    }

    if(wr == 0){
        wr = lair();
    }

    if(wl==0 || wr==0 || wl*dl != wr*dr){
        return 0;
    }else{
        return wl+wr;
    }
}


int main(){
    int T;
    cin>>T;
    while(T--){
        int res = lair();
        if(res == 0){
            cout<<"NO"<<endl;
        }else{
            cout<<"YES"<<endl;
        }
        if(T>0){
            cout<<endl;
        }
    }
}

// AC at 2020/07/21 23:58
```

---
ps：诸君，我喜欢代码。
20200725
最近还真是喜怒不定，感觉我已经没有容身之所了，开玩笑，那也是自以为的。
睡眠不足，精神紧张，导致大脑开始痛了，紧着着就是心痛了。连锁反应。
最近又想起了DELA大的【[我对孤独一无所知](https://www.bilibili.com/video/av42228720/)  】。如今重温，感受深同。
本周听完了盗墓笔记十年（广播剧）。那真是一个终点，时隔五年之久，终于补上了欠缺的一页。能够粉身碎骨万箭穿心之后，能够抵达终点，那是一种幸运，仍能不忘天真，那是一种强大。桃李春风一杯酒，江湖夜雨十年灯。

所有的一切又回到了原点，如今我才明白，从来也没有人会自愿来真正拯救你。他们每个人都在生死线上挣扎不能脱身，这是一个人人自危的时代，每个人都脆弱无比，他们比你还要害怕危险，害怕受到伤害。依靠他们是不可取的，在真正找到归宿之前不要选择盛放，那会让你沉溺于倾诉的毒瘾之中不能自拔，轻则痛彻心扉，重则伤筋断骨。
我们只要能够活下去就是一大胜利，这是第一步，第二步就是控制自己的活法，这样才能使得我们不会再回想起凝望深渊时的情景。但这远远还比不上直面旧日支配者吧，だぶん，哈哈。不过心理世界之深渊，着实诡秘莫测。

看来我依仗的也只能是这个了，算法救我实属不假。这其实是很危险的，将精神寄居于一个事物上，也就要要背负上不可预测之概率 of 流离失所之打击感。
等到什么时候我不会再将自己的生死寄托于不可获得之物上，我便安全了。
When I no longer need the psychological placebo for other people to give, I can reach my own paradise.
