# Proxy 代理

## 1. 概念

* 原意是代理，在这里表示由它来“代理”某些操作，可以译为“代理器”。
* 主要是在目标对象之前架设一层“拦截"，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制可以对外界的访问进行过滤和改写。

## 2. 语法

```javascript
let proxy = new Proxy(target, handler)
```

* `target`参数表示所要拦截的目标对象
* `handler`参数也是一个对象，用来定制拦截行为

拦截的方法：

* `get(target, key)`方法
* `set(target, key, value)`方法

## 3. 初体验

需求：如果访问的目标对象属性不存在的话，希望抛出一个错误

```javascript
let obj = {
	name: 'zhangsan',
	age: 30
}

let objProxy = new Proxy(obj, {
	get(target, key) {
		if (key in obj) {
			return obj[key]
		} else {
			throw new ReferenceError(`该对象上不存在${key}属性`)
		}
		return obj[key]
	}
})

console.log(objProxy.temp)
```

需求：如果设置年龄值小于0时，报错提示年龄无效。当设置字符串时，给字符串去除空格。

```javascript
let obj = {
	name: 'zhangsan',
	age: 30
}

let objProxy = new Proxy(obj, {
	get(target, key) {
		if (key in obj) {
			return obj[key]
		} else {
			throw new ReferenceError(`该对象上不存在${key}属性`)
		}
		return obj[key]
	},
	set(target, key, value) {
		if (key === 'age' && value < 0) {
			throw new RangeError('年龄值无效')
		}
        if (typeof value === 'string') {
			value = value.trim()
		}
		target[key] = value
	}
})

// objProxy.age = -5
objProxy.name = '   lisi'
console.log(objProxy.name)
```

