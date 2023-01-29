下载bat脚本
https://github.com/SuCicada/ipswitch
然后运行,按照提示输入即可

```
@echo off&color 1E&title IP地址快速切换器
echo ┌────────────────────────────┐
echo ｜                                                        ｜
echo ｜         切换网络环境，请输入当前所在位置               │
echo ｜                                                        ｜
echo └────────────────────────────┘
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"

:choice
set choice=
set /p choice=【外网】请选择1，【内网】请选择2 :[1,2]?
if %choice%==2 goto school_lan
if %choice%==1 (goto lab_lan) else (echo 输入错误，请重新输入&goto choice)


:lab_lan
set eth="本地连接"
set ip=10.0.0.
set netmask=255.255.255.0
set gw=10.0.0.1
set dns1=8.8.8.8
set dns2=
echo.
echo 切换到外网有线环境
echo.
goto switch


:school_lan
set eth="本地连接"
set ip=10.90.6.
set netmask=255.255.255.0
set gw=10.90.6.254
set dns1=202.207.208.8
set dns2=202.99.192.68
echo.
echo 切换到学校内网环境
echo.
goto switch

:switch
set code=
set /p code= 你的IP主机号[3-254]？ %ip%
set "ip=%ip%%code%"
echo 正在设置IP地址 %ip%
netsh interface ip set address %eth% static %ip% %netmask% %gw% 1
echo 正在设置首选DNS服务器 %dns1%
netsh interface ip set dns %eth% static %dns1% register=PRIMARY validate=no
if defined dns2 (
    echo 正在设置备用DNS服务器 %dns2%
    netsh interface ip add dns %eth% %dns2% index=2 validate=no
)
echo.
echo 您的IP地址切换成功，当前IP地址为%ip%
echo.
goto end


:public
echo 正在设置IP地址为自动获得
netsh interface ip set address %eth% dhcp
echo 设置首选DNS服务器为自动获得
netsh interface ip set dns %eth% dhcp
echo   正在自动获取IP，请稍侯...
echo.
for /L %%x in (1 1 10) do set /p gu=■<nul&ping /n 2 127.1>nul
echo 100%%
echo.
echo 您的IP地址将切换成功,当前IP地址为自动获取
echo.
goto end

:end
@echo on
@pause

```

