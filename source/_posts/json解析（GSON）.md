# json 解析（Gson）

##  Caused by: java.lang.IllegalStateException: Expected BEGIN_OBJECT but was STRING at line 1 column 36

这里遇到一个比较棘手的问题，原来项目中使用的不是Gson，客户端在请求json数据时如果没有具体的数据内容会返回空字符串，如：

{"result":{"errorMessage":"用户名/密码错误","errorCode":0},"data":""}

这里的data是表示没有具体的数据，但是在Gson解析时我们用来接受的数据却是具体的实体对象，bean定义如下：

	public class Result<T extends BaseEntity> implements Serializable {

	    private static final long serialVersionUID = -645821020648740998L;
	    private Status result;
	    private T data;
	
	
	    public Status getResult() {
	        return result;
	    }
	
	    public void setResult(Status result) {
	        this.result = result;
	    }
	
	    public T getData() {
	        return data;
	    }
	
	    public void setData(T data) {
	        this.data = data;
	    }
	}
所以这里在解析的时候就会报一个错误，大体内容是：解析到了一个String，但期望的是一个对象，start with ‘{’，也就是说我们的json应该是下面这种的：

{"result":{"errorMessage":"用户名/密码错误","errorCode":0},"data":null}

或者没有data，

{"result":{"errorMessage":"用户名/密码错误","errorCode":0}}



这里无法修改服务端的代码，所以只能在客户端对获取的数据进行转换，转换的方法是：

	JsonObject obj = new JsonParser().parse(json_str).getAsJsonObject();
	if (obj.get("data").toString().equals("\"\"")){
	    obj.remove("data");
	}
	Result<User> result;
	Gson _g = new GsonBuilder().serializeNulls().create();
	result = _g.fromJson(obj,new TypeToken<Result<User>>(){}.getType());
通过一个中间对象，JsonObject将data为空的json去除掉相应的data项。

或者直接将数据返回为空
	LiveInfoForGround liveInfoForHigh = null;
    if (response != null) {
        JsonObject obj = new JsonParser().parse(response).getAsJsonObject();
        if (obj != null && obj.get("data").toString().equals("\"\"")) {
            return null;
        }
        Gson gson = new Gson();
        liveInfoForHigh = gson.fromJson(response, LiveInfoForGround.class);
    }


详见参考JsonElement使用。

## 动态解析未知字段 key 方法

json数据中的字段key是动态可变的时候，由于Gson是使用静态注解的方式来设置实体对象的，因此我们很难直接对返回的类型来判断。但Gson在解析过程中如果不知道解析的字段，就会将所有变量存储在一个Map中，我们只要实例化这个map就能动态地取出key和value了。

先给出一段jsondata，这是天气预报的数据，其中day_20151002这种key是随日期而变化的，在实体类中就不能当做静态变量来处理，我们就通过map来取出其映射对象。

代码

	{ "resultcode":"200","reason":"successed!",  
    "result":{  
            "sk":{  
                 "temp":"24","wind_direction":"东北风","wind_strength":"2级","humidity":"28%","time":"17:38"  
                  },  
         "today":{  
                 "temperature":"15℃~26℃","weather":"多云转晴","wind":"东北风微风","week":"星期日","city":"桂林","date_y":"2015年10月11日","dressing_index":"舒适","dressing_advice":"建议着长袖T恤、衬衫加单裤等服装。年老体弱者宜着针织长袖衬衫、马甲和长裤。","uv_index":"弱","comfort_index":"","wash_index":"较适宜","travel_index":"较适宜","exercise_index":"较适宜","drying_index":""  
                 },  
        "future":{  
                 "day_20151011":{"temperature":"15℃~26℃","weather":"多云转晴","wind":"东北风微风","week":"星期日","date":"20151011"},  
                 "day_20151012":{"temperature":"16℃~27℃","weather":"晴转多云","wind":"微风","week":"星期一","date":"20151012"},  
                 "day_20151013":{"temperature":"16℃~26℃","weather":"多云转晴","wind":"微风","week":"星期二","date":"20151013"},  
                 "day_20151014":{"temperature":"17℃~27℃","weather":"晴","wind":"北风微风","week":"星期三","date":"20151014"},  
                 "day_20151015":{"temperature":"17℃~28℃","weather":"晴","wind":"北风微风","week":"星期四","date":"20151015"},  
                 "day_20151016":{"temperature":"17℃~30℃","weather":"晴","wind":"北风微风","week":"星期五","date":"20151016"},  
                 "day_20151017":{"temperature":"17℃~30℃","weather":"晴","wind":"北风微风","week":"星期六","date":"20151017"}  
                 }  
              },  
    "error_code":0  
	}  

javaBean 代码

	public class Response {
	    public String resultcode;
	    public String reason;
	    public ResultBean result;
	    public int error_code;
	  
	    public static class ResultBean {
	
		    public SkBean sk;
		    public TodayBean today;
	        public Map<String,FutureBean> future;

	        public static class SkBean {
	            public String temp;
	            public String wind_direction;
	            public String wind_strength;
	            public String humidity;
	            public String time;        
	        }
	
	        public static class TodayBean {
	            public String temperature;
	            public String weather;
	            public String wind;
	            public String week;
	            public String city;
	            public String date_y;
	            public String dressing_index;
	            public String dressing_advice;
	            public String uv_index;
	            public String comfort_index;
	            public String wash_index;
	            public String travel_index;
	            public String exercise_index;
	            public String drying_index;
	
	
	        }
	
	        public static class FutureBean {

	            public String temperature;
	            public String weather;
	            public String wind;
	            public String week;
	            public String date;
	
	
	        }
	    }
	}

注： android studio 中使用插件时（ 类 右键 --> generate --> GsonFormat ）时由于day_20151002这种key是随日期而变化的，而 gson 只作静态变量方式解析，会出现以下情况：

	public FutureBean future;

	public static class FutureBean {
            public Day20151011Bean day_20151011;
            
            public static class Day20151011Bean {
                public String temperature;
                public String weather;
                public String wind;
                public String week;
                public String date;
                
            }
           
        }

需要更改为 

	public Map<String,FutureBean> future;
	public static class FutureBean {
            public String temperature;
            public String weather;
            public String wind;
            public String week;
            public String date;


        }

最后的 bean 格式为：

	public class Response {
	    public String resultcode;
	    public String reason；
	    public ResultBean result;
	    public int error_code;
	 
	    public static class ResultBean {
		
	        public SkBean sk;
	        public TodayBean today;
	        public Map<String,FutureBean> future;
	
	        public static class SkBean {
	            public String temp;
	            public String wind_direction;
	            public String wind_strength;
	            public String humidity;
	            public String time;
	
	           
	        }
	
	        public static class TodayBean {
	            public String temperature;
	            public String weather;
	            public String wind;
	            public String week;
	            public String city;
	            public String date_y;
	            public String dressing_index;
	            public String dressing_advice;
	            public String uv_index;
	            public String comfort_index;
	            public String wash_index;
	            public String travel_index;
	            public String exercise_index;
	            public String drying_index;
	
	
	        }
	
	        public static class FutureBean {
		
	            public String temperature;
	            public String weather;
	            public String wind;
	            public String week;
	            public String date;
	
	
	        }
	    }
	}

1. 原始 jsonObject 和 jsonArray 解析

		Response response = null;
        try {
            JSONObject jsonObject = new JSONObject(json);
            if (jsonObject != null) {
                response = new Response();
                String resultcode = jsonObject.optString("resultcode");
                String reason = jsonObject.optString("reason");
                int error_code = jsonObject.optInt("error_code");

                response.resultcode = resultcode;
                response.reason = reason;
                response.error_code = error_code;

                JSONObject reslut = jsonObject.optJSONObject("result");
                // reslut.length() > 0 可防止 "result":"" 这种情况
                if (reslut != null && reslut.length() > 0) {
                    Response.ResultBean resultBean = new Response.ResultBean();

                    JSONObject sk = reslut.optJSONObject("sk");
                    if (sk != null) {
                        Response.ResultBean.SkBean skBean = new Response.ResultBean.SkBean();
                        String temp = sk.optString("temp");
                        String wind_direction = sk.optString("wind_direction");
                        skBean.temp = temp;
                        skBean.wind_direction = wind_direction;

                        resultBean.sk = skBean;
                    }


                    JSONObject today = reslut.optJSONObject("today");
                    if (today != null) {
                        Response.ResultBean.TodayBean todayBean = new Response.ResultBean.TodayBean();
                        String temperature = today.optString("temperature");
                        String weather = today.optString("weather");
                        todayBean.temperature = temperature;
                        todayBean.weather = weather;

                        resultBean.today = todayBean;
                    }

                    JSONObject future = reslut.optJSONObject("future");
                    if (future != null) {
                        HashMap<String, Response.ResultBean.FutureBean> futureBeanHashMap = new HashMap<>();

                        Iterator<String> keys = future.keys();
                        while (keys.hasNext()) {
                            String key = keys.next();
                            JSONObject futureItem = future.optJSONObject(key);
                            if (futureItem != null) {
                                Response.ResultBean.FutureBean futureBean = new Response.ResultBean.FutureBean();
                                String temperature = futureItem.optString("temperature");
                                String weather = futureItem.optString("weather");
                                futureBean.temperature = temperature;
                                futureBean.weather = weather;
                                futureBeanHashMap.put(key, futureBean);
                            }

                        }
                        resultBean.future = futureBeanHashMap;

                    }

                    response.result = resultBean;
                }

            }

        } catch (JSONException e) {
            e.printStackTrace();
        }
2. gson 解析


    	Gson gson = new Gson();	
	    Response response = gson.fromJson(json, Response.class);

很方便

## 服务器 json 格式不标准( 本来是 "data":{} ， 服务器 有时候返回 "data":""   ....)

	public static LiveInfo handleLiveInfo(String response) {
        LiveInfo liveInfo = null;
        if (response != null) {
            JsonObject obj = new JsonParser().parse(response).getAsJsonObject();
			// 防止 "data":"" 时 gson 格式报错 (BEGIN_WITH ....)
            if (obj != null && obj.get("data").toString().equals("\"\"")) {
                return null;
            }
            Gson gson = new Gson();
            liveInfo = gson.fromJson(response, LiveInfo.class);
        }

        return liveInfo;
    }

## Gson解析（List和Map）格式json数据

	public class jsonParse{  
  
          
  
	class City{  
  
        int id;  
  
        String name;  
  
        String code;  
  
        String map;  
  
	}  
        
  
	public static void main(String[] args) {  
  
        //列表/array 数据  
  
                 String str="[{'id': '1','code': 'bj','name': '北京','map': '39.90403, 116.40752599999996'}, {'id': '2','code': 'sz','name': '深圳','map': '22.543099, 114.05786799999998'}, {'id': '9','code': 'sh','name': '上海','map': '31.230393,121.473704'}, {'id': '10','code': 'gz','name': '广州','map': '23.129163,113.26443500000005'}]";  
  
        Gson gson=new Gson();  
  
        List<City> rs=new ArrayList<City>();  
  
         Type type = new TypeToken<ArrayList<City>>() {}.getType();  
  
          rs=gson.fromJson(str, type);  
  
          for(City o:rs){  
  
                  System.out.println(o.name);  
  
          }  
  
            
  
          //map数据  
  
          String jsonStr="{'1': {'id': '1','code': 'bj','name': '北京','map': '39.90403, 116.40752599999996'},'2': {'id': '2','code': 'sz','name': '深圳','map': '22.543099, 114.05786799999998'},'9': {'id': '9','code': 'sh','name': '上海','map': '31.230393,121.473704'},'10': {'id': '10','code': 'gz','name': '广州','map': '23.129163,113.26443500000005'}}";  
  
          Map<String, City> citys = gson.fromJson(jsonStr, new TypeToken<Map<String, City>>() {}.getType());   
  
          System.out.println(citys.get("1").name+"----------"+citys.get("2").code);  
  
	}  
 
	}  

## json 中 key 包含中文

	{"ret":0,
	    "response":{
	        "tag_category":{
	            "中国画":{
	                "年代":["先秦两汉","战国楚国","魏晋南北","隋唐五代","南宋北宋","元代","明清","近现代","年代不详","其他"],
	                "技法":["泼墨","工笔","写意","白描","写生","皴法","没骨","指头画","其他"],
	             
	            },
	            "书法":{
	                "分类":["书法","碑帖","写本写经","书札文牍","其他"],
	                "书体":["篆书","隶书","楷书","草书","行书","其他"],
	                
	            },
	            
	        }
	    }
	}

Json字段里面的Key存在中文，所以必须在相应的字段上使用@SerializedName("中国画")注解，给Key取别名。

如下：

	 @SerializedName("中国画")
    public ChinaPicture chinaPicture;
    /**
    	 * 中国画
     */
    public static class ChinaPicture {
        @SerializedName("年代")
        public List<String> years;
        @SerializedName("技法")
        public List<String> techniques;
        @SerializedName("题材")
        public List<String> topic;
        @SerializedName("规格")
        public List<String> specification;
    }




## 参考信息

[Gson解析JSON中动态未知字段key的方法](http://blog.csdn.net/chaosminds/article/details/49049455)
[在线JSON校验格式化工具(Be JSON)](http://www.bejson.com/)
[使用Gson解析复杂、变态的Json数据（包含中文key）](http://www.cnblogs.com/bianmajiang/p/3998083.html)


