查看官网 
https://www.jetbrains.com/help/clion/settings-languages-cpp-clangd.html中 关于code completion 的说明

以下配置开启路径：
>File | Settings | Languages and Frameworks | C/C++ | Clangd for Windows and Linux
CLion | Preferences | Languages and Frameworks | C/C++ | Clangd for macOS
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210324134521506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70)
我们得知 CLion 提供了两种代码提示引擎一种内置，一种是 Clangd。
默认的选择是只用Clangd，而不会优先用内置引擎。我们选择剩下两个会发现代码提示的速度快了很多。
这个现象能初步判断内置引擎比Clangd要快。至于更深层的原因还待研究。


