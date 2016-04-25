大题：生成 10 个 10-100 之间的随机数，并降序排

查证之后发现 JS 高程中文版 135 页说 (0, 1) ，而网上的普遍说法是 [0, 1) ，后一种就和 C 里面的一样。以前看书记得 js 的随机数和 C 的不一样。经过测试发现书上是错的，确实含 0 不含 1 。总结如下：
1.     获取 [a, b] ： Math.round(Math.random()*(b-a)+a)// 四舍五入
2.     获取 (a, b] ： Math.ceil(Math.random()*(b-a)+a)// 向上取整（天花板）
3.     获取 [a, b) ： Math.floor(Math.random()*(b-a)+a)// 向下取整（地板）
4.     获取 (a, b) ：好奇怪的需求，不如直接用第一种吧
测试 random 范围的代码如下：
var x = parseInt((Math.random()*90+10 + '').split('.')[0]);//取整数部分

js实现模板替换：
String.prototype.render = function(arr) {
	var replaceKey = [];
	var str = [];
	for (var key in arr) {
		replaceKey.push(key);
	str.push(arr[key]);
	}
	var format = this;
	for (var i =0;i<replaceKey.length;i++) {
		var reg = new RegExp("\\${" + replaceKey[i]+ "}","g");
		format = format.replace(reg,str[i]);
		console.log(format);
	}
	return format;
}

var greeting = 'my name is ${name},age ${age}';
var result = greeting.render({'name':'jack','age':16});
console.log(result);

怎么实现在一块画布上,画任意个圆不相交,且圆不能碰边界,要求做到时间和空间的平衡？
1.获取HttpRequest对象
/*获取HttpRequest对象，可以兼容各个浏览器 包括IE5.5+*/
function getHttpObject(){
  if(typeof XMLHttpRequest == "undefined"){//如果该对象未定义，则自定义该对象
         XMLHttpRequest = function(){
                 try{
                         return new ActiveXObject("Msxml2.XMLHTTP.6.0");
                 }catch(e){}
                 try{
                         return new ActiveXObject("Msxml2.XMLHTTP.3.0");
                 }catch(e){}
                 try{
                         return new ActiveXObject("Msxml2.XMLHTTP");
                 }catch(e){}
                 try{//老版本的 Internet Explorer （IE5 和 IE6）
                         return new ActiveXObject("Microsoft.XMLHTTP");
                 }catch(e){}
                
                 return false;
         }
  }
  return new XMLHttpRequest();
}
var xhr = getHttpObject();
2.onreadystatechange
xhr.onreadystatechange = function() {
	if (xrh.readyState = 4) {
		if (xhr.status >=200 && xhr.status < 300 || xhr.status ===304){
			//执行callback
		} else {
			//请求失败
		}
	}
}
3.open
xhr.open('get',url,true);	//准备异步请求
4.send
xhr.send(null);



var $div = $('#div');
alert($div.html());
alert($div[0].innerHTML);