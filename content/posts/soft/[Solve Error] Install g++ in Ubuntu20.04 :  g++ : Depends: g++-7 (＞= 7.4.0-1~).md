这是在Windows10上的Ubuntu WSL环境中遇到的问题。
目前的Ubuntu版本是`Ubuntu 20.04.1 LTS`。并且使用了阿里的apt镜像源。

**先说结论，Ubuntu版本之高使得本机使用apt源中没有所需的库版本。所以可以尝试将apt源换回官方源。然后`apt update`再安装g++。**

以下是断案过程。

----

在使用命令`sudo apt install g++`遇到了依赖问题。整个依赖链排查结果如下：
```shell
sucicada@20200702-143805:/etc/apt/sources.list.d$ sudo apt install g++
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 g++ : Depends: g++-7 (>= 7.4.0-1~) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
sucicada@20200702-143805:/etc/apt/sources.list.d$ sudo apt install g++-7
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 g++-7 : Depends: libstdc++-7-dev (= 7.5.0-3ubuntu1~18.04) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
sucicada@20200702-143805:/etc/apt/sources.list.d$ sudo apt install libstdc++-7-dev
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 libstdc++-7-dev : Depends: libc6-dev (>= 2.13-0ubuntu6) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
sucicada@20200702-143805:/etc/apt/sources.list.d$ sudo apt install libc6-dev
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 libc6-dev : Depends: libc6 (= 2.27-3ubuntu1.3) but 2.31-0ubuntu9.1 is to be installed
             Depends: libc-dev-bin (= 2.27-3ubuntu1.3) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

反正层层找下来问题是这个：
```
 libc6-dev : Depends: libc6 (= 2.27-3ubuntu1.3) but 2.31-0ubuntu9.1 is to be installed
```
库中的包版本和系统已安装的不符合。这个包是底层包不能删。



接下来我们使用`apt policy libc6-dev libc6`来检查下依赖关系。如下
```shell
sucicada@20200702-143805:/etc/apt/sources.list.d$ apt policy libc6-dev libc6
libc6-dev:
  Installed: (none)
  Candidate: 2.27-3ubuntu1.3
  Version table:
     2.27-3ubuntu1.3 500
        500 http://mirrors.aliyun.com/ubuntu bionic-updates/main amd64 Packages
        500 http://mirrors.aliyun.com/ubuntu bionic-proposed/main amd64 Packages
     2.27-3ubuntu1.2 500
        500 http://mirrors.aliyun.com/ubuntu bionic-security/main amd64 Packages
     2.27-3ubuntu1 500
        500 http://mirrors.aliyun.com/ubuntu bionic/main amd64 Packages
libc6:
  Installed: 2.31-0ubuntu9.1
  Candidate: 2.31-0ubuntu9.1
  Version table:
 *** 2.31-0ubuntu9.1 100
        100 /var/lib/dpkg/status
     2.27-3ubuntu1.3 500
        500 http://mirrors.aliyun.com/ubuntu bionic-updates/main amd64 Packages
        500 http://mirrors.aliyun.com/ubuntu bionic-proposed/main amd64 Packages
     2.27-3ubuntu1.2 500
        500 http://mirrors.aliyun.com/ubuntu bionic-security/main amd64 Packages
     2.27-3ubuntu1 500
        500 http://mirrors.aliyun.com/ubuntu bionic/main amd64 Packages
```
（小科普：\***代表库已安装，100代表已安装的地方，500可安装的远端源。这里能看到100和500对应有的不同版本）

通过以上信息我们能知道libc6-dev 这个库的安装在apt源中的候选是2.27版本，但是已经安装的libc6是2.31版本。而且因为我使用了阿里镜像源。所以由此可见阿里源中的版本知道2.27。冲突就是这么产生的。

所以既然如此，那么我们就把apt源换回官方源（好在当时sourse.list文件只是rename），然后`apt update`。再安装就好了。

如下，正确的依赖情况。

```shell
sucicada@20200702-143805:~/PROGRAM/GitHub/ghelper-subscriber-cli$ apt policy libc6-dev libc6
libc6-dev:
  Installed: 2.31-0ubuntu9.1
  Candidate: 2.31-0ubuntu9.1
  Version table:
 *** 2.31-0ubuntu9.1 500
        500 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages
        100 /var/lib/dpkg/status
     2.31-0ubuntu9 500
        500 http://archive.ubuntu.com/ubuntu focal/main amd64 Packages
libc6:
  Installed: 2.31-0ubuntu9.1
  Candidate: 2.31-0ubuntu9.1
  Version table:
 *** 2.31-0ubuntu9.1 500
        500 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages
        100 /var/lib/dpkg/status
     2.31-0ubuntu9 500
        500 http://archive.ubuntu.com/ubuntu focal/main amd64 Packages
        
```

这就是一个很舒服很正常的依赖信息了，令人赏心悦目。
果然Linux奇妙无穷。
