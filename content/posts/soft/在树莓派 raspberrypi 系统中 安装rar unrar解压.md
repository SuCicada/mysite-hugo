参考
https://raspberrypi.stackexchange.com/questions/3617/how-to-install-unrar-nonfree

> 1.卸载unrar-free。
$ sudo apt-get remove unrar-free
\
2.通过编辑确保您拥有源存储库/etc/apt/sources.list。
$ cat /etc/apt/sources.list
\# Default repository
deb http://archive.raspbian.org/raspbian wheezy main contrib non-free rpi
\# Source repository to add
deb-src http://archive.raspbian.org/raspbian wheezy main contrib non-free rpi
\
3.同步apt数据库。
$ sudo apt-get update
\
4.创建一个工作目录并移入其中。该unrar-nonfree命令将在此目录中构建。
$ cd $(mktemp -d)
\
5.安装所需的依赖项unrar-nonfree。
$ sudo apt-get build-dep unrar-nonfree
\
6.下载unrar-nonfree源代码并构建.deb软件包。
$ sudo apt-get source -b unrar-nonfree
\
7.安装生成的.deb包。它的名称取决于版本unrar-nonfree。
$ sudo dpkg -i unrar*.deb

如果第六步报错如下:

```
dpkg-buildpackage: info: binary-only upload (no source included)
W: 由于文件'unrar-nonfree_5.3.2-1+deb9u1.dsc'无法被用户'_apt'访问，已脱离沙盒并提权为根用户来进行下载。 - pkgAcquire::Run (13: 权限不够)
```
就不要sudo了,  进入root用户再执行命令试试


