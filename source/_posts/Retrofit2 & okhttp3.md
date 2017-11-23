# Retrofit 2.X && okhttp 3.X

## retrofit 2.x

## 基本用法

这里我们就主要说一说最常用的@GET   @POST的用法

假设URL为：http://ip.taobao.com/service/getIpInfo.php?ip=202.202.32.202

1. 定义接口

		public interface MyService1 {
		    //？参数为？前面的到 /（斜杠）之间的值
		    @GET("getIpInfo.php")
		    //Get中的注解用@Query
		    Call<IP> getIP(@Query("ip") String ip);
		    /*********************************************************************************/
		    @FormUrlEncoded
		    @POST("getIpInfo.php")
		    //Post中的注解一定要用@Field不能用@Query
		    Call<IP> postIP(@Field("ip") String ip);
		}

	@GET对应的是@Query，@POST对应的是@Field

* 实例化retrofit

        //实例化retrofit，配置好请求地址和解析方式
		Retrofit retrofit = new Retrofit.Builder()
        //★Retrofit2 的baseUlr 必须以 /（斜线） 结束，不然会抛出一个IllegalArgumentException
        .baseUrl("http://ip.taobao.com/service/")
        .addConverterFactory(GsonConverterFactory.create())//设置将json解析为javabean所用的方式
		.client(new OkHttpClient())
        .build();
        //通过retrofit创建第一步定义的接口的实例，
        //供在外部直接通过该实例调用该接口的getIP方法，完成网络请求
        MyService1 mservice =  retrofit.create(MyService1.class);

        //调用getIP（或者postIP）方法得到Call
		//          final Call<IP> call = taobaoIPService.getIP("202.202.32.202");
         final Call<IP> call = mservice.postIP("202.202.32.202");

3. 异步请求

		//          call.enqueue开启异步网络请求
        call.enqueue(new Callback<IP>() {
	        @Override
	        //请求成功
	        public void onResponse(Response<IP> response, Retrofit retrofit) {
	            IP body = response.body();
	            String result = body.getData().getRegion();
	            Log.e("TAG",result);
		//                3.可以直接更改UI，因为onResponse方法已经在UI线程中
		                tv.setText(result);
		//                4.取消请求
		                call.cancel();
		            }
		            @Override
		            //请求失败
		            public void onFailure(Throwable throwable) {
		                Log.e("TAG",throwable.toString());
		                tv.setText(throwable.toString());
		                throwable.printStackTrace();
		            }
		        });




## 参考信息
[好用的网络请求库Retrofit2（入门及讲解）](http://www.open-open.com/lib/view/open1461505284381.html)