### 10-Symbol的用法

ES6新增的数据类型：`Symbol`，`Symbol`的概念，`Symbol`的作用
`Symbol`的概念：声明一个独一无二的变量

``` js
{
	//声明，不需要new
	let a1 = Symbol();
	let a2 = Symbol();
	console.log(a1 === a2);//false

	//Symbol.for也可以生成独一无二的值
	//对于k值，如果之前已存在该k值，则调用该k值，否则调用Symbol
	let a3 = Symbol.for('a3');
	let a4 = Symbol.for('a3');
	console.log(a3 === a4);//true
}

{
	//Symbol的作用
	let a1 = Symbol.for('abc');
	let obj = {
		[a1]: '123',
		'abc': 345,
		'c': 456
	}
	console.log('obj',obj);
	//obj Object {abc: 345, c: 456, Symbol(abc): "123"}

	//使用常规的遍历，拿不到Symbol()属性
	//引入babel-polyfill
	for(let [key,value] of Object.entries(obj)) {
		console.log('let of',key,value);
		//let of abc 345
		//let of c 456
	}

	//取出Symbol的key对应的key,value
	//使用getOwnPropertySymbols这个方法，得到的是对象的数组，然后使用forEach方法
	Object.getOwnPropertySymbols(obj).forEach(function(item){
		console.log(item,obj[item]);//Symbol(abc) "123"
	})

	//取出对象的所有key，value
	//Reflect对象的ownKeys方法
	Reflect.ownKeys(obj).forEach(function(item) {
		console.log('ownKeys',item,obj[item]);
		//ownKeys abc 345
		//ownKeys c 456
		//ownKeys Symbol(abc) 123
	})
}
```



