
一段代码

```java
object ASD {
    def main(args: Array[String]): Unit = {
        def f(a: Any): Unit = {
            println(a)
        }

        f("sfsfsfdsdfd")

    }
}
```

如果我们想进行类似`ASD.main.f(xx)`或`ASD.f(xx)`的操作, 是否可行.
事实是残酷的, 它告诉我们不可行.

那么下面从反编译角度来探究为什么不可行:

首先我们打开编译后的 class 文件所在目录
有2个文件: `ASD.class`和`ASD$.class`

**ASD.class 节选**
```java
public final class ASD {
  public static void main(String[] paramArrayOfString) {
    ASD$.MODULE$.main(paramArrayOfString);
  }
}
```
这个不重要

**ASD$.class 节选**
```java
public final class ASD$ {
  public static final ASD$ MODULE$;
  
  private final void f$1(Object a) {
    Predef$.MODULE$.println(a);
  }
  
  public void main(String[] args) {
    f$1("sfsfsfdsdfd");
  }
  
  private ASD$() {
    MODULE$ = this;
  }
}
```

发现了吗, 内部函数 f 被编译为 私有的 final 方法f$1.

所以我们是无法从外部直接调用的.

但是理伦上我们是可以在 ASD 内部调用这个函数的.
是真的吗,你也是这么想的吗.
我们试试 `this.f$1(xxx)`
但是很遗憾, 
>value f$1 is not a member of object ASD
编译器看不懂我们意图, 甚至IDE都直接否决了我们的异想.

那么我们就是要调用这个方法怎么办呢. 
难道 javac 就注定要成为遥不可及的高冷存在吗. 
我们难道就没有更进一步的发展了吗.

不, 伟大的 semi 酱告诉你,阻碍是不存在的.
"反射, JVM最大的外挂."

```java
val clazz = Class.forName("scala.ASD$")
val m = clazz.getDeclaredMethod("f$1", classOf[Object])
val o = clazz.getDeclaredField("MODULE$").get(null)
m.setAccessible(true)
m.invoke(o, "234234234".asInstanceOf[Object])
```

わぁ、ありがとう、せみちゃん。


