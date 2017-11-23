# Android中实现滑动的七种方式

## android 坐标体系

### android 坐标系

Android坐标系的坐标原点位于屏幕的左上角

### 视图坐标系

视图坐标系的原点位于父视图的左上角

## 实现滑动方式 ##

### layout 方法 ###

在View进行绘制时，是调用onLayout()方法来确定View的位置的，同样我们也可以调用layout()方法来传入我们滑动后的坐标便可以实现View的滑动，当然坐标的获取我们可以在触控事件中进行获取，下面我们做一个View随手指进行滑动的小例子来进行说明。

	public class DragView extends View {
	    private int mLastX;
	    private int mLastY;
	    public DragView(Context context) {
	        this(context, null);
	    }
	
	    public DragView(Context context, AttributeSet attrs) {
	        this(context, attrs, 0);
	    }
	
	    public DragView(Context context, AttributeSet attrs, int defStyleAttr) {
	        super(context, attrs, defStyleAttr);
	    }
	
	    @Override
	    public boolean onTouchEvent(MotionEvent event) {
	        int x = (int) event.getX();
	        int y = (int) event.getY();
	        int lastX = 0, lastY = 0;
	        switch (event.getAction()){
	            case MotionEvent.ACTION_DOWN:
	                mLastX = x;
	                mLastY = y;
	                break;
	            case MotionEvent.ACTION_MOVE:
	                int offsetX = x - mLastX;
	                int offsetY = y - mLastY;
	                layout(getLeft() + offsetX, getTop() + offsetY,
	                        getRight() + offsetX, getBottom() + offsetY);
	                break;
	        }
	        return true;
	    }
	}

上面我们在触控事件中获取到获取到手指按下时的坐标(lastX, lastY)，然后在手指移动时不断计算X和Y方向上的偏移量，然后再调用layout()方法来改变View的位置从而实现滑动。当然上面我们是通过getX()和getY()来获取视图坐标来进行修改，我们也可以通过getRawX()和getRawY()来获取绝对坐标来实现上面的效果。代码如下:

	private int mLastX;
	private int mLastY;
	@Override
	public boolean onTouchEvent(MotionEvent event) {
	    int x = (int) event.getRawX();
	    int y = (int) event.getRawY();
	    switch (event.getAction()){
	        case MotionEvent.ACTION_DOWN:
	            mLastX = x;
	            mLastY = y;
	            break;
	        case MotionEvent.ACTION_MOVE:
	            int offsetX = x - mLastX;
	            int offsetY = y - mLastY;
	            layout(getLeft() + offsetX, getTop() + offsetY,
	                    getRight() + offsetX, getBottom() + offsetY);
	            //重新设置初始坐标
	            mLastX = x;
	            mLastY = y;
	            break;
	    }
	    return true;
	}

上面一定要注意，我们在改变完View的位置后必须调用设置初始坐标，这样才能准确获取偏移量。

### offsetLeftAndRight和offsetTopAndBottom ###

这一种方法和上一种方法大部分步骤都是相同的，只是在移动View上有所差别，代码如下（只替换 layout ）:

offsetLeftAndRight(offsetX);

offsetTopAndBottom(offsetY);


	public class DragView extends View {
	    private int mLastX;
	    private int mLastY;
	    public DragView(Context context) {
	        this(context, null);
	    }
	
	    public DragView(Context context, AttributeSet attrs) {
	        this(context, attrs, 0);
	    }
	
	    public DragView(Context context, AttributeSet attrs, int defStyleAttr) {
	        super(context, attrs, defStyleAttr);
	    }
	
	    @Override
	    public boolean onTouchEvent(MotionEvent event) {
	        int x = (int) event.getX();
	        int y = (int) event.getY();
	        int lastX = 0, lastY = 0;
	        switch (event.getAction()){
	            case MotionEvent.ACTION_DOWN:
	                mLastX = x;
	                mLastY = y;
	                break;
	            case MotionEvent.ACTION_MOVE:
	                int offsetX = x - mLastX;
	                int offsetY = y - mLastY;
	                offsetLeftAndRight(offsetX);
					offsetTopAndBottom(offsetY);
	                break;
	        }
	        return true;
	    }
	}


	

### 设置LayoutParams ###

LayoutParams可以通过改变的布局参数，我们可以通过下面的代码实现上面同样的效果。

	LinearLayout.LayoutParams layoutParams = (LinearLayout.LayoutParams) getLayoutParams();
	layoutParams.leftMargin = getLeft() + offsetX;
	layoutParams.topMargin = getTop() + offsetY;
	setLayoutParams(layoutParams);

注意:我们的LayoutParams可以通过getLayoutParams()方法来获取，但是要注意，如果View的父布局是LinearLayout，那么我们的LayoutParams就是LinearLayout.LayoutParams，如果View的父布局是RelativeLayout,则我们的LayoutParams就是RelativeLayout.LayoutParams。当然我们还有一种简单的方法，不用再管父布局的布局方式。代码如下:

	ViewGroup.MarginLayoutParams marginLayoutParams = (ViewGroup.MarginLayoutParams) getLayoutParams();
	marginLayoutParams.leftMargin = getLeft() + offsetX;
	marginLayoutParams.topMargin = getTop() + offsetY;
	setLayoutParams(marginLayoutParams);

### scrollTo和scrollBy方法 ###

**关于这两个方法我们需要仔细说一下其中的一些注意事项**

1 . scrollTo的参数是具体的一个坐标点(x, y), 而scrollBy的参数是在x, y方向上的坐标偏移

2 . scrollTo和scrollBy移动的是View的内容。这一点很重要!!!!

如果我们对ViewGroup使用scrollTo和scrollBy则移动的是内部的所有子View， 如果对TextView使用scrollTo和scrollBy则移动的是其中额文本。

3 . 视图移动还有一个不太好理解的地方在于坐标，我们下面结合图片来说明一下：

我们可以这样理解，我们的手机屏幕作为一个盖板，在手机屏幕下面是一个巨大的画布，我们的手机屏幕这个盖板是透明的，导致只有和手机屏幕重合的画布部分才会被我们看到，我们调用scrollTo和scrollBy也可以理解为是在移动手机上面的盖板。如图中所示，按钮在ViewGroup中的坐标是(20, 10),当我们调用scrollBy(20, 10)之后，就相当于移动了屏幕上的盖板，然后我们看到的按钮就到了ViewGroup的左上角。这样如果我们想让按钮在水平和竖直方向上各移动20和10个单位，我们就必须调用scrollBy(-20, -10)

经过了上面的知识准备，我们这里也使用scrollBy来实现前面实现的那个View随手指移动的小例子:

	((View)getParent()).scrollBy(-offsetX, -offsetY);

###  使用Scroller ###

> Scroller也是滑动中很重要的一个角色,进过前面的scrollTo和scrollBy大家也会发现，它们的移动时瞬间完成的，滑动显得十分突兀，Google为了改善用户体验，便给出了Scroller，它可以实现平滑的移动，从而使滑动过程更加真实，用户体验更好，下面我们先简单说说Scroller的实现原理。

Scroller的实现方式类似于scrollTo和scrollBy，scrollTo和scrollBy的移动都是从一个坐标点瞬间移动到另一个左边点，而Scroller则是将移动的这段距离切分成好几段的微小的位移，然后每一段调用scrollTo来不断移动这些微小的位移，由于人眼的视觉暂留效果，就会给人平滑移动的视觉效果。

> 下面我们在上一步的基础上增加一个小功能，第一部分还是View随手指移动，但是当我们松开手指时，让View自己平滑移动到最初始的位置(屏幕左上角),下面我们就来一步步介绍Scroller的用法

1 . 声明Scroller变量,并在构造方法中进行初始化

2 . 在触控事件的ACTION_UP(手指抬起)事件中传入开始滑动的坐标和需要滑动的距离并触发Scroller的滑动事件

3 . 重写computeScroll(),实现真正的滑动

下面是完整的代码示例:

	public class DragView extends View {
	    private int mLastX;
	    private int mLastY;
	    //声明Scroller变量
	    private Scroller mScroller;
	    public DragView(Context context) {
	        this(context, null);
	    }
	
	    public DragView(Context context, AttributeSet attrs) {
	        this(context, attrs, 0);
	    }
	
	    public DragView(Context context, AttributeSet attrs, int defStyleAttr) {
	        super(context, attrs, defStyleAttr);
	        //在构造方法中初始化Scroller变量
	        mScroller = new Scroller(context);
	    }
	
	    @Override
	    public boolean onTouchEvent(MotionEvent event) {
	        int x = (int) event.getRawX();
	        int y = (int) event.getRawY();
	        switch (event.getAction()){
	            case MotionEvent.ACTION_DOWN:
	                mLastX = x;
	                mLastY = y;
	                break;
	            case MotionEvent.ACTION_MOVE:
	                int offsetX = x - mLastX;
	                int offsetY = y - mLastY;
	                //实现View跟随手指移动的效果
	                ((View)getParent()).scrollBy(-offsetX, -offsetY);
	                //重新设置初始坐标
	                mLastX = x;
	                mLastY = y;
	                break;
	            case MotionEvent.ACTION_UP:
	                //当手指抬起时执行滑动过程
	                View view = (View) getParent();
	                mScroller.startScroll(view.getScrollX(), view.getScrollY(),
	                        view.getScrollX(), view.getScrollY(), 5000);
	                //调用重绘来间接调用computeScroll()方法
	                invalidate();
	                break;
	        }
	        return true;
	    }
	
	    @Override
	    public void computeScroll() {
	        super.computeScroll();
	        //判断滑动过程是否完成
	        if (mScroller.computeScrollOffset()){
	            ((View)getParent()).scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
	            //通过重绘来不断调用computeScroll()方法
	            invalidate();
	        }
	    }
	}

> 上面的代码View随手指移动的代码部分是与前面相同的，我们只说说Scroller的部分以及一些注意事项

1 .  startScroll()方法各参数的意义,我们可以看看下面的源码:

	/**
	 * Start scrolling by providing a starting point, the distance to travel,
	 * and the duration of the scroll.
	 *
	 * @param startX Starting horizontal scroll offset in pixels. Positive
	 *        numbers will scroll the content to the left.
	 * @param startY Starting vertical scroll offset in pixels. Positive numbers
	 *        will scroll the content up.
	 * @param dx Horizontal distance to travel. Positive numbers will scroll the
	 *        content to the left.
	 * @param dy Vertical distance to travel. Positive numbers will scroll the
	 *        content up.
	 * @param duration Duration of the scroll in milliseconds.
	 */
	public void startScroll(int startX, int startY, int dx, int dy, int duration)

可以看出startX和startY参数就是开始滚动的(x, y)坐标，那么我们就可以通过ViewGroup(子View的父视图)的getScrollX()和getScrollY()来获取,这里一定要注意，我们在滑动时的content就是子View，所以我们通过子View的父视图(ViewGroup)的getScrollX()和getScrollY()获取到的就是子View在X和Y方向上滑动的距离，即就是我们需要的当我们手指抬起时子View的(x, y)坐标。而如果我们对子View调用getScrollX()和getScrollY()方法，则获得的是子View内部的视图的滑动距离及坐标。

dx和dy分别是在X和Y方向上的偏移量，而且注释中说了，如果我们传入的dx和dy的值是正值，那么将会向上向左移动这个content(其实就是我们这里的View)，即我们就可以让子View回到左上角，这里我们还是可以借助于上一小节中提到的视图移动的概念，我们想让子View向坐上方移动，其实就是想让覆盖在上面的盖板向右下角移动，我们可以将dx和dy理解为父视图(覆盖在上面的盖板)的偏移量。

假设我们刚开始是让子View随手指向右下方移动，那么相当于覆盖在上面的盖板是向左上方移动，所以我们通过getScrollX()和getScrollY()获得的值是负值，我们现在松开手指想让子View向左上方移动(即回到屏幕左上角)，那么就相当于盖板向右下角移动，所以我们的dx和dy的值必须是-getScrollX()和-getScrollY()，此时的两个值都是正值。

2 . 由于我们的computeScroll()方法不会主动调用，但是我们又需要它不断调用从而不断进行微小移动从而实现平滑的滑动，所以我们可以通过下面的方法。

> 这三个按照以下顺序进行调用 invalidate()--->onDraw()--->computeScroll()，所以我们可以可以在ACTION_UP中调用完startScroll()方法后调用invalidate()方法,然后在computeScroll()方法中判断滑动是否结束,如果没结束，则通过getCurrX()和getCurrY()来获得当前需要移动的微小的位移的坐标点，然后传入scrollTo()方法中，这时候子View还只是移动了一小段距离，然后我们再次调用invalidate()方法，然后接着调用onDraw()方法，然后再次进入computeScroll()中再次让子View移动一小段距离，直到滑动结束,computeScrollOffset()返回false，则这个循环调用的过程结束，从而完成平滑移动的过程。

### 属性动画 ###

属性动画一样可以实现View的滑动，但是由于属性动画涉及到的知识点也是众多，这里不再展开来写，只是提供一个思路，后续后专门写一篇博客来说。

### ViewDragHelper ###

ViewDragHelper可以帮助我们实现各种滑动需求，但是它的使用也相对较复杂，所以准备专门写一篇博客来介绍他，这里只是给出一个概念

## 参考信息
[Android中实现滑动的七种方式](http://www.jianshu.com/p/4eaeee19c5e4)

