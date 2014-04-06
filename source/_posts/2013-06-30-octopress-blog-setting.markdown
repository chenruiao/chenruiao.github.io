---
layout: post
title: "Octopress博客设置"
date: 2013-06-26 22:02
comments: true
categories: Octopress
---
在上一篇《[Windows下搭建Octopress博客](http://oec2003.github.io/blog/2013/05/26/how-to-crate-octopress-blog-in-windows/)》中介绍了怎样在Windows环境中搭建Octopress博客，以及如何部署到Github中，本文主要讲解怎样对Octopress进行一些设置，比如修改博客标题、添加侧边栏等等。

##Octopress文件目录

首先来看下Octopress的文件目录吧，因为很多的设置就是从这里开始的。

![](http://ww1.sinaimg.cn/mw690/3cefded1gw1e52yyb8jz7j207f0e5mxp.jpg)

刚clone下来的Octopress源码没有最后的三个文件，当执行了rake install后会产生最后的三个目录。

* source：该目录是执行`rake install`后从`.themes\classic`目录中的source拷贝而来，并且添加了一个_posts目录了，当执行了`rake new_post['title']`后会在_post生成博文的markdown文件。包括后面的很多设置页是在该目录中进行；

* sass：也是在执行`rake install`后从`.themes\classic`目录中的source拷贝而来。Sass 扩展了 CSS3，增加了规则、变量、混入、选择器、继承等等特性。Sass 生成良好格式化的 CSS 代码，易于组织和维护。

* public：刚clone源码时public是个空目录，当执行了`rake generate`后会编译source的内容然后将编译后的内容复制到public目录中，在执行`rake deploy`后也是讲public的内容推送到服务器上，我们最终访问到的也是public的内容。这个目录中的内容一般不用我们手动修改，执行`rake generate`来生成。
<!--more-->
##基本设置
在Octopress目录中有一个名为_config.yml的文件：

```

url: http://oec2003.github.io                       #网站地址 
title: 冯威的个人博客                                #网站标题 
subtitle: 临渊羡鱼 不如退而结网.                      #网站副标题 
author: 冯威                                        #网站作者，通常显示在页尾和每篇文章的尾部 
simple_search: http://google.com/search            # 搜索引擎 
description:                                       #网站的描述，出现在HTML页面中的 meta 中的 description

```

在使用`rake new_post['title']`生成的markdown文件中，可以对每篇博文进行一些设置：

```

layout: post                                                         
title: "我的第一篇Octopress博客"                    #文章标题 
date: 2013-05-26 18:49                            #文章发布时间 
comments: true                                    #是否允许评论 
categories: Octopress                             #文章分类 

```

##设置网站的description和keywords
在网页的head部分设置name为description和keywords的meta元素可以让网页更容易被搜索引擎搜索到，下面说说怎样在Octopress中设置首页和每篇博文的description和keywords。

###设置首页
打开`source/index.html`文件，在文件的头部添加相应内容：

```

layout: default  
description: "冯威的个人博客" 
keywords: Asp.Net,C# 

```

###设置文章
文章的设置直接在每篇文章对应的`markdown`文件的头部添加相应内容积即可：

```

layout: post 
title: "我的第一篇Octopress博客" 
date: 2013-05-26 18:49 
comments: true 
categories: Octopress 
description: "我的第一篇Octopress博客" 
keywords: Octopress 

```

##导航栏设置

打开文件`source\_includes\custom\navigation.html`，默认情况下如下所示：

```

<ul class="main-navigation"> 
  <li><a href="{{ root_url }}/">Blog</a></li> 
  <li><a href="{{ root_url }}/blog/archives">Archives</a></li> 
</ul>

```

可以将Blog和Archives修改为首页和归档，不过记得将包含有中文的文件保存为无BOM的UTF-8编码格式。也可以在此添加一个标签页，比如加上一个关于，我们可以按下面步骤实现：

1. 使用命令`rake new_page['about']`创建一个页面，页面路径为`source\about\index.markdown`;

2. 修改上面的`navigation.html`为：

```

<ul class="main-navigation"> 
  <li><a href="{{ root_url }}/">首页</a></li> 
  <li><a href="{{ root_url }}/blog/archives">归档</a></li> 
  <li><a href="{{ root_url }}/about">关于</a></li> 
</ul>

```

##添加侧边栏文章分类
我们知道在文章的`markdown`文件中有一个`categories`可以进行设置文章的分类，设置了`categories`的文章在文章的尾部会显示分类的链接，如果想在侧边栏中展示所有的分类，可以按以下步骤进行：

1. 在`plugins`目录中创建`category_list_tag.rb`文件，文件内容如下：

```

module Jekyll 
  class CategoryListTag < Liquid::Tag 
    def render(context) 
      html = "" 
      categories = context.registers[:site].categories.keys 
      categories.sort.each do |category| 
        posts_in_category = context.registers[:site].categories[category].size 
        category_dir = context.registers[:site].config['category_dir'] 
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase) 
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n" 
      end 
      html 
    end 
  end 
end

Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)


```

2. 添加`source/_includes/asides/category_list.html`文件，内容如下：

```

<section> 
  <h1>文章分类</h1> 
  <ul id="categories"> 
    {\% category_list \%} 
  </ul> 
</section>


```

再次强调，文件中含有中文需要保存为无BOM的UTF-8格式

3. 修改`_config.yml`文件，在`default_asides`项中添加`asides/category_list.html`，值之间以逗号隔开。

```
default_asides: [asides/category_list.html, asides/recent_posts.html]

```


如果想将最新文章的`Recent Posts`修改为中文，修改`source\_includes\asides\recent_posts.html`即可。

在侧边栏中还可以添加其他的一些组件，如新浪微博、我的豆瓣、最新评论、标签云等，添加页面以及让其显示的方法和上面类似，只是在内容取值有所差别。

##添加评论
Octopress自身不支持评论功能，不过我们可以使用第三方的评论系统，国外的有[Disqus](http://www.disqus.com/)，国内的有[多说](http://dev.duoshuo.com/)等。下面介绍怎样在Octopress中使用[Disqus](http://www.disqus.com/)。

首先需要在Disqus注册一个账号，登录后点击Dashboard，然后添加站点信息

![](http://ww3.sinaimg.cn/mw690/3cefded1gw1e55j3dgu4zj20g008vt97.jpg)

然后在`_config.yml`文件中进行下面设置

```

\# Disqus Comments 
disqus_short_name: oec2003  \# oec2003为添加站点信息时的Site Shortname 
disqus_show_comment_count: true

```

添加版权声明
这里所说的版权声明是指每篇文章后面的版权信息

首先`source\_includes\post`目录中添加`license.html`文件，文件内容如下：

```

{\% if site.post_license  \%}

<DIV style="font-size:12px;BORDER-BOTTOM: #bbbbbb 1px solid; BORDER-LEFT: #bbbbbb 1px solid; BACKGROUND: #f6f6f6; HEIGHT: 120px; BORDER-TOP: #bbbbbb 1px solid; BORDER-RIGHT: #bbbbbb 1px solid" class=oec2003right>
<DIV style="MARGIN-TOP: 10px; FLOAT: left; MARGIN-LEFT: 5px; MARGIN-RIGHT: 10px">
<IMG alt="" src="http://images.cnblogs.com/cnblogs_com/oec2003/219566/r_fw90100.jpg" width=90 height=100></DIV>
<DIV style="LINE-HEIGHT: 200%; MARGIN-TOP: 10px; COLOR: #000000">
作者： <A href="http://oec2003.github.io/">冯威</A> <BR>
出处： <A href="http://oec2003.github.io/">http://oec2003.github.io/</A> 
<BR>本文基于<a target="_blank" title="Creative Commons Attribution 2.5 China Mainland License" href="http://creativecommons.org/licenses/by/2.5/cn/">
署名 2.5 中国大陆</a>许可协议发布，欢迎转载，演绎或用于商业目的，但是必须保留本文的署名
<a href="http://oec2003.github.io/">冯威</a>（包含链接）。 </DIV></DIV>

{\% endif \%}


```

在sass\custom\_styles.scss添加如下样式信息来控制版权信息的样式

```

.oec2003right
{
    background: #C3D9FF;
    height:120px;
    border:1px solid #BBBBBB;
}

.oec2003right a:link 
{
    color: #0057b6;
    text-decoration: none;
}
.oec2003right a:visited 
{
    color: #0057b6;
    text-decoration: none;
}
.oec2003right a:active,a:hover
{
    color: #0057b6;
    text-decoration: underline;
}

```

修改文件source\_layouts\post.html

![](http://ww4.sinaimg.cn/mw690/3cefded1gw1e56mhiqymaj20e306zaap.jpg)

在_config.yml添加配置项用来控制是否显示页面的版权信息

```

\# Post License
post_license: true

```

最终展示效果：

![](http://ww1.sinaimg.cn/mw690/3cefded1gw1e56nxa3e7mj20mp04vq3l.jpg)

##使文章以摘要形式展示

默认情况下在博客的首页是显示每篇文章的全部内容，更多时候我们只想在首页展示文章的一部分内容，当点击阅读全文时进入到文章的详细页面，在Octopress中可以很轻松实现该功能：

1. 在文章对应的markdown文件中，在需要显示在首页的文字后面添加<!—more—>，执行rake generate后在首页上会看到只显示<!—more—>前面的文字，文字后面会显示Read on链接，点击后进入文字的详细页面;

2. 如果想将Read on 修改为中文，可以修改_config.yml文件

```

excerpt_link: "阅读全文&rarr;"  # "Continue reading" link text at the bottom of excerpted articles

```

##几个参考
###添加最新评论到侧边栏

[http://yanping.me/cn/blog/2012/02/07/comment-sidebar/](http://yanping.me/cn/blog/2012/02/07/comment-sidebar/)

###添加标签云

[http://codemacro.com/2012/07/18/add-tag-to-octopress/](http://codemacro.com/2012/07/18/add-tag-to-octopress/)

##总结
其实所谓的设置都是去修改Octopress的源文件的一些内容，或是根据Octopress的文件组织方式去添加一些页面等，将Octopress的每个文件都打开看看基本就了解个大概了。
