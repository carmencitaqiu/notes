AMD和CMD:
AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。
类似的还有 CommoneJS Modules/2.0规范，是BravoJS在推广过程中对模块定义的规范化产出。
区别：
1.对于依赖的模块，AMD是提前执行，CMD是延迟执行。不过RequireJS从2.0开始，也改成可以延迟执行。CMD推崇as lazy as possible
2.CMD推崇依赖就近，AMD推崇依赖前置。
//CMD
define(function(require,exports,module){
	var a = require('./a');
	a.doSomething();
	//.....
	var b = require('./b');	//依赖可以就近书写
	b.doSomething();
})
//AMD 默认推荐
define(['./a','./b'],function(a,b){
	a.doSomething();
	b.doSomething();
})
3.AMD的API默认是一个当多个用，CMD的API严格区分，推崇职责单一。比如AMD里，require分全局require和局部require,都叫require。CMD里，没有全局require,而是根据模块系统的完备性，提供seajs.use来实现模块系统的加载启动。CMD里，每个API都简单纯粹。


CMD规范中，一个模块就是一个文件，代码的书写格式如下：
define(factory);
define Function
define 是一个全局函数，用来定义模块
define define(factory)
define接受factory参数，factory可以是一个函数，也可以是一个对象或字符串
factory为对象、字符串时，表示模块的接口就是该对象或字符串。比如可以如下定义一个JSON数据模块：
define({"foo":"bar"});
也可以通过字符串定义模板模块：
define('I am a template.My name is {{name}}.');
factory为函数时，表示是模块的构造方法。执行该构造方法，可以得到向外提供的接口。factory方法在执行时，默认会传入三个参数：require,exports和module:
define define(id?,deps?,facotry)
define也可以接受两个以上参数。字符串id表示模块标识，数组deps是模块依赖、
define('hello',['jquery'],function(require,exports,module){
	//模块代码
});
API Specification
define()function
The Asynchronous Module Definition(AMD)API specifies