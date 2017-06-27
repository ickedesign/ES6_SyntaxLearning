### Generator

基本概念，`next`函数的用法，`yield*`的语法

`Generator`：异步编程的一种解决方案，相对于`Promise`更高级

``` js
{
	//需要引入babel-polyfill
	//Generator基本定义
	let tell = function* () {
		yield 'a';
		yield 'b';
		return 'c'
	};

	//Generator是个函数，执行使用()
	let k = tell();

	//Generator返回的是Iterator接口
	console.log(k.next());//Object {value: "a", done: false}
	console.log(k.next());
	console.log(k.next());
	console.log(k.next());
	//异步操作的过程:进行到k.next()时则执行
}

{
	/*Generator函数和Iterator接口的关系，因为任何一个对象的Iterator接口
	部署在了Symbol.Iterator这个属性上，Generator函数就是一个遍历器生成
	函数，所以我们可以直接把它赋值给Symbol.Iterator，从而使这个对象也具
	备了Iterator接口*/
	//使用Generator也可以作为遍历器的一个返回值
	//给Object对象部署一个Iterator接口，使其可以使用for...of

	let obj = {};
	obj[Symbol.iterator] = function* () {
		yield 1;
		yield 2;
		yield 3;
	}

	for(let value of obj) {
		console.log('value',value);
		//value 1
		//value 2
		//value 3
	}
}

{
	//Generator在状态机下可以有它最大的优势
	//比如一种情形存在三种状态
	let state = function* () {
		//1，无限循环
		while(1) {
			yield 'A';
			yield 'B';
			yield 'C';
		}
	}
	let status = state();
	console.log(status.next());//Object {value: "A", done: false}
	console.log(status.next());//Object {value: "B", done: false}
	console.log(status.next());//Object {value: "C", done: false}
	console.log(status.next());//Object {value: "A", done: false}
	console.log(status.next());//Object {value: "B", done: false}
}

/*{
	//需要安装兼容包才不报错
	//async和await是Generator的一个语法糖
	//不同的写法
	let state =async function () {
		//1，无限循环
		while(1) {
			await 'A';
			await 'B';
			await 'C';
		}
	}
	let status = state();
	console.log(status.next());//Object {value: "A", done: false}
	console.log(status.next());//Object {value: "B", done: false}
	console.log(status.next());//Object {value: "C", done: false}
	console.log(status.next());//Object {value: "A", done: false}
	console.log(status.next());//Object {value: "B", done: false}
}*/

{
	//实例
	//抽奖次数的限制
	//解耦
	let draw = function(count) {
		//具体抽奖逻辑
		console.info(`剩余${count}次`);
	}
	//尽量少把变量存储为全局变量，一是影响性能，二是怕被用户修改，不安全
	//使用Generator来处理
	
	let residue = function* (count) {
		while(count > 0) {
			count --;
			//执行抽奖的具体逻辑
			yield draw(count);
		}
	}

	//执行抽奖的环节
	//num应由后台传回，此处省略该步骤
	let start = residue(5);
	let btn = document.createElement('button');
	btn.id = 'start';
	btn.textContent = '抽奖';
	document.body.appendChild(btn);
	document.getElementById('start').addEventListener('click',function() {
		start.next();
	},false)
	//剩余4次
	//剩余3次
	//剩余2次
	//剩余1次
	//剩余0次
}

{
	//假设场景：服务端的某个数据状态定期变化，我们前端需要定时去服务端取这个状态
	//因为http是无状态的链接。实时取得服务端的变化，一种是长轮询，一种是websocket
	//因为websocket的浏览器兼容不好，所以长轮询还是一个普遍的用法
	//之前的长轮询是使用定时器不断地去访问接口
	//使用Generator和Promise来让长轮询更加简洁
	
	let ajax = function* () {
		yield new Promise(function(resolve,reject) {
			//使用定时器来模拟请求的耗时
			setTimeout(function() {
				resolve({code:0})
			},200);
		})
	}

	//轮询的过程
	let pull = function() {
		let generator = ajax();
		//使用next()来让ajax函数执行第一次运行，返回Promise
		let step = generator.next();
		//value是Promise的一个实例
		step.value.then(function(d) {
			//不是新数据时，再次请求
			if(d.code != 0) {
				setTimeout(function() {
					console.info('wait');//若{code:1}，则不断轮询
					pull();
				},1000)
			}else {
				console.info(d);//Object {code: 0}
			}
		})
	}

	pull();
}
```







