# android studio adb shell 命令 #

## 获取手机分辨率 ##

**通用方法：**

	>adb shell dumpsys window displays |head -n 3

输出类似如下信息：

>WINDOW MANAGER DISPLAY CONTENTS (dumpsys window displays)
>
> Display: mDisplayId=0
>
> init=1080x1920 440dpi cur=1080x1920 app=1080x1920 
>
>rng=1080x1025-1920x1865

**高通平台**

>adb shell wm size

输出：

>Physical size: 1080x1920
