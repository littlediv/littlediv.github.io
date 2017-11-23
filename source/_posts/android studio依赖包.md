# Android Stuido 依赖包

## 网络不好，使用本地包

### 在AndroidStudio中如何显示出依赖库的本地存储路径

    task showMeCache << {
    configurations.compile.each { println it }
    }

将代码放在 Module 的 build.gradle 最下方
然后在控制台执行

    gradle showMeCache

即可看到所有依赖对应的路径
  

    :app:showMeCache
    /Users/mao/workpace/person/AppPlus/app/libs/lite-orm-1.7.0.jar
    /Users/mao/workpace/person/AppPlus/app/libs/umeng-update-v2.6.0.1.jar
    /Users/mao/Downloads/android/sdk/extras/android/m2repository/com/android/support/appcompat-v7/23.1.1/appcompat-v7-23.1.1.aar
    /Users/mao/Downloads/android/sdk/extras/android/m2repository/com/android/support/design/23.1.1/design-23.1.1.aar
    /Users/mao/.gradle/caches/modules-2/files-2.1/com.jenzz/materialpreference/1.3/def38f1784f5f789b10ed388e385f7857e765405/materialpreference-1.3.aar

参考信息： [在AndroidStudio中如何显示出依赖库的本地存储路径](http://blog.csdn.net/asd6340370/article/details/51829847)

依赖路径（约在如下目录 androidSDK 和用户 .gradle

1. D:\Android\Sdk\extras\android\m2repository\com\android\support
2. C:\Users\mac\.gradle\caches\modules-2\files-2.1

（在AndroidStudio中的"External Libraries"下有引用的library的列表, 选择某个library右键->"Library Properties ..."就可以看到你引用的库本地的存放路径）

### 添加本地 arr 文件依赖

1. 在Module中建立"libs"目录(如果没有的话)
2. 把aar文件拷入libs目录(假设为abc.aar) 
3. 在Module的build.gradle中加入：    
    repositories {

    flatDir {

    dirs 'libs'

    }

    }    

4. 在Module的build.gradle中的dependencies中加入：

    compile(name:'abc',ext:'aar')

参考信息： [Android Studio如何加本地aar文件依赖](http://www.jianshu.com/p/799887025ff7)
