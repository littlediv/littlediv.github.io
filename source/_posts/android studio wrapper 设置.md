# android studio wrapper 设置 #

## wrapper 位置 ##

1. 默认路径


	C:\Users\XXX\.gradle\wrapper\dists\gradle-2.8-all\ah86jmo43de9lfa8xg9ux3c4h

2. 本地路径

	D:\android-studio\gradle\gradle-2.10

## 	修改wrapper  ##

1. 配置gradle时我们必须在{project.dir}\gradle\wrapper\gradle-wrapper.properties文件中配置gradle

	distributionUrl=https://services.gradle.org/distributions/gradle-2.8-all.zip

	改成

	distributionUrl=https://services.gradle.org/distributions/gradle-2.10-all.zip

	或使用本地文件

	distributionUrl=file\:/C:/Users/XXX/.gradle\wrapper/dists/gradle-2.8-all.zip
	(不知道哪的问题不可行)

2. android studio 设置

	file -> settings -> build,execution,deployment -> build tools -> gradle

	project-level settings
	
	点选 use local gradle sidtribution

	gradle home D:\android-studio\gradle\gradle-2.10
	




## 参考信息 ##

[Android Studio遇到的那些坑及爬坑方法](http://blog.csdn.net/cdwlx/article/details/51581663)

[ android studio 配置本地gradle](http://blog.csdn.net/kasieryang/article/details/51842953)

[AndroidStudio gradle配置](http://www.cnblogs.com/wxishang1991/p/5457878.html)