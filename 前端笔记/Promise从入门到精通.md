# Promise从入门到精通

## 1. 准备

## 2. Promise的理解和使用

### 2.1 Promise是什么

#### 2.1.1 理解

1. 抽象表达：
   1. Promise是一门新的技术（ES6的规范）
   2. Promise是JavaScript中进行异步编程的新解决方案

> 以前的解决方案是使用纯回调函数的方式。
>
> 几种异步编程：
>
> * fs文件操作
>
>   ```javascript
>   require('fs').readFile('./index.html', (err, data) => {})
>   ```
>
> * 数据库操作
>
> * AJAX请求
>
>   ```javascript
>   $.get('/server', (data) => {})
>   ```
>
> * 定时器
>
>   ```javascript
>   setTimeout(() => {}, 1000)
>   ```

2. 具体表达
   1. 从语法上来说：Promise是一个构造函数
   2. 从功能上来说：promise对象用来封装一个异步操作并可以获取其成功/失败的结果值

#### 2.1.2 Promise的状态改变

> Promise 的状态
>
> 实例对象中的一个属性 [PromiseState]
>
> * pending 未决定的
> * fulfilled/resolved 成功
> * rejected 失败
>
> Promise 对象的值
>
> 实例对象中的另一个属性 [PromiseResult]
>
> * 保存的是异步任务成功或失败的结果
> * 只有resolve和reject能够对这个值进行操作

1. `pending`变为`fulfilled`
2. `pending`变为`rejected`

> 只有这两种状态的改变，且一个promise对象的状态只能改变一次。无论变为成功还是失败，都会有一个结果数据。

#### 2.1.3 Promise的基本流程

![Promise的基本流程](F:\技术资料\前端面试知识点\Promise.jpg)

### 2.2 为什么要用Promise？

#### 2.2.1 指定回调函数的方式更加灵活

1. 以前的旧方法：必须在启动异步任务前指定好异步处理方法
2. Promise：启动异步任务 => 返回promise对象 => 给promise对象绑定回调函数（甚至可以在异步任务结束后指定多个回调函数）

#### 2.2.2 支持链式调用，可以解决回调地狱问题

1. 什么是回调地狱？

   回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调执行条件。以下代码就是一个回调地狱。

   ```javascript
   asyncFunc1(opt, (...args1) => {
       asyncFunc2(opt, (...args2) => {
           asyncFunc3(opt, (...args3) => {
               asyncFunc4(opt, (...args4) => {
                   // some operation
               })
           })
       })
   })
   ```

2. 回调地狱的缺点？

   * 不便于阅读
   * 不便于异常处理


> Promise 初体验
>
> 实现：点击按钮，1秒后显示是否中奖（中奖概率30%），若中奖，弹出“恭喜恭喜，奖品为10万元劳斯莱斯优惠券”；若未中奖，弹出“再接再厉”。无论中奖与否，都提示抽取的号码。
>
> ```javascript
> // 生成随机数
> function rand(m, n) {
> 	return Math.ceil(Math.random() * (n - m + 1)) + m - 1;
> }
> 
> // 获取元素对象
> const btn = document.querySelector('#btn');
> // 绑定单击事件
> btn.addEventListener('click', function() {
>     // 使用Promise实现
>     // Promise在实例化时需要接收一个参数，而这个参数是函数类型的值，它包括两个参数(resolve, reject)
> 	// resolve 解决 reject 拒绝 它们均为函数类型的数据
>     // Promise能够包裹异步操作。
> 	const p = new Promise((resolve, reject) => {
> 		setTimeout(() => {
> 			// 获取从1~100的一个随机数
> 			let n = rand(1, 100);
> 			if (n <= 30) {
> 				resolve(n); // 调用resolve()函数后可以将promise对象的状态设置为'成功'
> 			} else {
> 				reject(n);  // 调用resolve()函数后可以将promise对象的状态设置为'失败'
> 			}
> 		}, 1000)
> 	});
> 				
> 	// 调用then()方法
> 	// then()方法需要接受两个参数，而这两个参数都是函数类型的值
>     // 第一个函数是执行成功时的回调函数，参数通常为 value 值
> 	// 第二个函数是执行失败时的回调函数，参数通常为 reason 原因
> 	p.then((value) => {
> 		alert(`恭喜恭喜，奖品为10万元劳斯莱斯优惠券，您的中奖号码为${value}`)
> 	}, (reason) => {
> 		alert(`再接再厉，您的号码为${reason}`)
> 	})
> })
> ```

### 2.3 如何使用Promise

#### 2.3.1 API

1. Promise构造函数：`Promise(executor) {}`
   1. `executor`函数：执行器函数 `(resolve, reject) => {}`
   2. `resolve`函数：内部定义成功时我们调用的函数 `value => {}`
   3. `reject`函数：内部定义失败时我们调用的函数 `reason => {}`

> `executor`执行器函数会在Promise内部立即同步调用，异步操作在执行器中执行。

2. `Promise.prototype.then()`方法：`(onResolved, onRejected) => {}`
   1. `onResolved`函数：成功的回调函数 `value => {}`
   2. `onRejected`函数：失败的回调函数 `reason => {}`

> 指定用于得到成功value的成功回调和用于得到失败reason的失败回调返回一个新的promise对象。

3. `Promise.prototype.catch()`方法：`(onRejected) => {}`
   * `onRejected`函数：失败的回调函数 `reason => {}`

4. `Promise.resolve()`方法：`(value) => {}`

   * `value`：成功的数据或promise对象

   > 如果传入的参数为非promise类型的对象，则返回的结果为成功的promise对象；如果传入的参数为Promise对象，则参数的结果决定了返回的结果。

5. `Promise.reject()`方法：`(reason) => {}`

   * `reason`：失败的原因

   > 无论传入的是数字、字符串还是成功的Promise对象，返回状态永远是失败的，返回结果为传入的参数（即使它是一个成功的Promise对象）。

6. `Promise.all()`方法：`(promises) => {}`

   * `promises`：包含n个promises的数组
   * 返回的结果
     * 成功：成功的Promise对象，结果为所有成功Promise的结果值数组
     * 失败：失败的Promise对象，结果为失败的Promise的结果

   > 返回一个新的promise，只有所有的promise都成功才算成功，只要有一个失败了就直接失败。

7. `Promise.race()`方法：`(promises) => {}`

   * `promises`：包含n个promises的数组。
   * 结果由第一个改变状态的Promise决定。

#### 2.3.2 Promise的几个关键问题

##### 1. 如何改变promise的状态？

1. `resolve(value)`：如果当前是pending就会变为resolved
2. `reject(reason)`：如果当前是pending就会变为rejected
3. 抛出异常：如果当前是pending就会变为rejected

```javascript
let p = new Promise((resolve, reject) => {
    // 1. resolve函数
    // resolve('ok')  // pending -> fulfilled
    // 2. reject函数
    // reject('error')  // pending -> rejected
    // 3. 抛出异常
    throw '出问题了';
})
```

##### 2. 一个promise指定多个成功/失败的回调函数，都会调用吗？

当promise改变为对应状态时则都会调用。

```javascript
let p = new Promise((resolve, reject) => {
    resolve('ok')
})

p.then(value => {
    console.log(value)
})

p.then(value => {
    alert(value)
})
```

##### 3. 改变promise状态和**指定**回调函数谁先谁后？

1. 都有可能，正常情况下是先指定回调再改变状态，但也可以先改变状态再**指定**回调
2. 如何先改状态再指定回调？
   1. 在执行器中直接调用`resolve()`/`rejected()`
   2. 延迟更长时间才调用`then()`
3. 什么时候才能得到数据？
   1. 如果先指定的回调，那当状态发生改变时，回调函数就会调用，得到数据
   2. 如果先改变的状态，那当指定回调时，回调函数就会调用，得到数据

```javascript
// 先改变状态，再指定回调
let p = new Promise((resolve, reject) => {
    resolve('ok');
})

p.then(value => {
    console.log(value)
}, reason => {})
```

```javascript
// 先指定回调，再改变状态，执行回调的时机在改变状态之后
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok');
    }, 1000)
})

p.then(value => {
    console.log(value)
}, reason => {})
```



##### 4. `Promise.then()`返回的新Promise的结果状态由什么决定？

1. 简单表达：由`then()`指定的回调函数执行的结果决定
2. 详细表达：
   1. 如果抛出异常，新Promise变为rejected，reason为抛出的异常
   2. 如果返回的是非Promise的任意值，新Promise变为resolved，value为返回的值
   3. 如果返回的是另一个新Promise，此Promise的结果就会成为新Promise的结果

```javascript
let result = p.then(value => {
    // console.log(value)
    // 1. 抛出异常
    // throw '出了问题'; // rejected
    // 2. 非Promise的任意值
    // return 521;
    // 3. 返回结果是Promise对象
    return new Promise((resolve, reject) => {
        reject('error')
    })
}, reason => {
    console.warn(reason)
})
```

##### 5. `Promise`如何串联多个操作任务？

1. `Promise`的`then()`方法返回一个新的promise，可以看成`then()`的链式调用

2. 通过`then()`的链式调用串联多个同步/异步任务

   ```javascript
   let p = new Promise((resolve, reject) => {
   	setTimeout(() => {
           resolve('OK');
   	}, 1000);
   });
         
   p.then(value => {
       return new Promise((resolve, reject) => {
           resolve('success');
       });
   }).then(value => {
       console.log(value)  // 打印 success
   }).then(value => {
       console.log(value)  // 打印 undefined
   })
   ```

##### 6. `Promise`异常穿透

1. 当使用promise的then链式调用时，可以在最后指定失败的回调

2. 前面任何操作出了异常，都会传到最后失败的回调中处理

   ```javascript
   let p = new Promise((resolve, reject) => {
   	setTimeout(() => {
           reject('Err');
   	}, 1000);
   });
   
   p.then(value => {
       throw '失败啦';
   }).then(value => {
       console.log('111')
   }).then(value => {
       console.log('222')
   }).catch(reason => {
       console.warn(reason)
   })
   
   // 返回结果：失败啦
   ```

##### 7. 中断`Promise`链

1. 当使用promise的then链式调用时，在中间中断，不再调用后面的回调函数
2. **有且只有一种办法**：在回调函数中返回一个pending状态的promise对象。因为状态没有改变，后边的回调函数都不会执行。

```javascript
p.then(value => {
    console.log('111')
    return new Promise(() => {});
}).then(value => {
    console.log('222')
}).then(value => {
    console.log('333')
}).catch(reason => {
    console.warn(reason)
})

// 返回结果：111
```

## 3. 自定义（手写）Promise

## 4. `async`与`await`

### 4.1 `async`函数

1. 函数的返回值为promise对象
2. promise对象的结果有`async`函数执行的返回值决定

```javascript
async function main() {
	// 1. 如果返回值是一个非promise类型的数据
	// return 521;
	// 2. 如果返回的是一个Promise对象
	// return new Promise((resolve, reject) => {
	// 	resolve('success')
	// })
	// 3. 抛出异常
	throw 'oh no';
}
```

> `async`函数的返回规则与`then()`一致。

### 4.2 `await`表达式

1. `await`右侧的表达式一般为Promise对象，但也可以是其他的值
2. 如果表达式是promise对象，`await`返回的是promise成功的值
3. 如果表达式是其他值，直接将此值作为`await`的返回值

### 4.3 注意

1. `await`必须写在`async`函数中，但`async`函数中可以没有`await`
2. 如果`await`的Promise失败了，就会抛出异常，需要通过`try...catch`捕获处理

```javascript
async function main() {
	let p = new Promise((resolve, reject) => {
		resolve('OK')
	})

	let p1 = new Promise((resolve, reject) => {
		reject('Error')
	})
	// 1. 右侧为promise的情况
	// let res = await p;
	// 2. 右侧为其他类型的数据
	// let res2 = await 20;
	// 3. 如果Promise是失败的状态
	try {
		let res3 = await p1;
	} catch (e) {
		//TODO handle the exception
		console.log(e);
	}
}
```

### 案例：文件操作

读取三个文件并将它们的内容拼接输出。

* 回调函数实现

  ```javascript
  const fs = require('fs');
  
  fs.readFile('./resource/1.txt', (err, data1) => {
  	if (err) throw err;
  	fs.readFile('./resource/2.txt', (err, data2) => {
  		if (err) throw err;
  		fs.readFile('./resource/3.txt', (err, data3) => {
  			if (err) throw err;
  			console.log(data1 + data2 + data3)
  		})
  	})
  })
  ```

* `async`与`await`结合

  ```javascript
  const fs = require('fs');
  const util = require('util');
  const mineReadFile = util.promisify(fs.readFile);
  
  async function main() {
  	try {
  		// 读取每个文件的内容
  		let data1 = await mineReadFile('./resource/1.txt')
  		let data2 = await mineReadFile('./resource/2.txt')
  		let data3 = await mineReadFile('./resource/3.txt')
  		console.log(data1 + data2 + data3)
  	} catch (error) {
  		console.log(error)
  	}
  }
  
  main()
  ```

