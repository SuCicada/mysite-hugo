我用的是windows。你可以试试以**管理员模式运行终端**。

我在用python安装第三方库时，使用如下方式

```
pip install xxxx.whl
```
但是无论如何都只显示到

> Installing collected packages: xxxx

然后就没了，和网上说的出来什么success提示完全不，然后我通过import来验证，也是没有安装上的。
一定要出现

> Successful installed xxxx

才算成功。
如果你的问题不是这样的，请查查看别的家的博客。



花了一个晚上都没有安上pygame。
于是第二天在整整一个下午琢磨了两个小时为什么。重启电脑，将python卸载再安装，卸载再安装，卸载再安装。删除easy_install 重装ez。删除pip，重装pip。重装wheel，但是如上面那样也失败了。然后我终于发现问题所在了。
我把powershell当成了是默认的管理员模式，于是我就一直处于安装不成功的地步。
所以说，如果你也安装不上，试试**管理员模式运行终端**。

真的是气，那些三步安装库的人根本不说。