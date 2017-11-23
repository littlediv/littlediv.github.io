---
title: ubuntu 搭建 hexo 
tags: ubuntu hexo
grammar_cjkRuby: true
---


## 安装 Node.js

	 sudo add-apt-repository ppa:chris-lea/node.js
	 sudo apt-get update
	 sudo apt-get install nodejs

## 安装 Git
	
	sudo apt-get install git

## 安装 npm

	sudo apt-get install npm
	
## 安装 Hexo

	sudo npm install hexo -g
	(安装半天装不上，更换了命令  
	sudo npm install hexo-cli -g
	npm install hexo –save)
	
## 初始你博客的根目录（或者cd到指定目录下，然后执行hexo init）

	hexo init <dir> 
	
## 安装依赖包

	npm install
	
## 本地查看

	hexo server
	在浏览器中输入localhost:4000即可看到效果
	
## 生成静态网站
	hexo generate
	
## 关联github

### 创建代码仓库
	进入自己的账号后，左上角有个“+”，点击可选择“New repository” 进入 Create a new repository页面
	Repository name 一定要写 XXX.github.io(XXX是账号名称)
	Description 随便写
	免费的只能选 Prblic
	勾选 Initialize this repository with a README
	Add a license: 先随便选个，我选的GUN General Public License v3.0
	最后点击 Create repository
	
### 设置站点主题
	在XXX.github.io的repository下点击settings,在下面的内容中选择“GitHub Pages”中“Automatic Page Generator”的“Launch Automatic Page Generator”，进入主页设计页面

	点选“README.md load”，点击 “Continue To Layouts”，选择模板样式，点击左上角的 “Publish page” 发布页面
	
### 配置SSH

	让本地git项目与远程github相关联
	安装git后，也带了ssh
	输入 ssh-keygen -t rsa 接着接着连续三个回车键（不需要输入密码）
	在打印的信息中可以看到生成的id_rsa 和 id_rsa.pub 以及它们所在的路径
	在最原始的github网页中，点击左上角，有个向下箭头，点击“settings”
	在左侧点击 “SSH and GPG keys”
	接着右上角点击“ New SSH key”
	tilte不用写
	把id_rsa.pub里面的内容全部复制到Key里面
	点击“ Add SSH key”，即可完成
	之后会给你注册的邮箱发送相关信息，不用管

	可在git中输入 ssh -T git@github.com 测试，有“successful”字样就成功了
	
## 编辑文章

### 新建文章
	hexo new “标题”

在 _posts 目录下会生成文件标题.md

title: Hello World
date: 2015-07-30 07:56:29 #发表日期，一般不改动
categories: hexo #文章文类
tags: [hexo,github] #文章标签，多于一项时用这种格式
正文，使用Markdown语法书写

### 编辑完后保存, 预览
	hexo server
	
## hexo 部署
	执行下列指令完成部署
	npm install hexo-deployer-git --save
	hexo generate
	hexo deploy
	
## 问题
### hexo无法上传到github

终于找到原因了，真的坑，在_config.yml中名/值中要有空格例如：type：（空格）git，虽然之前在万能的谷歌搜索了，但是关键词没有用对，这个检索就出来了，https://github.com/hexojs/hexo/issues/1154

我真是醉了。
ps。最新版的hexo貌似要在第一次hexo d的之前，要有npm install hexo-deployer-git --save具体看链接描述
如果你碰到问题了我发现他们的官方github的issue上一搬都能找到答案，真心建议，遇到问题就上这里就行了

## 如何解决github+Hexo的博客多终端同步问题

### 在其中一个终端操作，push本地文件夹Hexo中的必要文件到yourname.github.io的hexo分支上

	git init  //初始化本地仓库
	git add source //将必要的文件依次添加，有些文件夹如npm install产生的node_modules由于路径过长不好处理，所以这里没有用`git add .`命令了，而是依次添加必要文件，如下图所示
	git commit -m "Blog Source Hexo"
	git branch hexo  //新建hexo分支
	git checkout hexo  //切换到hexo分支上
	git remote add origin git@github.com:yourname/yourname.github.io.git  //将本地与Github项目对接
	git push origin hexo  //push到Github项目的hexo分支上
	
这样你的github项目中就会多出一个Hexo分支，这个就是用于多终端同步关键的部分。

### 另一终端完成clone和push更新

此时在另一终端更新博客，只需要将Github的hexo分支clone下来，进行初次的相关配置

	git clone -b hexo git@github.com:yourname/yourname.github.io.git  //将Github中hexo分支clone到本地
	cd  yourname.github.io  //切换到刚刚clone的文件夹内
	npm install    //注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再init
	hexo new post "new blog name"   //新建一个.md文件，并编辑完成自己的博客内容
	git add source  //经测试每次只要更新sorcerer中的文件到Github中即可，因为只是新建了一篇新博客
	git commit -m "XX"
	git push origin hexo  //更新分支
	hexo d -g   //push更新完分支之后将自己写的博客对接到自己搭的博客网站上，同时同步了Github中的master
	
### 不同终端间愉快地玩耍

在不同的终端已经做完配置，就可以愉快的分享自己更新的博客 
进入自己相应的文件夹

	git pull origin hexo  //先pull完成本地与远端的融合
	hexo new post " new blog name"
	git add source
	git commit -m "XX"
	git push origin hexo
	hexo d -g



## 参考信息

[ ubuntu下使用hexo搭建博客](http://blog.csdn.net/kesarchen/article/details/50579550)

[Ubuntu上用hexo建立自己的博客](http://www.jianshu.com/p/24cb74aeb0a3)

[如何解决github+Hexo的博客多终端同步问题](http://blog.csdn.net/Monkey_LZL/article/details/60870891)

[搭建hexo博客并简单的实现多终端同步](https://righere.github.io/2016/10/10/install-hexo/)

[小白教你在Ubuntu上用Hexo搭建博客托管到github](http://www.jianshu.com/p/a0a27d840992)