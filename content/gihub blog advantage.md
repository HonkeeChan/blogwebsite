Date: 2015-10-7
Title: Windows下用Pelican搭建Github Blog高级篇
Tags: 教程
Category: Blog
Slug: github-blog-advantage

##前言
前一篇文章讲了如何在Windows下用Pelican搭建Github Blog, 为了使博客更美观，我们可以使用主题和插件。
##主题
安装主题，[Pelican主题](http://www.pelicanthemes.com/ "")有很多，我的主题是[pelican-bootstrap3](https://github.com/DandyDev/pelican-bootstrap3)。添加主题非常简单，将主题下载到博客的目录下，在pelicanconf.py中添加
```bash
THEME = 'path/to/the/theme'
```
路径是从pelicanconf.py到那个主题的相对路径。
##安装第三方评论系统
第三方评论系统也有很多，如Disqus，多说，等等。我这里用了Disqus。在Disqus上申请一个账号，记住Shortname，在pelicanconf.py添加
```bash
DISQUS_SITENAME = Shortname
```

##添加插件
