
### WSL2 是什么
全称为 Windows Subsystem for Linux2。是一个能在Windows上运行Linux运行环境的工具。是WSL的第二代。

简单来说就是Windows下的Linux虚拟机。

### 比虚拟机好在哪里

- 启动速度超快。文件访问跨系统访问无限制（因为两个系统之间的目录是通过网络映射挂载的）
- Windows、Linux 命令混合使用。无论在哪个系统的终端，都可以使用另一个系统的命令。（比如在Linux中使用`explorer.exe .` 看看会发生什么）

### 比WSL1好在哪
最主要体现在网络隔离、以及进程管理（比如ps）。
WSL2使用类似NAT的虚拟机网络模式。这样的好处就是Linux和Windows的网段是隔离的。
而WSL1更像是运行在Windows上的一个程序，所以网络IP、端口、进程状态等都是用的Windows宿主机。
所以这么来看WSL2更像是纯粹的一个虚拟机了。

### 安装&配置
- [在 Windows 中运行 Linux：WSL 2 使用入门](https://zhuanlan.zhihu.com/p/69121280)

- [適用於Linux的Windows子系統(Windows Subsystem for Linux；WSL)](https://www.lijyyh.com/2020/07/linuxwindowswindows-subsystem-for.html)

### 进阶提升效率
- Windows Terminal

  比PowerShell 更好用、更美观的Windows终端。支持窗口多开，WSL直开。

  [玩转WSL(2)之安装并配置Windows Terminal](https://zhuanlan.zhihu.com/p/199748950)

- Ctrl+Alt+T 开启终端。

  下载**WinHotKey**，设置Windows Terminal默认窗口为WSL。

  体验原生Linux终端的快感。

### 肯定有用的脚本
- [ ] todo

#### 衍生资源
[WSL 和 WSL2 简单对比](https://www.v2ex.com/t/587642)
