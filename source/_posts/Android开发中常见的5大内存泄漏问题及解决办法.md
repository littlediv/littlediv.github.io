# Android开发中常见的5大内存泄漏问题及解决办法 #

在android开发中，内存泄漏是比较常见的问题，有过一些android编程经历的童鞋应该都遇到过，但为什么会出现内存泄漏呢？内存泄漏又有什么影响呢？


在android程序开发中，当一个对象已经不需要再使用了，本该被回收时，而另外一个正在使用的对象持有它的引用从而导致它不能被回收，这就导致本该被回收的对象不能被回收而停留在堆内存中，内存泄漏就产生了。

内存泄漏有什么影响呢？它是造成应用程序OOM的主要原因之一。由于android系统为每个应用程序分配的内存有限，当一个应用中产生的内存泄漏比较多时，就难免会导致应用所需要的内存超过这个系统分配的内存限额，这就造成了内存溢出而导致应用Crash。

了解了内存泄漏的原因及影响后，我们需要做的就是掌握常见的内存泄漏，并在以后的android程序开发中，尽量避免它。下面小编搜罗了5个android开发中比较常见的内存泄漏问题及解决办法，分享给大家，一起来看看吧。
 
## 一、单例造成的内存泄漏

Android的单例模式非常受开发者的喜爱，不过使用的不恰当的话也会造成内存泄漏。因为单例的静态特性使得单例的生命周期和应用的生命周期一样长，这就说明了如果一个对象已经不需要使用了，而单例对象还持有该对象的引用，那么这个对象将不能被正常回收，这就导致了内存泄漏。

如下这个典例：

	public class AppManager {
	    private static AppManager instance;
	    private Context context;
	    private AppManager(Context context) {
	        this.context = context;
	    }
	    public static AppManager getInstance(Context context) {
	        if (instance != null) {
	            instance = new AppManager(context);
	        }
	        return instance;
	    }
	}
 
这是一个普通的单例模式，当创建这个单例的时候，由于需要传入一个Context，所以这个Context的生命周期的长短至关重要：

1、传入的是Application的Context：这将没有任何问题，因为单例的生命周期和Application的一样长 ；

2、传入的是Activity的Context：当这个Context所对应的Activity退出时，由于该Context和Activity的生命周期一样长（Activity间接继承于Context），所以当前Activity退出时它的内存并不会被回收，因为单例对象持有该Activity的引用。
 
所以正确的单例应该修改为下面这种方式：

	public class AppManager {
	    private static AppManager instance;
	    private Context context;
	    private AppManager(Context context) {
	        this.context = context.getApplicationContext();
	    }
	    public static AppManager getInstance(Context context) {
	        if (instance != null) {
	            instance = new AppManager(context);
	        }
	        return instance;
	    }
	}
 
这样不管传入什么Context最终将使用Application的Context，而单例的生命周期和应用的一样长，这样就防止了内存泄漏。
 
## 二、非静态内部类创建静态实例造成的内存泄漏
 
有的时候我们可能会在启动频繁的Activity中，为了避免重复创建相同的数据资源，会出现这种写法：
 
	public class MainActivity extends AppCompatActivity {
	    private static TestResource mResource = null;
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        if(mManager == null){
	            mManager = new TestResource();
	        }
	        //...
	    }
	    class TestResource {
	        //...
	    }
	}
 
这样就在Activity内部创建了一个非静态内部类的单例，每次启动Activity时都会使用该单例的数据，这样虽然避免了资源的重复创建，不过这种写法却会造成内存泄漏，因为非静态内部类默认会持有外部类的引用，而又使用了该非静态内部类创建了一个静态的实例，该实例的生命周期和应用的一样长，这就导致了该静态实例一直会持有该Activity的引用，导致Activity的内存资源不能正常回收。

正确的做法为：
将该内部类设为静态内部类或将该内部类抽取出来封装成一个单例，如果需要使用Context，请使用ApplicationContext 。
 
## 三、Handler造成的内存泄漏
 
Handler的使用造成的内存泄漏问题应该说最为常见了，平时在处理网络任务或者封装一些请求回调等api都应该会借助Handler来处理，对于Handler的使用代码编写一不规范即有可能造成内存泄漏，如下示例：
 
	public class MainActivity extends AppCompatActivity {
	    private Handler mHandler = new Handler() {
	        @Override
	        public void handleMessage(Message msg) {
	            //...
	        }
	    };
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        loadData();
	    }
	    private void loadData(){
	        //...request
	        Message message = Message.obtain();
	        mHandler.sendMessage(message);
	    }
	}
 
这种创建Handler的方式会造成内存泄漏，由于mHandler是Handler的非静态匿名内部类的实例，所以它持有外部类Activity的引用，我们知道消息队列是在一个Looper线程中不断轮询处理消息，那么当这个Activity退出时消息队列中还有未处理的消息或者正在处理消息，而消息队列中的Message持有mHandler实例的引用，mHandler又持有Activity的引用，所以导致该Activity的内存资源无法及时回收，引发内存泄漏，所以另外一种做法为：
 
	public class MainActivity extends AppCompatActivity {
	    private MyHandler mHandler = new MyHandler(this);
	    private TextView mTextView ;
	    private static class MyHandler extends Handler {
	        private WeakReference<Context> reference;
	        public MyHandler(Context context) {
	            reference = new WeakReference<>(context);
	        }
	        @Override
	        public void handleMessage(Message msg) {
	            MainActivity activity = (MainActivity) reference.get();
	            if(activity != null){
	                activity.mTextView.setText("");
	            }
	        }
	    }
	 
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        mTextView = (TextView)findViewById(R.id.textview);
	        loadData();
	    }
	 
	    private void loadData() {
	        //...request
	        Message message = Message.obtain();
	        mHandler.sendMessage(message);
	    }
	}
 
创建一个静态Handler内部类，然后对Handler持有的对象使用弱引用，这样在回收时也可以回收Handler持有的对象，这样虽然避免了Activity泄漏，不过Looper线程的消息队列中还是可能会有待处理的消息，所以我们在Activity的Destroy时或者Stop时应该移除消息队列中的消息，更准确的做法如下：
 
	public class MainActivity extends AppCompatActivity {
	    private MyHandler mHandler = new MyHandler(this);
	    private TextView mTextView ;
	    private static class MyHandler extends Handler {
	        private WeakReference<Context> reference;
	        public MyHandler(Context context) {
	            reference = new WeakReference<>(context);
	        }
	        @Override
	        public void handleMessage(Message msg) {
	            MainActivity activity = (MainActivity) reference.get();
	            if(activity != null){
	                activity.mTextView.setText("");
	            }
	        }
	    }
	 
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        mTextView = (TextView)findViewById(R.id.textview);
	        loadData();
	    }
	 
	    private void loadData() {
	        //...request
	        Message message = Message.obtain();
	        mHandler.sendMessage(message);
	    }
	 
	    @Override
	    protected void onDestroy() {
	        super.onDestroy();
	        mHandler.removeCallbacksAndMessages(null);
	    }
	}
 
使用mHandler.removeCallbacksAndMessages(null);是移除消息队列中所有消息和所有的Runnable。当然也可以使用mHandler.removeCallbacks();或mHandler.removeMessages();来移除指定的Runnable和Message。
 
## 四、线程造成的内存泄漏
 
对于线程造成的内存泄漏，也是平时比较常见的，如下这两个示例可能每个人都这样写过：
 
//——————test1

        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                SystemClock.sleep(10000);
                return null;
            }
        }.execute();

//——————test2

        new Thread(new Runnable() {
            @Override
            public void run() {
                SystemClock.sleep(10000);
            }
        }).start();
 

上面的异步任务和Runnable都是一个匿名内部类，因此它们对当前Activity都有一个隐式引用。如果Activity在销毁之前，任务还未完成， 那么将导致Activity的内存资源无法回收，造成内存泄漏。正确的做法还是使用静态内部类的方式，如下：
 
    static class MyAsyncTask extends AsyncTask<Void, Void, Void> {
        private WeakReference<Context> weakReference;
 
        public MyAsyncTask(Context context) {
            weakReference = new WeakReference<>(context);
        }
 
        @Override
        protected Void doInBackground(Void... params) {
            SystemClock.sleep(10000);
            return null;
        }
 
        @Override
        protected void onPostExecute(Void aVoid) {
            super.onPostExecute(aVoid);
            MainActivity activity = (MainActivity) weakReference.get();
            if (activity != null) {
                //...
            }
        }
    }
    static class MyRunnable implements Runnable{
        @Override
        public void run() {
            SystemClock.sleep(10000);
        }
    }

//——————

    new Thread(new MyRunnable()).start();
    new MyAsyncTask(this).execute();
 
这样就避免了Activity的内存资源泄漏，当然在Activity销毁时候也应该取消相应的任务AsyncTask::cancel()，避免任务在后台执行浪费资源。
 
## 五、资源未关闭造成的内存泄漏
 
对于使用了BraodcastReceiver，ContentObserver，File，Cursor，Stream，Bitmap等资源的使用，应该在Activity销毁时及时关闭或者注销，否则这些资源将不会被回收，造成内存泄漏。
 
以上就是android编程中，常见的5大内存泄漏问题及相应的解决办法，如果大家在编程中遇到了上述泄漏问题，不妨可以试试对应的方法。如果大家还有什么疑问，可以去“学习问答”版块直接提出。

## 参考信息

[Android开发中常见的5大内存泄漏问题及解决办法](http://www.maiziedu.com/article/9126/)