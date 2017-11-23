# Android studio Glide 缓存 时间 #

##  清除Glide缓存 ##

Glide自带清除缓存的功能,分别对应Glide.get(context).clearDiskCache();(清除磁盘缓存)与Glide.get(context).clearMemory();(清除内存缓存)两个方法.其中clearDiskCache()方法必须运行在子线程,clearMemory()方法必须运行在主线程,这是这两个方法所强制要求的,详见源码.

## 更新本地 url 路径 ##

对于有些图片框架，没有提供重新设置 url对应的缓存内容的api

这个问题的解决方案是这样的：
xxx.com/image/1.png 和xxx.com/image/1.png ?1469247425923
这2个url 获取到的图片是一样的

so，当你app里面更改了图片，而服务器里图片url是固定不变的， 你只需要在你 的url地址后面 加个 ?和一些字符串，如时间戳，那么用这个    新的url       替换你的旧的url，然后用 图片框架重新加载一遍。


	if(头像上传成功了){
        String newURL=BCUtil.reSetHeadImageURL(mySharedPreferences.getUserLoginHeadURL());
        mySharedPreferences.saveUserLoginHeadURL(newURL); 
        Glide.with(AccountDetailActivity.this).load( mySharedPreferences.getUserLoginHeadURL()).into(image_head) ;
    }

    public static String reSetHeadImageURL(String oldURL) {
        String newURL;
        int position = oldURL.indexOf("?");
        if (position > 0) {
            newURL = oldURL.substring(0, position);
            newURL = newURL + "?" + System.currentTimeMillis();
        } else {
            newURL = oldURL + "?" + System.currentTimeMillis();
        }
       return newURL;
    }


## signature ##

	Glide.with(yourFragment)
    .load(yourFileDataModel)
    .signature(new StringSignature(yourVersionMetadata))
    .into(yourImageView);

## 参考信息

[Glide获取缓存大小并清除缓存图片](http://www.jianshu.com/p/468bd4621f6e)

[glide等图片缓存框架替换缓存图片解决方案](http://blog.csdn.net/wangtoo520/article/details/53164521)

[Caching and Cache Invalidation](https://github.com/bumptech/glide/wiki/Caching-and-Cache-Invalidation#cache-invalidation)