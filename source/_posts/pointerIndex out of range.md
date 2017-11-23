# pointerIndex out of range

ViewPager 中添加 PhotoView 放大缩小图片，当缩小到很小了，会发生错误，网上回答是 ViewPager 的问题

我的 viewpager 解决方案: 
1 继承 view  
2 重写 dispatchTouchEvent
3 抓住异常


	public class FixedViewPager extends ViewPager {
	        public FixedViewPager(Context context) {
	               super(context);
	       }
	
	        public FixedViewPager(Context context, AttributeSet attrs) {
	               super(context, attrs);
	       }
	
	        @Override
	        public boolean dispatchTouchEvent(MotionEvent ev) {
	               try {
	                      return super .dispatchTouchEvent(ev);
	              } catch (IllegalArgumentException ignored) {
	              } catch (ArrayIndexOutOfBoundsException e) {
	              }
	
	               return false ;
	
	       }
	}

网上有其它方法 如:
重写onInterceptTouchEvent 和onTouchEvent方法
try catch 该两个方法，形如下面：

	try{  
	super.onInterceptTouchEvent(MotionEvent ev)  
	} catch(ILLegalArgumentException ex) {  
	}  
	return false;  
	try{  
	super.onTouchEvent(MotionEvent ev)  
	} catch(ILLegalArgumentException ex) {  
	}  
	return false;  

------------------

	@Override
	    public boolean onInterceptTouchEvent(MotionEvent ev) {
	        try {
	            return super.onInterceptTouchEvent(ev);
	        } catch (Exception e) {
	            // ignore it
	        }
	        return false;
	    }
	
	
	    @Override
	    public boolean onTouchEvent(MotionEvent ev) {
	        try {
	            return super.onTouchEvent(ev);
	        } catch (IllegalArgumentException ex) {
	            // ignore it
	        }
	        return false;
	    }