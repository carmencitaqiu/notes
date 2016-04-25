1.解释下TCP三次握手
客户端发一个SYN同步序列报文，服务器回一个ACK应答报文，客户端再回一个ACK
HTTP:HTTP协议是无状态的，对于一个浏览器发出的多次请求，WEB服务器无法区分 是不是来源于同一个浏览器。
2.HTTP头中哪些是和缓存相关的？
Request:
Cache
	Cache-Control:max-age=0  /*以秒为单位*/
	If-Modified-Since:Wed,07 Dec 2011 10:21:11 GMT /*缓存文件的最后修改时间*/
	If-None-Match:"fadfadsfffcv213"	 /*缓存文件的ETag值*/
	Cache-Control:no-cache;	/*不使用缓存*/
Response:
Cache:
	Age:5324534,
	Date:Sun,18 Dec 2011 10:21:11 GMT /*当前resposne发送的时间*/
	Expires:Tue,Dec 2011 10:21:11 GMT /*缓存过期的时间（绝对时间）*/
	Last-Modified:Mon 19 Nov 2012 08:38:01 GMT /*服务器端文件的最后修改时间*/
如何判断缓存新鲜度
(1) 浏览器把缓存文件的最后修改时间通过header "If-Modified-Since"来告诉服务器
(2) 浏览器把缓存文件的ETag，通过 header"If-None-Match",来告诉服务器

通过最后修改时间，来判断缓存新鲜度
1.浏览器客户端想请求一个文档，首先检查本地缓存，发现存在这个文档的缓存，获取缓存中文档的最后修改时间，通过:If-Modified-Since,发送Request给Web服务器
2.Web服务器收到request,将服务器的文档修改时间(Last-Modified)跟If-Modified-Since比较，如果时间一样，说明缓存是最新的，服务器将发送 304 Not Modified给浏览器客户端，告诉客户端直接使用缓存里的版本。
3.如已经更新，则发送最新的给客户端，HTTP/1.1 200 OK

ETag:实体标签Entity Tag的缩写，根据实体内容生成的一段hash字符串，可以标识资源的状态，当资源发送改变时，ETag也随之发生变化，
ETag是Web服务端产生的，然后发给浏览器客户端，能解决一些Last-Modified无法解决的问题：
(1)某些服务器不能精确得到文件的最后修改时间
(2)某些文件的修改非常频繁
(3)一些文件的最后修改时间改变了，但是内容并未改变。 我们不希望客户端认为这个文件修改了


3.cookie有哪几个组成?
Set－Cookie: NAME=VALUE；Expires=DATE；Path=PATH；Domain=DOMAIN_NAME；SECURENAME=VALUE：
浏览器在发送cookie时会发送哪几个部分？
浏览器把Cookie通过HTTP request中的Cookie header发送给服务器
服务器通过 HTTP Resposne中的"Set-Cookie:header"把cookie发送给浏览器
自动登录：
1. 用户打开IE浏览器，在地址栏上输入www.cnblogs.com.

2. IE首先会在硬盘中查找关于cnblogs.com的cookie. 然后把cookie放到HTTP Request中，再把Request发给Web服务器。

3. Web服务器返回博客园首页（你会看到你已经登陆了）。


会话cookie: 是一种临时的cookie，它记录了用户访问站点时的设置和偏好，关闭浏览器，会话cookie就被删除了
持久cookie: 存储在硬盘上，（不管浏览器退出，或者电脑重启，持久cookie都存在）， 持久cookie有过期时间



5 . 又是一个很老题目，浏览器地址栏输入一个qq.com，发生了什么，讲出你所知道的所有的内容。 
DNS解析:这一步包括 DNS 具体的查找过程，包括：浏览器缓存->系统缓存->路由器缓存...
-> 建立连接,发送数据包:浏览器向服务器发送一个http请求，根据http协议要求，
组织一个请求的数据包，里面包含大量请求信息，包括请求的资源路径、你的身份
-> 服务器响应请求，返回浏览器:服务器的永久重定向响应 服务器给浏览器响应一个301永久重定向响应，这样浏览器就会访问
-> 浏览器跟踪重定向地址 
--> 服务器处理请求 服务器返回一个 HTTP 响应
->浏览器渲染程序页面：解析html构建dom树->构建render树->布局render树->绘制render树
浏览器发送静态资源请求

所以浏览器知道要把它们缓存多长时间。还有，每个响应都可能包含像版本号一样工作的ETag头（被请求变量的实体值），如果浏览器观察到文件的版本 ETag信息已经存在，就马上停止这个文件的传输。

CDN:Facebook利用内容分发网络（CDN）分发像图片，CSS表和JavaScript文件这些静态文件。所以，这些文件会在全球很多CDN的数据中心中留下备份。
浏览器发送异步请求
页面构建完成



