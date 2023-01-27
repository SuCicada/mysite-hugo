见以下链接
[istringstream、ostringstream、stringstream 类介绍 .](http://www.cnblogs.com/gamesky/archive/2013/01/09/2852356.html)

0、C++的输入输出分为三种：

(1)基于控制台的I/O

```
#include<iostream>
```

    

(2)基于文件的I/O

```
#include<fstream>
```
        

(3)基于字符串的I/O

```
#include<sstream>
```


----------


str()：使istringstream对象返回一个string字符串

```
stringstream::clear()//多次使用，先清空此对象的流，不能使用stream.str(""); 
                     //实际上，它并不清空任何内容，它只是重置了流的状态标志而已 
```
    

ps:但如果你要在程序中用同一个流，反复读写大量的数据，将会造成大量的内存消耗，                    这时候，需要适时地清除一下缓冲 (用 stream.str("") )。