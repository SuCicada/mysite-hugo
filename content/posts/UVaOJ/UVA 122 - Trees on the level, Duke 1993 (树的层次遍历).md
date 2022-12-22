>例题6-7 树的层次遍历（Trees on the level, Duke 1993, UVa 122）
输入一棵二叉树，你的任务是按从上到下、从左到右的顺序输出各个结点的值。每个结
点都按照从根结点到它的移动序列给出（L表示左，R表示右）。在输入中，每个结点的左
括号和右括号之间没有空格，相邻结点之间用一个空格隔开。每棵树的输入用一对空括
号“()”结束（这对括号本身不代表一个结点），如图6-3所示。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200717234609833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
图6-3 一棵二叉树
注意，如果从根到某个叶结点的路径上有的结点没有在输入中给出，或者给出超过一
次，应当输出-1。结点个数不超过256。
**样例输入：**
(11,LL) (7,LLL) (8,R)
(5,) (4,L) (13,RL) (2,LLR) (1,RRR) (4,RR) ()
(3,L) (4,R) ()
**样例输出：**
5 4 8 11 13 4 7 2 1
not complete

[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=58)

----
 祭出对象大法

```cpp
#include<iostream>
#include<sstream>
#include<string>
#include<cstring>
#include<cstdio>
#include<cstdlib>
using namespace std;
class Node{
public:
    int value;
    Node* left;
    Node* right;
};

Node* queue[256];
void show(Node* root){
    memset(queue,0,sizeof(queue));
    int qbegin=0;
    int qend=0;
    int qlen = sizeof(queue)/sizeof(Node*); 
    stringstream out;

    queue[qend] = root;
    qend = (qend+1) % qlen; 

    while(qbegin!=qend){
        Node* nownode = queue[qbegin];
        // cout<<nownode->value<<endl;
        if(nownode!=root) out<<" ";

        if(nownode->value){
            out<<nownode->value;
        }else{
            out.str("not complete");
            // cout<<"not complete";
            break;
        }

        qbegin  = (qbegin+1) % qlen;
        if(nownode->left){
            queue[qend] = nownode->left;
            qend = (qend+1) % qlen; 
        }
            
        if(nownode->right){
            queue[qend] = nownode->right;
            qend = (qend+1) % qlen; 
        }
    }
    cout<<out.str()<<endl;
}

int main()
{
    string nodestr;
    int newtree = 1;
    Node* root;
    int badtree = 0;
    while (cin>>nodestr){
        if(newtree == 1){
            /* 新的树 */
            newtree = 0;
            root = new Node();
            // nownode = root;
            badtree = 0;
        }

        int comma = nodestr.find(',');
        if(comma == nodestr.npos){
            /* 树木结束了,该输出结果了  */

            if(badtree){
                cout<<"not complete"<<endl;
            }else{
                show(root);
            }
            newtree = 1;
        }else{
            Node* nownode = root;
            if(!badtree){
                int size = nodestr.size();
                int n = atoi(nodestr.substr(1,comma-1).c_str());
                for(int i=comma+1;i<size-1;i++){
                    Node** nextstep;
                    if(nodestr[i] == 'L'){
                        nextstep = &(nownode->left);
                    }else if(nodestr[i] == 'R'){
                        nextstep = &(nownode->right);
                    }

                    if(!(*nextstep)){
                        *nextstep = new Node();
                    }
                    nownode = *nextstep;
                }
                // cout<<nodestr<<" "<<comma<<" "<<size<<" "<<" "<<" ";
                // cout<<nownode<<" nownode->value  "<<nownode->value<<"   n "<<n<<endl;
                if(nownode->value){
                    badtree = 1;
                }else{
                    nownode->value = n;
                }
            }
        }
        
    }



    return 0;
}
// AC at 2020/3/17
```
----
开始还债
