---
layout: post
title: "Windows下搭建Octopress博客"
date: 2013-05-26 21:21
comments: true
categories: Octopress
---
##您需要掌握的
* 使用[Octopress](http://octopress.org/)来搭建博客，还是有一定门槛的，看完本文后，希望您不会觉得很难。

* [Octopress](http://octopress.org/) 是一款基于 Jekyll 的静态站点生成系统，使用Ruby实现，所以您需要懂点Ruby的知识，其实会几个命令就行；


* [Octopress](http://octopress.org/)的博客内容是通过Markdown来书写，所以您需要了解Markdown的编写规则，[Markdown 语法说明](http://wowubuntu.com/markdown/)可以让你在几分钟之内熟悉Markdown的语法，在Windows下可以使用[MarkDown Pad](http://www.markdownpad.com/)或是使用在线的编辑器[http://mahua.jser.me/](http://mahua.jser.me/ "http://mahua.jser.me/")进行编辑；

* Octopress通常会部署在GitHub上，所以您需要会一些简单的Git命令以及Gtihub的使用。

##准备
* 系统：Windows 2003 Server、Windows 7、Windows 2008R2 Server，这三个系统用下面的版本都安装成功过；

* Git：[http://msysgit.googlecode.com/files/Git-1.8.1.2-preview20130201.exe](http://msysgit.googlecode.com/files/Git-1.8.1.2-preview20130201.exe "http://msysgit.googlecode.com/files/Git-1.8.1.2-preview20130201.exe")

* Ruby：[http://files.rubyforge.vm.bytemark.co.uk/rubyinstaller/rubyinstaller-1.9.3-p429.exe](http://files.rubyforge.vm.bytemark.co.uk/rubyinstaller/rubyinstaller-1.9.3-p429.exe "http://files.rubyforge.vm.bytemark.co.uk/rubyinstaller/rubyinstaller-1.9.3-p429.exe")

* DevKit：[http://cloud.github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe](http://cloud.github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe "http://cloud.github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe")

* Python：[http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi](http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi "http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi")

* Octopress：git://github.com/imathis/octopress.git
<!--more-->
##安装
###安装Git
Windows下安装Git很简单，一路next就可以了。

###安装Ruby
Ruby的安装也是一路next就可以，不过记得勾选`Add Ruby executables to your PATH`，将Ruby的执行路径加入到环境变量中，如果忘记勾选，也可以手动设置。安装完后可以在命令提示符中输入`ruby –version` 来确认是否安装成功。

###安装DevKit
DevKit下载下来的是一个自压缩文件，我们将其解压到`D:/DevKit`，有两点需要注意：

1. 解压目录中没有有中文和空格；

2. 必须先安装Ruby，而且Ruby需要是RubyInstallser安装。

解压DevKit后，在命令行输入以下命令来进行安装：
```
d: 
cd DevKit
ruby dk.rb init 
ruby dk.rb install
```
###安装Python
安装Python,也是一路next就可以，博客的代码高亮用到了Python的Pygments模块，在Python中安装第三方库需要使用easy_install，在下面地址下载跟Python相对应的安装程序安装后就可以使用easy_install了。

[https://pypi.python.org/pypi/setuptools](https://pypi.python.org/pypi/setuptools "https://pypi.python.org/pypi/setuptools")

easy_install会安装在Python安装目录的Scripts目录中，例如我的Python目录是`C:\Python27`，所以需要将`C:\Python27\Scripts`目录加入到环境变量中才能在命令提示符中使用easy_install命令。

在命令提示符中输入如下命令就可以安装Pygments了。

```
easy_install pygments
```

###安装Octopress
首先在GitBash中输入如下命令将Octopress代码拉到本地，

```
cd d:/GitProject
git clone git://github.com/imathis/octopress.git octopress
```

然后需要安装Octopress的依赖项，安装依赖项需要用到Ruby的gem，使用下面的命令可以更换gem的更新源，使用国内的淘宝镜像速度相对快点。

```
gem sources -a http://ruby.taobao.org/
gem sources -r http://rubygems.org/
gem sources -l
```

修改Octopress目录下的Gemfile文件，将第一行的`http://rubygems.org/` 修改为`http://ruby.taobao.org/`

在命令提示符中进入到Octopress目录，输入下面命令进行依赖项的安装

```
gem install bundler
bundle install
```

输入下面的命令来安装Octopress的默认主题

```
rake install
```

到此所有的安装工作已经结束，输入下面的命令可以在本地进行预览。

```
rake preview
```

![Octopress](http://ww2.sinaimg.cn/mw690/3cefded1gw1e52yy7oi85j20lx08taaz.jpg)

##解决中文问题
如果博客中包含中文，需要进行如下设置：

1. 在环境变量中设置下面的键值对；


```
LANG=zh_CN.UTF-8 
LC_ALL=zh_CN.UTF-8
```

2. 含有中文的文件需要保存为UTF-8无BOM格式编码。

3. 在Ruby的安装路径找到 文件convertible.rb

`C:\Ruby193\lib\ruby\gems\1.9.1\gems\jekyll-0.12.0\lib\jekyll\convertible.rb`

将27行修改为：`self.content = File.read(File.join(base, name), :encoding => 'utf-8')`

##在Octopress中添加文章
使用下面命令可以在Octopress中添加文章

![addpost](http://ww2.sinaimg.cn/mw690/3cefded1gw1e52yy8dmzcj20hm01taa5.jpg)


**注意，`rake new_post['my first octopress blog']`中的`my first octopress blog` 并不是博客标题，而是和生成的文件名以及url地址有关，该名称不支持中文。博客标题可以在生成的markdown文件中修改。生成的markdown文件在`octopress/source/_posts`目录中。 **

编辑markdown文件，将标题可以修改为中文标题，还可以设置分类等信息以及编写正文部分

![content](http://ww4.sinaimg.cn/mw690/3cefded1gw1e52yy94hbij20cp06ljrx.jpg)

每次执行了添加博客的命令，或是修改了现有博客的内容后，都要执行下面命令进行重新生成

```
rake generate
```

如果之前有输入`rake preview`的命令提示符窗口没关，可以直接`localhost:4000`来进行预览，否则需要重新执行下`rake preview`才能进行预览。

##将Octopress发布到Github
首先需要您有一个Github的账号，并且知道怎么样将Git项目推送到Github上，具体配置可以参考我之前的博文《[Windows 下使用Git管理Github项目](http://www.cnblogs.com/oec2003/archive/2012/02/06/2741993.html)》

登录到Github，创建一个名为`username.github.com的repository`，例如我创建的为`oec2003.github.com`；

在命令提示符中进入到Octopress目录，输入下面命令：

```
rake setup_github_pages
```

按照提示输入新建的repository的地址，例如我的地址为：

```
git@github.com:oec2003/oec2003.github.com.git
```

执行命令`rake deploy` 就可以将本地的内容发布到Github上。

最后需要将Octopress的源文件推送到Github的Source分支上，执行下面命令即可：

```
git add .
git commit -m “your message”
git push origin source
```

##总结
如果喜欢写博客有很多的平台可以选择，像博客园就是Net平台下很好的博客平台，如果想搭建自己的个人博客有独立的域名，WordPress是不错的选择，如果您喜欢折腾，不妨试试Octopress。在环境搭建好的情况下，使用Octopress写博客大致有一下几个步骤：

1. 执行`rake new_post['title']`来生成一个博文；

2. 找对生成的markdown文件，编辑内容，当然是使用markdown语法来编辑；

3. 执行`rake generate`来生成文章；

4. 执行`rake preview`在本地预览；

5. 执行`rake deploy`发布到Github中。

在安装的过程中可能会碰到各种问题，根据错误提示信息google，肯定会找到答案。

最后推荐一个安装Octopress的视频：

[http://happycasts.net/episodes/36?autoplay=true](http://happycasts.net/episodes/36?autoplay=true "http://happycasts.net/episodes/36?autoplay=true")
