参考于[cin、cin.get()、cin.getline()、getline()、gets()等函数的用法](https://www.cnblogs.com/wanghao111/archive/2009/09/05/1560822.html)

1. **cin>>**
(1) >> 是会过滤掉不可见字符（如 空格 回车，TAB 等）
2. **cin.get()**
(1) cin.get(char);//接受一个字符
(2) cin.get(char*,接收字符数目);//可以接收空格，接收数目=实际接收字符+1个'\0'
3. **cin.getline()**
(1) cin.getline(char*,接收数目);
//可以接收空格，接收数目=实际接收字符+1个'\0'
(2) cin.getline(char*,接收数目,结束字符) 
//当第三个参数省略时，系统默认为'\0' 
//cin.getline(str,5,'a');当输入abcdef时输出abcd，输入jkaljkljkl时，输出jk


`#include<string>`
1.getline(cin,string str);//可以接收空格
2.gets(char *str);//可以接收空格
3.getchar()//getchar()是C语言的函数，C++也可以兼容，但是尽量不用或少用；