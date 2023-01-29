virtualbox 报错 ,看提示让执行以下

```
sudo /sbin/vboxconfig
```

如果报错:

> `vboxdrv.sh: failed: Cannot change group vboxusers for device /dev/vboxdrv.`
 
 那么应该是本用户没有加入vboxusers.
 参考:
https://ubuntuforums.org/archive/index.php/t-790303.html
执行:

```
sudo addgroup vboxusers 
sudo adduser your_username vboxusers 
sudo /etc/init.d/vboxdrv setup 
```

 即可

------------------------------------------
错误原因应该是我卸载mysql时将一些旧的配置删了,我不知道,我相信没人会闲到犯和我一样的错误
