# android fragment 填坑 #

## onAttach(Context context) 不执行 ##

 	@Override
    public void onAttach(Context context) {
        super.onAttach(context);

        mActivity = (Activity) context;
    }


	@Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
    }


app.fragment

| API | onAttach(Context context) | onAttach(Activity activity) |
| --- | -----:					  | :---:|
| 23以下   | 不执行 | 执行|
| 23及以上   | 执行 | 废弃|

在SDK 23以上的API中，Fragment的onAttach(Activity activity) is deprecated（过时了），取而代之的是onAttach(Context context），然而新的onAttach方法在API小于23的设备上运行，会出现不被调用的情况。

**解决方法**

第一种
---------------

 API小于19的设备上，用import android.support.v4.Fragment代替android.app.Fragment

第二种
-----------

	
	@Override
    public void onAttach(Context context) {
        if (context instanceof OnCoverChangeListener) {
            mListener = (ABC_Listener) context;
        } else {
            throw new RuntimeException(context.toString()
                    + " must implement ABC_Listener");
        }
        super.onAttach(context);
    }

    //SDK API<23时，onAttach(Context)不执行，需要使用onAttach(Activity)。Fragment自身的Bug，v4的没有此问题
    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.M) {
            if (activity instanceof OnCoverChangeListener) {
                mListener = (ABC_Listener) activity;
            } else {
                throw new RuntimeException(activity.toString()
                        + " must implement ABC_Listener");
            }
        }

    }


## Fragment页面加载混乱 视图重叠问题  ##

1 在 xml 中加载 fragment 不会出现重叠问题

xml

	<fragment
        android:id="@+id/fg_conversation"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:name="com.example.mac.consultationhall.fragment.ConversationFragment" />

fragmentActicity 

	ConversationFragment conversationFragment = (ConversationFragment) fm.findFragmentById(R.id.fg_conversation)



2 动态加载 会出现重叠

重现：
1.手机的 “设置” - “开发者选项” - 打开”不保留活动”(主要用于模拟Activity被及时回收) 
2.把 app 切换到后台，再重新打开，通过点按不同的 tab 来切换 Fragment 


方法二、（推荐使用）

重写onAttachFragment，重新让新的Fragment指向了原本未被销毁的fragment，它就是onAttach方法对应的Fragment对象

 	@Override
    public void onAttachFragment(Fragment fragment) {
        if (tab1 == null && fragment instanceof Tab1Fragment)
            tab1 = fragment;
        if (tab2 == null && fragment instanceof Tab2Fragment)
            tab2 = fragment;
        if (tab3 == null && fragment instanceof Tab3Fragment)
            tab3 = fragment;
        if (tab4 == null && fragment instanceof Tab4Fragment)
            tab4 = fragment;
    }


## 参考信息 ##

[关于Fragment重叠问题分析和解决](http://blog.csdn.net/whitley_gong/article/details/51987911)

[Fragment页面加载混乱 视图重叠问题 如何优雅的解决](http://blog.5ibc.net/p/12607.html)