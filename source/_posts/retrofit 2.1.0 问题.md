# retrofit 2.1.0 问题

## 即使response存在问题onResponse依然被调用

在Retrofit 1.9中，如果获取的 response 不能背解析成定义好的对象，则会调用failure。但是在Retrofit 2.0中，不管 response 是否能被解析。onResponse总是会被调用。但是在结果不能背解析的情况下，response.body()会返回null。别忘了处理这种情况。

如果response存在什么问题，比如404什么的，onResponse也会被调用。你可以从response.errorBody().string()中获取错误信息的主体。

**问题**

retrofit 2.1.0 onResponse 只接受 200 ， 如 404 错误 还是在onFailure 中 以异常信息抛出， 不能做信息处理

## data 数据格式问题

	{  
		data:"你好",  
		errorCode:200,  
		moreInfo:"错误详情"  
	}

如果 data 不是对象，而是 String 或 int 怎么解析