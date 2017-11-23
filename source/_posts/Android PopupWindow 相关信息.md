# Android PopupWindow

## 一般用法

	View view = View.inflate(context, R.layout.layout_view, null);
	PopupWindow popupWindow = new PopupWindow(view, width, height, true);
	popupWindow.setBackgroundDrawable(new ColorDrawable(0x00000000));
	popupWindow.setOutsideTouchable(true);
	popupWindow.setAnimationStyle(R.style.pop_animation_style);
	popupWindow.showAtLocation(rootView, Gravity.CENTER, 0, 0);

## 细节分析

### 构造函数

	public PopupWindow(View contentView, int width, int height, boolean focusable)

contentView为要显示的view，width和height为宽和高，值为像素值，也可以是MATCHT_PARENT和WRAP_CONTENT。
focusable为是否可以获得焦点，这是一个很重要的参数，也可以通过

	public void setFocusable(boolean focusable)

来设置，如果focusable为false，在一个Activity弹出一个PopupWindow，按返回键，由于PopupWindow没有焦点，会直接退出Activity。如果focusable为true，PopupWindow弹出后，所有的触屏和物理按键都有PopupWindows处理。

如果PopupWindow中有Editor的话，focusable要为true。

### 动画

	<style name="pop_animation_style">
	    <item name="android:windowEnterAnimation">@anim/pop_center_in</item>
	    <item name="android:windowExitAnimation">@anim/pop_center_out</item>
	</style>

	popupWindow.setAnimationStyle(R.style.pop_animation_style);

### PopupWindow 显示问题

需要在新建时设置 PopupWindow 的宽高，否则不显示；如果还不显示， 可尝试在 xml 根布局中添加背景
(mPopupWindow.update(width, height);)

### 设置宽高

有两种方法设置PopupWindow的大小：

调用有宽高参数的构造函数：

	LayoutInflater inflater = (LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
	View contentview = inflater.inflate(R.layout.popup_process, null);
	PopupWindow popupWindow = new PopupWindow(contentview,LayoutParams.WRAP_CONTENT,
	LayoutParams.WRAP_CONTENT);

通过setWidth和setHeight设置

	PopupWindow popupWindow = new PopupWindow(contentview);
	popupWindow.setWidth(LayoutParams.WRAP_CONTENT);           
	popupWindow.setHeight(LayoutParams.WRAP_CONTENT);

两种办法是等效的，不管采用何种办法，必须设置宽和高，否则不显示任何东西.

这里的WRAP_CONTENT可以换成fill_parent 也可以是具体的数值，它是指PopupWindow的大小，也就是contentview的大小，注意popupwindow根据这个大小显示你的View，如果你的View本身是从xml得到的，那么xml的第一层view的大小属性将被忽略。**相当于popupWindow的width和height属性直接和第一层View相对应。**

### 点击外部区域可关闭 PopupWindow

需要手动设置 PopupWindow 背景 popupWindow.setBackgroundDrawable(new ColorDrawable(0x00000000));
和 popupWindow.setOutsideTouchable(true);

### PopupWindow showAtLocation和showAsDropDown参数分析

showAtLocation 中传入的 view 对弹出框基本没有影响，屏幕原点是屏幕的左上角 


PopupWindow 的这两个方法都是控制PopupWindow 出现的，具体分析如下：

1. showAtLocation，例如：showAtLocation(findViewById(R.id.search_ib), Gravity.TOP | Gravity.RIGHT,10, 10);
第一个参数：这个view是要能获取到window唯一标示的（也就是只要能获取到window 标示，view是什么控件都可以），应该是标示这个pw添加到哪个window里面，对控制pw出现位置没有影响；
第二个参数：请记住屏幕原点是屏幕的左上角。Gravity.TOP | Gravity.RIGHT指的就是屏幕的右上角，那么pw的中心点坐标是（屏幕宽,0）。pw默认是在屏幕的中间，也就是Gravity.LEFT表示pw的中心点坐标是(0,1/2屏幕高)；
第三、四个参数：偏移量的方向与第二个参数有关。Gravity.TOP | Gravity.RIGHT，以屏幕右上角为原点，pw往X轴负方向偏移10个像素，往Y轴正方向偏移10个像素；如果是Gravity.BOTTOM| Gravity.LEFT，以屏幕左下角为原点，pw往X轴正方向偏移10个像素，往Y轴正方向偏移10个像素。
注意：这个偏移量可以是正的，也可以是负的。无论偏移多大，pw是不会跑出屏幕。具体往轴的那个方向偏移，跟第二个参数有关，对于Gravity.CENTER的情况，偏移量负表示往轴的负方向，正往轴的正方向

2. showAsDropDown，例如：showAsDropDown(MainActivity.this.findViewById(R.id.logo_iv),100,0)，
以R.id.logo_iv的左下角为原点，向X轴正方向偏移100个像素，Y轴方向偏移0个像素。
注意：这个偏移量可以是正的，也可以是负的。无论偏移多大，pw是不会跑出屏幕。

## PopupWindow怎么合理控制弹出位置（showAtLocation）

	// 一个自定义的布局，作为显示的内容
	Context context = null;　　// 真实环境中要赋值
	int layoutId = 0;　　　　　　// 布局ID
	View contentView = LayoutInflater.from(context).inflate(layoutId, null);
	      
	final PopupWindow popupWindow = new PopupWindow(contentView,
	                LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT, true);
	
	popupWindow.setTouchable(true);
	// 如果不设置PopupWindow的背景，有些版本就会出现一个问题：无论是点击外部区域还是Back键都无法dismiss弹框
	// 这里单独写一篇文章来分析
	popupWindow.setBackgroundDrawable(new ColorDrawable());
	// 设置好参数之后再show
	popupWindow.showAsDropDown(contentView);

如果创建PopupWindow的时候没有指定高宽，那么showAsDropDown默认只会向下弹出显示，这种情况有个最明显的缺点就是：弹窗口可能被屏幕截断，显示不全，所以需要使用到另外一个方法showAtLocation,这个的坐标是相对于整个屏幕的，所以需要我们自己计算位置。

如下图所示，我们可以根据屏幕左上角的坐标A，屏幕高宽，点击View的左上角的坐标C，点击View的大小以及PopupWindow布局的大小计算出PopupWindow的显示位置B

计算方法源码如下：

	/**
	     * 计算出来的位置，y方向就在anchorView的上面和下面对齐显示，x方向就是与屏幕右边对齐显示
	     * 如果anchorView的位置有变化，就可以适当自己额外加入偏移来修正
	     * @param anchorView  呼出window的view
	     * @param contentView   window的内容布局
	     * @return window显示的左上角的xOff,yOff坐标
     */
    private static int[] calculatePopWindowPos(final View anchorView, final View contentView) {
        final int windowPos[] = new int[2];
        final int anchorLoc[] = new int[2];
		// 获取锚点View在屏幕上的左上角坐标位置
        anchorView.getLocationOnScreen(anchorLoc);
        final int anchorHeight = anchorView.getHeight();
        // 获取屏幕的高宽
        final int screenHeight = ScreenUtils.getScreenHeight(anchorView.getContext());
        final int screenWidth = ScreenUtils.getScreenWidth(anchorView.getContext());
        contentView.measure(View.MeasureSpec.UNSPECIFIED, View.MeasureSpec.UNSPECIFIED);
        // 计算contentView的高宽
        final int windowHeight = contentView.getMeasuredHeight();
        final int windowWidth = contentView.getMeasuredWidth();
        // 判断需要向上弹出还是向下弹出显示
        final boolean isNeedShowUp = (screenHeight - anchorLoc[1] - anchorHeight < windowHeight);
        if (isNeedShowUp) {
            windowPos[0] = screenWidth - windowWidth;
            windowPos[1] = anchorLoc[1] - windowHeight;
        } else {
            windowPos[0] = screenWidth - windowWidth;
            windowPos[1] = anchorLoc[1] + anchorHeight;
        }
        return windowPos;
    }

接下来调用showAtLoaction显示：

	View windowContentViewRoot = 我们要设置给PopupWindow进行显示的View
	int windowPos[] = calculatePopWindowPos(view, windowContentViewRoot);
	int xOff = 20；// 可以自己调整偏移
	windowPos[0] -= xOff;
	popupwindow.showAtLocation(view, Gravity.TOP | Gravity.START, windowPos[0], windowPos[1]);
	// windowContentViewRoot是根布局View

上面的例子只是提供了一种计算方式，在实际开发中可以根据需求自己计算，比如anchorView在左边的情况，在中间的情况，可以根据实际需求写一个弹出位置能够自适应的PopupWindow。

补充上获取屏幕高宽的代码ScreenUtils.java：

	/**
		* 获取屏幕高度(px)
     */
    public static int getScreenHeight(Context context) {
        return context.getResources().getDisplayMetrics().heightPixels;
    }
    /**
		* 获取屏幕宽度(px)
     */
    public static int getScreenWidth(Context context) {
        return context.getResources().getDisplayMetrics().widthPixels;
    }


## onCreate方法中调用PopupWindow的错误：android.view.WindowManager$BadTo ##

如题：在activity的oncreate方法中使用popupwindow出现以下错误：
Android.view.WindowManager$BadTokenException: Unable to add window --
token null is not valid; is your activity running?
错误代码如下 ：

	pop= newPopupWindow(pop_view,320,250);
	pop.showAtLocation(parent, Gravity.TOP,0, 0);

解决方法：
应把pop.showAtLocation(parent, Gravity.TOP,0, 0)这一句移出oncreate方法，在控件渲染完毕后再使用。
但是移出oncreate方法的话该移到哪里去呢？网友的方法大概是这几种：
1、移到事件中（比如一个button的click事件中）;

2、移到子线程中;另起一线程,在线程中不断循环，直到判断控件是否渲染完毕(如长宽大于0),不推荐。。。

3、移到重写的控件(parent)中,在控件ondraw()完后生成pop。

ps：1、2绝对没问题，3没测试过。 

后来在网上找到一个绝佳的方法：

	@Override

	public void onWindowFocusChanged(boolean hasFocus) {
	
		 //
		 TODO Auto-generated method stub
		
		 super.onWindowFocusChanged(hasFocus);
		
		 if(hasFocus){
		
		  showPopupWindow(getApplicationContext());
		
		 }
	
	}

其中showPopupWindow(getApplicationContext())是我自己定义的专门显示popupwindow的一个函数。
当activity获得焦点之后，activity是加载完毕的了，这个方法的技巧性比较强，很难想到。

## 参考信息
[PopupWindow showAtLocation和showAsDropDown参数分析](http://blog.csdn.net/ben0612/article/details/43191205)

[Android PopupWindow怎么合理控制弹出位置(showAtLocation)](http://www.cnblogs.com/popfisher/p/5608436.html)

[Android PopupWindow详解](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/0702/1627.html)

[onCreate方法中调用PopupWindow的错误：android.view.WindowManager$BadTo](http://blog.csdn.net/djy1135/article/details/13264211)