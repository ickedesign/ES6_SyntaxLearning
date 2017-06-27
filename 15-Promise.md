### 15-Promise

什么是异步，Promise的作用，Promise的基本用法

异步：A执行完就执行B，可使用回调，事件触发或Promise来实现

``` js
{
	//ES5中的回调
	let ajax = function(callback) {
		console.log('执行');
		setTimeout(function() {
			callback && callback.call();
		}, 1000);
	};
	ajax(function() {
		console.log('timeout1');
		//先输出“执行”,一秒钟后输出timeout1
	})
	//如果有多个回调，那么这种方法会变得复杂，不易维护
}

{
	//使用Promise
	//不考虑阻塞的情况
	let ajax = function() {
		console.log('执行2');
		//resolve执行,reject拒绝
		return new Promise(function(resolve,reject){
			setTimeout(function() {
				resolve();
			}, 1000);
		})
	}

	//ajax().then()中的一个参数表示resolve
	ajax().then(function() {
		console.log('promise','timeout2');
		//先输出“执行2”，一秒后，输出promise timeout2
	})
}

{
	//使用Promise来执行多次异步操作
	let ajax = function() {
		console.log('执行3');
		//resolve执行,reject拒绝
		return new Promise(function(resolve,reject) {
			setTimeout(function() {
				resolve();
			}, 1000);
		})
	}

	ajax()
		.then(function() {
		console.log('timeout3');
		return new Promise(function(resolve,reject) {
			setTimeout(function() {
				resolve();
			}, 1000);			
		})
	})
		.then(function(){
		console.log('timeout4');
		//先输出“执行3”，一秒后输出timeout3，两秒后输出timeout4
	})
}

{
	//捕获Promise中的异常错误
	//catch方法的使用
	let ajax = function (num) {
		console.log('执行4');
		return new Promise(function(resolve,reject){
			if(num > 5) {
				resolve();
			}else {
				throw new Error('出错了');
			}
		})
	}

	ajax(6).then(function() {
		console.log('log',6);//log 6
	}).catch(function(err) {
		console.log('catch',err);//若num为3，则输出catch Error: 出错了
	})
}

{
	//Promise.all()方法的使用，把多个promise实例当做一个promise实例
	//所有图片加载完再添加到页面，提高用户体验
	function loadImg(src) {
		return new Promise((resolve,reject) => {
			//以异步的方式来加载图片
			let img = document.createElement('img');
			img.src = src;
			img.onload = function() {
				resolve(img);
			}
			img.onerror = function(err) {
				reject(err);
			}
		})
	}

	function showImgs(imgs) {
		imgs.forEach(function(img) {
			document.body.appendChild(img);
		})
	}

	//只有所有的Promise完成加载后，才调用这个新的Promise对象
	Promise.all([
    loadImg('http://i4.buimg.com/567571/df1ef0720bea6832.png'),
    loadImg('http://i4.buimg.com/567751/2b07ee25b08930ba.png'),
    loadImg('http://i2.muimg.com/567751/5eb8190d6b2a1c9c.png')
	]).then(showImgs);
}

{
	//Promise的race方法的使用
	//有一个图片加载完就添加到页面
	function loadImg(src) {
		return new Promise((resolve,reject) => {
			//以异步的方式来加载图片
			let img = document.createElement('img');
			img.src = src;
			img.onload = function() {
				resolve(img);
			}
			img.onerror = function(err) {
				reject(err);
			}
		})
	}

	function showImgs(img) {
		let p = document.createElement('p');
		p.appendChild(img);
		document.body.appendChild(p);
	}

	//有一个实例发生改变，则Promise发生改变
	Promise.race([
    loadImg('http://i4.buimg.com/567571/df1ef0720bea6832.png'),
    loadImg('http://i4.buimg.com/567751/2b07ee25b08930ba.png'),
    loadImg('http://i2.muimg.com/567751/5eb8190d6b2a1c9c.png')
	]).then(showImgs);	
}
```


