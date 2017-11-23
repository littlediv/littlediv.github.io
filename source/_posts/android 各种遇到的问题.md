# android 各种遇到的问题

## android.support.v4.view.ViewPager cannot be cast to android.widget.TextView

fragment

	@Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
		// 1 出错
        View view = inflater.inflate(R.layout.fragment_page, container);//布局转换为视图控件
        TextView textView = (TextView) view;//向下转型
		//2 出错
		TextView textView = (TextView) inflater.inflate(R.layout.fragment_page, container)；

        textView.setText("Fragment #" + mPage);
        return view;
    }

fragment_page 布局文件

	<?xml version="1.0" encoding="utf-8"?>
	<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"/>

解决方法：

布局文件添加 id

	<?xml version="1.0" encoding="utf-8"?>
	<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/my_text"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"/>

fragment onCreateView 更改为

	View view = inflater.inflate(R.layout.fragment_page, container);
	TextView textView = (TextView) view.findViewById(R.id.my_text);
	textView.setText("Fragment #" + mPage);

