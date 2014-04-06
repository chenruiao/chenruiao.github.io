---
layout: post
title: "Windows中搭建已存在的Octopress环境"
date: 2013-08-10 08:54
comments: true
categories: Octopress
---


当我们需要在不同的电脑上来对同一个Octopress博客进行维护的时候就需要针对已存在的Octopress来设置环境了，

##安装相应的软件：

1. Git：[http://msysgit.googlecode.com/files/Git-1.8.1.2-preview20130201.exe ](http://msysgit.googlecode.com/files/Git-1.8.1.2-preview20130201.exe )
1. Ruby：[http://files.rubyforge.vm.bytemark.co.uk/rubyinstaller/rubyinstaller-1.9.3-p429.exe](http://files.rubyforge.vm.bytemark.co.uk/rubyinstaller/rubyinstaller-1.9.3-p429.exe)
1. DevKit：[http://cloud.github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe](http://cloud.github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe)
1. Python：[http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi](http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi)

关于软件的安装在《[Windows下搭建Octopress博客](http://www.cnblogs.com/oec2003/archive/2013/05/27/3100896.html)》中有详细的介绍。
<!--more-->
##拉Octopress代码到本地
使用下面命令将已存在的Octopress代码拉到本地

```
git clone -b source git@github.com:oec2003/oec2003.github.com.git
git clone  git@github.com:oec2003/oec2003.github.com.git _deploy

```

##编写文章

```
cd Octopress
rake new_post['new post title']
rake generate
rake preview
rake deploy

```

##推送到github

```
cd Octopress
git add .
git commit -m 'message'
git push origin source

```

##从github上获取最新

比如在公司发布了一篇博文，回到家里想继续发博文，就需要先将github上的最新代码拉到本地：

```
cd Octopress
cd _deploy
git pull origin master
cd ..
git pull origin source

```

今天在家里的另一台电脑上进行生成文章时，发现当执行了命令`rake generate` 后，生成到public目录中相应的页面为空，没有任何内容，但在命令行中命令还是正常执行了，没有出现异常。最后查出原因是因为Python的安装目录没有添加到环境变量中。所以建议在准备环境时就就将git、ruby、python的安装目录都添加到环境变量中。