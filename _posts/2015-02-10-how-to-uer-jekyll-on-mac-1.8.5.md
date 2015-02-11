---
title: 如何在mac 2.8.5上使用jekyll
layout: post
categories: 博客
tags: jekyll ruby mac
---

##1 安装

以下是依赖关系。mac安装jekyll最麻烦的地方在于升级ruby到1.9.3。由于对mac ruby不熟悉，安装ruby 1.9.3花费了很多时间。

{% highlight bash %}
gem install jekyll
    |__ rvm install 1.9.3
        |__  brew install [packages]
            |__ reinstall brew
{% endhighlight %}


##2 配置

###2.1 端口

jekyll默认使用4000作为服务端口。个人喜欢8888比较吉利。

{% highlight bash %}
touch _config.yml
echo 'port: 8888' >> _config.xml
{% endhighlight %}

###2.2 中文字符

帖子中提到比较多点的是jekyll读文件的encoding问题:[jekyll-markdown-utf-8](http://stackoverflow.com/questions/20673983/jekyll-markdown-utf-8)。
我用的是mac，不存在gbk和utf的bom问题。但是展现的时候需要增加html meta, 代码如下。可以写到模板中，避免代码拷来拷去。

{% highlight html %}
<link rel="stylesheet" type="text/css" href="/assets/css/pygments.css">
{% endhighlight %}


###2.3 模板

参考了以下模板: [lanyon](https://github.com/poole/lanyon)。比较清爽。

##参考：

- [Installing Ruby 2.0.0 with RVM and Homebrew on Mac OS X 10.8 Mountain Lion](http://stackoverflow.com/questions/20673983/jekyll-markdown-utf-8)
- [jekyll中文网](http://jekyllcn.com/)
- [代码高亮](http://zyzhang.github.io/blog/2012/08/31/highlight-with-Jekyll-and-Pygments/)
