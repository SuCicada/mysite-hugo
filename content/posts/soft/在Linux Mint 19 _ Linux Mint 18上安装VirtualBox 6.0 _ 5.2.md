如果你直接可以
`sudo apt-get install virtualbox-6.0`那就相安无事

否则参考
https://www.itzgeek.com/how-tos/linux/linux-mint-how-tos/install-virtualbox-4-3-on-linux-mint-17.html

> 打开终端并将Oracle VirtualBox存储库的公钥导入您的系统。
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add  - 
使用以下命令添加VirtualBox存储库。
**### Linux Mint 19 ###**
echo“deb [arch = amd64] http://download.virtualbox.org/virtualbox/debian bionic contrib”| sudo tee /etc/apt/sources.list.d/virtualbox.list
**### Linux Mint 18 ###**
echo“deb http://download.virtualbox.org/virtualbox/debian xenial contrib”| sudo tee /etc/apt/sources.list.d/virtualbox.list
更新存储库索引数据库。
sudo apt-get update
使用apt命令安装VirtualBox。
VirtualBox 6.0：
sudo apt-get install -y virtualbox-6.0
VirtualBox 5.2：
sudo apt-get install -y virtualbox-5.2


-----------------------------
经历:
先参看官网方法:https://www.virtualbox.org/wiki/Linux_Downloads

```
deb https://download.virtualbox.org/virtualbox/debian <mydist> contrib
```
增加源,  \<mydist> 里添加ubuntu的发行版   使用

```
lsb_release -sc
或  lsb_release -a 等查看
```
 **但是这里是linux mint**
使用命令显示出来的是  sylvia , 并不存在于vbox的版本中


