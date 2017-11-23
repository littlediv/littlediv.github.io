# Android Studio 文件存储

## 可能遇到的问题

android系统自身自带有存储，另外也可以通过sd卡来扩充存储空间。前者好比pc中的硬盘，后者好移动硬盘。 前者空间较小，后者空间大，但后者不一定可用。 开发应用，处理本地数据存取时，可能会遇到这些问题：

1. 需要判断sd卡是否可用: 占用过多机身内部存储，容易招致用户反感，优先将数据存放于sd卡;

* 应用数据存放路径，同其他应用应该保持一致，应用卸载时，清除数据:
	* 标新立异在sd卡根目录建一个目录，招致用户反感
	* 用户卸载应用后，残留目录或者数据在用户机器上，招致用户反感
* 需要判断两者的可用空间: sd卡存在时，可用空间反而小于机身内部存储，这时应该选用机身存储;

* 数据安全性，本应用数据不愿意被其他应用读写;
* 图片缓存等，不应该被扫描加入到用户相册等媒体库中去。

## 路径规律

一般地，通过 Context  和  Environment 相关的方法获取文件存取的路径。

通过这两个类可获取各种路径，如图：

	    ($rootDir)
	+- /data                -> Environment.getDataDirectory()
	|   |
	|   |   ($appDataDir)
	|   +- data/com.srain.cube.sample
	|       |
	|       |   ($filesDir)
	|       +- files            -> Context.getFilesDir() / Context.getFileStreamPath("")
	|       |       |
	|       |       +- file1    -> Context.getFileStreamPath("file1")
	|       |   ($cacheDir)
	|       +- cache            -> Context.getCacheDir()
	|       |
	|       +- app_$name        ->(Context.getDir(String name, int mode)
	|
	|   ($rootDir)
	+- /storage/sdcard0     -> Environment.getExternalStorageDirectory()
    |                       / Environment.getExternalStoragePublicDirectory("")
    |
    +- dir1             -> Environment.getExternalStoragePublicDirectory("dir1")
    |
    |   ($appDataDir)
    +- Andorid/data/com.srain.cube.sample
        |
        |   ($filesDir)
        +- files        -> Context.getExternalFilesDir("")
        |   |
        |   +- file1    -> Context.getExternalFilesDir("file1")
        |   +- Music    -> Context.getExternalFilesDir(Environment.Music);
        |   +- Picture  -> ... Environment.Picture
        |   +- ...
        |
        |   ($cacheDir)
        +- cache        -> Context.getExternalCacheDir()
        |
        +- ???

## 判断 SD 卡可用


	/**
	 * Check if the primary "external" storage device is available.
	 * 
	 * 
	 * @return
	 */
	public static boolean hasSDCardMounted() {

    String state = Environment.getExternalStorageState();
    if (state != null && state.equals(Environment.MEDIA_MOUNTED)) {
        return true;
    } else {
        return false;
    }
	}


## 参考信息

[Android文件存储使用参考 - liaohuqiu](http://www.tuicool.com/articles/AvUnqiy)




