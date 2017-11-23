# 杂项

## 不同的点击事件

1 relativelayout 代码有点击监听

textview drawableright tv.selector  

单击rl drawable可变化

imagebutton selector 不变

(imageview selector 变化了。。)

单击 ib ib.selector 变化  rl不能收到监听事件

2 imageview selector 要可变的话

android:clickalbe = "true"
或代码里做了监听事件

（可能tv 等同理）


## viewpager 和 fragmentPageradapter 使用
new 3 个fragment 都生成了，但第三个 fragment 控件id 都为空， 没执行 oncreateview 方法

---使用 viewpager.setOffscreenPageLimit(3)  可行 *（不知道为什么）