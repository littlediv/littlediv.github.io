# android button 显示 #

1.问题：

在 RelativeLayout 中 xml Button 在同一级的 view 下面，但却显示在了 view 上方

解决方案

在button 中添加
android:stateListAnimator="@null"

----------

可以看到，Button跑到了最上面，这是为毛啊？没找到答案，但找到了不是办法的解决办法，就是在Button上包一层布局即可。

后续。。

在Stack Overflow上找到了答案：
Starting with Lollipop, a StateListAnimator controlling elevation was added to the default Button style. In my experience, this forces buttons to appear above everything else regardless of placement in XML (or programmatic addition in your case). That might be what you're experiencing.
To resolve this you can add a custom StateListAnimator if you need it or simply set it to null if you don't.
XML:
android:stateListAnimator="@null"

Java:
Button button = new Button(this); button.setText("Button"); if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) { button.setStateListAnimator(null); }

More details: Android 5.0 android:elevation Works for View, but not Button?

也就是说在5.0以上，系统会在button上wrapper上一层效果，就是StateListAnimator。去掉它或者自定义一个就可以解决上述问题。


## 参考信息 ##

[FrameLayout&RelativeLayout下，和Button同级的View需注意](http://www.jianshu.com/p/850f14d11a0e)


