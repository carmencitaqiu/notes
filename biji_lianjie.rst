(一) HTML

1.语义化相关

2. 块级元素与行内元素
行内元素：<a> <b> <button> <input> <label> <select> <textarea>
块级元素：<p> <div> <h1>~<h6> <ul>  html5新增的<article> <audio> <canvas>
3. 几个常用标签以及其解释

4.viewport
viewport总共有5个属性，分别如下：

content height width initial-scale user-scalable target
<meta name="viewport"
content="
height = [ pixel_value |device-height] ,
width = [ pixel_value |device-width ] ,
initial-scale = float_value , minimum-scale = float_value , maximum-scale = float_value ,
user-scalable =[yes | no] ,
target- densitydpi = [ dpi_value | device-dpi| high-dpi | medium-dpi | low-dpi] "
/>
viewport的5个属性


（二）CSS

1. BFC相关知识点

2. float相关知识点

float属性值有以下几种：left,right,none,inherit;
float不合理之处：使元素脱离文档流，不占据空间，会破坏行内框inline boxes导致line boxes高度坍塌，需要闭合浮动，解决引起的坍塌。
方法：① 添加新标签并为其设置 clear:both 
②:after伪元素：利用浮动元素的容器的::after来清除，::after只对块级元素有用，因此display:block
.clearfix:aftere {
	content: ".";
	display:block;
	height:0;
	visibility:hidden;
	clear:both;
}
.clearfix {
	*zoom:1;
}
③为父元素设置overflow触发器BFC，强制包裹浮动元素:给浮动元素的容器添加overflow:hidden;使用overflow:hidden之后，浮动元素又回到了容器中，把容器高度撑起。



3 浏览器兼容性问题
① <meta http-equiv="X-UA-Compatible" content="IE=8" 或 content ="chrome=1">
使IE5,6,7兼容8
	<script src="http://ir7-js.google.comde.com/svn/version/2.0(beta)/IE8.js" type="text/javascript"></script>
	<meta http-equiv="X-UA-Compatible" content="IE=edge.chrome=1"/>
FF:设置padding后，div会增加height和width,IE不会，所以要用!important 多设一个height,width;FF支持!important,IE则忽略，可用!important为FF特别设置样式
在FF和IE中的BOX模型解释不一致导致相差2px,解决方法；
div {margin:30px !important;margin:28px;} 重复定义对于IE来说，会按照最后一个来执行
ul和ol列表缩进问题
消除ul,ol等列表的缩进时，样式写成：list-style:none;margin:0px;padding:0px; //因为margin属性对IE有效，padding属性对FireFox有效
写法不同，例如圆角：前缀：-webkit, -moz, -o ,box-shadow
-webkit-box-shadow: 0 2px 3px rgba(0, 0, 0, .1);
-moz-box-shadow: 0 2px 3px rgba(0, 0, 0, .1);
-o-box-shadow: 0 2px 3px rgba(0, 0, 0, .1);
box-shadow: 0 2px 3px rgba(0, 0, 0, .1);

IE6存在的bug:设置为float的div在ie下设置的margin会加倍，俗称IE 6双边距。解决方案：display:inline;
CSS Hack原理:即使我们在属性名称前面加上一个下划线_，IE 6 照样可以识别该属性，而且只有IE 6可以识别。

4.移动端适配
(1)固定宽度，viewport缩放 需要根据屏幕宽度来动态生成viewport，生成的 viewport 基本是这样
<meta name="viewport" content="width=640,initial-scale=0.5,maximum-scale=0.5,minimum-scale=0.5,user-scalable=no">
640 是我们根据设计图定下的，0.5 是根据屏幕宽度动态生成的
(2) 使用EM屏幕宽度设置 rem的大小，rem就是相对于根元素<html>的font-size来做计算
(2)DPI 根据设备像素比（window.devicePixelRatio）给<html>设置data-dpr
（三）JS
基本数据类型：String Number Boolean Object Null Undefined

js的隐式类型转换 
(1)"+" "-"
"+" var a = 11,b='22'; 
var c = a+b;
"-" var a = 11,b='5';
var c = a-b; 
typeof c -> number
(2)if
var obj = {name:'jack'}
if (obj) {}隐式转换
强制转换：Boolean(0) Number(undefined) String(null)

1 闭包
当一个函数内部匿名函数用到了自己的变量，并且这个匿名函数被返回了，就建立了一个闭包。

function a () {
	var i =0;
	function b() {
		console.log(i);
	}
	return b;
}
//就算a调用结束被销毁，i也会存在不会消失。
//当a定义时，js解释器会将函数a的作用域链设置为定义a时所在环境

2 事件流与事件委托，事件阻止冒泡

DOM事件流的三个阶段：事件捕获、目标、冒泡
	<div id="p">
		parent
		<div id="c">
		child
		</div>
	</div>
c.addEventListener('click',function(e){},true);	//捕获
c.addEventListener('click',function(e){},false); //冒泡
顺序：父节点捕获->子节点捕获->子节点冒泡->父节点冒泡
e.target,e.currentTarget的区别
e.stopPropogation 取消冒泡，前提是e.bubbles=true才有用
e.cancelable 是否取消事件的默认行为 ->parentDefault 取消事件默认行为
dom.dispatchEvent(event):触发模拟事件
事件代理,实现了事件委托：
$('body').delegate('#div1',function(e){
	alert('div1');
});
(1)document对象很快可以访问，可以再任何时间点为它添加事件处理
(2)用时更少，因为所需DOM引用更少
(3)占用内存空间更少，提升整体性能
浏览器兼容写法：
target-->srcElement(IE)
preventDefault()--->event.returnValue=(IE)
阻止冒泡：stopPropagation()-->event.cancelBubble=true;
添加事件：addEventListener()-->attachEvent(IE)

阻止默认事件：event.preventDefault-->event.returnValue=false;

3 原型,面向对象继承，与作用域相关知识点
function Animal(name) {
	this.name = name;
	this.showName = function() {
		console.log(this.name);
	}
}
或者：
Animal.prototype.showName = function() {
	console.log(this.name);
}
function Cat(name) {
	Animal.call(this.name);
}
Cat.prototype = new Animal();
function Dog(name) {
	Animal.apply(this.name);
}
Dog.prototype = new Animal();
var cat = new Cat("Black cat");	//call必须是object
var dog = new Dog(["Black Dog"]);	//apply必须是array
每当函数在调用时，其活动对象会自动取得两个特殊变量，this和arguments
内部函数在搜索这两个变量时，只会搜索到其活动对象为止，不过把外部作用域中的this对象保存在一个闭包能够访问的变量里，就可以让闭包方位这对象了
var name = "The window";
var obj = {
	name:"My Object",
	getNameFunc:function() {
		var that = this;
		return function() {
			return that.name;
		};
	}
};
JS所有的函数都有一个prototype(原型)属性，这个属性是一个指向一个对象的指针，而这个对象用途是包含可以由特定类型的所有实例共享的属性和方法。
prototype:就是通过调用构造函数而创建的那个对象实例的原型方法，特点是让所有对象实例共享它所包含的属性和方法
isPrototypeOf:Person.prototype.isPrototypeof(person);
hasOwnProperty:person.hasOwnProperty("name");

4 jQuery
jQuery中将数组转化为字符串，再转化回来
$fn.stringifyArray = function(array) {
	return JSON.stringify(array);
}
$fn.parseArray = function(array) {
	return JSON.parse(array);
}
5
5 js优化问题

综合

1. 简述前端MVC
Model:是数据模型，一个比较稳定的模块，不会经常变化并且可被重用，每一次数据变化会有一个通知机制，通知所有的controller对数据变化做出响应。
View:视图，前端中为html模板，Controller会根据数组组装为最终的html字符串，然后展示
Controller:html标签当然需要一些关键model值用于controller获取model相关标志;
a.用户在View上进行操作————比如在文本框输入数值或是点击按钮
b.Controller处理这个动作,并通过Model对数据进行增删改查，将其传递到View
c.View数据展示给用户
优点：绑定Model，当Model改变时，View接收到通知，重新渲染。不需要手动指定页面元素更新HTML，改变Model时直接改变View


Backbone总的理解：
(1)模型Models进行数据处理，创建验证销毁或者保存到服务器
(2)合集Collections用来枚举
(3)Views通过事件来进行处理，与现有的Application 通过RESTful JSON接口进行交互
MVVM模式：------- Model-View-ViewModel
Model  --数据访问层  View --UI界面  
ViewModel --它是View的抽象，负责View与Model之间信息转换，将View的Command传送到Model
View与ViewModel连接的方式：
Binding Data      -----实现数据的传递
Command          -----实现操作的调用
AttachBehavior  -----实现控件加载过程中的操作
 View省去了大量的逻辑代码，转移到了ViewModel,而是执行一些命令向其请求一个动作。相反ViewModel与Model通讯，来更新UI，实现了松耦合，提高了可测试性。

2. web前端优化详解
传输层面：减少请求数，降低请求量
执行层面： 减少重绘，回流
传输层面是优化的核心点，对浏览器的优化有一个基本认识：
a.网页自上而下的解析渲染，边解析边渲染，页面内CSS文件会阻塞渲染
b.浏览器在document下载结束后会检查静态资源，新开线程下载，无序并发会导致主资源速度下降，从而影响首屏渲染
c.浏览器缓存可用时使用缓存资源，这是可以避免请求体的传输，对性能有极大提高。


减少请求数：合并样式，脚本文件(grunt),合并背景图片,CSS3图标字体IconFront
降低请求量：优化静态资源，比如用Zepto替代jQuery,阉割IScroll,图片资源延时加载
缓存为王：对更新较慢的资源，如大图片做缓存(浏览器缓存，localStorage是本地缓存)
按需加载：先加载主要资源，其余的非首屏资源滚动延时加载

(1)localstorage：常用语ajax请求缓存，有500万字符的限制，基本来说就是5M左右的限制，不被爬虫识别，不能跨域共享，所以不要用以存储业务关键信息，更加不要存储安全信息，不可读写，所以不能用它来做页面通信
(2)Application cache：HTML5新增api，存储的是一般是静态资源，允许浏览器请求这些资源时不必通过网络
(3)lazyload:为img标签src设置统一的图片链接，而将真实链接地址装在自定义属性中。
所以开始时候图片是不会加载的，我们将满足条件的图片的src重置为自定义属性便可实现延迟加载功能

CDN:静态资源和页面部署之间存在时间间隙，迫使前端静态资源发布必须采用非覆盖式。浏览器缓存只是用于提升二次访问的速度，对于首次访问的加速，需要从网络层面进行优化，
因此：对静态资源的部署要做两点改变：
(1)将静态资源部署到不同网络线路的服务器中，以加速对应网络中CDN节点无缓存时溯源的速度。
(2)加载静态资源时使用与页面不同的域名，一方面是便于接入为CDN而设置的智能DNS解析服务，另一方面因为静态资源和主页面不同域，这样加载资源的HTTP请求就不会带上主页面中的Cookie等数据，较少了数据传输量，又进一步加快网络访问。
3. 编程题，实现回到顶部功能
① 数组去重
function unique(array) {
	var n = [];	//临时数组；
	for (var i =0; i< array.length;i++)
	{
		if (n.indexOf(array[i])==-1) 
			n.push(array[i]);
	}
	return n;
}
(2)数组操作方法一览
arrayObj.push()//将一个或多个新元素添加到末尾
arrayObj.unshift()//将一个或多个元素添加到开头
arrayObj.splice(insertPos,insertObj),将一个或多个元素插入到数组的指定位置，插入位置的元素自动后移
arrayObj.pop() 	//移除最后一个元素，并返回该元素值
arrayObj.shift()	//移除最前一个元素，返回该元素值
arrayObj.splice(deletePos,deleteCount);	//删除从指定位置deletePos开始的指定数量deleteCount的元素，数组形式返回所移除的元素。
arrayObj.splice(start,end);	//以数组的形式返回数组的一部分，不包括end
arrayObj.concat([]);//将多个数组连接成一个新数组
arrayObj.sort();	//排序
arrayObj.splice(0);	//数组的拷贝
4.模块化编程
AMD规范：Asynchronous Module Definition的编写，意思就是"异步模块定义"，采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。
模块化编程用于解决：
① 解决单文件变量命名冲突问题
② 解决前端多人协作问题
③ 解决文件依赖问题
requirejs的优势：
(1)防止js加载阻塞后面渲染 
(2)每个模块是一个单独的js文件，加载多个模块就会有多个http请求，影响性能。requirejs提供了一个优化工具，当模块部署完毕后，可以用这个工具将多个模块合并在一个文件中，减少http请求数。