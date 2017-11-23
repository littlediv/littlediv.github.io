# getColor过时

getColor方法在6.0中已经过时:

	@ColorInt
	@Deprecated
	public int getColor(@ColorRes int id) throws NotFoundException {
	    return getColor(id, null);
	}

可以参考以下方法:
使用

	ContextCompat.getColor(context, R.color.my_color)


This is the source code:

//源码

	public static final int getColor(Context context, int id) {
	    final int version = Build.VERSION.SDK_INT;
	    if (version >= 23) {
	        return ContextCompatApi23.getColor(context, id);
	    } else {
	        return context.getResources().getColor(id);
	    }
	}