参考
http://xingda1989.iteye.com/blog/1969908
https://blog.csdn.net/edin_blackpoint/article/details/70860101
`sudo apt install openconnect`

> 在/etc/vpc/目录下新建vpnc-script 文件
文件内容可以到此处拷贝
http://git.infradead.org/users/dwmw2/vpnc-scripts.git/blob_plain/HEAD:/vpnc-script

`sudo openconnect --juniper -u [你的学号] --script /etc/vpnc/vpnc-script [你学校的提供的vpn的url]`