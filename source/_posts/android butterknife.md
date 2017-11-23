# android butterknife #

## 引入类库（ android studio ） ##

1 最外层 bulid.gradle 

添加 classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'

	buildscript {
	    repositories {
	        jcenter()
	    }
	    dependencies {
	        classpath 'com.android.tools.build:gradle:2.2.3'
	
	        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
	
	        // NOTE: Do not place your application dependencies here; they belong
	        // in the individual module build.gradle files
	    }
	}


2 app 项目 build.gradle

添加 
apply plugin: 'android-apt'

compile 'com.jakewharton:butterknife:8.4.0'
apt 'com.jakewharton:butterknife-compiler:8.4.0'

	apply plugin: 'android-apt

	dependencies {
	    compile fileTree(include: ['*.jar'], dir: 'libs')
	    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
	        exclude group: 'com.android.support', module: 'support-annotations'
	    })
	    compile 'com.android.support:appcompat-v7:25.3.1'
	    testCompile 'junit:junit:4.12'
	    compile project(':easeui')
	
	    compile 'com.jakewharton:butterknife:8.4.0'
	    apt 'com.jakewharton:butterknife-compiler:8.4.0'
	}

## 插件 ##

butterKnife Zelezny
