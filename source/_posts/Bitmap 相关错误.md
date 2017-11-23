# Bitmap 相关错误

# android Immutable bitmap passed to Canvas constructor异常

出现Immutable bitmap passed to Canvas constructor错误的原因是如果不用copy的方法，直接引用会对资源文件进行修改，而Android是不允许在代码里修改res文件里的图片

解决办法如下：

使用


````BitmapFactory.decodeResource(getResources(), R.drawable.xiao).copy(Bitmap.Config.ARGB_8888, true);  ````

替换


````BitmapFactory.decodeResource(getResources(), R.drawable.xiao); ````