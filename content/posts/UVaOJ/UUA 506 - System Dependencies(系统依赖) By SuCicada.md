---
date: 2020/09/20
---
>例题6-21 系统依赖（System Dependencies, ACM/ICPC World Finals 1997, UVa506）
软件组件之间可能会有依赖关系，例如，TELNET和FTP都依赖于TCP/IP。你的任务是
模拟安装和卸载软件组件的过程。首先是一些DEPEND指令，说明软件之间的依赖关系（保
证不存在循环依赖），然后是一些INSTALL、REMOVE和LIST指令，如表6-1所示。
表6-1 指令说明

| 指令 |说明|
| --- | --- |
| DEPEND item1 item2 [item3 …] |item1依赖组件item2, item3, …
|INSTALL item1 |安装item1和它的依赖（已安装过的不用重新安装） |
|REMOVE item1 | 卸载item1和它的依赖（如果某组件还被其他显式安装的组件所依赖，则不能卸载这个组件） |
| LIST |输出所有已安装组件
>在INSTALL指令中提到的组件称为显式安装，这些组件必须用REMOVE指令显式删除。
同样地，被这些显式安装组件所直接或间接依赖的其他组件也不能在REMOVE指令中删除。
每行指令包含不超过80个字符，所有组件名称都是大小写敏感的。指令名称均为大写字母。
**Sample Input**
DEPEND TELNET TCPIP NETCARD
DEPEND TCPIP NETCARD
DEPEND DNS TCPIP NETCARD
DEPEND BROWSER TCPIP HTML
INSTALL NETCARD
INSTALL TELNET
INSTALL foo
REMOVE NETCARD
INSTALL BROWSER
INSTALL DNS
LIST
REMOVE TELNET
REMOVE NETCARD
REMOVE DNS
REMOVE NETCARD
INSTALL NETCARD
REMOVE TCPIP
REMOVE BROWSER
REMOVE TCPIP
END
**Sample Output**
DEPEND TELNET TCPIP NETCARD
DEPEND TCPIP NETCARD
DEPEND DNS TCPIP NETCARD
DEPEND BROWSER TCPIP HTML
INSTALL NETCARD
Installing NETCARD
INSTALL TELNET
Installing TCPIP
Installing TELNET
INSTALL foo
Installing foo
REMOVE NETCARD
NETCARD is still needed.
INSTALL BROWSER
Installing HTML
Installing BROWSER
INSTALL DNS
Installing DNS
LIST
NETCARD
TCPIP
TELNET
foo
HTML
BROWSER
DNS
REMOVE TELNET
Removing TELNET
REMOVE NETCARD
NETCARD is still needed.
REMOVE DNS
Removing DNS
REMOVE NETCARD
NETCARD is still needed.
INSTALL NETCARD
NETCARD is already installed.
REMOVE TCPIP
TCPIP is still needed.
REMOVE BROWSER
Removing BROWSER
Removing HTML
Removing TCPIP
REMOVE TCPIP
TCPIP is not installed.
END


[本家地址](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=838&page=show_problem&problem=447)

---
这道题挺好玩的，（主要是不难）。
DAG什么的就行。具体设计都在注释里。
果然这类工程性设计才适合我吗，（哭，我才不要（仅限于此）
```cpp
#include<iostream>
#include<string>
#include<vector>
#include<sstream>
#include<map> 
#include<set>
#include<deque>
using namespace std;
                
              
/*              
    app -> List(app)
    map< string, vector<string>> 
    install : 根据依赖关系, 递归找父节点, 从父节点开始依次安装, 父节点设置为被依赖 
    remove  : 根据依赖关系, 先删自己, 在找父节点, 父节点若没有依赖-> 删,递归找父节点
    list    : 
*/            
                
class App{
public:
    int install;           // 1: 隐式安装 2:显式安装
    set<string> depended;  // 被依赖 
    int id;                // 安装id, 按照安装顺序递增
    vector<string> depend; // 依赖项
};

// 存储所有app  name -> App
map<string,App> table;

// 安装的
map<int,string> installed;

/* 依赖关系 k -dep-> list(v)  */
map<string,vector<string> > dep_table;

// 被依赖 k <-dep- list(v)
map<string,vector<string> > deped_table;

int installed_now_id = 0; // 用于记录最新app 的id

void list();
void newapp(string app);

// 存在未提及的app
// 循环依赖app: 设置其为被依赖
//      若父app 已安装: 不管
//      若父app 未安装: 递归,参数中依赖为1
void install(string app, int depended){
    App &aaa = table[app];
    if(aaa.install){
        if(!depended){
            cout<<"   "<<app<<" is already installed."<<endl;
        }
        return;
    }
    int size = aaa.depend.size();
    for(int i=0;i<size;i++){
        string father_app = aaa.depend[i];
        if(table[father_app].install == 0){
            install(father_app, 1);
        }
        // table[father_app].depended += 1;
        table[father_app].depended.insert(app);
    }
    // 循环走完父app们就应该都好了.(安装以及设置依赖项)
    // 安装自己
    cout<<"   Installing "<<app<<endl;
    if(!depended)
        aaa.install = 2;
    else
        aaa.install = 1;
    // aaa.depended += depended;

    // 记录在安装列表, id==-1 代表之前未安装过.
    if(aaa.id==-1){
        aaa.id = installed_now_id++;
        installed[aaa.id] = app;
    }
}

// 先判断有没有被依赖, 有则退出, 没有则
//     先删自己, 再对父app减一个依赖, 然后判断父app 若没有依赖就 递归remove 
int remove(string app, int depended){
    App &aaa = table[app];
    if(!aaa.install){
        cout<<"   "<<app<<" is not installed."<<endl;
        return 0;
    }
    int size = aaa.depend.size();
    if(!aaa.depended.empty()){
        cout<<"   "<<app<<" is still needed."<<endl;
        return 0;  // no 
    }
    if(depended && aaa.install==2){
        /* 某app依赖删除 and 显式安装 -> 不删 */
        return 0;
    }
    cout<<"   Removing "<<app<<endl;
    aaa.install = 0;
    for(int i=0;i<size;i++){
        string father_app = aaa.depend[i];
        App &faaa = table[father_app];
        faaa.depended.erase(app) ;

        if(faaa.depended.empty()){
            remove(father_app,1); 
            // faaa.install = 0;
        }
    }

    // 清除在安装列表, id==-1 代表之前未安装过.
    installed.erase(aaa.id);
    aaa.id = -1;
    return 1;
}

void list(){
    for(map<int,string>::iterator i=installed.begin();i!=installed.end();i++){
        cout<<"   "<<(i->second)<<endl;
    }
}
void show(){
    cout<<"==========================="<<endl;
    for(map<string,App>::iterator i=table.begin();i!=table.end();i++){
        cout<<"["<<i->first<<"]"<<endl;
        cout<<(i->second).install<<" "<<(i->second).id<<endl;
        for(int ii=0;ii<(i->second).depend.size();ii++){
            cout<<(i->second).depend[ii]<<" ";
        }
        cout<<endl;
        for(set<string>::iterator ii=(i->second).depended.begin();ii!=(i->second).depended.end();ii++){
            cout<<*ii<<" ";
        }
        cout<<endl;
    }
    cout<<"--------------------"<<endl;
}

void newapp(string app){
    if(!table.count(app)){
        App a;
        a.install=0;
        // a.depended=0;
        a.id=-1;
        table[app] = a;
    }
}

int main(){
    string str;
    stringstream ss;
    while(getline(cin,str)){
        do{
            ss.clear();
            ss<<str;
            cout<< str <<endl;;
            string head;
            ss>>head;

            string s;
            if(head == "DEPEND"){
                string app;
                ss>>app;
                newapp(app);
                while(ss>>s){
                    table[app].depend.push_back(s);
                    newapp(s);
                }
            }else if(head == "INSTALL"){
                // show();
                ss>>s;
                if(!table.count(s)){
                    newapp(s);
                }
                install(s,0);
            }else if(head == "REMOVE"){
                ss>>s;
                remove(s,0);
            }else if(head == "LIST"){
                list();
            }else if(head == "END"){
                break;
            }


            // show();


            getline(cin,str);
        }while(1);

    }
    return 0;
}

// AC at 2020/09/20
```

---
ps：没什么好说的，好几天才一道题，简直没救
