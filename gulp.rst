1全局安装
npm install --global gulp
2.作为项目的开发依赖
npm install --save-dev gulp
3.在项目根目录下创建一个名为gulpfile.js的文件
var gulp = require('gulp');
gulp.task('default',function() {});
4.运行 gulp  
单独执行特定的task,输入 gulp <>
整合streams来处理错误
在stream中发生一个错误的话，它会被直接抛出，除非已经有一个时间监听器监听着error时间。这在处理一个比较长的管道操作的时候回显得比较棘手。
var combiner = require('stream-combiner2');
var uglify = require('gulp-uglify');
var gulp = require('gulp');

gulp.task('test',function(){
	var combined = combiner.obj([
		gulp.src('bootstrap/js/*.js'),
		uglify();
		gulp.dest('public/bootstrap');
	]);

	//任何在上面的stream中发生的错误，都不会抛出
	//而是会被监听器捕获
	combined.on('error',console.error.bind(console));
	return combined;
});
//删除文件和文件夹
你也许会想要在编译文件之前删除一些文件。由于删除文件和文件内容并没有太大关系，所以没必要去用一个gulp插件。最好的一个选择是使用一个原生的node模块。
因为 del模块支持多个文件以及globbing,因此，在这个例子中，我们将使用它来删除文件：
npm install --save-dev gulp del

├── dist
│   ├── report.csv
│   ├── desktop
│   └── mobile
│       ├── app.js
│       ├── deploy.json
│       └── index.html
└── src
在 gulpfile中，我们希望在运行我们的编译任务之前，将mobile文件的内容先清理掉。
var gulp = require('gulp');
var del = require('del');
gulp.task('clean:mobile',function(cb) {
	del([
		'dist/report.csv',
		//这里我们使用一个通配模式来匹配"mobile"文件夹中的所有东西
		'dist/mobile/**/*',
		//我们不希望删掉这个文件，所以我们取反这个匹配模式
		'!dist/mobile/deploy.json'
	],cb);
});
gulp.task('default',[clean:mobile]);
//在管道中删除文件
你可能需要在管道中将一些处理的文件删掉，使用vinyl-paths模块来简单地获取stream中每个文件的路径，然后传给del方法
npm install --save-dev gulp del vinyl-paths
.
├── tmp
│   ├── rainbow.js
│   └── unicorn.js
└── dist


var gulp = require('gulp');
var stripDebug = require('gulp-strip-debug');	//仅用于本例做演示
var del = require('del');
var vinyPaths = require('vinyl-paths');

gulp.task('clean:tmp',function(){
	return gulp.src('tmp/*')
	.pipe(stripDebug())
	.pipe(gulp.dest('dist'))
	.pipe(vinyPaths(del))
});
gulp.task('default',['clean:tmp']);


//增量编译打包，包括处理整理所涉及的所有文件
在做增量打包的时候，有一个比较麻烦的事情，你常常希望操作的是所有处理过的文件，而不仅仅是单个的文件。举个例子，你想要只对更改的文件做代码lint操作，以及一些模块封装的操作，然后将他们与其他已经lint过的，以及已经进行过模块封装的文件合并到一起。如果不用到临时文件的话，这将会非常困难。
var gulp = require('gulp');
var header = require('gulp-header');
var footer = require('gulp-footer');
var concat = require('gulp-concat');
var jshint = require('gulp-jshint');
var cached = require('gulp-cached');
var remember = require('gulp-remember');
var scriptGlob = 'src/**/*.js';
gulp.task('scripts',function(){
	return gulp.src(scriptsGlob)
	.pipe(cached('scripts'))	//只传递更改过的文件
	.pipe(jshint())		//对这些更改过的文件做一些特殊的处理
	.pipe(header('(function () {'))	//比如 jshinting ^^^
	.pipe(footer('})();'))
	.pipe(remeber('scripts'))
	.pipe(concat('app.js'))
	.pipe(gulp.dest('public/'));
});

gulp.task('watch',function() {
	var watch = gulp.watch(scriptsGlob,['scripts']);	//监视与 scripts 任务中同样的文件
	watcher.on('change',function(event){
		if (event.type === 'deleted') {					//如果一个文件被删除了，则将它忘记
			delete cached.caches.scripts[event.path];	//gulp-cacehd的删除api
			remember.forget('scripts',event.path);	//gulp-remember 的删除
		}
	});
});