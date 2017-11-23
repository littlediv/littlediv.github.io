# Android 启动白屏 #

## 启动页面 Android Starting Window(Preview Window) ##

当打开一个Activity时，如果这个Activity所属的应用还没有在运行，系统会为这个Activity所属的应用创建一个进程，但进程的创建与初始化都需要时间，在这个动作完成之前系统要做什么呢？如果没有任何反应的话，如果程序初始化的时间很长，用户可能还以为没有点到相应的位置。但此时所启动的程序还没初始化完，既无法显示程序，又不能停在原处不做任何动作，怎么办？这就有了**Starting Window**的概念，也可以称之为**Preview Window**。

Starting Window就是一个用于在应用程序进程创建并初始化成功前显示的临时窗口，拥有的Window Type是TYPE_APPLICATION_STARTING。在程序初始化完成前显示这个窗口，以告知用户系统已经知道了他要打开这个应用并做出了响应，当程序初始化完成后显示用户UI并移除这个窗口。

这个Starting Window我们都见过，不过可能没留意过，其实就是开启程序时黑屏的那个窗口，够丑的。不过也没办法，每个程序的界面都不是同的，系统只有默认显示一个很简单的窗口了。

如果所谓的Starting Window只是一个黑屏的窗口的话，那这个功能未免也太鸡肋了。其实系统是可以根据每个程序的Theme显示不同的样子的。

启动应用的时候，虽然我们的程序还没初始化，但程序内的组件可是在程序安装的时候就被系统分析注册了的。我们可以针对每个Application和Activity设置不同的Theme，系统就是根据这个Theme初始化Starting Window的。Window布局的顶层是DecorView，Starting Window就是显示一个空的但是应用了Activity指定的Theme（如果Activity没有指定就用Application的）的DecorView。

在Theme中可以指定很多东西，如ActionBar的样式，窗口的背景，Activity的图标等，通过给Activity指定Theme，系统就可以在我们的应用初始化完成之前将这个Theme应用到Starting Window，这样看起来就像我们的应用已经启动起来了，只是数据内容还没有初始化好。

所以，如果你的Activity的背景只是简单的纯色的话，最好直接通过Theme把它应用到Activity的Background，而不是设置为顶层Layout的背景，如果真的需要给顶层Layout设置背景，也可以给android:windowBackground设置一个和Activity UI相似的背景，为了防止Overdraw，在Activity的onCreate中通过setWindowBackground()再把窗口的背景设置为null。

## 解决方法 ##

设置主题 （设置透明背景 或 添加与 Splash 启动页相同的图片）

	<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
	    ......
	    <item name="android:windowIsTranslucent">true</item>
	    <item name="android:windowNoTitle">true</item>
	</style>


	<style name="AppSplash" parent="android:Theme">    
	    <item name="android:windowBackground">@drawable/ipod_bg</item>    
	    <item name="android:windowNoTitle">true</item>    
	</style>  

应用主题

	<activity   
       android:theme="@style/AppSplash"  
       android:name=".SplashActivity" >  
    </activity>  

tip:

如果在 splash 的 theme 设置背景的话，那么就可以去掉 layout 中的相同图片，防止可能出现的图片跳动现象

## 参考信息 ##

[Android Starting Window(Preview Window)](http://www.cnblogs.com/angeldevil/p/3801209.html)

[Android 应用启动界面分析（Starting Window）](http://www.2cto.com/kf/201604/503761.html)

[Android App 启动页(Splash)黑/白闪屏现象产生原因与解决办法](http://blog.csdn.net/zivensonice/article/details/51691136)