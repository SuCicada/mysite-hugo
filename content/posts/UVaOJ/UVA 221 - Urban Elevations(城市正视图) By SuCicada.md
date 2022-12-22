>例题5-12 城市正视图（Urban Elevations, ACM/ICPC World Finals 1992, UVa221）
如图5-4所示，有n（n≤100）个建筑物。左侧是俯视图（左上角为建筑物编号，右下角
为高度），右侧是从南向北看的正视图。
图5-4 建筑俯视图与正视图
输入每个建筑物左下角坐标（即x、y坐标的最小值）、宽度（即x方向的长度）、深度
（即y方向的长度）和高度（以上数据均为实数），输出正视图中能看到的所有建筑物，按
照左下角x坐标从小到大进行排序。左下角x坐标相同时，按y坐标从小到大排序。
输入保证不同的x坐标不会很接近（即任意两个x坐标要么完全相同，要么差别足够大，
不会引起精度问题）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201007184126835.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70#pic_center)
**Sample Input**
14
160 0 30 60 30
125 0 32 28 60
95 0 27 28 40
70 35 19 55 90
0 0 60 35 80
0 40 29 20 60
35 40 25 45 80
0 67 25 20 50
0 92 90 20 80
95 38 55 12 50
95 60 60 13 30
95 80 45 25 50
165 65 15 15 25
165 85 10 15 35
0
**Sample Output**
For map #1, the visible buildings are numbered as follows:
5 9 4 3 10 2 1 14


[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=835&page=show_problem&problem=157)

----
x，y：建筑位置，z：建筑高度
存储所有的建筑的头尾x坐标。
然后依次遍历建筑，判断每个建筑是否能够显现。
判断的方式就是依次遍历每个建筑，然后针对每个建筑遍历x坐标，寻找能够显示出建筑的x坐标范围。
判断能否显示的方式：先判断x坐标范围是否在建筑x范围内，若在，遍历建筑群，比较同样在x范围内的建筑的y坐标。以此得出是否能够显示此建筑。
（这已经有3层循环了，得利于建筑最多只有100个）

---
```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
/*
两个方块的比较:
比 x, y, 高
先比 x, 小的 小
    x 相同 比


存方块 vector<Build> 
存x   map<int, vector<int> >  x->List[index]



插入新的build
比x, 若已有 
     若无则插入, 
            =>  得到前一个: 得到前一个中组中的最后一个(即宽度最大的)  比其是否 > this
                                                                若 < : 无视
                                                                若 > : 比y
                得到后一个: 比 this.宽度+x 是否大于 后一个的x
    
    
- 
- 得到显示的方块
- 排序

*/

class Build{
public:
    double x,y,w,d,h;
    int index;
    bool operator <(const Build &bb){
        if(x < bb.x) return true;
        else if(x==bb.x) return y < bb.y;
        else return false;
    }
};
vector<Build> buildList;

vector<int> xList;

int isShow(int bi, int xx){
    if(buildList[bi].x < xx && buildList[bi].x+buildList[bi].w > xx){
        for(int i=0;i<buildList.size();i++){
            if(i!=bi && 
                buildList[i].x < xx && buildList[i].x+buildList[i].w > xx && 
                buildList[i].y < buildList[bi].y && 
                buildList[i].h >= buildList[bi].h){
                return 0;
            }
        }
        return 1;
    }
    /* 不在范围 */
    return -1;
}

int main(){
    int T;
    int N=1;
    while(cin>>T && T){
        buildList.clear();
        xList.clear();
        for(int i=1;i<=T;i++){
            double x,y,w,d,h;
            cin>>x>>y>>w>>d>>h;
            Build b;
            b.x = x;
            b.y = y;
            b.w = w;
            b.d = d;
            b.h = h;
            b.index = i;
            buildList.push_back(b);
            xList.push_back(x);
            xList.push_back(x+w);
        }

        sort(buildList.begin(), buildList.end());
        sort(xList.begin(), xList.end());

        vector<int>::iterator vi = unique(xList.begin(), xList.end());
        xList.erase(vi,xList.end());

        if(N>1){
            cout<<endl;
        }
        cout<<"For map #"<< N++ <<", the visible buildings are numbered as follows:"<<endl;
        int one = 0;
        for(int i=0;i<buildList.size();i++){
            int show = 0;
            for(int j=0;j<xList.size()-1;j++){
                int xi = (xList[j] + xList[j+1]) / 2;
                show = isShow(i,xi);
                if(show==1){
                    break;
                }
            }
            if(show==1){
                if(one){
                    cout<<" ";
                }
                cout<<buildList[i].index;
                one = 1;
            }
        }
        cout<<endl;

    }
    return 0;
}

// AC at 2020/09/10
```

----
ps：这个题是后面雕塑家的铺垫，百思不得，痛苦万分，想到了一个方向但是由于不知道对错而不敢继续深入，这是一个问题，理应的思维方式是dfs形式，但是我由于踌躇变成了bfs。要冷静，要好好思考，不能被外物干扰。
现实中的诱惑越来越多，甚至医治我们的“良药”也变成了诱惑，任何缓解型“药物“都有成瘾性。我们要想一个办法才行。
同伴有时候救命稻草，有时却又变成隐形荆棘。坚定自我，我们还是需要同伴的，因为即便是自我解决方案也是依靠创造虚假同伴来间接解决。
人的需求之一：群体的认可。人作为群体趋向型生物，离开群体便会开始走向异变。我在思考：群居的必须性，如何做到呢。

