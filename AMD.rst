异步模块定义(AMD)制定了定义模块的规则，这样模块和模块的依赖可以被异步加载。这和浏览器的异步加载模块的环境刚好适应（浏览器同步加载模块会导致性能、可用性、调试和跨域访问等问题）
API说明
define()函数
本规范只定义了一个函数"define"，它是全局变量，
define(id?,dependencies,factory);
--模块名是由一个或多个单词以正斜杠为分隔符拼接成的字符串
--单词须为驼峰形式，或者"."，".."
--模块名不允许文件扩展名的形式，如".js"
--模块名可以为 "相对的" 或 "顶级的"。如果首字符为"."或".."则为"相对的"模块名
--顶级的模块名从根命名空间的概念模块解析
--相对的模块名从 "require" 书写和调用的模块解析

上文引用的CommonJS模块id属性id模块
相对模块名解析示例：
如果模块"a/b/c"请求 "../d",则解析为"a/d"
如果模块"a/b/c"请求"./e",则解析为 "a/b/e"
如果AMD的实现支持加载器插件(Loader-Plugins),则"!"

依赖
dependencies,是个定义中模块所依赖模块的数组。依赖模块必须根据模块的工厂方法优先级执行，并且执行的结果应该按照依赖数组中的位置顺序以参数的形式传入（定义中模块的）工厂方法中、
依赖的模块名如果是相对的，应该解析为相对定义中的模块。换句话说，相对名解析为相对模块的名字，并非相对于寻找该模块的名字的路径。
本规范定义了三种特殊的依赖关键字。如果"require","exports",或"module"出现在依赖列表中，参数应该按照CommonJS模块规范自由变量去解析。
依赖参数是可选的，如果忽略此参数，它应该默认为["require","exports","module"]。然而，如果工厂方法的形参个数小于3，加载器会选择以函数指定的参数个数调用工厂方法。
工厂方法
第三个参数，factory, 为模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值。
例子：
使用require和exports:创建一个名为"alpha"的模块，使用了require,exports,和名为"beta"的模块
define("alpha",["require","exports","beta"],function(require,exports,beta){
	exports.verb = function() {
		return beta.verb();
		//Or:
		return require("beta").verb();
	}
});
//一个返回对象的匿名函数
define(["alpha"],function(alpha){
	return {
		verb:function(){
			return alpha.verb()+2;
		};
	};
});
//一个没有依赖性的模块可以直接定义对象：
define({
	add:function(x,y){
		return x+y;
	}
});
//一个使用了简单CommonJS转换的模块定义
define(function(require,exports,module){
	var a = require('a'),
		b = require('b');
	exports.action = function() {};
});
