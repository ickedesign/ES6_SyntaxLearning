### 13-Ploxy和Reflect

概念和适用场景

``` js
{
	//供应商
	let obj = {
		time: '2017-03-11',
		name: 'net',
		_r: 123
	};

	//代理商
	let monitor = new Proxy(obj,{
		//拦截对象属性的读取
		get(target,key) {
			//把所有的值2017替换成2018
			return target[key].replace('2017','2018')
		},
		//拦截对象设置属性
		//target指obj对象,key指obj的属性,value指的是值
		set(target,key,value) {
			if(key === 'name') {
				return target[key] = value;
			}else {
				return target[key];
			}
		},
		//拦截Key in object操作
		has(target,key) {
			if(key === 'name') {
				return target[key]
			}else {
				return false;
			}
		},
		//拦截delete
		deleteProperty(target,key) {
			//如果属性中包含key则允许删除
			if(key.indexOf('_') > -1){
				delete target[key];
				return true;
			}else {
				return target[key];
			}
		},
		//拦截Object.keys,Object.getOwnPropertySymbols,Object.getOwnPropertyNames
		ownKeys(target) {
			//Object.keys(target)把原始对象的所有属性Keys取出
			//filter(过滤)
			return Object.keys(target).filter(item => item != 'time');
		}
	});

	console.log('get',monitor.time);//get 2018-03-11

	//用户访问,读取修改monitor的属性
	monitor.time = '2018';
	monitor.name = 'HoneyJin';
	console.log('set',monitor.time,monitor.name);
	//set 2018-03-11 HoneyJin

	console.log('has','name' in monitor,'time' in monitor);
	//has true false
	
	/*
	 *delete monitor.time;
	 *console.log('delete',monitor);
	 * //delete Object {time: "2017-03-11", name: "HoneyJin", _r: 123}

	 *delete monitor._r;
	 *console.log('delete',monitor);	
	 * //delete Object {time: "2017-03-11", name: "HoneyJin"}
	 */

	 //读取不到time属性
	 console.log('ownkeys',Object.keys(monitor));//ownkeys ["name", "_r"]

}

{
	//Reflect和proxy的方法差不多，只是调用方式不同
	let obj = {
		time: '2017-03-11',
		name: 'net',
		_r: 123
	};

	console.log('reflect get',Reflect.get(obj,'time'));
	//reflect get 2017-03-11
	
	Reflect.set(obj,'name','Hello Jin')
	console.log('reflect set',Reflect.get(obj,'name'));
	//reflect set Hello Jin

	console.log('has',Reflect.has(obj,'name'));//has true	
}

{
	//适用场景
	//数据的校验，和业务解耦
	//这种做法维护性高，健壮性，整洁度，复用性好
	function validator(target,validator) {
		return new Proxy(target,{
			_validator: validator,//保存配置选项
			set(target,key,value,proxy) {
				//判断是否存在key属性
				if(target.hasOwnProperty(key)){
					let va = this._validator[key];
					//判断是否存在key对应的value值
					if(!!va(value)) {
						return Reflect.set(target,key,value,proxy);
					}else {
						throw Error(`不能设置${key}到${value}`);
					}
				}else {
					throw Error(`${key} 不存在`)
				}
			}
		})
	}

	//设置校验的条件
	const personValidators = {
		//字符串
		name(val) {
			return typeof val === 'string';
		},
		age(val) {
			return typeof val === 'number' && val > 18;
		}
	}

	//Person类，构造函数
	class Person {
		constructor(name,age) {
			this.name = name;
			this.age = age;
			//返回的proxy对象拦截了Person对象
			//this指的是Person的实例
			return validator(this,personValidators)
		}
	}

	const person = new Person('lilei',30);

	console.info(person);//Object {name: "lilei", age: 30}

	//person.name = 48;//Uncaught Error: 不能设置name到48

	person.name = 'han mei mei';
	console.info(person);//Object {name: "han mei mei", age: 30}
}
```



