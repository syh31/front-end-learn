# ECMAScript 6

## 1. `let`和`const`

## 2. 解构赋值与模板字符串

## 3. 简化对象写法

## 4. 箭头函数

箭头函数中 this 是静态的，始终指向函数声明时所在作用域下的 this 值。

```javascript
function getName() {
	console.log(this.name)
}

let getName2 = () => {
	console.log(this.name)
}

// 设置 window 对象的 name 属性
window.name = '尚硅谷'
const school = {
	name: 'ATGUIGU'
}

// 直接调用
getName() // 尚硅谷
getName2() // 尚硅谷

// call 方法调用
getName.call(school) // ATGUIGU
getName2.call(school) // 尚硅谷
```

## 5. 函数参数默认值

## 6. `rest`参数

## 7. 扩展运算符

## 8. `Symbol`类型

## 9. 迭代器

### 手写`Iterator`迭代器

有一个`group`对象，包括字符串`name`和数组`stus`，当使用`for...of...`遍历时，去遍历`stus`中的值。

```javascript
const group = {
    name: '终极一班',
    stus: ['xiaoming', 'xiaoning', 'xiaotian', 'knight'],
    [Symbol.iterator]() {
        // 索引变量
        let index = 0
        let _this = this
        return {
            next: function() {
                if (index < _this.stus.length) {
                    const result = {
                        value: _this.stus[index],
                        done: false
                    }
                    index++
                    // 返回结果
                    return result
                } else {
                    return { value: undefined, done: true }
                }
            }
        }
    }
}
```

## 10. `Generator`生成器函数

`Generator`生成器函数使用`*`定义

在生成器函数中可以使用`yield`关键字分割代码

```javascript
function * gen() {
    console.log(111)
    yield '两只老虎爱跳舞'
    console.log(222)
    yield '小兔子乖乖拔萝卜'
    console.log(333)
    yield '我和小鸭子学走路'
    console.log(444)
}

let iterator = gen()
iterator.next()  // 111
iterator.next()  // 222
iterator.next()  // 333
iterator.next()  // 444
```

使用`for...of...`遍历生成器函数

```javascript
function * gen() {
    yield '两只老虎爱跳舞'
    yield '小兔子乖乖拔萝卜'
    yield '我和小鸭子学走路'
}

for(let v of gen()) {
	console.log(v)
}
```

打印结果：

> 两只老虎爱跳舞
> 小兔子乖乖拔萝卜
> 我和小鸭子学走路

### 参数传递

`Generator`函数可以接收参数；`next`方法可以传入实参,参数作为之前`yield`的返回结果

```javascript
function * gen(arg) {
	console.log(arg)
    let one = yield 111
	console.log(one)
	let two = yield 222
	console.log(two)
	let three = yield 333
	console.log(three)
}

// 执行迭代器对象
let iterator = gen('AAA')
console.log(iterator.next())
// next方法可以传入实参,参数作为第一个yield的返回结果
console.log(iterator.next('BBB'))
console.log(iterator.next('CCC'))
console.log(iterator.next('DDD'))
```

控制台输出结果：

>AAA
>{ value: 111, done: false }
>BBB
>{ value: 222, done: false }
>CCC
>{ value: 333, done: false }
>DDD
>{ value: undefined, done: true }

### 异步编程

#### 实例一

需求：一秒后控制台输出111，两秒后输出222，三秒后输出333

**回调地狱**

```javascript
setTimeout(() => {
	console.log(111)
	setTimeout(() => {
		console.log(222)
		setTimeout(() => {
			console.log(333)
		}, 3000)
	}, 2000)
}, 1000)
```

**生成器函数**

```javascript
function one() {
	setTimeout(() => {
		console.log(111)
		iterator.next()
	}, 1000)
}

function two() {
	setTimeout(() => {
		console.log(222)
		iterator.next()
	}, 2000)
}

function three() {
	setTimeout(() => {
		console.log(333)
		iterator.next()
	}, 3000)
}

function * gen() {
	yield one()
	yield two()
	yield three()
}

// 调用生成器函数
let iterator = gen()
iterator.next()
```

#### 实例二

模拟获取数据：用户数据->订单数据->商品数据

```javascript
function getUsers() {
	setTimeout(() => {
		let data = '用户数据'
		// 调用 next 方法，并且将数据传入
		iterator.next(data)
	}, 1000)
}

function getOrders() {
	setTimeout(() => {
		let data = '订单数据'
		iterator.next(data)
	}, 1000)
}

function getGoods() {
	setTimeout(() => {
		let data = '商品数据'
		iterator.next(data)
	}, 1000)
}

function * gen() {
	let users = yield getUsers()
	console.log(users)
	let orders = yield getOrders()
	console.log(orders)
	let goods = yield getGoods()
	console.log(goods)
}

let iterator = gen()
iterator.next()
```

## 11. Promise

## 12. `Set`集合

* 添加`add()`
* 删除`delete()`
* 检测`has()`
* 清空`clear()`

### 数组去重

```javascript
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1]
let result = [...new Set(arr)]
```

### 交集

```javascript
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1]
let arr2 = [4, 5, 6, 5, 6]
let result = [...new Set(arr)].filter(item => new Set(arr2).has(item))
```

### 并集

```javascript
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1]
let arr2 = [4, 5, 6, 5, 6]
let union = [...new Set([...arr, ...arr2])]
```

### 差集

```javascript
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1]
let arr2 = [4, 5, 6, 5, 6]
let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)))
```

## 13. `Map`

## 14. `class`类

### 类的静态成员

静态属性：`class` 本身的属性，即直接定义在类内部的属性（ `Class.propname` ），不需要实例化。 ES6 中规定，`class `内部只有静态方法，没有静态属性。

```javascript
function Phone() {
	
}
			
/* 
	以下方式定义的属性是属于函数对象的而不属于实例对象
	这样的属性在类中我们称为静态成员
*/
Phone.name = '手机'
Phone.change = function() {
	console.log('我可以改变世界')
}
			
let p = new Phone()  // undefined
console.log(p.change())  // Uncaught TypeError: p.change is not a function		
```

上面方式定义的属性`name`和`change`是属于函数对象`Phone`的，而不属于实例对象`p`，这样的属性在`class`类中我们称之为**静态成员**。

```javascript
class Example {
// 新提案
    static a = 2;
}

// 目前可行写法
Example.b = 2;
```

### 类继承

ES5构造函数对象的原型链继承

```javascript

function Phone(brand, price) {
	this.brand = brand
	this.price = price
}

Phone.prototype.makeCall = function() {
	console.log('我可以打电话')
}

function SmartPhone(brand, price, color, size) {
	Phone.call(this, brand, price)
	this.color = color
	this.size = size
}

// 设置子级构造函数的原型
SmartPhone.prototype = new Phone
SmartPhone.prototype.constructor = SmartPhone

// 声明子类的方法
SmartPhone.prototype.photo = function() {
	console.log('我可以拍照')
}

SmartPhone.prototype.playGame = function() {
	console.log('我可以玩游戏')
}

const huawei = new SmartPhone('华为', 4699, '黑色', '6 Inch')
console.log(huawei)
```

类的继承

```javascript
class Phone {
	constructor(brand, price) {
		this.brand = brand
		this.price = price
	}

	makeCall() {
		console.log('我可以打电话')
	}
}

class SmartPhone extends Phone {
	constructor(brand, price, color, size) {
		super(brand, price)
		this.color = color
		this.size = size
	}

	photo() {
		console.log('我可以拍照')
	}

	playGame() {
		console.log('我可以玩游戏')
	}
}

const xiaomi = new SmartPhone('小米', 2999, '黑色', '6.5 Inch')
console.log(xiaomi)
```

### `class`中`getter`和`setter`的设置

```javascript
class Phone {
	get price() {
		console.log('读取价格属性')
		return 1999
	}

	set price(newValue) {
		console.log('修改价格属性')
	}
}

let s = new Phone()
console.log(s.price)
s.price = 'free'
```

> 读取价格属性
>
> 1999
>
> 修改价格属性

## 15. 数值扩展

### `Number.EPSILON`

`JavaScript`所能表示的最小精度，它的值接近于2.220446049250313e-16

`JavaScript`中会有精度丢失

```javascript
console.log(0.1 + 0.2)  // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3) // false
```

> 计算机中的数字都是以二进制存储的，二进制浮点数表示法并不能精确的表示类似0.1这样 的简单的数字
>
> 如果要计算 0.1 + 0.2 的结果，计算机会先把 0.1 和 0.2 分别转化成二进制，然后相加，最后再把相加得到的结果转为十进制
>
> 但有一些浮点数在转化为二进制时，会出现无限循环 。比如， 十进制的 0.1 转化为二进制，会得到如下结果：
>
> 0.1 => 0.0001 1001 1001 1001…（无限循环）
>
> 0.2 => 0.0011 0011 0011 0011…（无限循环）
>
> 而存储结构中的尾数部分最多只能表示 53 位。为了能表示 0.1，只能模仿十进制进行四舍五入了，但二进制只有 0 和 1 ， 于是变为 0 舍 1 入 。 因此，0.1 在计算机里的二进制表示形式如下：
>
> 0.1 => 0.0001 1001 1001 1001 1001 1001 1001 1001 1001 1001 1001 1001 1001 101
>
> 0.2 => 0.0011 0011 0011 0011 0011 0011 0011 0011 0011 0011 0011 0011 0011 001
>
> 用标准计数法表示如下：
>
> 0.1 => (−1)0 × 2^4 × (1.1001100110011001100110011001100110011001100110011010)2
>
> 0.2 => (−1)0 × 2^3 × (1.1001100110011001100110011001100110011001100110011010)2
>
> 在计算浮点数相加时，需要先进行 “对位”，将较小的指数化为较大的指数，并将小数部分相应右移：
>
> 最终，“0.1 + 0.2” 在计算机里的计算过程如下：
>
> ![img](https://www.zhoulujun.cn/uploadfile/net/2019/0514/20190514202312948884135.png)
>
> 经过上面的计算过程，0.1 + 0.2 得到的结果也可以表示为：
>
> (−1)0 × 2−2 × (1.0011001100110011001100110011001100110011001100110100)2=>.0.30000000000000004
>
> 通过 JS 将这个二进制结果转化为十进制表示：
>
> (-1)**0 \* 2**-2 * (0b10011001100110011001100110011001100110011001100110100 * 2**-52); [//0.30000000000000004](https://0.30000000000000004/)
>
> console.log(0.1 + 0.2) ; // 0.30000000000000004
>
> 这是一个典型的精度丢失案例，从上面的计算过程可以看出，0.1 和 0.2 在转换为二进制时就发生了一次精度丢失，而对于计算后的二进制又有一次精度丢失 。因此，得到的结果是不准确的。

可以通过该属性来解决

```javascript
function equal(a, b) {
	return Math.abs(a - b) < Number.EPSILON
}

console.log(equal(0.1 + 0.2, 0.3))  // true
```

### 二进制和八进制

二进制加‘0b’，八进制加‘0o’，十六进制加‘0x’

### `Number.isFinite`

检测一个数值是否为有限数

```javascript
console.log(Number.isFinite(100))  // true
console.log(Number.isFinite(100 / 0))  // false
console.log(Number.isFinite(Infinity))  // false
```

### `Number.isNaN`

检测一个数值是否为`NaN`

### `Number.parseInt`/`Number.parseFloat`

将字符串转换为`Number`类型

### `Number.isInteger`

判断一个数值是否为整数

### `Math.trunc`

**`Math.trunc()`** 方法会将数字的小数部分去掉，只保留整数部分。

不像 `Math` 的其他三个方法： [`Math.floor()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)、[`Math.ceil()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/ceil)、[`Math.round()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/round) ，`Math.trunc()` 的执行逻辑很简单，仅仅是**删除**掉数字的小数部分和小数点，不管参数是正数还是负数。

传入该方法的参数会被隐式转换成数字类型。

### `Math.sign`

**`Math.sign()`** 函数返回一个数字的符号, 指示数字是正数，负数还是零。

```javascript
console.log(Math.sign(100))  // 1
console.log(Math.sign(0))  // 0
console.log(Math.sign(-50))  // -1
```

## 16. 对象方法扩展

### `Object.is`

**`Object.is()` **方法判断两个值是否为[同一个值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness)。

```javascript
console.log(Object.is(120, 120))  // true
console.log(Object.is(NaN, NaN))  // true
console.log(NaN === NaN)  // false
```

> **NaN**与任何值去做比较结果都是`false`，包括它本身

### `Object.assign`

**`Object.assign()`** 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

当两个对象里面属性相同时，第二个参数会覆盖第一个参数

```javascript
const config1 = {
	host: 'localhost',
	port: '3306',
	name: 'root',
	pass: 'root'
}

const config2 = {
	host: 'http://atguigu.com/',
	port: '33060',
	name: 'atguigu.com',
	pass: 'iloveyou'
}

console.log(Object.assign(config1, config2))
```

> {
> host: 'http://atguigu.com/',
> port: '33060',
> name: 'atguigu.com',
> pass: 'iloveyou'
> }

当两个对象里面属性不同时：

```javascript
const config1 = {
	host: 'localhost',
	port: '3306',
	name: 'root',
	pass: 'root',
	test: 'test'
}

const config2 = {
	host: 'http://atguigu.com/',
	port: '33060',
	name: 'atguigu.com',
	pass: 'iloveyou',
	test2: 'test'
}

console.log(Object.assign(config1, config2))
```

> {
>   host: 'http://atguigu.com/',
>   port: '33060',
>   name: 'atguigu.com',
>   pass: 'iloveyou',
>   test: 'test',
>   test2: 'test'
> }

### `Object.setPrototypeOf`/`Object.getPrototypeOf`

不建议使用。

## 17. 模块化

模块化是指将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来。

### 模块化的优点

1. 防止命名冲突
2. 代码复用
3. 高维护性

### 模块化规范产品
ES6之前的模块化规范有 ：

1. CommonJS => NodeJS、Browserify
2. AMD => requireJS
3. CMD => seaJS

### 暴露方式

#### 1. 分别暴露

m1.js

```javascript
// 分别暴露
export let school = '尚硅谷'

export function teach() {
	console.log('I can teach you.')
}
```

#### 2. 统一暴露

m2.js

```javascript
// 统一暴露
let school = 'atguigu'

function findJob () {
	console.log('We can help you.')
}

export {school, findJob}
```

#### 3. 默认暴露

m3.js

```javascript
// 默认暴露
export default {
	school: 'ATGUIGU',
	change: function () {
		console.log('GO GO')
	}
}
```

### 引入方式

#### 1. 通用导入方式

```javascript
import * as m1 from './js/m1.js'
console.log(m1)

import * as m2 from './js/m2.js'
console.log(m2)

import * as m3 from './js/m3.js'
console.log(m3)
```

#### 2. 解构赋值

```javascript
import {school, teach} from './js/m1.js'
import {school as guigu, findJob} from './js/m2.js'
import {default as m3} from './js/m3.js'
console.log(school, teach) 
console.log(guigu, findJob)
console.log(m3)
```

#### 3. 简便形式（只对默认暴露有效）

```javascript
import m3 from './js/m3.js'
console.log(m3)
```

#### 4. 通过入口文件引入

app.js

```javascript
// 入口文件

// 模块引入
import * as m1 from './m1.js'
import * as m2 from './m2.js'
import * as m3 from './m3.js'

console.log(m1)
console.log(m2)
console.log(m3)
```

index.html

```html
<script src="./js/app.js" type="module"></script>
```



