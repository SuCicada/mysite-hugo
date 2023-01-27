#原代码来自
https://code.activestate.com/recipes/134892-getch-like-unbuffered-character-reading-from-stdin/
同时支持windows和unix平台

```
class _Getch:
    """Gets a single character from standard input.  Does not echo to the
screen."""
    def __init__(self):
        try:
            self.impl = _GetchWindows()
        except ImportError:
            self.impl = _GetchUnix()

    def __call__(self): return self.impl()


class _GetchUnix:
    def __init__(self):
        import tty, sys

    def __call__(self):
        import sys, tty, termios
        fd = sys.stdin.fileno()
        old_settings = termios.tcgetattr(fd)
        try:
            tty.setraw(sys.stdin.fileno())
            ch = sys.stdin.read(1)
        finally:
            termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
        return ch


class _GetchWindows:
    def __init__(self):
        import msvcrt

    def __call__(self):
        import msvcrt
        return msvcrt.getch()


getch = _Getch()
```
使用的时候可以这么用,比如输入时不仅可以将输入的字符显示在屏幕上,就像是input(), 同时还可以对其每个字符都在输入时进行操作, 最后按下回车键退出输入
```
import sys
ss =''
while True:
    s = Getch()
    if s == "\r":  # 注意回车键是 \r, 不是 \n
        break
    print(s,end='')
    sys.stdout.flush()  # 刷新缓冲区,不然无法显示到屏幕上
    ss += s
print()
print(ss)

```

还可以在类中加上
```
def __str__(self):     
	return self.impl  # 返回是其实是个字符串
```
然后使用:
```
str = str(_Getch())  # 自动调用__str__(self)方法
```
----------------------------------------
#然后是我的精简魔改版

```
def Getch():
    def _GetchUnix():
        import sys, tty, termios
        fd = sys.stdin.fileno()
        old_settings = termios.tcgetattr(fd)
        try:
            tty.setraw(sys.stdin.fileno())
            ch = sys.stdin.read(1)
        finally:
            termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
        return ch

    def _GetchWindows():
        import msvcrt
        return msvcrt.getch()

    try:
        impl = _GetchWindows()
    except ImportError:
        impl = _GetchUnix()
    return impl
```
使用:
```
str = Getch()
```
即可

---------------------------------------
#魔改精简版2
上一个代码如果把Getch()放到循环里会有个问题,就是上下左右键可以控制光标的移动,这有些惊奇,但也是我不想要的
```
class Getch():
    def __init__(self):
        try:
            self.impl = self._GetchWindows()
        except ImportError:
            self.impl = self._GetchUnix()

    def __str__(self):
        return self.impl

    def _GetchUnix(self):
        import sys, tty, termios
        fd = sys.stdin.fileno()
        old_settings = termios.tcgetattr(fd)
        try:
            tty.setraw(sys.stdin.fileno())
            ch = sys.stdin.read(1)
        finally:
            termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
        return ch

    def _GetchWindows(self):
        import msvcrt
        return msvcrt.getch()
```
使用
```

import sys
ss =''
while True:
    s = Getch()
    s = str(Getch())
    if s == "\r":
        break
    print(s,end='')
    sys.stdout.flush()
    ss += s
```
-------
#方法3.使用getch库
这是一个仿照windows下conio.h中的getch()函数的库
https://pypi.org/project/getch/

-------------------
产生这个问题的原因是使用tcp连接通信,在终端输入一句话时,如果只输入了一半,突然接收到了对方发来的消息,就会从光标处打印在屏幕上,那么我输入的话就被打断了.
查了很多网页,不知道getch这么个东西,学C语言时压根不知道,太差了.