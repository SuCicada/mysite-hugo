始苦寻无果，终幸得其所

参考项目的issue
> [https://github.com/doujiang24/lua-resty-kafka/issues/5](https://github.com/doujiang24/lua-resty-kafka/issues/5)

这个issue已经将问题说的很明白了
在我电脑上当时的表现就是lua从kafka获取到的broker的host一直是`ubuntu`
而我明明给lua中配置的host是ip地址。
而ubuntu这个域名在/etc/hosts中有设置，但是在我去掉域名解析，重启电脑之后，还是没有解决。
因为有这个被动ip转成了本地域名的灵异现象存在，使得lua-resty-kafka库无法解析域名，导致生产者无法向kafka推送数据。

而解决办法则是在kafka的配置文件 `server.properties`中设置

```host.name= {你的ip或域名}```

----
更新: 原因研究

根据:[kafka主机名解析hostname](https://blog.csdn.net/lvtula/article/details/90175432)
 
 我们查看zookeeper中关于kafka的brokers的信息
 
>  [zk: localhost:2181(CONNECTED) 0] get /brokers/ids/0
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://ubuntu:9092"],"jmx_port":-1,"host":"ubuntu","timestamp":"1584515322686","port":9092,"version":4}

从中果然发现了`ubuntu:9092`，所以问题出在zookeeper。

然后继续搜索，找到
 [如何避免将Kafka broker机器的hostname注册进zookeeper](https://blog.csdn.net/wxlchinaren/article/details/83303708)

其中提到了配置文件的注释提示信息，翻回去一看
> \# Hostname and port the broker will advertise to producers and consumers. If not set, 
> \# it uses the value for "listeners" if configured.  Otherwise, it will use the value
> \# **returned from java.net.InetAddress.getCanonicalHostName().**
>#advertised.listeners=PLAINTEXT://your.host.name:9092

果然，如果没有配置就使用` java.net.InetAddress.getCanonicalHostName()`获得。
到服务器上使用命令执行一看

```java
java> java.net.InetAddress.getLocalHost().getCanonicalHostName();
java.lang.String res0 = "ubuntu"
```
果然如此！

---

那么getLocalHost又是怎么得到的hostname呢，根据IDE的调试功能最终得到
`    public native String getLocalHostName() throws UnknownHostException;`
定义，这是一个native方法，调用的是`Java_java_net_Inet6AddressImpl_getLocalHostName`方法

根据：[JVM源码系列: java InetAddress.getLocalHost() 在linux里实现
](https://blog.csdn.net/raintungli/article/details/8191701)
 得知是根据系统的hostname决定的
 
 使用了`hostname xxx`临时改变机器hostname之后，测试发现果然如此。

java得到的hostname也变了。

---

那么要亲眼看一下：
下载openjdk的jdk8源码，查看到 `jdk-83bbe56ecea1\src\solaris\native\java\net\Inet6AddressImpl.c`文件下的68行为
`Java_java_net_Inet6AddressImpl_getLocalHostName`
```
JNIEXPORT jstring JNICALL
Java_java_net_Inet6AddressImpl_getLocalHostName(JNIEnv *env, jobject this) 
 ```
其中有一个`JVM_GetHostName`是被博客们广泛查看的。
那么找到这个函数的定义，在`jdk-83bbe56ecea1\src\share\javavm\export\jvm.h`
中第1302行
```
JNIEXPORT int JNICALL
JVM_GetHostName(char* name, int namelen);
```

但是要继续寻找就不能在这里了。
在查看了[JNI/NDK开发指南（一）—— JNI开发流程及HelloWorld](https://blog.csdn.net/xyang81/article/details/41777471) 之后得知要想明白这个方法更具体的定义要去Java编译器中，那么让我们去下载JVM虚拟机中的源码。

-----
从openjdk官网下载java8的 hotspot源码。
经过查找，发现在`hotspot-d17814ea88e3\src\share\vm\prims\jvm.cpp`的第4062行
```
JVM_LEAF(int, JVM_GetHostName(char* name, int namelen))
  JVMWrapper("JVM_GetHostName");
  return os::get_host_name(name, namelen);
JVM_END
```
也就是博客们俗称的宏定义，怪我，完全不知道这玩意在这种地方。
其中我们发现一个os域下的`get_host_name`方法。
然后寻找到`hotspot-d17814ea88e3\src\share\vm\runtime\os.hpp`中的第753行。
```
static int get_host_name(char* name, int namelen);
```
和
`hotspot-d17814ea88e3\src\os\linux\vm\os_linux.inline.hpp`中第225行
```
inline int os::get_host_name(char* name, int namelen) {
  return ::gethostname(name, namelen);
}
```
问题就出在这个`::gethostname(name, namelen)`函数
这是一个系统级的函数

----
待查

