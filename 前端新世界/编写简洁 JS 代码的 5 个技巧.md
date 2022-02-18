# 编写简洁 JS 代码的 5 个技巧

> 本文翻译自：https://dev.to/daliboru/5-neat-javascript-tips-284o

在这篇文章中，我将向大家展示编写简洁JS代码的5个技巧，这些技巧将帮助你成为更好的开发人员。阅读这篇文章需要具备一些JavaScript基础，但我相信大家都能够读懂。

这些技巧包括：

1. 解构
2. 控制台提示
3. 动态键名
4. 使用Set作为数据结构
5. 基于回调的API->Promises

## 解构

通过举例来说明，往往是一种更为友好的阐述方式。假设我们有一个tiger（老虎）对象，对象中有一些关于老虎的属性，且我们需要一个函数来判断tiger是否endangered（濒临灭绝）。

```javascript
const tiger = {
  specific: 'Bengal',
  latin: 'Panthera tigris tigris',
  endangered: true,
  weight: 325,
  diet: 'fox',
  legs: 4
}

// Bad code 💩
function isEndangered(tiger){
  if (tiger.endangered) {
    return `${tiger.specific} tiger (${tiger.latin}) is endangered!`
  } else {
    return `${tiger.specific} tiger (${tiger.latin}) is not 
      endangered.`
  }  
}

// Good code 👍
function isEndangered({endangered, specific, latin}){
  if (endangered) {
    return `${specific} tiger (${latin}) is endangered!`;
  } else {
    return `${specific} tiger (${latin}) is not 
      endangered.`;
  }  
}
// or 
function isEndangered(tiger) {
  const {endangered, specific, latin} = tiger;
  // the rest is the same
```

## 控制台提示

**代码执行时间**

使用`console.time`和`console.timeEnd`确定代码执行有多快（或多慢）？

举个例子：

```javascript
console.time('TEST')

//some random code to be tested

console.timeEnd('TEST')
```

**自定义样式的日志**

要获得自定义输出，我们将像下面那样添加`%c`，然后将实际的CSS作为第二个参数。

```javascript
console.log('%c AWESOME', 'color: indigo; font-size:100px')
```

**输出表格信息**

当你要记录对象的数组时，`console.table`会派上用场。

```javascript
// x,y,z are objects
console.table([x, y, z])
```

**堆栈跟踪日志**

如果要获取函数调用的堆栈跟踪，可以使用`console.trace`

```javascript
function foo(){
  function bar(){
    console.trace('test')
  }
  bar();
}

foo();
```

## 动态键名

一个超级有用的技巧哦！

```javascript
const key = 'dynamic'

const obj = {
  dynamic: 'hey',
  [key]: 'howdy'
}

obj.dynamic // hey
obj[key] // howdy
obj['dynamic'] //hey
obj.key // howdy
```

## Set作为数据结构

如果我要求你从数字数组中删除重复项。你会怎么做？

可以使用`Set`作为数据结构来改善应用程序的功能和性能。下面是一个示例，使用Set对象从数字数组中删除重复项。

```javascript
const arr = [1, 2, 2, 3]
const newArr = new Set(arr)
const unique = [...newArr]

// unique - [1, 2, 3]
```

`Set`和`Map`类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在`Set`中，没有重复的key。

## 基于回调的API -> Promises

为了使代码更整洁和更高效，你可以将回调（ourCallbackFn）转换为promise，成为一个函数。

```javascript
// we start with this 
async function foo() {
  const x = await something1()
  const y = await something2()

  ourCallbackFn(){
    // ...
  }
}

// the transformation
async function foo() {
  const x = await something1()
  const y = await something2()

  await promiseCallbackFn() //👀
}

function promiseCallbackFn() {
  return new Promise((resolve, reject) => {
    ourCallbackFn((err, data) => { //👀
      if (err) {
        reject(err)
      } else {
        resolve(data)
      }
    })
  })
}
```

以上就是这5个JavaScript技巧了。很整洁不是吗？

感谢大家的阅读。希望能对大家有所帮助。