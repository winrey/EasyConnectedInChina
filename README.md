# 汇总各种工具国内镜像源和设置镜像源的方法
【目前施工ing....】

欢迎大家一同参与XD~

汇总brew，pip，nodejs等各种工具国内镜像源和设置镜像源的方法

<h2 id="contents">目录</h2>

[目录](#contents)  
[前言](#front)  
[正文](#text)  
&nbsp; &nbsp;[pip](#pip)  
&nbsp; &nbsp;[npm](#npm)   
&nbsp; &nbsp;[brew](#brew)    
&nbsp; &nbsp;[docker](#docker)
&nbsp; &nbsp;[yum](#yum)
[参考 / 感谢](#thanks-to)  

<h1 id="text">正文</h1>
<h2 id="front"> 前言</h2>
过于国内特殊的网络环境，想比大家都经历过低速下载、404、丢失链接等等等等等问题。

这非常影响我们的工作效率，而百度一个一个找国内源的更换方法则非常麻烦，特此在这里进行一个小汇总。

如果大家遇到操作问题可以提交issue，也欢迎大家也pull request来贡献自己知道的源和方法~

<h2 id="pip"> pip</h2>

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
- linux与mac的设置的文件在~/.pip/pip.conf，
- 
      vim ~/.pip/pip.conf
- windows在%HOMEPATH%\pip\pip.ini
- 如果没有创建即可。

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

<h2 id="npm">npm</h2>

### 源地址
- 官方源：<https://registry.npmjs.org/>  
(搜索网址：<https://www.npmjs.com/>)
- 淘宝源 <http://registry.npm.taobao.org/>  
(搜索网址：<http://npm.taobao.org/>)
- cnpmjs <http://r.cnpmjs.org/>  
(搜索网址：<http://cnpmjs.org/>)

### 使用方法
请注意，使用镜像库均不能publish，如需publish需要换回官方库
#### 方法一：使用cnpm替代
安装cnpm：
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
之后使用cnpm替代npm命令即可。支持除publish外所有命令：
```
cnpm install [name]
```
官方网址：<http://npm.taobao.org/>

#### 方法二：临时替换
在执行npm命令时指定参数registry即可：
```
npm --registry https://registry.npm.taobao.org install express
```

#### 方法三：持久使用
在命令行中执行：
```
npm config set registry https://registry.npm.taobao.org
```
<b>或者</b>在`~/.npmrc`中添加
```
registry = https://registry.npm.taobao.org
```
（执行`vim ~/.npmrc`即可更改`.npmrc`内容）

查看更改是否生效：
```
npm config get registry
```
或者
```
npm info express
```

<h2 id="brew">brew</h2>

### 说明
brew为macOS中的软件包管理工具，相当于linux中的apt、yum。可以用brew下载命令行程序，brew cask下载编译完成的二进制GUI程序。  
但是国内速度感人，因此我们要替换成国内源。
### 方法
替换国内源要替换几个地址。  

* homebrew-core.git库地址。是brew的主程序地址

```shell
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```
    
其中___https://mirrors.ustc.edu.cn/brew.git___为源地址。我们这里使用的是中科大的源作为演示。

* brew.git库地址。记录更新和安装脚本。

```shell
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```
    
其中***https://mirrors.ustc.edu.cn/homebrew-core.git***为源地址。

* 二进制库Homebrew Bottles源
    * bash用户：
    
```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile  
source ~/.bash_profile  
```
其中***https://mirrors.ustc.edu.cn/homebrew-bottles***为源地址。
        
    * zsh用户：
    
```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```
其中***https://mirrors.ustc.edu.cn/homebrew-bottles***为源地址。
        
* 都替换完成后执行`brew update`即可。


### 源地址
#### 官方源

| 类型 | 地址 |
| -------- | ------- |
| brew.git | <https://github.com/Homebrew/brew.git> |
| homebrew-core.git | <https://github.com/Homebrew/homebrew-core.git> |
| Homebrew Bottles | / |

#### 中科大源

| 类型 | 地址 |
| -------- | ------- |
| brew.git | <https://mirrors.ustc.edu.cn/brew.git> |
| homebrew-core.git | <https://mirrors.ustc.edu.cn/homebrew-core.git> |
| Homebrew Bottles | <https://mirrors.ustc.edu.cn/homebrew-bottles> |

#### 清华源

| 类型 | 地址 |
| -------- | ------- |
| brew.git | <https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git> |
| homebrew-core.git | <https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git> |
| homebrew-science.git | <https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-science.git> |
| homebrew-python.git | <https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-python.git> |
| Homebrew Bottles | <https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles> |

<h2 id="docker">docker</h2>

### 概述
docker的国外源的速度感人，为了避免影响生产效率，建议更换为国内源。  
一般国内源比较常用的是阿里源和DAO源。  
由于使用镜像需要注册相应的账号，每个人获得的镜像链接也不相同。 **因此在这里我们不提供具体的方法和链接，但是给出镜像源官方的指导链接，大家可以按照官方指导的步骤操作。**  
值得注意的是， mac上有两种Docker，依赖的技术不一样，一个可以使用命令改更改（老版Docker Toolbox依赖VirtualBox），另一个建议使用GUI修改（新版Docker for mac依赖HyperKit）。  
Linux的修改方式与Docker Toolbox相同。

### 阿里源
* 设置教程 + 个人加速链接  
<https://cr.console.aliyun.com/cn-hangzhou/mirrors>

### DaoCloud

* 加速链接获取与简短教程  
<https://www.daocloud.io/mirror#accelerator-doc>  
  
* 详细wiki  
<http://guide.daocloud.io/dcs/docker-9153151.html>


<h2 id="yum"> yum</h2>

### 源地址

- 清华大学 <https://mirrors.tuna.tsinghua.edu.cn/centos>

### 使用方法

首先备份 CentOS-Base.repo 
```
sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```
之后启用 TUNA 软件仓库， 将以下内容写入 /etc/yum.repos.d/CentOS-Base.repo
```
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/os/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
name=CentOS-$releasever - Updates
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/updates/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/extras/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/centosplus/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```


<h2 id="thanks-to">参考 / 感谢</h2>

- pip取材：  
<https://www.cnblogs.com/sunnydou/p/5801760.html>
- npm取材：  
<https://www.cnblogs.com/martinl/p/6513143.html>  
<http://cnodejs.org/topic/4f9904f9407edba21468f31e>
- brew取材：  
<https://thief.one/2017/08/24/1/>
