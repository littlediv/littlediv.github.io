# android  打包相关问题#

## * What went wrong:
Execution failed for task ':app:transformClassesWithDexForRelease'.
> com.android.build.api.transform.TransformException: com.android.ide.common.process.ProcessException: java.util.concurrent.ExecutionException:
com.android.dex.DexException: Multiple dex files define Landroid/support/v4/accessibilityservice/AccessibilityServiceInfoCompat$AccessibilitySer
viceInfoVersionImpl;

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

BUILD FAILED

查看是否引用多余的 v4 jar 

>最后发现其实是引用工程libs目录下有一个android-support-v4.jar包，通过compile fileTree(include: '*.jar', dir: 'libs')来引入v4包的，把这个jar包删除，改为gradle引入 compile 'com.android.support:support-v4:24.1.1'问题解决，可能是gradle引入的gradle才能自动解决重复的包，而通过jar包引入的gradle处理不了）

设置GRADLE_HOME环境变量和path，具体地址可查看android studio -- file --setting 搜gradle

命令行执行gradle -v正常的话
到项目路径执行gradle -q app:dependencies 或到app module下执行gradle -q dependencies查看依赖树


	D:\Travel>gradle -q app:dependencies

	------------------------------------------------------------
	Project :app
	------------------------------------------------------------
	
	_debugAndroidTestAnnotationProcessor - ## Internal use, do not manually configure ##
	No dependencies
	
	_debugAndroidTestApk - ## Internal use, do not manually configure ##
	No dependencies
	
	_debugAndroidTestCompile - ## Internal use, do not manually configure ##
	No dependencies
	
	_debugAnnotationProcessor - ## Internal use, do not manually configure ##
	No dependencies
	
	_debugApk - ## Internal use, do not manually configure ##
	+--- com.meituan.android.walle:library:1.1.5
	|    +--- com.android.support:support-annotations:24.1.1 -> 25.2.0
	|    \--- com.meituan.android.walle:payload_reader:1.1.5
	+--- com.android.support:design:23.2.1
	|    +--- com.android.support:support-v4:23.2.1
	|    |    \--- com.android.support:support-annotations:23.2.1 -> 25.2.0
	|    +--- com.android.support:appcompat-v7:23.2.1
	|    |    +--- com.android.support:animated-vector-drawable:23.2.1
	|    |    |    \--- com.android.support:support-vector-drawable:23.2.1
	|    |    |         \--- com.android.support:support-v4:23.2.1 (*)
	|    |    +--- com.android.support:support-vector-drawable:23.2.1 (*)
	|    |    \--- com.android.support:support-v4:23.2.1 (*)
	|    \--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0
	|         +--- com.android.support:support-annotations:25.2.0
	|         +--- com.android.support:support-compat:25.2.0
	|         |    \--- com.android.support:support-annotations:25.2.0
	|         \--- com.android.support:support-core-ui:25.2.0
	|              +--- com.android.support:support-annotations:25.2.0
	|              \--- com.android.support:support-compat:25.2.0 (*)
	+--- com.android.support:appcompat-v7:23.2.1 (*)
	+--- com.squareup.retrofit2:retrofit:2.1.0
	|    \--- com.squareup.okhttp3:okhttp:3.3.0 -> 3.4.1
	|         \--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.retrofit2:converter-gson:2.1.0
	|    +--- com.squareup.retrofit2:retrofit:2.1.0 (*)
	|    \--- com.google.code.gson:gson:2.7
	+--- com.squareup.okhttp3:logging-interceptor:3.1.2
	|    \--- com.squareup.okhttp3:okhttp:3.1.2 -> 3.4.1 (*)
	+--- com.apkfuns.logutils:library:1.0.6
	+--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.okhttp3:okhttp:3.4.1 (*)
	+--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0 (*)
	+--- com.android.support:cardview-v7:23.2.1
	+--- com.jakewharton:butterknife:8.4.0
	|    +--- com.jakewharton:butterknife-annotations:8.4.0
	|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	|    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	+--- com.squareup.picasso:picasso:2.5.2
	+--- com.yanzhenjie:recyclerview-swipe:1.0.4
	|    \--- com.android.support:recyclerview-v7:25.2.0 (*)
	+--- com.google.zxing:core:3.2.1
	\--- com.journeyapps:zxing-android-embedded:3.3.0
	     +--- com.google.zxing:core:3.2.1
	     \--- com.android.support:support-v4:23.1.0 -> 23.2.1 (*)
	
	_debugCompile - ## Internal use, do not manually configure ##
	+--- com.meituan.android.walle:library:1.1.5
	|    +--- com.android.support:support-annotations:24.1.1 -> 25.2.0
	|    \--- com.meituan.android.walle:payload_reader:1.1.5
	+--- com.android.support:design:23.2.1
	|    +--- com.android.support:support-v4:23.2.1
	|    |    \--- com.android.support:support-annotations:23.2.1 -> 25.2.0
	|    +--- com.android.support:appcompat-v7:23.2.1
	|    |    +--- com.android.support:animated-vector-drawable:23.2.1
	|    |    |    \--- com.android.support:support-vector-drawable:23.2.1
	|    |    |         \--- com.android.support:support-v4:23.2.1 (*)
	|    |    +--- com.android.support:support-vector-drawable:23.2.1 (*)
	|    |    \--- com.android.support:support-v4:23.2.1 (*)
	|    \--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0
	|         +--- com.android.support:support-annotations:25.2.0
	|         +--- com.android.support:support-compat:25.2.0
	|         |    \--- com.android.support:support-annotations:25.2.0
	|         \--- com.android.support:support-core-ui:25.2.0
	|              +--- com.android.support:support-annotations:25.2.0
	|              \--- com.android.support:support-compat:25.2.0 (*)
	+--- com.android.support:appcompat-v7:23.2.1 (*)
	+--- com.squareup.retrofit2:retrofit:2.1.0
	|    \--- com.squareup.okhttp3:okhttp:3.3.0 -> 3.4.1
	|         \--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.retrofit2:converter-gson:2.1.0
	|    +--- com.squareup.retrofit2:retrofit:2.1.0 (*)
	|    \--- com.google.code.gson:gson:2.7
	+--- com.squareup.okhttp3:logging-interceptor:3.1.2
	|    \--- com.squareup.okhttp3:okhttp:3.1.2 -> 3.4.1 (*)
	+--- com.apkfuns.logutils:library:1.0.6
	+--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.okhttp3:okhttp:3.4.1 (*)
	+--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0 (*)
	+--- com.android.support:cardview-v7:23.2.1
	+--- com.jakewharton:butterknife:8.4.0
	|    +--- com.jakewharton:butterknife-annotations:8.4.0
	|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	|    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	+--- com.squareup.picasso:picasso:2.5.2
	+--- com.yanzhenjie:recyclerview-swipe:1.0.4
	|    \--- com.android.support:recyclerview-v7:25.2.0 (*)
	+--- com.google.zxing:core:3.2.1
	\--- com.journeyapps:zxing-android-embedded:3.3.0
	     +--- com.google.zxing:core:3.2.1
	     \--- com.android.support:support-v4:23.1.0 -> 23.2.1 (*)
	
	_debugUnitTestAnnotationProcessor - ## Internal use, do not manually configure ##
	No dependencies
	
	_debugUnitTestApk - ## Internal use, do not manually configure ##
	\--- junit:junit:4.12
	     \--- org.hamcrest:hamcrest-core:1.3
	
	_debugUnitTestCompile - ## Internal use, do not manually configure ##
	\--- junit:junit:4.12
	     \--- org.hamcrest:hamcrest-core:1.3
	
	_releaseAnnotationProcessor - ## Internal use, do not manually configure ##
	No dependencies
	
	_releaseApk - ## Internal use, do not manually configure ##
	+--- com.meituan.android.walle:library:1.1.5
	|    +--- com.android.support:support-annotations:24.1.1 -> 25.2.0
	|    \--- com.meituan.android.walle:payload_reader:1.1.5
	+--- com.android.support:design:23.2.1
	|    +--- com.android.support:support-v4:23.2.1
	|    |    \--- com.android.support:support-annotations:23.2.1 -> 25.2.0
	|    +--- com.android.support:appcompat-v7:23.2.1
	|    |    +--- com.android.support:animated-vector-drawable:23.2.1
	|    |    |    \--- com.android.support:support-vector-drawable:23.2.1
	|    |    |         \--- com.android.support:support-v4:23.2.1 (*)
	|    |    +--- com.android.support:support-vector-drawable:23.2.1 (*)
	|    |    \--- com.android.support:support-v4:23.2.1 (*)
	|    \--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0
	|         +--- com.android.support:support-annotations:25.2.0
	|         +--- com.android.support:support-compat:25.2.0
	|         |    \--- com.android.support:support-annotations:25.2.0
	|         \--- com.android.support:support-core-ui:25.2.0
	|              +--- com.android.support:support-annotations:25.2.0
	|              \--- com.android.support:support-compat:25.2.0 (*)
	+--- com.android.support:appcompat-v7:23.2.1 (*)
	+--- com.squareup.retrofit2:retrofit:2.1.0
	|    \--- com.squareup.okhttp3:okhttp:3.3.0 -> 3.4.1
	|         \--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.retrofit2:converter-gson:2.1.0
	|    +--- com.squareup.retrofit2:retrofit:2.1.0 (*)
	|    \--- com.google.code.gson:gson:2.7
	+--- com.squareup.okhttp3:logging-interceptor:3.1.2
	|    \--- com.squareup.okhttp3:okhttp:3.1.2 -> 3.4.1 (*)
	+--- com.apkfuns.logutils:library:1.0.6
	+--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.okhttp3:okhttp:3.4.1 (*)
	+--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0 (*)
	+--- com.android.support:cardview-v7:23.2.1
	+--- com.jakewharton:butterknife:8.4.0
	|    +--- com.jakewharton:butterknife-annotations:8.4.0
	|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	|    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	+--- com.squareup.picasso:picasso:2.5.2
	+--- com.yanzhenjie:recyclerview-swipe:1.0.4
	|    \--- com.android.support:recyclerview-v7:25.2.0 (*)
	+--- com.google.zxing:core:3.2.1
	\--- com.journeyapps:zxing-android-embedded:3.3.0
	     +--- com.google.zxing:core:3.2.1
	     \--- com.android.support:support-v4:23.1.0 -> 23.2.1 (*)
	
	_releaseCompile - ## Internal use, do not manually configure ##
	+--- com.meituan.android.walle:library:1.1.5
	|    +--- com.android.support:support-annotations:24.1.1 -> 25.2.0
	|    \--- com.meituan.android.walle:payload_reader:1.1.5
	+--- com.android.support:design:23.2.1
	|    +--- com.android.support:support-v4:23.2.1
	|    |    \--- com.android.support:support-annotations:23.2.1 -> 25.2.0
	|    +--- com.android.support:appcompat-v7:23.2.1
	|    |    +--- com.android.support:animated-vector-drawable:23.2.1
	|    |    |    \--- com.android.support:support-vector-drawable:23.2.1
	|    |    |         \--- com.android.support:support-v4:23.2.1 (*)
	|    |    +--- com.android.support:support-vector-drawable:23.2.1 (*)
	|    |    \--- com.android.support:support-v4:23.2.1 (*)
	|    \--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0
	|         +--- com.android.support:support-annotations:25.2.0
	|         +--- com.android.support:support-compat:25.2.0
	|         |    \--- com.android.support:support-annotations:25.2.0
	|         \--- com.android.support:support-core-ui:25.2.0
	|              +--- com.android.support:support-annotations:25.2.0
	|              \--- com.android.support:support-compat:25.2.0 (*)
	+--- com.android.support:appcompat-v7:23.2.1 (*)
	+--- com.squareup.retrofit2:retrofit:2.1.0
	|    \--- com.squareup.okhttp3:okhttp:3.3.0 -> 3.4.1
	|         \--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.retrofit2:converter-gson:2.1.0
	|    +--- com.squareup.retrofit2:retrofit:2.1.0 (*)
	|    \--- com.google.code.gson:gson:2.7
	+--- com.squareup.okhttp3:logging-interceptor:3.1.2
	|    \--- com.squareup.okhttp3:okhttp:3.1.2 -> 3.4.1 (*)
	+--- com.apkfuns.logutils:library:1.0.6
	+--- com.squareup.okio:okio:1.9.0
	+--- com.squareup.okhttp3:okhttp:3.4.1 (*)
	+--- com.android.support:recyclerview-v7:23.2.1 -> 25.2.0 (*)
	+--- com.android.support:cardview-v7:23.2.1
	+--- com.jakewharton:butterknife:8.4.0
	|    +--- com.jakewharton:butterknife-annotations:8.4.0
	|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	|    \--- com.android.support:support-annotations:24.1.0 -> 25.2.0
	+--- com.squareup.picasso:picasso:2.5.2
	+--- com.yanzhenjie:recyclerview-swipe:1.0.4
	|    \--- com.android.support:recyclerview-v7:25.2.0 (*)
	+--- com.google.zxing:core:3.2.1
	\--- com.journeyapps:zxing-android-embedded:3.3.0
	     +--- com.google.zxing:core:3.2.1
	     \--- com.android.support:support-v4:23.1.0 -> 23.2.1 (*)
	
	_releaseUnitTestAnnotationProcessor - ## Internal use, do not manually configure ##
	No dependencies
	
	_releaseUnitTestApk - ## Internal use, do not manually configure ##
	\--- junit:junit:4.12
	     \--- org.hamcrest:hamcrest-core:1.3
	
	_releaseUnitTestCompile - ## Internal use, do not manually configure ##
	\--- junit:junit:4.12
	     \--- org.hamcrest:hamcrest-core:1.3
	
	androidJacocoAgent - The Jacoco agent to use to get coverage data.
	\--- org.jacoco:org.jacoco.agent:0.7.5.201505241946
	
	androidJacocoAnt - The Jacoco ant tasks to use to get execute Gradle tasks.
	\--- org.jacoco:org.jacoco.ant:0.7.5.201505241946
	     +--- org.jacoco:org.jacoco.core:0.7.5.201505241946
	     |    \--- org.ow2.asm:asm-debug-all:5.0.1
	     +--- org.jacoco:org.jacoco.report:0.7.5.201505241946
	     |    +--- org.jacoco:org.jacoco.core:0.7.5.201505241946 (*)
	     |    \--- org.ow2.asm:asm-debug-all:5.0.1
	     \--- org.jacoco:org.jacoco.agent:0.7.5.201505241946
	
	androidTestAnnotationProcessor - Classpath for the annotation processor for 'androidTest'.
	No dependencies
	
	androidTestApk - Classpath packaged with the compiled 'androidTest' classes.
	No dependencies
	
	androidTestApt


不同包引入重复冲突了，各种exclude，如下，
	
>compile ('com.android.support:recyclerview-v7:+'){
>exclude module: 'support-v4'    //exclude group: "com.android.support", module: "support-v4"
>}





	