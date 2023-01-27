先ctrl+`安装packa：
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
使用快捷键ctrl+shift+p，输入packet control install package

这个网站上可以自制主题
http://tmtheme-editor.herokuapp.com/#!/editor/local/undefined  
之后将文件放到home/peng/.config/sublime-text-3/Packages

从preferences里的theme选项可以换了