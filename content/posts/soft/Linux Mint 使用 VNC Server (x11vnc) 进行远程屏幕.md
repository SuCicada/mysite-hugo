https://community.linuxmint.com/tutorial/view/2334

> This tutorial was adapted from here.
> 
> 1. Remove the default Vino server:
> 
> **<div style="font:red'">`sudo apt-get -y remove vino`<div>**
> 
> 2. Install x11vnc:
> 
> `sudo apt-get -y install x11vnc`
> 
> 3. Create the directory for the password file:
> 
> `sudo mkdir /etc/x11vnc`
> 
> 4. Create the encrypted password file:
> 
> **`sudo x11vnc --storepasswd /etc/x11vnc/vncpwd`**
> 
> You will be asked to enter and verify the password.  Then press Y to
> save the password file.
> 
> 5. Create the systemd service file for the x11vnc service:
> 
> **`sudo xed /lib/systemd/system/x11vnc.service`**
> 
> Copy/Paste this code into the empty file:
> `复制以下文字`

    [Unit]
    Description=Start x11vnc at startup.
    After=multi-user.target
    
    [Service]
    Type=simple
    ExecStart=/usr/bin/x11vnc -auth guess -forever -noxdamage -repeat -rfbauth /etc/x11vnc/vncpwd -rfbport 5900 -shared
    
    [Install]
    WantedBy=multi-user.target
    

> 6: Reload the services:
> 
> **`sudo systemctl daemon-reload`**
> 
> 7. Enable the x11vnc service at boot time:
> 
> **`sudo systemctl enable x11vnc.service`**
> 
> 8. Start the service:
> 
> Either reboot or
> 
> **`sudo systemctl start x11vnc.service`**


抱怨一句:
百度上根本什么有用的都没有,我要的是mint,搜出来的都是些啥,连搜索引擎都懒成这样,

