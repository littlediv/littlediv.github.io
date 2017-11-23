# Andoid studio 更改包名 #

在 Android studio 所建工程中，左侧 Project 选项卡中有 Project 、 Android 、 Packages 等选项， 选择 Project 即可。

第一步：在 Project 选项右侧有个类似齿轮的图片， 点击后可任选以下两种操作

1. 如果勾选的是 Flatten Packages， 需要取消勾选的 Hide Empty Middle Packages, 这样可以将所有包依次显示出来，需要修改那个名字则选中该位置右键 Refactor -> Rename 更改即可
2. 如果是取消勾选的 Flatten Packages，需要取消勾选的 Compact Empty Middle Packages， 这样可以将所有包依次阶梯状显示出来，需要修改那个名字则选中该位置右键 Refactor -> Rename 更改即可

**注意：只能修改最后一级的名称**

如果想要添加一级名称，需要显示上面介绍的显示结构后， 右键 New -> Package 新建后，将下级移动到新建名称下


第二步：build.gradle(module：APP名字)文件中的 applicationId 改成之后的包名 (可以在 manifest 的根节点下直接复制 package )

第三步： clean 和 bebuild

第四步： 可能需要重启 studio

## 参考信息 ##

[Android Studio 修改包名最便捷做法](http://www.cnblogs.com/Kyouhui/p/4632813.html)


