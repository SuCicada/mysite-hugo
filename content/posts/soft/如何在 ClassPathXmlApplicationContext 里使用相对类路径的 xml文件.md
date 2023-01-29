参考https://blog.csdn.net/upxiaofeng/article/details/53332226
```java
URL a = this.getClass().getResource("");  // 获取ClassPath的绝对URI路径
// or  URL a = 写你的类名.getResource("");   // 这里是反射的知识,
String aa = a.toString();   //  转字符串
int begin = aa.indexOf("/bin")+5;  
aa = aa.substring(begin);  // 切去   aa就是了

```
比如
```java
 String xmlPath = MainApp2.class.getResource("").toString();
 xmlPath = xmlPath.substring(xmlPath.indexOf("/bin/")+5);
```

