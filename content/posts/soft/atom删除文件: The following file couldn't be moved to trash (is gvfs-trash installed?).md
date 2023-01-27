[参考这个](https://dev-notes.eu/2016/07/delete-files-in-atom-under-ubuntu-16-04/)

在使用加环境变量无果
环境
ubuntu 16
atom 1.4

执行以下命令
```
sudo mkdir -p /.Trash-1000/{expunged,files,info}

sudo chown -R $USER /.Trash-1000
```
