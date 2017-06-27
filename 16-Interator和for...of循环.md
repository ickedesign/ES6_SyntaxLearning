### Iterator和for...of循环

什么是`Iterator`接口,`Iterator`的基本用法，`for...of`

**概念：**JavaScript原有的表示“集合”的数据结构，主要是数组（`Array`）和对象（`Object`），ES6又添加了`Map`和`Set`。这样就有了四种数据集合，用户还可以组合使用它们，定义自己的数据结构，比如数组的成员是`Map`，`Map`的成员是对象。这样就需要一种统一的接口机制，来处理所有不同的数据结构。
遍历器（`Iterator`）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署`Iterator`接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

`for..of`循环的过程就是不断调用`Iterator`

``` js
{
	//调用Symbol.iterator这个接口	
	let arr = ['hello','world'];
	let map = arr[Symbol.iterator]();
	console.log(map.next());
	console.log(map.next());
	console.log(map.next());
	//Object {value: "hello", done: false}
	//Object {value: "world", done: false}
	//Object {value: undefined, done: true}
}

{
	//自定义Interator接口
	//object无内置Interator接口
	let obj = {
		start: [1,3,2],
		end: [7,9,8],
		//声明一个iterator接口方法
		[Symbol.iterator](){
			let self = this;
			let index = 0;
			let arr = self.start.concat(self.end);
			let len = arr.length;
			return {
				next() {
		    	//先遍历start，再遍历end
		    	if(index < len) {
		    		return {
		    			value: arr[index++],
		    			done: false
		    		}
		    	}else {
		    		return {
		    			value: arr[index++],
		    			done: true
		    		}
		    	}
				}
			}
		}
	}

	//使用for..of
	for(let key of obj) {
		console.log(key);
		//1
		//3
		//2
		//7
		//9
		//8
	}
}

{
	//for...of
	let arr = ['hello','world'];
	for(let value of arr) {
		console.log('value',value);
		//value hello
		//value world
	}
}
```







