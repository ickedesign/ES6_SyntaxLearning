### 12-Map,Set与数组和对象的比较

数据结构的横向对比，增查改删

**总结：**优先考虑Map和Set；能使用map就不使用array；若强调存储的唯一性则使用Set

```js
{
	//Map和数组的对比
	let map =new Map();
	let array = [];
	//增
	//map.set(k,value);
	map.set('t',1);
	//push或者unshift
	array.push({t:1});

	console.info('map-array',map,array);
	//map-array Map {"t" => 1} [{t:1}]

	//查
	let map_exist = map.has('t');
	let array_exist = array.find(item=>item.t);

	console.info('map-array-exist',map_exist,array_exist);
	//map-array-exist true {t:1}
	
	//改
	map.set('t',2);
	array.forEach(item => item.t?item.t = 2:'');
	console.info('map-array-modify',map,array);
	//map-array-modify Map {"t" => 2} [{t:2}]

	//删
	map.delete('t');
	//先找索引,ES5中提供了findIndex方法
	let index = array.findIndex(item => item.t);
	array.splice(index,1);
	console.info('map-array-empty',map,array);
	//map-array-empty Map {} []
}

{
	//Set和数组的对比
	let set = new Set();
	let array = [];

	// 增
	set.add({t:1});
	array.push({t:1});

	console.info('set-array',set,array);
	//set-array Set {{t: 1}} [{t:1}]

	//查
	let item1 = {t:1};
	let set2 = new Set();
	set2.add(item1);
	let set_exist = set2.has(item1);//直接在has()中写入{t:1},出现的是false
	let array_exist = array.find(item=>item.t);

	console.info('map-array-exist',set2,set_exist,array_exist);	
	//map-array-exist Set {Object {t: 1}} true Object {t: 1}

	//改
	set.forEach(item => item.t?item.t =2:'');
	array.forEach(item => item.t?item.t = 2:'');
	console.info('set-array-modify',set,array);
	//set-array-modify Set {t:2} [{t:2}]

	//删
	//Set先找到值再删除
	set.forEach(item => item.t ? set.delete(item) : '');
	//Array先找到索引再删除
	let index = array.findIndex(item => item.t);
	array.splice(index,1);
	console.info('set-array-empty',set,array);
	//set-array-empty Set {} []
}

{
	//Map,Set和Object的对比
	let item = {t:1};
	let map = new Map();
	let set = new Set();
	let obj = {};

	//增
	map.set('t',1);
	set.add(item);
	obj['t'] = 1;

	console.info('map-set-obj',obj,map,set);
	//map-set-obj Object {t: 1} Map {"t" => 1} Set {Object {t: 1}}
	
	//查
	console.info({
		map_exist: map.has('t'),
		set_exist: set.has(item),
		obj_exist: 't' in obj
	})
	//Object {map_exist: true, set_exist: true, obj_exist: true}

	//改
	map.set('t',2);
	item.t = 2;
	obj['t'] = 2;
	console.info('map-set-obj-modify',obj,map,set);
	//map-set-obj-modify Object {t: 2} Map {"t" => 2} Set {Object {t: 2}}

	//删
	map.delete('t');
	set.delete(item);
	delete obj['t'];
	console.info('map-set-obj-delete',obj,map,set);
	//map-set-obj-delete Object {} Map {} Set {}
}


```



