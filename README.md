# EasyConnectedInChina
【目前施工ing....】

欢迎大家一同参与XD~

汇总apt，pip，nodejs等各种工具国内镜像源和设置镜像源的方法

## 前言
过于国内特殊的网络环境，想比大家都经历过低速下载、404、丢失链接等等等等等问题。

这非常影响我们的工作效率，而百度一个一个找国内源的更换方法则非常麻烦，特此在这里进行一个小汇总。

如果大家遇到操作问题可以提交issue，也欢迎大家也pull request来贡献自己知道的源和方法~

## pip
### 源地址
- 阿里云 <https://mirrors.aliyun.com/pypi/simple/>
- 中国科技大学 <https://pypi.mirrors.ustc.edu.cn/simple/>
- 豆瓣(douban) <http://pypi.douban.com/simple/>
- 清华大学 <https://pypi.tuna.tsinghua.edu.cn/simple/>
- 中国科学技术大学 <http://pypi.mirrors.ustc.edu.cn/simple/>

### 使用方法
#### 方法一：临时使用
直接在pip后加-i后跟这次使用的源即可，例：

    pip install web.py -i https://mirrors.aliyun.com/pypi/simple/

指令中的网址为上方的源地址。

如果出现带有`trusted-host`字样的报错，这是由源不为https协议导致的，使用：

    pip install web.py -i http://pypi.douban.com/simple --trusted-host pypi.douban.com

添加信任主机即可。

#### 方法二：更改默认源
创建或修改配置文件（一般都是创建）
- linux的文件在~/.pip/pip.conf，
-     vim ~/.pip/pip.conf
- windows在%HOMEPATH%\pip\pip.ini

更改内容：
```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
```
或
```
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
```

文件中的网址为上方的源地址。
刚刚下面的内容是http协议源的实例。需要添加信任。
保存退出即可。

#### 方法三：python代码更改安装源
临时使用其他源安装软件包的python脚本如下：
```
#!/usr/bin/python
 
import os
 
package = raw_input("Please input the package which you want to install!\n")
command = "pip install %s -i https://mirrors.aliyun.com/pypi/simple/" % package
# http源的代码实例如下
# command = "pip install %s -i http://pypi.mirrors.ustc.edu.cn/simple --trusted-host pypi.mirrors.ustc.edu.cn" % package
os.system(command)
```


## 参考 / 感谢
- pip取材：<https://www.cnblogs.com/sunnydou/p/5801760.html>