### let  

- let的作用域
 - {}包起来的是块作用域,i脱离了块作用域，导致了引用错误
 - ES6强制开启了严格模式：变量未声明不能引用，否则会报引用错误，而不是报undefined错误 
	``` js
	function test(){
	  for(let i = 0;i < 3;i++) {
	    console.log(i); //0,1,2
	  }
	  	console.log(i); //"i is not defined"
	}
	```

	``` js
	function test(){
	  for(var i = 0;i < 3;i++) {
	    console.log(i); //0,1,2
	  }
	  	console.log(i); //3
	}
	```



- let定义变量
 - let不能重复定义变量，无法正常编译
	``` js 
	let a = 1;
	let a = 2; //报错
	```

	

### const

- const也有块作用域的特点
- const定义变量
 - const用来定义常量，只读不能改 
	``` js
	const PI = 3.1415926;
	PI = 8; //报错SyntaxError
	```
 - const声明时必须赋值 
	``` js
	const PI;
	PI = 8; //报错unexpected token编译不完整
	```
 - JSON对象是引用类型，指向对象存储的指针，指针不变，对象本身可变
	``` js
	const k = {
	  a: 1
	}
	k.b = 3;//编译成功
	```

