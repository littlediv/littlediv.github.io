# Markdown 教程

(MarkDownPad2)

## 标题

	# 一级标题
    ## 二级标题
    ### 三级标题
	#### 四级标题
	##### 五级标题
	###### 六级标题

过在文字下方添加“=”和“-”，他们分别表示一级标题和二级标题。

	你好
	==

## 加粗，斜体

	加粗 **内容** 或 __内容__（两个下划线）
	斜体 *内容*	或 _内容_ （一个下划线）

## 层次

	1. 第一节
	* 第二节(你不用敲 "2"，自动就有了）
    	* 第一小节（推荐每层次缩进四个空格）
        	* 小小节 1
        	* 小小节 2
		* 第二小节
	“*” 后面要加空格，这是必须的，除了 *，还可以使用 + 或者 -。

1. 第一节
	* 第二节(你不用敲 "2"，自动就有了）
    	* 第一小节（推荐每层次缩进四个空格）
        	* 小小节 1
        	* 小小节 2
		* 第二小节
	

##  链接，图片

Markdown中有两种方式，实现链接，分别为内联方式和引用方式。
内联方式：This is an [example link](http://example.com/).
引用方式：
I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].  

[1]: http://google.com/        "Google" 
[2]: http://search.yahoo.com/  "Yahoo Search" 
[3]: http://search.msn.com/    "MSN Search"

图片的处理方式和链接的处理方式，非常的类似。
内联方式：![alt text](/path/to/img.jpg "Title")
引用方式：
![alt text][id] 

[id]: /path/to/img.jpg "Title"



	链接 []()
	图片 ![]()



## 字体、颜色

	<font face="黑体">我是黑体字</font>
	<font face="微软雅黑">我是微软雅黑</font>
	<font face="STCAIYUN">我是华文彩云</font>
	<font color=#0099ff size=12 face="黑体">黑体</font>
	<font color=#00ffff size=3>null</font>
	<font color=gray size=5>gray</font>

<font face="黑体">我是黑体字</font>

<font face="微软雅黑">我是微软雅黑</font>

<font face="STCAIYUN">我是华文彩云</font>

<font color=#0099ff size=12 face="黑体">黑体</font>

<font color=#00ffff size=3>null</font>

<font color=gray size=5>gray</font>

## 段落缩进（空格）

	半方大的空白&ensp;或&#8194;看，飞碟
	全方大的空白&emsp;或&#8195;看，飞碟
	不断行的空白格&nbsp;或&#160;看，飞碟
	&emsp;&emsp;段落从此开始。

半方大的空白&ensp;或&#8194;看，飞碟

全方大的空白&emsp;或&#8195;看，飞碟

不断行的空白格&nbsp;或&#160;看，飞碟

&emsp;&emsp;段落从此开始。

## 引用（块注释 blockquote）

通过在文字开头添加“>”表示块注释。（当>和文字之间添加五个blank时，块注释的文字会有变化。）

	> 这是引用
	 
> 这是引用

##  脚注（footnote） ##

实现方式如下：

hello[^hello]
[^hello]: hi

## 表格 ##

今天在用Markdown Pad学习Markdown语法时发现无法输入表格，后来在官网上找到解决方案。

找到工具--选项--Markdown，然后更改Markdown处理器为Markdown（扩展），点击保存并关闭后，即可在Markdown Pad中输入表格了。

邮箱：

Soar360@live.com

授权秘钥：

GBPduHjWfJU1mZqcPM3BikjYKF6xKhlKIys3i1MU2eJHqWGImDHzWdD6xhMNLGVpbP2M5SN6bnxn2kSE8qHqNY5QaaRxmO3YSMHxlv2EYpjdwLcPwfeTG7kUdnhKE0vVy4RidP6Y2wZ0q74f47fzsZo45JE2hfQBFi2O9Jldjp1mW8HUpTtLA2a5/sQytXJUQl/QKO0jUQY4pa5CCx20sV1ClOTZtAGngSOJtIOFXK599sBr5aIEFyH0K7H4BoNMiiDMnxt1rD8Vb/ikJdhGMMQr0R4B+L3nWU97eaVPTRKfWGDE8/eAgKzpGwrQQoDh+nzX1xoVQ8NAuH+s4UcSeQ==

| 姓名     | 年龄| 性别|
|:--------|---------:|:-------:|
| 张三| 18| 女      |
| 小明| 23| 男      |

## 其他

---

[5. 其他][null-link]

你可能还没注意到本文每部分之间的分割线和 `其他` 的链接其实没有链接
我爱 `分割线`， 我爱 [**链接**][null-link]，哪怕它只有颜色~

[null-link]: chrome://not-a-link

	---
	
	# [5. 其他][null-link]
	
	你可能还没注意到本文每部分之间的分割线和 `其他` 的链接其实没有链接
	我爱 `分割线`， 我爱 [**链接**][null-link]，哪怕它只有颜色~
	
	[null-link]: chrome://not-a-link
	“---” 的上下最好各空一行

**P.S.** 补充一种高端的链接: [鼠标移过来，**先别单击** ~][hover]
[hover]: http://www.google.com.sg "Google Sg 更快，更好用。好，现在单击吧"

	**P.S.** 补充一种高端的链接: [鼠标移过来，**先别单击** ~][hover]
	[hover]: http://www.google.com.sg "Google Sg 更快，更好用。好，现在单击吧"

 图片链接：(点击图片可跳转）

	[![][jane-eyre-pic]][jane-eyre-douban]
	[jane-eyre-pic]: http://img3.douban.com/mpic/s1108264.jpg
	[jane-eyre-douban]: http://book.douban.com/subject/1141406/

参考信息：

[Markdown 简明教程](http://www.jianshu.com/p/7bd23251da0a)
[Markdown 11种基本语法](http://www.cnblogs.com/hnrainll/p/3514637.html)
