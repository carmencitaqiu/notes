// H5跳转Native页面
// 跳转是Hybrid比用API之一
(1)页面内跳转，与Hybrid无关 (2)H5跳转Native界面 (3)H5新开Webview跳转H5页面，一般为页面动画切换
比如携程H5页面要去到酒店Native某一个页面可以这样：
requestHybird({
	tagname: 'forward',
	param: {
		//要去的页面
		topage:'hotel/detail',
		type: 'native',
		id:20151031
	}
});
比如H5新开Webview的方式跳转H5页面便可以这样:
requestHybrid({
	tagname:'forward',
	param: {
		topage:'hotel/detail',
		type:'webview',
		id: 20151031
	}
});
① header左侧与右侧可配置，显示为文字或者图标（这里要求header实现主流图标，并且也可由业务控制图标），并需要控制其点击回调

② header的title可设置为单标题或者主标题、子标题类型，并且可配置lefticon与righticon（icon居中）

③ 满足一些特殊配置，比如标签类header

所以，站在前端业务方来说，header的使用方式为（其中tagname是不允许重复的）

//Native以及前端框架会对特殊tagname的标识做默认回调，如果未注册callback，或者点击回调callback无法返回则执行默认方法
//back前端默认执行History.back,如果不可后退则指定URL，Native如果检测到不可后退则返回Native大首页
this.header.set({
	left: [
	{
		tagname:'back',
		value:'回退',
		lefticon:'back',
		callback:function(){}
	}],
	right: [
	{
		tagname:'search',
		callback:function(){}
	},
	//自定义图标
	tagname:'me',
	icon:'hotel/me.png',
	callback:function(){}
	],
	title:'title',
	title:['title','subtitle'],
	title: {
	value:'title',
	righticon:'down',
	callback:function(){}
	}
});
this.header.set({
	back:function(){},
	title:''
});
this.header.set({
	left: [{
		tagname:'back',
		callback:function(){}
	}],
	title:'',
});
//为完成Native端的实现，这里会新增两个接口，向Native注册事件，以及注销事件
var registerHybridCallback = function(ns,name,callback) {
	if (!window.Hybrid[ns]) window.Hybrid[ns] = {};
	window.Hybrid[ns][name] = callback;
};
var unRegisterHybridCallback = function(ns) {
	if (!window.Hybrid[ns]) return;
	delete window.Hybrid[ns];
};
