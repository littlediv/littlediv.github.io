# curl 相关命令 #

## curl -I "http://www.baidu.com"

只返回 header 相关信息

	C:\Users\mac>curl -I "http://www.xinhong.net/app/xinHongMeteo.apk"
	HTTP/1.1 200 OK
	Server: nginx/1.10.1
	Date: Thu, 26 Oct 2017 07:24:27 GMT
	Content-Type: application/octet-stream
	Content-Length: 16997573
	Last-Modified: Mon, 16 Oct 2017 08:01:27 GMT
	Connection: keep-alive
	ETag: "59e46757-1035cc5"
	Accept-Ranges: bytes

## C:\Users\mac>curl --range 0-99  "http://httpd.apache.org/images/httpd_logo_wide_new.png"

发送字节范围下载，下载最开始的99字节



