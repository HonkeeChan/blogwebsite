Date: 2015-10-7
Title: Windows下用Pelican搭建Github Blog
Tags: 教程
Category: Blog
Slug: window-pelican-github

##前言
网上也有好多关于在windows下用Pelican创建Blog的文章，但好多文章一个流程走下去，不说为什么那么做，那个命令是用来干嘛的，所以如果在那个流程中遇到什么问题就很难解决。
##目录


##搭建需求
Python
pip
Pelican
Markdown
Git
Markdown编辑器
make

###Python安装
可以直接到官网下载Python安装包，也可以安装Anaconda。
Anaconda集成了Python很多常用的工具和库，如pip。

###pip安装
pip是Python的一个包管理器，非常方便。如果安装Python的时候通过Anaconda安装就自带pip，否则须自行安装pip，网上搜吧。

###Pelican Markdown安装
这是一个python库，用来将Markdown文件转换为Html文件需要的工具。安装方法如下： 
```bash
pip install pelican
pip install markdown
```

###Git安装
可以通过官网上的Github Desktop去安装，不过这样安装的话下载速度会比较慢。可以搜索Github for windows离线版之类的关键词搜到离线包，下载安装即可。

###Markdown编辑器
Markdown编辑器有好多。windows下有Markdown Pad，MdCharm等等吧。Markdown pad我下载后无法安装，所以我就用MdCharm。我想编辑器应该都差不多了吧，反正一边写，另一边能看就可以了。

##开始搭建博客
好了现在环境是配好了，开始搭建我们的博客吧。
```bash
mkdir blog
cd blog
pelican-quickstart
```
mkdir,cd这两个就不用说了吧。
pelican-quickstart 是用pelican库去创建一个项目。根据提示一步步输入相应的配置项，不知道如何设置的接受默认即可，后续可以通过编辑pelicanconf.py文件更改配置)
一下是生成的目录结构

```bash
blog/
├── content              # 存放输入的源文件
│   └── (pages)          # 存放手工创建的静态页面
├── output               # 生成的输出文件
├── develop_server.sh    # 方便开启测试服务器
├── Makefile             # 方便管理博客的Makefile
├── pelicanconf.py       # 主配置文件
└── publishconf.py       # 主发布文件，可删除
```
###写博文
在content目录下用Markdown语法来写一篇文章。
文章的开头一定要有虾米那这些关键字，make publish的时候会报错。
```bash
Date: xxxx-xx-xx
Title: xxxx
Tags: xxxxx
Category: xxxxx
Slug: xxxxxx
```
写完博文之后就可以通过下面的命令生成网页了。
```bash
make publish
```
运行完这个命令后会在output文件夹里生成好多网页和文件夹，就是这个网站的css,image还有各种其他东西。要注意的是，这个命令会将原来的东西删除了再重新生成的。下面会讲到如何配置才能不全部删除再生成。
如果想通过网页看看博客的效果可以用下面这个命令
```bash
make serve
```
执行上面那条命令就会建一个服务器吧。可在本机`http://127.0.0.1:8000` 看到效果。有些浏览器可能说没有证书没法打开。我用Opera无法打开，用Chrome正常打开。

到此为止，在本地的工作就完成了。需要将本地的项目上传到github上了。
进入到output文件夹。
用下面这条命令生成一个.git文件，记录这个项目是否修改之类的信息。
```bash
git init
```
将这个文件夹与github上的仓库绑定。就是说之后push的时候知道往哪push
```bash
git remote add origin https://github.com/username/username.github.io.git
```
将修改的文件信息添加到.git中
```bash
git add .
git commit -m "update"
```
将文件上传到github上
```bash
git push origin master
```
就这样，github上的文件就跟你本地上的一样了。可以通过访问site_username.github.io来访问你的内容了。

为了开发方便，你也可以用它的makefile文件去发布你的网页。
makefile文件有两个地方需要修改的。

```bash
publish:
	$(PELICAN) $(INPUTDIR) -o $(OUTPUTDIR) -s $(CONFFILE) $(PELICANOPTS)

github: publish
	cd $(OUTPUTDIR) && git add .
	cd $(OUTPUTDIR) && git commit -am 'commit' 
	cd $(OUTPUTDIR) && git push  origin $(GITHUB_PAGES_BRANCH)
```
原来文件是这样的
```bash
publish:
	$(PELICAN) $(INPUTDIR) -o $(OUTPUTDIR) -s $(PUBLISHCONF) $(PELICANOPTS)

github: publish
	ghp-import -m "Generate Pelican site" -b $(GITHUB_PAGES_BRANCH) $(OUTPUTDIR)
	git push origin $(GITHUB_PAGES_BRANCH)
```
原来的publish中的$(PUBLISHCONF)指向的脚本会清楚原来文件夹里的东西再生成html文件的，而$(CONFFILE)指向的脚本只是更新。我们.git仓库的东西不希望被删除，所以一定要修改成上面的那个形式。
github那一栏原来的内容我不太懂，但是主要就是想执行我们之前通过命令行进行上传的3条命令。所以我就改为上面所示，之所以每条命令前面都要加"cd $(OUTPUTDIR) &&"是因为执行完一条命令后它的当前目录就变为了执行make命令的目录，不相信的朋友可以在github那一栏中加一句命令dir看看。



