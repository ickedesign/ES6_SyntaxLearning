### Decorator

基本概念，基本用法

修饰器是一个函数，用来修改类的行为

**环境配置：**
- 需要额外安装一个插件“babel-plugin-transform-decorators-legacy”
- 在入口文件引入“babel-polyfill”
- “.babelrc”文件也要添加改行："plugins":["transform-decorators-legecy"]

``` js
{
	//基本用法
	//修饰器提供的一个功能，限制某个属性是只读的
	//target指修改的类本身，name指修改的类名称,descriptor指该属性的描述对象
	//readonly修饰器函数，修改类的行为
	let readonly = function(target,name,descriptor) {
		descriptor.writable = false;
		return descriptor;
	};

	class Test {
		//找到readonly这个修饰器
		@readonly
		time() {
			return '2017-06-28';
		}
	}

	let test = new Test();

  /*
  test.time = function() {
	console.log('reset time');
	//index.js:8774 Uncaught TypeError: Cannot assign to read only property 'time' of object '#<Test>'
	}
	*/

	console.log(test.time());//2017-06-28
}

{
	//该修饰器的功能是在类上新增一个静态属性myname，且属性值为hello
	let typename = function(target,name,descriptor) {
		target.myname = 'hello';
	}

	//在定义类的外面加修饰器，只允许在前面加
	@typename
	class Test{

	}

	console.log('类修饰符',Test.myname);//修饰符 hello
}

//一个第三方的库，包含了很多修饰器
//第三方修饰器的js库:core-decorators;
//npm install core-decorators --save-dev

{
	//埋点系统
	//把埋点系统抽离出来，作为可复用的模块
	let log = type => {
		return function(target,name,descriptor) {
			let src_method = descriptor.value;
			//rest参数
			descriptor.value=(...arg) => {
				src_method.apply(target,arg);
				//模拟一个埋点，在真实的业务开发中换成埋点函数即可
				console.log(`log ${type}`)
			}
		}
	}

	class AD {
		@log('show')
		show() {
			console.info('ad is show');
		}
		@log('click')
		click() {
			console.info('ad is click');
		}
	}

	let ad = new AD();
	ad.show();
	ad.click();
	//ad is show
	//log show
	//ad is click
	//log click
}
```