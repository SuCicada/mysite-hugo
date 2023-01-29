先说概念：
> **Scala的尾递归会被编译器自动优化成循环**

<a href="#main0" target="_self">主题直通车</a>

### 先来简单看下一个简单验证方法

#### 对比普通的递归：

```java
    def fun2(x: Int): Int = {
        if (x == 1)
            throw new Exception("nooo")
        else
            fun2(x - 1) + 0
    }
```
##### 结果：

```java
Exception in thread "main" java.lang.Exception: nooo
	at Main$.fun2(Main.scala:17)
	at Main$.fun2(Main.scala:19)
	at Main$.fun2(Main.scala:19)
	at Main$.fun2(Main.scala:19)
	at Main$.fun2(Main.scala:19)
	。。。。
	at Main$.fun2(Main.scala:19)
	at Main$.main(Main.scala:25)
	at Main.main(Main.scala)
```
##### 现象：
我们能看到在递归结束前，这个方法已经进入自己很多次了。

---

#### 尾递归
```java
    def fun(x: Int): Int = {
        if (x == 0)
            throw new Exception("nooo")
        else
            fun(x - 1)
    }
```
**结果：**

```java
Exception in thread "main" java.lang.Exception: nooo
	at Main$.fun(Main.scala:9)
	at Main$.main(Main.scala:25)
	at Main.main(Main.scala)
```

##### 现象
只进入这个方法一次

#### 结论
从中我们能看到尾递归的确不会一直调用自己

----
----

<h1 id="main0">。</h1>

## 字节码验证
假设咱们知道了JITWatch的使用。

1.先看主函数，从bytecode区域，我们能看到在标号为8 这一行
```python
 8: invokevirtual   #30  // Method fun:(I)I
```
得知 `invokevirtual`指令是用来执行函数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200117182545175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)

2. 我们看“普通递归”，在红线那一行 `13: invokevirtual   #20  // Method fun2:(I)I`，对应着`fun2(x-1)+1`这条命令。
这代表 这里的调用自己，是真的在调用自己。使用`invokevirtual`再次进入自己

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200117183250160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)


3. 这次呢，是尾递归，我们发现这次没有`invokevirtual` 了呢，为之代替的是一个名为`goto`的指令。他goto到哪了呢，到**0**这行了。也就是这个函数的开头。这也就是循环的代表。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020011718361381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
---

### 结论：
看来确实如此呢

> **Scala的尾递归会被编译器自动优化成循环**

