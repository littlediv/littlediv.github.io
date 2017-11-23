# Python 安装模块 #

---

pip install 一般也不用考虑某些库用C语言写的，需要编译的问题：

1 首先，安装python时，一定勾选pip install选项；

2 http://www.lfd.uci.edu/~gohlke/pythonlibs/zpcorkgj/opencv_python-3.1.0-cp35-cp35m-win_amd64.whl   上该网站找到需要的库：（下面以安装opencv为例）


如图，放在了F盘，！！！！！！！！！！注意不要重命名，不然下一步会出错的！！！！！！！！！！

4 amd      pip install wheel       pip install wheel      如果首次用，会安装pip。再次用时就会出现上图内容：Requirment already satisfied........

5    来了
首先转到whl文件所在位置》    

 pip install blablabla.whl(!!!!重要事情再说一遍，不要改文件名，不然会出错，我也母鸡为毛啊！！！)》     Succcessfully installed opencv-python-3.1.0----------------------大功告成啊

6   不放心，再确认一下是否安装成功

进入python进入pythonhelp ('modules')会得到已安装的全部模块，1就ok了




---

我也是 win7 64 bit 环境，简单说一下省去你自己查找的麻烦（经验）。（为避免歧义下文某些句子结尾木标点。）

安装包：首先，安装 python 时记得选同时安装 pip 以及添加 path。如果这项漏选了可以再次安装选择 change...

--> 然后所有 modules 都可以用 pip 来安装，在 cmd 下直接 pip install whatevermodule

--> 无网络或无法链接网络时可以手动下载 module 到某 path 下，然后 pip install path\whatevermodule

查看已安装的包：cmd 下 pip freeze

## 使用 urllib ##

安装 python 3.6

[urllib 网址](http://www.lfd.uci.edu/~gohlke/pythonlibs/)

本身 python 带了 urllib 和 urllib2 的合集

import urllib 

错误：name 'urllib' is not defined

正确导入： from urllib import request

## 参考信息 ##

[Python模块如何安装 并确认模块已经安装好](https://www.zhihu.com/question/21695470)



