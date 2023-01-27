### 1. docker 安装
[参考](https://www.jianshu.com/p/80e3fd18a17e)

```bash
sudo apt-get update
sudo dpkg --configure -a
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
```
### 1.5 docker换源（可选）
[参考](https://www.jianshu.com/p/405fe33b9032)

### 2. docker验证
```bash
docker --version
sudo docker run hello-world
```
### 3. docker非root使用
[参考](https://blog.csdn.net/lynnyq/article/details/79080405)
```bash
sudo usermod -aG docker $USER
sudo service docker restart
newgrp docker
```
### 4. hadoop的docker image
参考：[基于Docker搭建Hadoop集群之升级版](https://kiwenlau.com/2016/06/12/160612-hadoop-cluster-docker-update/)

### 5. 防重启卡死
[参考](https://blog.csdn.net/leon1827/article/details/89764181)
```bash
sudo apt-get install open-vm-tools-desktop
```
### 6. 使用其自带的重新构建docker
#### 6.1 apt慢
系统先换源 `/etc/apt/source.list`
在DockerFile中加
```
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN  apt-get clean
```
#### 6.2 wget慢
直接设置DockerFile中的环境变量代理\
因为本机使用ssr做代理，所以ip设为局域网ssr代理机器的ip。
```
ENV http_proxy "http://xxxxx:1080"
ENV HTTP_PROXY "http://xxxxx:1080"
ENV https_proxy "http://xxxxx:1080"
ENV HTTPS_PROXY "http://xxxxx:1080"
```
然后速度飞起
（因为是虚拟机中的docker，所以设置的ip实际上是主机的，这里存在麻烦的地方，直接设置ip不解耦，所以可以考虑使用域名配置一下，最简单的做法就是在hosts文件中。）
