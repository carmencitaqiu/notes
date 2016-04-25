一 .2015 题目
 
 
我遇到的题目： 6 个选择其中 3 个多选， 1 个填空， 6 个大题。客服姐姐说题目是随机给的（因为给了一个时段考试，而不是统一时间点开考），不过题型应该是固定的。
 
 
1.     单选：一个数组，两个引用，相互赋值，问输出
眩晕抗性 -30%
 
 
2.     单选：问一个 return 匿名函数的函数的执行结果，内部还有 apply
眩晕抗性再 -69%
 
 
3.     单选：问字符串替换结果是什么，当然，又是套了几层，绕了几圈
眩晕抗性再 -1% ，嗯，做完这道给彻底绕晕了
 
 
4.     多选：移动端，如果 A 按钮上覆盖了一个 B 按钮，给 B 按钮的 touchstart 事件处理器中添加什么处理能让 A 触发 click
 
按钮被挡住了，要想恢复交互性，隐藏遮挡物就好了，只敢选了 visible 一个，因为不确定题目是不是要在本次交互中触发 click ，不清楚 display:none 和从 DOM 中删除会不会影响冒泡，没敢选
 
 
经过测试，发现本题根本不存在冒泡（好吧，当时可能没睡醒），顺便再测试了一下有冒泡的情况，发现不影响冒泡，即便 remove 掉了，还是会冒泡
 

测试代码如下：
<!--
<div id="div1" class="div"></div>
<div id="div2" class="div"></div>
-->
 
<div id="div1" class="div">
<div id="div2" class="div"></div>
</div>
 
<style>
.div{
width:50px;
height:50px;
position:absolute;
top:0;
left:0;
}
#div1{
background-color:red;
}
#div2{
background-color:green;
}
</style>
<script>
var div1 = document.getElementById('div1');
var div2 = document.getElementById('div2');
div1.onclick = function(){
  alert('红色');
}
div2.onclick = function(){
  alert('绿色');
 
  //this.style.display = 'none';//本次交互中不会触发红色，下次交互会触发，会冒泡
  //this.style.visibility = 'hidden';//同上
  //this.parentNode.removeChild(this);//同上
}
</script>
 
 
5.     多选：前端优化，下列哪一个可以减少 HTTP 请求数
最近正在翻译 Yahoo! 的 30 几条前端优化原则，压力不大
 

6.     多选：题目忘记了
记得除了前端优化的，其它两道都没有绝对把握
 
 
7.     填空：个人博客地址
想了下填了 cnblogs ，因为个人网站做得还不完善，拿不出手
 
 
8.     大题：生成 10 个 10-100 之间的随机数，并降序排列
 
 
隐约记得书上说 Math.random 返回 (0, 1] 值
 
 
查证之后发现 JS 高程中文版 135 页说 (0, 1) ，而网上的普遍说法是 [0, 1) ，后一种就和 C 里面的一样。以前看书记得 js 的随机数和 C 的不一样。经过测试发现书上是错的，确实含 0 不含 1 。总结如下：
1.     获取 [a, b] ： Math.round(Math.random()*(b-a)+a)// 四舍五入
2.     获取 (a, b] ： Math.ceil(Math.random()*(b-a)+a)// 向上取整（天花板）
3.     获取 [a, b) ： Math.floor(Math.random()*(b-a)+a)// 向下取整（地板）
4.     获取 (a, b) ：好奇怪的需求，不如直接用第一种吧
测试 random 范围的代码如下：
var x = parseInt((Math.random()*90+10 + '').split('.')[0]);//取整数部分
9.     大题：实现 IOS 风格的 switch 按钮，要求用多种方式实现
 
 
花了太多时间， “ 实现 ” 是要用嘴实现还是用代码？用代码写了个小实现，七八分钟就过去了，划不来
 
 
10. 大题：给 String 添加原型方法，实现简单的模版替换
考原型和正则表达式，不会在原型方法中获取字符串的值，书中说一般不要给原型加自定义属性，会污染环境，就没太在意这方面，只注重了去理解原型，构造函数，作用域链的本质及其关系，结果。。
 
 
查了一下，发现 this 就是原字符串的值，阿席巴思密达 ~~~ 代码如下：
function strcat(str){
  return this + str;
}
 
String.prototype.strcat = strcat;
alert('xi'.strcat(' ba'));
 

11. 大题：如何在画布上画出任意多个边界不相交的圆，考虑时间和空间的平衡
 
后半句感觉是要写代码，前半句又不像，最后没时间了，就卖了个萌 ——“ 最简单的方法是画同心圆 ” ，好吧，希望能让改卷的大大心情愉快
 
 
12. 大题：实现 loadScript(url, callback) 异步加载脚本，完成之后执行回调函数，要求支持 IE
 
 
非要支持 IE 吗，时间不够了，只好写出步骤注释
 
 
整理的代码库里收藏了 xhr ，如下：
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
xhr.onreadystatechange = function(){
  if(xhr.readyState === 4){
    if(xhr.status >= 200 && xhr.status < 300 || xhr.status === 304){
    //执行callback
  }
  else{
    //请求失败
  }
  }
}
xhr.open('get', url, true);//准备异步请求，get是为了响应速度
xhr.send(null);//发送请求，null是为了兼容性
 
 
在线笔试让人写这种东西真的好吗？
 
 
根据园友 @ 李明夕和 @ 老楼在评论中的分析，我应该是理解错题意了，不过没关系，代码如下：
<div id="div">白色变红色</div>
<script>
var script = document.createElement('script');
var callback = function(){
  $('#div').css('color', 'red');//用刚刚加载好的jquery修改样式
}
 
if('onload' in script){
  script.onload = function(){
    alert('ok');
  callback();
  }
}
else{
    script.onreadystatechange = function(){
    if(this.readyState === 'loaded' || this.readyState === 'complete'){
      alert('ok');
    callback();
    }
    else{
       alert('ERROR!!');
    }
  }
}
 
script.async = 'async';
//script.src = 'test.js';
script.src = 'http://ayqy.net/script/jquery.min.js';//加载远程资源更容易测试异步
document.body.appendChild(script);
</script>
<script>
alert('suppose to see this first');
</script>
 
 
用 script 标签动态加载（并执行）脚本需要注意以下几点问题：
1.     IE8- 支持 readystatechange 和 async ； Chrome 和 FF 不支持 readystatechange ，支持 load ，支持 async ； IE9/10 、 Opera 同时支持 readystatechange ， load 和 async
2.     虽然 readystatechange 是 HTML5 事件，不过 FF 和 Chrome 至今都没有实现它
3.     诡异的是 IE6 先 ok 再向下执行再 ok 再 ERROR ， IE7/8 先 ERROR 再向下执行再 ok ， IE9+ 未知。而 FF 和 Chrome 正常，先向下执行，再 ok 。
4.     需要注意 IE9/10 和 Opera 两者都支持的，所以不要用类似于 elem.onload=elem.onreadystatechange=handler; 的代码，因为在 IE9/10 和 Opera 中会触发多次，本来 onload 里面并没有各个状态值（都是 undefined ），不会触发多次，但 IE 的实现很诡异，所以，有风险
5.     为了避免 IE 中多次触发回调函数，应该在 ok 之后移除 onreadystatechange 事件处理器，保证只触发一次
 
 
 
13. 大题：实现 JQuery 中的 html 方法
 
 
看时间紧迫，过于紧张了，看到题目的时候眼睛罗圈了，理解成了实现 JQuery 中把字符串转 HTML 元素的方法，过于复杂，简单的写了思路。交了卷才发现看错题了。。。
 
 
JQuery 中还有比 html 方法更容易实现的吗？代码如下：
function html(elem){
  return elem.innerHTML;
}
//此处没有完全实现，因为JQ的html方法有三种形式：html(), html(str), html(fun)，分别用来获取/设置/用函数设置innerHTML
查看了 JQuery 内部，发现差不多就是这样实现的，效果一样，测试代码如下：
var $div = $('#div');
alert($div.html());
alert($div[0].innerHTML);
//在IE中标签都是大写的，其它浏览器中是小写
 
 