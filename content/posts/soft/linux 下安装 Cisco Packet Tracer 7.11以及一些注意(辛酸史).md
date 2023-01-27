https://blog.csdn.net/qq_35882901/article/details/77652571
https://linux.cn/article-5576-1.html

开启登录问题
https://blog.csdn.net/u012321131/article/details/78587383

到软件的根目录下  执行以下脚本
`sudo ./set_ptenv.sh
sudo ./set_qtenv.sh`

**一定按步骤走  **
**安装在默认的/opt/pt    如果因为安装在家目录而导致闪退  就重新安装在默认路径下**

一些依赖库
`sudo apt install libqt5scripttools5`
`sudo apt-get install  libqt5multimedia5-plugins` 

-----------------------
2018/9/18
因为3560的三层交换机的端口信息不显示,忍不了linux的bug,下了6版本的linux版本.
安装失败,
换回7版本,(核心已转储).....made?
一个晚上的尝试,发现只是在当前的这个用户下运行不了,.....放弃了,
**那么用wine吧**
满心欢喜的安装好了,然后打算慢慢的把7版本的实验在6版本上重打一遍,
但是,cpu占用28,温度81.......耗不起
**那么赞颂我们伟大的虚拟机吧**
virtualbox的无缝模式太强大了,wine什么的掰掰吧,

