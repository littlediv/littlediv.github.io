#  RecyclerView中的item的match_parent属性失效问题解决方案 以及 LayoutInflater #

## 解决方案 ##

item使用RelativeLayout布局，并且布局中的view至少有一个layout_alignParentRight=true

在adapte中的onCreateViewHolder，使用


	public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {  
	       //View view = View.inflate(parent.getContext(), R.layout.item_fra_main2, null);  
	     View view = mInflater.from(mContext).inflate(R.layout.item_fra_main2, parent, false);  
	     ViewHolder holder = new ViewHolder(view);  
	     return holder;  
	 } 

分析

	View.inflate(parent.getContext(), R.layout.item_fra_main2, parent);  
	  对应的是：LayoutInflate.inflate(resource , parent, true);//在RecycleView下奔溃  


崩溃信息

>The specified child already has a parent. You must call removeView() on the child's parent first

修改为

	View.inflate(parent.getContext(), R.layout.item_fra_main2, null);  
	  对应的是：LayoutInflate.inflate(resource , null, false);//match_parent属性失效  

再次修改

	LayoutInflater.from(context).inflate(R.layout.item_fra_main2, parent, false);      
	对应的是：LayoutInflate.inflate(resource , parent, false);//就需要这种方案  


## LayoutInflater  ##

LayoutInflater 主要是用于加载布局的

用法一

	LayoutInflater layoutInflater = LayoutInflater.from(context);  

用法二

	LayoutInflater layoutInflater = (LayoutInflater) context  
        .getSystemService(Context.LAYOUT_INFLATER_SERVICE);  

其实第一种就是第二种的简单写法，只是Android给我们做了一下封装而已。得到了LayoutInflater的实例之后就可以调用它的inflate()方法来加载布局了，如下所示：

	layoutInflater.inflate(resourceId, root);  

inflate()方法一般接收两个参数，第一个参数就是要加载的布局id，第二个参数是指给该布局的外部再嵌套一层父布局，如果不需要就直接传null。这样就成功成功创建了一个布局的实例，之后再将它添加到指定的位置就可以显示出来了。


	inflate(int resource, ViewGroup root, boolean attachToRoot)  


1. 如果root为null，attachToRoot将失去作用，设置任何值都没有意义。
2. 如果root不为null，attachToRoot设为true，则会给加载的布局文件的指定一个父布局，即root。
3. 如果root不为null，attachToRoot设为false，则会将布局文件最外层的所有layout属性进行设置，当该view被添加到父view当中时，这些layout属性会自动生效。
4. 在不设置attachToRoot参数的情况下，如果root不为null，attachToRoot参数默认为true。

## 参考信息 ##

[ RecyclerView中的item的match_parent属性失效问题解决方案](http://blog.csdn.net/overseasandroid/article/details/51840819)

[Android LayoutInflater原理分析，带你一步步深入了解View(一)](http://blog.csdn.net/guolin_blog/article/details/12921889)

