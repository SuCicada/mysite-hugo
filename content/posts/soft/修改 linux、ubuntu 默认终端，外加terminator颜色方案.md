需求：使用 cinnamon 桌面，想要修改右键打开的终端为terminator

先安装 dconf-tools
```
 sudo apt-get install dconf-tools
```
依次进入
org -> cinnamon -> desktop -> default-applications -> terminal

然后将键值改成你想要的
比如terminator为

key | value
-|-
exec: | x-terminal-emulator |
exec-arg:  |  -e |

最后来一个一键脚本
```
gsettings set org.gnome.desktop.default-applications.terminal exec /usr/bin/x-terminal-emulator
gsettings set org.gnome.desktop.default-applications.terminal exec-arg "-e"
```

terminator颜色方案
配置文件 ~/.config/terminator/config
```
[global_config]
  always_split_with_profile = True
  enabled_plugins = LaunchpadCodeURLHandler, APTURLHandler, LaunchpadBugURLHandler
  focus = system
  geometry_hinting = False
  handle_size = 2
  inactive_color_offset = 1.0
  title_font = mry_KacstQurn Bold 11
  title_hide_sizetext = True
  title_receive_bg_color = "#729fcf"
[keybindings]
  paste = <Primary>v
[layouts]
  [[default]]
    [[[child1]]]
      parent = window0
      profile = default
      type = Terminal
    [[[window0]]]
      parent = ""
      type = Window
[plugins]
[profiles]
  [[default]]
    background_color = "#3e3535"
    background_darkness = 0.89
    background_image = /home/peng/Pictures/pixiv/pixiv7/illust_10209207_20180121_090855.jpg
    background_type = transparent
    cursor_color = "#dcdcdc"
    cursor_shape = underline
    custom_command = tmux
    font = Ubuntu Mono 18
    foreground_color = "#ffffff"
    login_shell = True
    show_titlebar = False
    urgent_bell = True
    use_system_font = False
```

