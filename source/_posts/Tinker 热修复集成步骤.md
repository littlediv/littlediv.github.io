# Tinker 热修复集成步骤 #

## tinker-sample-android ##

gradle 2.10
gradle-plugin 2.2.2 (我使用 1.5.0 也没问题)

[github 下载地址](https://github.com/Tencent/tinker)

（可参考 wiki）

只导入 tinker-sample-android 
gradle 设置 2.10 设置方法见 《android studio wrapper 设置》

**添加依赖** [gradle接入](https://github.com/Tencent/tinker/wiki/Tinker-%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97)

在项目的build.gradle中，添加tinker-patch-gradle-plugin的依赖

	buildscript {
	    dependencies {
	        classpath ('com.tencent.tinker:tinker-patch-gradle-plugin:1.7.7')
	    }
	}

然后在app的gradle文件app/build.gradle，我们需要添加tinker的库依赖以及apply tinker的gradle插件.

	dependencies {
	    //可选，用于生成application类 
	    provided('com.tencent.tinker:tinker-android-anno:1.7.7')
	    //tinker的核心库
	    compile('com.tencent.tinker:tinker-android-lib:1.7.7') 
	}
	...
	...
	//apply tinker插件
	apply plugin: 'com.tencent.tinker.patch'

 **sample 使用方法**

调用**assembleDebug**编译，我们会将编译过的包保存在build/bakApk中。然后我们将它安装到手机，点击SHOW INFO按钮，可以看到补丁并没有加载.（显示 patch is not loaded）

修改代码，例如将MainActivity中I am on patch onCreate的Log打开。然后我们需要修改build.gradle中的参数，将步骤一编译保存的安装包路径拷贝到tinkerPatch中的oldApk参数中。

	在 bulid/bakApk 下生成 类似 app-debug-0222-15-01-08.apk 的安装包，在 build.gradle 中做如下替换
	
	//oldApk = getOldApkPath()
	oldApk = "${bakPath}/app-debug-0222-15-01-08.apk"

调用**tinkerPatchDebug**, 补丁包与相关日志会保存在/build/outputs/tinkerPatch/。然后我们将patch_signed_7zip.apk推送到手机的sdcard中。

点击LOAD PATCH按钮, 如果看到patch success, please restart process的toast，即可锁屏或者点击KILL SELF按钮

我们可以看到的确出现了I am on patch onCreate日志，同时点击SHOW INFO按钮，显示补丁包的确已经加载成功了。

如果使用 gradle assembleDebug 编译出现问题（因为我电脑之前是 gradle 2.8，  会出现 2.8 和 2.10 的问题）
可使用 gradle assembleDebug （前提已经配置了本地 gradle）

## Release //todo ##

## TinkerPatch补丁管理后台 ##

[TinkerPatch补丁管理后台](http://www.tinkerpatch.com/)

www.tinkerpatch.com 是第三方开发基于CDN分发的补丁管理后台。它提供了脚本后台托管，版本管理，保证传输安全等功能，让你无需搭建一个后台，无需关心部署操作，只需引入一个 SDK 即可立即使用 Tinker。

此外，TinkerPatch 平台增加了一键傻瓜式接入/编译管理优化等功能，它的Github地址为TinkerPatch。


### 问题 ###


[can’t get git rev, you should add git to system path or just input test value, such as ‘testTinkerId’](http://blog.csdn.net/gyh790005156/article/details/52796641)

微信团队的Android热修复框架Tinker的build.gradle里面有这样一行代码：

	def gitSha() {
	    try {
	        String gitRev = 'git rev-parse --short HEAD'.execute().text.trim()
	        if (gitRev == null) {
	            throw new GradleException("can't get git rev, you should add git to system path or just input test value, such as 'testTinkerId'")
	        }
	        return gitRev
	    } catch (Exception e) {
	        throw new GradleException("can't get git rev, you should add git to system path or just input test value, such as 'testTinkerId'")
	    }
	}

用于获取一个字符串，作为TINKER_ID。

但是直接用android引用tinker-sample-android项目，会报错：Error:Execution failed for 
task ‘:app:tinkerProcessDebugManifest’. tinkerId is not set

解决方法 

（1）安装Git 

（略）比较简单直接去官网下载：https://git-scm.com/downloads 

（2）将项目与git建立关联 

进入tinker-sample-android目录，输入 git init，然后就会看见这个 .git 的隐藏文件

（3）studio配置git 

file -> settings ->version control -> git 

path to git exexutable 添加 安装 git.exe 路径

可以点击右侧的test，显示出版本号表示路径没问题。  

（4）给项目设置版本管理 

version control 右侧会显示该项目有 git

这个时候再次同步Gradle，还是失败，不要放弃。

我们随便把项目中一个文件加入版本控制，并且Commit一下。

> 这个时候如果还是失败，报错can’t get git rev, you should add git to system path or just input test value, such as ‘testTinkerId’，这是因为没有配置git环境变量，百度一下配置git环境变量，然后重新打开一下android studio，就能通过了。

最后在\app\build\intermediates\tinker_intermediates\AndroidManifest.xml下就能看到

	<meta-data adnroid:name="TINKER_ID" android:value="906cce0"

**But 如果还报 tinkerId is not set 的错, 可在下面的问题找答案**

很多人遇到的第一个错误就是提示 tinkerId is not set ，这个是由于要获取Git的提交版本号，设置到Tinker，如果不是通过git clone方式下载的就可能出现这个错误，其实可以简单粗暴的方式解决，那就是在app/build.gradle中更改以下代码：

tinkerId = getTinkerIdValue()

更改为

tinkerId = "TinkerSample"//或者其他你想要的id


------

[将Tinke集成到自己的项目](http://blog.csdn.net/xiejc01/article/details/52735920)



## 参考信息 ##

[android 微信热修复Tinker接入过程以及使用方法](http://blog.csdn.net/a750457103/article/details/52815096)


[微信热修复tinker及tinker server快速接入](http://www.jianshu.com/p/b3a26ad37e56) 

[github gradle --- jp1017/tinker-sample-android](https://github.com/jp1017/tinker-sample-android/blob/master/app/build.gradle)

[将Tinke集成到自己的项目](http://blog.csdn.net/xiejc01/article/details/52735920)

[Android Tinker 简单好用的热修复 热更新](http://www.jianshu.com/p/3bd2cf801e4c)

[Tinker 接入指南](https://github.com/Tencent/tinker/wiki/Tinker-%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97)

[Tinker 接入指南 wiki](https://github.com/Tencent/tinker/wiki)

[android 微信热修复Tinker接入过程以及使用方法](http://blog.csdn.net/a750457103/article/details/52815096)

[android笔记之 tinker初步集成](http://blog.csdn.net/hunanqi/article/details/54379209)


[Tinker 常见问题](http://blog.csdn.net/tyk9999tyk/article/details/53391519#comments)