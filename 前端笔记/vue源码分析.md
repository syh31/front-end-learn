# Vue源码分析

## 准备知识

### 1. 将类数组对象转化为数组

```html
<ul>
	<li>test1</li>
	<li>test2</li>
	<li>test3</li>
</ul>
```

```javascript
const lis = document.getElementsByTagName('li')
console.log(lis instanceof Array, lis.forEach)
const lis2 = Array.from(lis) // es6
console.log(lis2 instanceof Array)
const lis3 = Array.prototype.slice.call(lis) // es5
console.log(lis3 instanceof Array)
```

控制台输出：

```bash
false undefined
prepare.html:23 true ƒ forEach() { [native code] }
prepare.html:25 true ƒ forEach() { [native code] }
```

### 2. 获取节点类型

#### DOM节点

根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：

- 整个文档是一个文档节点（Document）
- 每个 HTML 元素是元素节点（Element）
- HTML 元素内的文本是文本节点（Text）
- 每个 HTML 属性是属性节点（Attr）
- 注释是注释节点

#### HTML DOM 节点树

HTML DOM 将 HTML 文档视作树结构。这种结构被称为*节点树*：

##### HTML DOM Tree 实例

![HTML DOM Node Tree](https://www.w3school.com.cn/i/ct_htmltree.gif)

通过 HTML DOM，树中的所有节点均可通过 JavaScript 进行访问。所有 HTML 元素（节点）均可被修改，也可以创建或删除节点。

```html
<div id="test">IT</div>
```

```javascript
const elementNode = document.getElementById('test')
const attrNode = elementNode.getAttributeNode('id')
const textNode = elementNode.firstChild
console.log(elementNode.nodeType, attrNode.nodeType, textNode.nodeType)
```

输出结果：

```bash
1 2 3
```

### 3. `Object.defineProperty()`方法

`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

> **备注：**应当直接在 [`Object`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object) 构造器对象上调用此方法，而不是在任意一个 `Object` 类型的实例上调用。

#### 语法

```
Object.defineProperty(obj, prop, descriptor)
```

##### 参数

- `obj`

  要定义属性的对象。

- `prop`

  要定义或修改的属性的名称或 [`Symbol`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 。

- `descriptor`

  要定义或修改的属性描述符。
  
  数据描述符（4个）：
  
  * `configurable`: 是否可以重新定义，**默认为** **`false`**。
  
  * `enumerable`: 是否可以枚举，**默认为** **`false`**。
  
  * `value`: 初始值，**默认为** **`undefined`**。
  
  * `writable`: 是否可以修改属性值，**默认为** **`false`**。
  
  访问描述符（2个）：
  
  * `get`: 回调函数，根据其他相关的属性动态计算得到当前属性值，**默认为** **`undefined`**。
  
  * `set`: 回调函数，监视当前属性值的变化，更新其他相关的属性值，**默认为** **`undefined`**。

##### 返回值

被传递给函数的对象。

> 在ES6中，由于 Symbol类型的特殊性，用Symbol类型的值来做对象的key与常规的定义或修改不同，而`Object.defineProperty` 是定义key为Symbol的属性的方法之一。

```javascript
const obj = {
    firstName: 'A',
    lastName: 'B'
}
// 给obj添加fullName属性，并且能随firstName和lastName变化
Object.defineProperty(obj, 'fullName', {
    get() {
        return this.firstName + '-' + this.lastName
    },
    set(value) {
        const names = value.split('-')
        this.firstName = names[0]
        this.lastName = names[1]
    }
})

console.log(obj.fullName)
obj.firstName = 'C'
obj.lastName = 'D'
console.log(obj.fullName)
obj.fullName = 'E-F'
console.log(obj.firstName, obj.lastName)
```

输出结果：

```bash
A-B
C-D
E F
```

```javascript
Object.defineProperty(obj, 'fullName2', {
    configurable: false,
    enumerable: true,
    value: 'G-H',
    writable: false
})
console.log(obj.fullName2)
obj.fullName2 = 'J-K'
console.log(obj.fullName2)

Object.defineProperty(obj, 'fullName2', {
    configurable: false,
    enumerable: false,
    value: 'G-H',
    writable: true
})
```

输出结果：

```bash
G-H
G-H

Object.defineProperty(obj, 'fullName2', {
       ^
       
TypeError: Cannot redefine property: fullName2
```

> 该语法仅支持IE9及以上，因此导致vue不支持IE8及以下。

### 4. `Object.keys()`方法

得到对象自身所有可枚举属性组成的数组。

```javascript
const names = Object.keys(obj)
console.log(names)
```

输出结果：

```bash
[ 'firstName', 'lastName', 'fullName2' ]
```

### 5. `Object.prototype.hasOwnProperty()`方法

`hasOwnProperty()` 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。

```javascript
console.log(obj.hasOwnProperty('fullName'), obj.hasOwnProperty('toString'))
```

输出结果：

```bash
true false
```

### 6. DocumentFragment文档片段

Document：对应显示的页面，包含n个Element，一旦更新Document内部的某个元素，界面也会更新。

DocumentFragment：内存中保存n个Element的容器对象（不与界面关联），如果更新Fragment中的某个元素，界面不会改变。

##### 实现：将li标签中的文本替换为atguigu

```html
<ul id="fragment_test">
	<li>test1</li>
	<li>test2</li>
	<li>test3</li>
</ul>
```


```javascript
const ul = document.getElementById('fragment_test')
// 1. 创建fragment
const fragment = document.createDocumentFragment()
// 2. 取出ul中所有的子节点保存到fragment中
// 换行是一个文本节点
let child
while (child = ul.firstChild) {
    // 因为每个节点只能有一个父节点，所以不会死循环
    // appendChild方法先将child从ul中移除，然后将child添加为fragment的子节点
    fragment.appendChild(child)
}
// 3. 更新fragment中所有li的文本
// childNodes获取所有的子节点，children获取所有的子标签
// 先将伪数组转换成真数组
Array.from(fragment.childNodes).forEach(node => {
    if (node.nodeType === 1) {
        // 说明当前节点为元素节点
        node.textContent = 'atguigu'
    }
})
// 4. 将fragment插入到ul中
ul.appendChild(fragment)
```

## 数据代理

### 1. 数据代理

数据代理是通过一个对象代理对另一个对象中属性的操作（读/写）。

Vue中的数据代理是通过`vm`对象来代理data对象中所有属性的操作。优点是能够更方便的操作`data`中的数据

### 2. 基本实现流程

1. 通过`Object.defineProperty()`给`vm`添加与`data`对象的属性对应的属性描述符。
2. 所有添加的属性都包含`getter`/`setter`。
3. `getter`/`setter`内部去操作data中对应的属性数据。

## 模板解析

### 1. 模板解析的基本流程

1. 将el的所有子节点取出, 添加到一个新建的文档fragment对象中

2. 对fragment中的所有层次子节点递归进行编译解析处理

   * 对大括号表达式文本节点进行解析

   * 对元素节点的指令属性进行解析

     * 事件指令解析

     * 一般指令解析

3. 将解析后的fragment 添加到el 中显示

### 2. 大括号表达式解析

1. 根据正则对象得到匹配出的表达式字符串: 子匹配/RegExp.$1
2. 从data 中取出表达式对应的属性值
3. 将属性值设置为文本节点的textContent

### 3. 事件指令解析

1. 从指令名中取出事件名
2. 根据指令的值(表达式)从`methods`中得到对应的事件处理函数对象
3. 给当前元素节点绑定指定事件名和回调函数的dom事件监听
4. 指令解析完后，移除此指令属性

### 4. 一般指令解析

1. 得到指令名和指令值(表达式)
2. 从`data`中根据表达式得到对应的值
3. 根据指令名确定需要操作元素节点的什么属性
   * `v-text`---textContent 属性
   * `v-html`---innerHTML 属性
   * `v-class`--className 属性
4. 将得到的表达式的值设置到对应的属性上
5. 移除元素的指令属性

## 数据绑定

### 1. 数据绑定

一旦更新了`data`中的某个属性数据，所有界面上直接使用或间接使用了此属性的节点都会更新。

### 2. 数据劫持

1. 数据劫持是Vue中用来实现数据绑定的一种技术
2. 基本思想：通过`Object.defineProperty()`来监视`data`中所有属性（任意层次）数据的变化，一旦变化就去更新页面。

### 3. 四个重要对象

##### 1. Observer

1. 用来对data中所有属性数据进行劫持的构造函数

2. 给data中所有属性重新定义属性描述（主要是get/set方法）

3. 为data中的每个属性创建对应的Dep对象

##### 2. Dep (Depend)

1. data中的每个属性（所有层次）都对应一个Dep对象

2. 它的实例在什么时候创建？

   初始化中的给`data`的属性进行数据劫持时创建的。

   > * 在初始化在初始化 define data 中各个属性时创建对应的 dep 对象
   > * 在 data 中的某个属性值被设置为新的对象时

3. 它的数量是多少？

   与`data`中的属性是一一对应的。

4. Dep对象的结构

```javascript
{
    id: 0, // 每个dep都有一个唯一标识id,
    subs: [] // n个相关的Watcher对象的容器（数组）
}
```

5. subs属性说明
   * 当Watcher被创建时，内部将当前watcher 对象添加到对应的 dep 对象的subs 中
   * 当此 data 属性的值发生改变时, subs 中所有的 watcher 都会收到更新的通知，从而最终更新对应的界面

##### 3. Compiler

1. 用来解析模板页面的对象的构造函数(一个实例)
2. 利用 compile 对象解析模板页面
3. 每解析一个表达式(非事件指令)都会创建一个对应的 watcher 对象, 并建立 watcher 与 dep 的关系
4. complier 与 watcher 关系: 一对多的关系

##### 4. Watcher

1. 模板中每个非事件指令或表达式都对应一个 watcher 对象

2. 监视当前表达式数据的变化

3. 它的实例在什么时候创建？

   初始化中的解析大括号表达式/一般指令时创建。

4. 它的数量是多少？

   与模板中表达式（不包含事件指令）的个数一一对应。

5. Watcher对象的数据结构

```javascript
this.cb = cb  // 当表达式所对应的数据发生改变时用于更新界面的回调函数
this.vm = vm  // vm对象
this.exp = exp  // 对应指令的表达式
this.depIds = {"depid": "dep"}  // 相关的n个Dep对象的容器（对象）
this.value = this.get()  // 当前表达式对应的value值
```

##### 5. Dep与Watcher之间的关系

* 什么关系？

  多对多的关系。

  * data属性 -> Dep -> n个Watcher（n>1：当模板中有多个表达式使用了此属性时：`{{ a }}` `v-text="a"`）
  * 表达式 -> Watcher -> n个Dep（n>1：当使用多层表达式时：`a.name.last`）

* 如何建立关系？

  使用data中属性的get方法中建立。

* 什么时候建立的？

  初始化的解析模块中的表达式创建Watcher对象时建立。
  
* 数据绑定使用到 2 个核心技术
  * `Object.defineProperty()`
  * 消息订阅与发布

## MVVM原理图分析

![](F:\技术资料\前端面试知识点\MVVM原理图.jpg)

##### 初始化

Compile -> Watcher: 为表达式创建对应的watcher，指定更新的函数

Dep: 与data中的属性一一对应

* subs: 保存n个Watcher的数组容器

##### 更新

Dep -> Watcher: 通知所有相关的Watcher

![MVVM原理图](F:\技术资料\前端面试知识点\MVVM.png)

## 双向数据绑定

1. 双向数据绑定是建立在单向数据绑定(model==>View)的基础之上的
2. 双向数据绑定的实现流程:
   1. 在解析 v-model 指令时, 给当前元素添加事件监听（输入框触发方式为input）
   2. 当 input 的 value 发生改变时, 将最新的值赋值给当前表达式所对应的 data 属性

