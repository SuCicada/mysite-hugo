[sshpass方式](第一种方式)
[expect方式](第二种方式)

### 第一种方式
* 通过 `sshpass` 来
```bash
 sshpass -p "xxxx" ssh root@xxx.xx.xxx
 sshpass -p "xxxx" scp xxxx root@xxxx:/xxxxx
```
但是sshpass好像不能回显. 对于scp不太方便
但是对于ssh确实很好用的


### 第二种方式
* 通过expect 
  [用法参考](https://likegeeks.com/expect-command/)

比如使用ssh
```bash
#!/usr/bin/expect

set timeout 30 

spawn ssh root@sucicada.tk
expect "password:"  
send "Ubuntu2019\n"
interact 
```
----

通过以上方式可以使用一个脚本就能登录到远程主机上了
