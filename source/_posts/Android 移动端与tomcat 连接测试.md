# Android 移动端与tomcat 连接测试

## 使用模拟器，而不是真机，请求本地Tomcat ,HTTP 访问报错：
exception = failed to connect to /127.0.0.1 (port 8089): connect failed: ECONNREFUSED (Connection refused)
10-19 02:28:04.166: W/BroadcastQueue(336): Timeout of broadcast BroadcastRecord{dc8a0f6 u0 android.intent.action.BOOT_COMPLETED} -


------>解决：不能用localhost/127.0.0.1 访问，把localhost改为10.0.2.2已经解决。


## 手机连接PC Tomcat:
1.设置PC端的防火墙，参考：http://blog.csdn.net/chendc201/article/details/22905489
2.手机和PC同在一个局域网下的AP
3. 直接访问http://192.168.14.24:8089/ 测试


如果有个无线路由器，电脑和Android手机都连接上了这个路由器的话，就可以通过无线局域网来访问电脑端的tomcat，从而让电脑成为手机服务的服务端。

开启电脑的CMD，输入ipconfig。可查看到本机的局域网地址。

可知我本机的地址为：http://10.0.0.2。

在手机上，查看wifi的属性，可知局域网的地址为：10.0.0.3。两台设备都在同一个局域网里面的。

开启tomcat，打开一个浏览器，在地址栏里面输入地址+端口号。默认的端口号为8080.所以就是输入：http://10.0.0.2:8080/

电脑上能访问，那么手机上应该也能访问了。打开手机的浏览器，在里面输入一样的地址，发现手机打开网页很慢，最后提示无法访问。由此可知应该是电脑上的防火墙限制了，关闭电脑上的防火墙：

再刷新一下手机，发现终于可以访问了。以后开发手机客户端就可以连接服务器，访问电脑上的资源和操作数据库了……