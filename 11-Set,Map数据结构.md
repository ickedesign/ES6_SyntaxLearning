### 11-Set,Map数据结构

`Set`的用法，`WeakSet`的用法，`Map`的用法，`Weakmap`的用法

```js
{
	//Set的普通定义方法
	//Set的数据类型是个集合
	//往Set中增加元素使用add方法
	let list = new Set();
	list.add(5);
	list.add(7);

	//size类似数组中的length
	console.log('size',list.size);//size 2
}

{
	//另一种Set的定义方法
	let arr = [1,2,3,4,5];
	//该list现在表示的是Set数据类型
	let list = new Set(arr);

	console.log('size',list.size);//5
}

{
	//Set数据类型中的元素不能重复
	let list = new Set();
	list.add(1);
	list.add(2);
	list.add(1);

	console.log('list',list);//list Set {1,2}

	//使用该特性，可用于去重
	let arr = [1,2,3,1,2,'2'];
	let list2 = new Set(arr);

	console.log('unique',list2);//unique Set {1, 2, 3,"2"}
}

{
	//Set实例的几个方法
	let arr = ['add','delete','clear','has'];
	let list = new Set(arr);

	console.log('has',list.has('add'));//true
	console.log('delete',list.delete('add'),list);//delete true Set {"delete", "clear", "has"}
	list.clear();
	console.log('list',list);//list Set {}
}

{
	//Set实例的遍历，读取
	let arr = ['add','delete','clear','has'];
	let list = new Set(arr);

	//key,value
	for(let key of list.keys()){
		console.log('keys',key);
		//keys add
		//keys delete
		//keys clear
		//keys has
	}

	for(let value of list.values()){
		console.log('values',value);
		//values add
		//values delete
		//values clear
		//values has	
	}	

	for(let value of list){
		console.log('values',value);
		//values add
		//values delete
		//values clear
		//values has	
	}	

	//forEach
	for(let [key,value] of list.entries()){
		console.log('entries',key,value);
		//entries add add
		//entries delete delete
		//entries clear clear
		//entries has	has
	}	

	list.forEach(function(item){
		console.log(item);
		//add
		//delete
		//clear
		//has
	})	
}

{
	//WeakSet
	//WeakSet只支持对象，不与垃圾回收挂钩
	//添加一个对象，只引用地址，也不会检测地址是否被垃圾回收掉了
	let weakList = new WeakSet();

	let arg = {};

	weakList.add(arg);
	console.log('weakList',weakList);//weakList WeakSet {Object {}}
	//weakList.add(2);//报错，只支持对象
	//k值必须是对象，没有size属性，不能使用clear方法，不能遍历
}

{
	//map
	//第一种定义方式
	let map = new Map();
	let arr = ['123'];

	//map添加元素使用set(k,value)，不是add
	//k可以是所有数据类型
	map.set(arr,456);
	console.log('map',map,map.get(arr));
	//map Map {["123"] => 456} 456
}

{
	//第二种定义方式
	let map = new Map([['a',123],['b',456]]);
	console.log('map args',map);
	//map args Map {"a" => 123, "b" => 456}
	
	//属性和方法
	console.log('size',map.size);//size 2
	console.log('get',map.get('a'));//get 123
	console.log('delete',map.delete('a'),map);//delete true Map {"b" => 456}
	//也有clear方法
	//遍历和Set类似，也有keys/values/entries,forEach的遍历方法
}

{
	//WeakMap
	//k值必须是对象，没有size属性，不能使用clear方法，不能遍历
	let weakmap = new WeakMap();

	let o = {};
	weakmap.set(o,123);
	weakmap.set({a: 1,b: 2},345);
	console.log(weakmap.get(o));//123
	console.log(weakmap);//WeakMap {Object {} => 123, Object {a: 1, b: 2} => 345}
}
```



