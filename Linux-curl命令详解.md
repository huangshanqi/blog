---
title: Linux-curl命令详解
date: 2018/11/8 20:46:25
categories: 
- Linux学习
tags:
- curl

---

# **Linux-curl命令详解**

默认curl使用GET方式请求数据，这种方式下直接通过URL传递数据  
可以通过 --data/-d 方式指定使用POST方式传递数据  
-,表示数据来自stdin，即标准输入设备

	# GET
	curl -u username https://api.github.com/user?access_token=XXXXXXXXXX
	
	# POST
	curl -u username --data "param1=value1&param2=value" https://api.github.com
	
	# 也可以指定一个文件，将该文件中的内容当作数据传递给服务器端
	curl --data @filename https://github.api.com/authorizations
	
注：默认情况下，通过POST方式传递过去的数据中若有特殊字符，首先需要将特殊字符转义在传递给服务器端，如value值中包含有空格，则需要先将空格转换成%20，如：

	curl -d "value%201" http://hostname.com
	
在新版本的CURL中，提供了新的选项 --data-urlencode，通过该选项提供的参数会自动转义特殊字符。

	curl --data-urlencode "value 1" http://hostname.com
	
除了使用GET和POST协议外，还可以通过 -X 选项指定其它协议，如：

	curl -I -X DELETE https://api.github.cim
	
上传文件

	curl --form "fileupload=@filename.txt" http://hostname/resource
	

---

**-A, --user-agent <agent string>**  
**说明**：设置用户代理发送给服务器。有些网站访问会提示只能使用IE浏览器来访问，这是因为这些网站设置了检查用户代理，可以使用curl把用户代理设置为IE，这样就可以访问了。     
**例子**：

	curl URL --user-agent "Mozilla/5.0" 
	
	curl URL -A "Mozilla/5.0"

---
 
**-b, --cookie <name=data>**  
**说明**：使用网站cookie信息，该选项来指定cookie，多个cookie使用分号分隔   
**例子**：

	# 自定义cookie信息
	curl http://man.linuxde.net --cookie "user=root;pass=123456"

	# 将网站的cookies信息保存到sugarcookies文件中
	curl -D sugarcookies http://localhost/sugarcrm/index.php
	 
	# 使用上次保存的cookie信息为文件sugarcookies
	curl -b sugarcookies http://localhost/sugarcrm/index.php
---

**-C, --continue-at <offset>**  
**说明**：通过使用-C选项可对大文件使用断点续传功能  
**例子**：

	# 当文件在下载完成之前结束该进程
	$ curl -O http://www.gnu.org/software/gettext/manual/gettext.html
	##############             20.1%
	
	# 通过添加-C选项继续对该文件进行下载，已经下载过的文件不会被重新下载
	curl -C - -O http://www.gnu.org/software/gettext/manual/gettext.html
	###############            21.1%

---

**-d, --data <data>**  
**说明**：HTTP POST方式传送数据   
**例子**：

	curl -X POST --data "data=xxx" example.com/form.cgi
	
	如果你的数据没有经过表单编码，还可以让curl为你编码，参数是“--data-urlencode”
	
	$ curl -X POST--data-urlencode "date=April 1" example.com/form.cgi
	
	
---

**-D, --dump-header <file>**  
**说明**：保存网站cookie信息  
**例子**：

	# 将网站的cookies信息保存到sugarcookies文件中
	curl -D sugarcookies http://localhost/sugarcrm/index.php
	 
	# 使用上次保存的cookie信息
	curl -b sugarcookies http://localhost/sugarcrm/index.php
---
 
**--data-ascii <data>**  
**说明**：以ascii的方式post数据，等价于-d     
**例子**：

---
 
**--data-binary <data>**  
**说明**：以二进制的方式post数据，等价于-d   
**例子**：

---
	 
**--data-urlencode <data>**  
**说明**：通过该选项提供的参数会自动转义特殊字符  
**例子**：

	curl --data-urlencode "value 1" http://hostname.com
	“value 1”中的空格就不需要转义为20%了
---
  
**-e, --referer <URL>**  
**说明**：设置参照页字符串。参照页是位于HTTP头部中的一个字符串，用来表示用户是从哪个页面到达当前页面的，如果用户点击网页A中的某个连接，那么用户就会跳转到B网页，网页B头部的参照页字符串就包含网页A的URL    
**例子**：

	curl --referer http://www.google.com http://man.linuxde.net
	

---

**-F, --form <name=content>**  
**说明**：模拟http表单提交数据  
**例子**：
	
	假定文件上传的表单是下面这样
	
		<form method="POST" enctype='multipart/form-data' action="upload.cgi">
	　　　　<input type=file name=upload>
	　　　　<input type=submit name=press value="OK">
	　　</form>
	　　
	你可以用curl这样上传文件：
	$ curl --form upload=@localfilename --form press=OK [URL]
	
---

**-G, --get**  
**说明**：
**例子**：

---

**-H, --header <header>**
**说明**：使用-H"头部信息"传递多个头部信息  
**例子**：

	curl -H "Host:man.linuxde.net" -H "accept-language:zh-cn" URL
	
---

**-i, --include**  
**说明**：输出时包括protocol头信息,显示http response的头信息，连同网页代码一起,而“-I”则只输出头部信息。     
**例子**：

	curl -i www.baidu.com
	HTTP/1.1 200 OK
	Server: bfe/1.0.8.18
	Date: Sun, 22 Oct 2017 06:05:32 GMT
	Content-Type: text/html
	Content-Length: 2381
	Last-Modified: Mon, 23 Jan 2017 13:27:32 GMT
	Connection: Keep-Alive
	ETag: "588604c4-94d"
	Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
	Pragma: no-cache
	Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/
	Accept-Ranges: bytes

	<!DOCTYPE html>
	<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> </body> </html>

---

**-I, --head**  
**说明**：只显示请求头信息，只打印出HTTP头部信息 
**例子**：

	curl -I www.baidu.com
	
	HTTP/1.1 200 OK
	Server: bfe/1.0.8.18
	Date: Sun, 22 Oct 2017 06:01:12 GMT
	Content-Type: text/html
	Content-Length: 277
	Last-Modified: Mon, 13 Jun 2016 02:50:04 GMT
	Connection: Keep-Alive
	ETag: "575e1f5c-115"
	Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
	Pragma: no-cache
	Accept-Ranges: bytes

---

**--limit-rate <speed>**  
**说明**：对CURL的最大网络使用进行限制  
**例子**：
 	
 	载速度最大不会超过1000B/second
 	 
	curl --limit-rate 1000B -O http://www.gnu.org/software/gettext/manual/gettext.html

---

**-L, --location**  
**说明**：通过-L选项进行重定向。默认情况下CURL不会发送HTTP Location headers(重定向).当一个被请求页面移动到另一个站点时，会发送一个HTTP Loaction header作为请求，然后将请求重定向到新的地址上。  
**例子**：

	访问google.com时，会自动将地址重定向到google.com.hk上。
	
	curl http://www.google.com
	<HTML>
	<HEAD>
	    <meta http-equiv="content-type" content="text/html;charset=utf-8">
	    <TITLE>302 Moved</TITLE>
	</HEAD>
	<BODY>
	    <H1>302 Moved</H1>
	    The document has moved
	    <A HREF="http://www.google.com.hk/url?sa=p&amp;hl=zh-CN&amp;pref=hkredirect&amp;pval=yes&amp;q=http://www.google.com.hk/&amp;ust=1379402837567135amp;usg=AFQjCNF3o7umf3jyJpNDPuF7KTibavE4aA">here</A>.
	</BODY>
	</HTML>
	
	上述输出说明所请求的档案被转移到了http://www.google.com.hk。
	
	这是可以通过使用-L选项进行强制重定向
	让curl使用地址重定向，此时会查询google.com.hk站点
	curl -L http://www.google.com

---

**-o, --output <file>**  
**说明**：将文件保存为命令行中指定的文件名的文件中  
**例子**：

	将文件下载到本地并命名为mygettext.html
	curl -o mygettext.html http://www.gnu.org/software/gettext/manual/gettext.html
		
---

**-O, --remote-name**  
**说明**：使用URL中默认的文件名保存文件到本地  
**例子**：

	# 将文件保存到本地并命名为gettext.html
	curl -O http://www.gnu.org/software/gettext/manual/gettext.html
	
	同时获取多个文件,若同时从同一站点下载多个文件时，curl会尝试重用链接(connection)。
	curl -O URL1 -O URL2
	
	# 下载ftp文件
	curl -u ftpuser:ftppass -O ftp://ftp_server/public_html/xss.php
	
---

**--retry <num>**  
**说明**：传输出现问题时，重试的次数   
**例子**：

	curl --retry 3 www.assfsfsfsfs.com
	Warning: Transient problem: timeout Will retry in 1 seconds. 3 retries left.
	Warning: Transient problem: timeout Will retry in 2 seconds. 2 retries left.
	Warning: Transient problem: timeout Will retry in 4 seconds. 1 retries left.
	curl: (6) Could not resolve host: www.assfsfsfsfs.com


---

**-s, --silent**  
**说明**：静默模式,就是不显示错误和进度  
**例子**：

	curl -s www.baidu.com
	
---

**-S, --show-error**  
**说明**：显示错误   
**例子**：

	curl -S www.baidu.com
	
---

**-T, --upload-file <file>**  
**说明**：通过 -T 选项可将指定的本地文件上传到服务器上  
**例子**：

	# 将myfile.txt文件上传到服务器
	curl -u ftpuser:ftppass -T myfile.txt ftp://ftp.testserver.com
	
	# 同时上传多个文件
	curl -u ftpuser:ftppass -T "{file1,file2}" ftp://ftp.testserver.com
	
	# 从标准输入获取内容保存到服务器指定的文件中
	curl -u ftpuser:ftppass -T - ftp://ftp.testserver.com/myfile_1.txt

---

**-u, --user \<user:password\>**  
**说明**：在访问需要授权的页面时，可通过-u选项提供用户名和密码进行授权  
**例子**：

	curl -u username:password URL
	 
	# 通常的做法是在命令行只输入用户名，之后会提示输入密码，这样可以保证在查看历史记录时不会将密码泄露 
	curl -u username URL
---

**-v, --verbose**  
**说明**：`-v`参数可以显示一次http通信的整个过程，包括端口连接和http request头信息。    
**例子**：

	curl -v www.baidu.com?t=0000
	* Rebuilt URL to: www.baidu.com/?t=0000
	*   Trying 61.135.169.125...
	* TCP_NODELAY set
	* Connected to www.baidu.com (61.135.169.125) port 80 (#0)
	> GET /?t=0000 HTTP/1.1
	> Host: www.baidu.com
	> User-Agent: curl/7.51.0
	> Accept: */*
	> 
	< HTTP/1.1 200 OK
	< Server: bfe/1.0.8.18
	< Date: Sun, 22 Oct 2017 06:15:35 GMT
	< Content-Type: text/html
	< Content-Length: 2381
	< Last-Modified: Mon, 23 Jan 2017 13:27:32 GMT
	< Connection: Keep-Alive
	< ETag: "588604c4-94d"
	< Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
	< Pragma: no-cache
	< Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/
	< Accept-Ranges: bytes
	< 
	<!DOCTYPE html>
	<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> </body> </html>
	* Curl_http_done: called premature == 0
	* Connection #0 to host www.baidu.com left intact

---

**-X, --request <command>**  
**说明**：通过 -X 选项指定请求使用的HTTP请求方法，默认curl使用GET方式请求数据  
**例子**：

	curl -I -X DELETE https://api.github.cim

---


**-x, --proxy <[protocol://][user:password@]proxyhost[:port]>**  
**说明**：-x 选项可以为CURL添加代理功能  
**例子**：

	# 指定代理主机和端口
	curl -x proxysever.test.com:3128 http://google.co.in

---

