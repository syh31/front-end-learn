# 70 个 JavaScript 面试题及答案集锦

嗨，大家好！

首先声明一点，因为文章会很长，所以这 70 个 JavaScript 面试题将会被拆分为七个部分。每个面试题`都提供了参考答案。当然大家也可以自己尝试解答一下。

## 1. `undefined` 和 `null` 之间有什么区别？

在了解`undefined`和`null`之间的区别之前，我们先得了解它们之间的相似之处。

它们都属于JavaScript的7种原始类型。

```javascript
let primitiveTypes = ['string','number','null','undefined','boolean','symbol', 'bigint'];
```

它们都是假值。当使用`Boolean(value)`或`!!value`将其转换为 boolean 时，所得的值为 false。

```javascript
console.log(!!null); //logs false
console.log(!!undefined); //logs false
 
console.log(Boolean(null)); //logs false
console.log(Boolean(undefined)); //logs false
```

现在让我们说说差异之处。

`undefined`是尚未分配特定值的变量的默认值；或没有显式返回值的函数，例如`console.log(1)`；或对象中不存在的属性。JavaScript引擎为我们分配了`undefined`这个值以实现上述目的。

```javascript
let _thisIsUndefined;
const doNothing = () => { };
const someObj = {
    a: "ay",
    b: "bee",
    c: "si"
};

console.log(_thisIsUndefined); //logs undefined
console.log(doNothing()); //logs undefined
console.log(someObj["d"]); //logs undefined
```

`null`意味着“变量值为空”。`null`是已明确定义给变量的值。在示例中，当fs.readFile方法未引发错误时，我们将得到的值为null。

```javascript
fs.readFile('path/to/file', (e, data) => {
    console.log(e); //it logs null when no error occurred
    if (e) {
        console.log(e);
    }
    console.log(data);
});
```

比较`null`和`undefined`时，使用`==`时为true，使用`===`时为false。对于`==`和`===`的区别，我们将在下一篇文章中阐述

```javascript
console.log(null == undefined); // logs true
console.log(null === undefined); // logs false
```

## 2. `&&` 运算符的作用是什么？

`&&`运算符会在其操作数中找到第一个为假的表达式，并将其返回；如果未找到任何为假的表达式，则将返回最后一个表达式。它采取短路以防止不必要的工作。在我的一个项目中，当关闭数据库连接时，我就在catch块中使用过了此功能。

```javascript
console.log(false && 1 && []); //logs false
console.log(" " && true && 5); //logs 5
```

使用`if`语句：

```javascript
const router: Router = Router();

router.get('/endpoint', (req: Request, res: Response) => {
    let conMobile: PoolConnection;
    try {
        //do some db operations
    } catch (e) {
        if (conMobile) {
            conMobile.release();
        }
    }
});
```

使用&&运算符：

```javascript
const router: Router = Router();

router.get('/endpoint', (req: Request, res: Response) => {
    let conMobile: PoolConnection;
    try {
        //do some db operations
    } catch (e) {
        conMobile && conMobile.release()
    }
});
```

## 3. `||` 运算符的作用是什么？

`||`运算符会在其操作数中找到第一个为真的表达式，然后将其返回。这也采用了短路以防止不必要的工作。在支持ES6默认函数参数之前，它曾用于初始化函数中的默认参数值。

```javascript
console.log(null || 1 || undefined); //logs 1

function logName(name) {
  var n = name || "Mark";
  console.log(n);
}

logName(); //logs "Mark"
```

## 4. 使用 `+` 或一元加操作符是将字符串转换为数字的最快方法吗？

根据MDN文档，`+` 是将字符串转换为数字的最快方法，因为如果已经是数字，则对值不执行任何操作。

## 5. 什么是DOM？

DOM即Document Object Model（文档对象模型），是HTML和XML文档的接口（API）。当浏览器第一次读取（解析）我们的HTML文档时，它会创建一个大对象，一个非常大的基于HTML文档的对象，这就是DOM。它是从HTML文档建模的树状结构。DOM用于交互和修改DOM结构或特定的元素或节点。

假设，我们有这样的HTML结构。

```html
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document Object Model</title>
</head>

<body>
   <div>
      <p>
         <span></span>
      </p>
      <label></label>
      <input>
   </div>
</body>

</html>
```

DOM等效于：

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibugX3Hm3p4picBibBYniboUwyic8y7sx7J4wRaxoNu188ESB3WIHLzlETL6EkYUppdiafdmLUtppWB3MJNcv3fhXIBw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

JavaScript中的`document`对象表示DOM。它为我们提供了许多可用于选择元素以更新元素内容的方法。

## 6. 什么是事件传递？

当事件发生在DOM元素上时，该事件并不完全发生在一个元素上。在“冒泡阶段”中，事件冒泡或向上传递至其父代，其祖代，其祖代的父代，直到到达`window`为止；而在“捕获阶段”中，事件则从`window`开始向下直到触发该事件的元素或`event.target`。

事件传递分为三个阶段：

- 捕获阶段 – 事件从`window`开始，然后下降到每个元素，直到到达目标元素。
- 目标阶段 – 事件已达到目标元素。
- 冒泡阶段 – 事件从目标元素冒泡，然后上升到每个元素，直到到达`window`。

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibugX3Hm3p4picBibBYniboUwyic8y7sx7J4wWicUYlW8EdWMEXHEbvqW77QibxOnGq4CgjZkXhia0ZYliaL7S393vx9mMg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 7. 什么是事件冒泡？

当事件发生在DOM元素上时，该事件并不完全发生在一个元素上。在冒泡阶段，事件向上冒泡，或去到它的父代，祖代，祖代的父代，直到到达`window`为止。

假设有这样的html标记：

```html
<div class="grandparent">
    <div class="parent">
        <div class="child">1</div>
    </div>
</div>
```

以及js代码：

```javascript
function addEvent(el, event, callback, isCapture = false) {
  if (!el || !event || !callback || typeof callback !== 'function') return;
  if (typeof el === 'string') {
    el = document.querySelector(el);
  };
  el.addEventListener(event, callback, isCapture);
}

addEvent(document, 'DOMContentLoaded', () => {
  const child = document.querySelector('.child');
  const parent = document.querySelector('.parent');
  const grandparent = document.querySelector('.grandparent');

  addEvent(child, 'click', function (e) {
    console.log('child');
  });

  addEvent(parent, 'click', function (e) {
    console.log('parent');
  });

  addEvent(grandparent, 'click', function (e) {
    console.log('grandparent');
  });

  addEvent(document, 'click', function (e) {
    console.log('document');
  });

  addEvent('html', 'click', function (e) {
    console.log('html');
  })

  addEvent(window, 'click', function (e) {
    console.log('window');
  })

});
```

`addEventListener`方法的第三个可选参数`useCapture`，其默认值如果为false，事件将在冒泡阶段中触发；默认值如果为true，则事件将在捕获阶段中触发。如果单击`child`元素，它将分别在控制台上打印`child`，`parent`，`grandparent`，`html`，`document`和`window`。这就是事件冒泡。

## 8. 什么是事件捕获？

当事件发生在DOM元素上时，该事件并不完全发生在一个元素上。在捕获阶段，事件从`window`出发一直向下直到到达触发事件的元素。

如果我们有这样的示例html标记：

```html
<div class="grandparent">
    <div class="parent">
        <div class="child">1</div>
    </div>
</div>
```

以及js代码：

```javascript
function addEvent(el, event, callback, isCapture = false) {
  if (!el || !event || !callback || typeof callback !== 'function') return;
  if (typeof el === 'string') {
    el = document.querySelector(el);
  };
  el.addEventListener(event, callback, isCapture);
}

addEvent(document, 'DOMContentLoaded', () => {
  const child = document.querySelector('.child');
  const parent = document.querySelector('.parent');
  const grandparent = document.querySelector('.grandparent');

  addEvent(child, 'click', function (e) {
    console.log('child');
  }, true);

  addEvent(parent, 'click', function (e) {
    console.log('parent');
  }, true);

  addEvent(grandparent, 'click', function (e) {
    console.log('grandparent');
  }, true);

  addEvent(document, 'click', function (e) {
    console.log('document');
  }, true);

  addEvent('html', 'click', function (e) {
    console.log('html');
  }, true)

  addEvent(window, 'click', function (e) {
    console.log('window');
  }, true)

});
```

`addEventListener`方法的第三个可选参数`useCapture`，其默认值如果为false，事件将在冒泡阶段中触发；如果默认值为true，则事件将在捕获阶段中触发。如果我们单击`child`元素，它将分别在控制台上打印`window`，`document`，`html`，`grandparent`，`parent`和`child`。这就是事件捕获。

## 9. `event.preventDefault()`和`event.stopPropagation()`方法之间有什么区别？

`event.preventDefault()`方法可阻止元素的默认行为。如果在`form`元素中使用，它将阻止其提交。如果在`anchor`元素中使用，它将阻止其导航。如果用于`contextmenu`，它将阻止其显示或展示。而`event.stopPropagation()`方法则是阻止事件的传递，或者在冒泡或捕获阶段阻止事件触发。

## 10.如何知道是否在元素中使用了`event.preventDefault()`方法？

我们可以在事件对象中使用`event.defaultPrevented`属性。它返回boolean，指出是否在特定元素中调用`event.preventDefault()`。

## 11. 为什么代码`obj.someprop.x`会引发错误？

```javascript
const obj = {};
console.log(obj.someprop.x);
```

显然，这里引发错误是因为我们尝试访问`someprop`属性中的一个`x`属性，而`someprop`属性的值为undefined。记住属性在本身不存在的对象中，并且其原型的默认值为undefined，而undefined没有属性`x`。

## 12. 什么是`event.target`？

简单来说，`event.target`是发生事件的元素或触发事件的元素。

下面是一个HTML示例

```html
<div onclick="clickFunc(event)" style="text-align: center;margin:15px;
border:1px solid red;border-radius:3px;">
    <div style="margin: 25px; border:1px solid royalblue;border-radius:3px;">
        <div style="margin:25px;border:1px solid skyblue;border-radius:3px;">
            <button style="margin:10px">
                Button
            </button>
        </div>
    </div>
</div>
```

简单的JavaScript代码如下：

```javascript
function clickFunc(event) {
    console.log(event.target);
}
```

如果单击按钮，即使我们将事件附加在最外面的`div`上，它也会输出按钮对象标记，因为它将始终输出该按钮，所以我们可以得出结论`event.target`是触发事件的元素。

## 13. 什么是`event.currentTarget`？

`event.currentTarget`是绑定事件的元素。

我们依然使用问题12中的HTML标记代码：

```html
<div onclick="clickFunc(event)" style="text-align: center;margin:15px;
border:1px solid red;border-radius:3px;">
    <div style="margin: 25px; border:1px solid royalblue;border-radius:3px;">
        <div style="margin:25px;border:1px solid skyblue;border-radius:3px;">
            <button style="margin:10px">
                Button
            </button>
        </div>
    </div>
</div>
```

稍微更改JS代码：

```javascript
function clickFunc(event) {
  console.log(event.currentTarget);
}
```

这时候即使我们单击该按钮，它也会输出最外面的div标记。在此示例中，我们可以得出结论，`event.currentTarget`是绑定事件的元素。

## 14.  `==` 和 `===` 有什么区别？

`==`和`===`之间的区别在于，`==`在强制转换后根据值进行比较，而`===`在不强制转换下根据值和类型进行比较。

要深入研究`==`。先说说强制转换。

强制转换指的是将值转换为另一种类型的过程。在这种情况下，`==`会执行隐式强制转换。在比较两个值之前，`==`会有一些要执行的条件。

假设我们比较`x == y`值。

1. 如果`x`和`y`类型相同，则通过`===`运算符对它们进行比较。
2. 如果`x`为`null`且`y`为`undefined`，返回true。
3. 如果`x`为`undefined`且y为`null`，返回true。
4. 如果`x`是number类型，`y`是string类型，返回`x == toNumber(y)`。
5. 如果`x`是string类型，`y`是number类型，则返回`toNumber(x)== y`。
6. 如果`x`为boolean类型，则返回`toNumber(x)== y`。
7. 如果`y`是boolean类型，则返回`x == toNumber(y)`。
8. 如果`x`是string，symbol或number，且`y`是object类型，则返回`x == toPrimitive(y)`。
9. 如果`x`是object类型，且`y`是string，symbol或number，则返回`toPrimitive(x)== y`。
10. 返回false。

> 注意：toPrimitive在对象中首先使用valueOf方法，然后使用toString方法来获取该对象的初始值。

让我们举个例子。

| x                 | y         | x == y |
| :---------------- | :-------- | :----- |
| 5                 | 5         | true   |
| 1                 | '1'       | true   |
| null              | undefined | true   |
| 0                 | false     | true   |
| '1,2'             | [1,2]     | true   |
| '[object Object]' | {}        | true   |

这些示例返回的都是true。

第一个示例进入条件1，因为`x`和`y`具有相同的类型和值。

第二个示例进入条件4，因为在比较之前`y`被转换成了number。

第三个示例进入条件2。

第四个示例进入条件7，因为`y`为boolean。

第五个示例进入条件8。数组通过`toString()`方法被转换为string，即返回`1,2`。

最后一个示例进入条件10。对象通过`toString()`方法被转换为string，即返回`[object Object]`。

| x                 | y         | x === y |
| :---------------- | :-------- | :------ |
| 5                 | 5         | true    |
| 1                 | '1'       | false   |
| null              | undefined | false   |
| 0                 | false     | false   |
| '1,2'             | [1,2]     | false   |
| '[object Object]' | {}        | false   |

如果我们使用`===`运算符，那么除了第一个示例以外，其他所有的比较都将返回false，因为`x`和`y`的类型不同；而第一个示例返回true是因为两者的类型和值相同。

## 15. 为什么在JavaScript中比较两个相似的对象时，会返回false？

假设有这么一个例子：

```javascript
let a = { a: 1 };
let b = { a: 1 };
let c = a;

console.log(a === b); // logs false even though they have the same property
console.log(a === c); // logs true hmm
```

JavaScript比较对象和基本类型的方式有所不同。在比较基本类型时，按值比较，而在比较对象时，按引用或存储变量在内存中的地址进行比较。这就是为什么第一个`console.log`语句返回false，而第二个`console.log`语句返回true的原因。a和c有相同的引用，而a和b则不是。

## 16. `!!`运算符是做什么用的？

双非运算符`!!`会将右侧的值强制转换为布尔值。基本上，这是将值转换为布尔值的一个花俏方法。

```javascript
console.log(!!null); //logs false
console.log(!!undefined); //logs false
console.log(!!''); //logs false
console.log(!!0); //logs false
console.log(!!NaN); //logs false
console.log(!!' '); //logs true
console.log(!!{}); //logs true
console.log(!![]); //logs true
console.log(!!1); //logs true
console.log(!![].length); //logs false
```

## 17. 如何在一行代码中计算多个表达式？

我们可以使用`,`或逗号运算符在同一行中计算多个表达式。它按照左到右的顺序求值，并返回右边最后一个项或最后一个操作数的值。

```javascript
let x = 5;

x = (x++ , x = addFive(x), x *= 2, x -= 5, x += 10);

function addFive(num) {
  return num + 5;
}
```

如果你输出`x`的值，那么结果是27。首先，对`x`的值做自增，得到6；然后调用函数`addFive(6)`并将6作为参数传递，再将结果赋值给`x`，新的`x`值为11。之后，将`x`的当前值乘以2并将其赋值给`x`，`x`的新值为22。再然后，将`x`的当前值减去5并将结果赋值给`x`，`x`的值变为17。最后，我们将`x`的值加上10，再将更新后的值赋值给`x`，现在`x`的值为27。

## 18. Hoisting（变量提升）是什么？

Hoisting是一个术语，在JavaScript中，可以在使用变量之后对其进行声明。

换句话说，可以在声明变量之前使用它。

在理解Hoisting之前，先得解释执行上下文。

执行上下文是当前正在执行的“代码环境”。执行上下文有两个阶段：编译和执行。

**编译**——在此阶段，程序会获取所有函数声明并将其提升到其作用域的顶部，以便我们稍后可以引用它们并获取所有变量声明（使用var关键字进行声明），和提升它们并为其提供未定义的默认值。

**执行**——在此阶段，程序为先前被提升的变量分配值，并执行或调用函数（对象中的方法）。

注意：仅提升函数声明和声明var关键字的变量，而非函数表达式或箭头函数，let和const关键字。

好的，假设下面的全局范围内有这样的代码：

```javascript
console.log(y);
y = 1;
console.log(y);
console.log(greet("Mark"));

function greet(name){
  return 'Hello ' + name + '!';
}

var y;
```

这段代码分别输出了`undefined`，`1`，`Hello Mark！`。

因此，编译阶段将如下所示：

```javascript
function greet(name) {
  return 'Hello ' + name + '!';
}

var y; //implicit "undefined" assignment

//waiting for "compilation" phase to finish

//then start "execution" phase
/*
console.log(y);
y = 1;
console.log(y);
console.log(greet("Mark"));
*/
```

出于演示的目的，此处注释了变量赋值和函数调用的部分。

编译阶段完成后，程序会启动执行阶段——调用方法，并将值分配给变量。

```javascript
function greet(name) {
  return 'Hello ' + name + '!';
}

var y;

//start "execution" phase

console.log(y);
y = 1;
console.log(y);
console.log(greet("Mark"));
```

## 19. Scope（作用域）是什么？

JavaScript的作用域是我们可以有效访问变量或函数的区域。JavaScript具有三种类型的作用域：**全局作用域**，**函数作用域**和**块级作用域（ES6）**。

**全局作用域**——全局名称空间中声明的变量或函数位于全局作用域内，因此我们可以在代码的任何位置访问到它们。

```javascript
//global namespace
var g = "global";

function globalFunc() {
    function innerFunc() {
        console.log(g); // can access "g" because "g" is a global variable
    }
    innerFunc();
}  
```

**函数作用域**——在函数内部声明的变量、函数和参数可在该函数内部访问，但不能在该函数外部访问。

```javascript
function myFavoriteFunc(a) {
    if (true) {
        var b = "Hello " + a;
    }
    return b;
}
myFavoriteFunc("World");

console.log(a); // Throws a ReferenceError "a" is not defined
console.log(b); // does not continue here 
```

**块级作用域**——在块`{}`中声明的变量（`let`，`const`）只能在这里面访问。

```javascript
function testBlock() {
    if (true) {
        let z = 5;
    }
    return z;
}

testBlock(); // Throws a ReferenceError "z" is not defined
```

作用域也是一组用于查找变量的规则。如果变量在当前作用域中不存在，那么程序就会去外部作用域中查找并搜索变量，如果变量仍然不存在，那么就会再次查找直到到达全局作用域；如果变量存在，那么我们就可以使用它，如果不存在就会抛出错误。程序会搜索最近的变量，直到找到才会停止搜索和查找。这被称为作用域链。

```javascript
/* Scope Chain
Inside inner function perspective

inner's scope -> outer's scope -> global's scope
*/


//Global Scope
var variable1 = "Comrades";
var variable2 = "Sayonara";

function outer() {
    //outer's scope
    var variable1 = "World";
    function inner() {
        //inner's scope
        var variable2 = "Hello";
        console.log(variable2 + " " + variable1);
    }
    inner();
}
outer();
// logs Hello World 
// because (variable2 = "Hello") and (variable1 = "World") are the nearest 
// variables inside inner's scope.
```

##  

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibugX3Hm3p4pfCIPqiaZrX3WkxR7EFicoKcWiak2uTWM9A114OaEx8be6ibH3JicUWPParnyrlzTxolkKNe1dL0AB9ibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 20. 闭包是什么？

这可能是所有这些问题中最难的一个问题，因为闭包是一个有争议的话题。因此，我将根据我自己的理解进行解释。

闭包只是一个函数的能力，它使得能够在声明时记住其在当前作用域，父函数作用域，父代的父函数作用域上的变量和参数的引用，直到在作用域链的帮助下达到全局作用域的。基本上，它是在声明函数时创建的作用域。

举例子是解释闭包的好方法。

```javascript
//Global's Scope
var globalVar = "abc";

function a() {
    //testClosures's Scope
    console.log(globalVar);
}

a(); //logs "abc" 
/* Scope Chain
    Inside a function perspective

    a's scope -> global's scope  
*/
```

在此示例中，当我们声明`a`函数时，全局作用域是`a`的闭包的一部分。

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibugX3Hm3p4pfCIPqiaZrX3WkxR7EFicoKciamJ2s0Q21Q7MG9V3AIibSQWoHMOq5VFl9sZe0DichmcX4KrBz6NexuKw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

变量`globalVar`在上图中没有值的原因是该变量的值可以根据调用`a`函数的位置和时间而改变。

在上面的示例中，`globalVar`变量的值为`abc`。

好，让我们举一个复杂的例子。

```javascript
var globalVar = "global";
var outerVar = "outer"

function outerFunc(outerParam) {
  function innerFunc(innerParam) {
    console.log(globalVar, outerParam, innerParam);
  }
  return innerFunc;
}

const x = outerFunc(outerVar);
outerVar = "outer-2";
globalVar = "guess"
x("inner");
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibugX3Hm3p4pfCIPqiaZrX3WkxR7EFicoKcfleMnN9mmSAqHj6VetR0wlxFBPYYQ5qlVdFEqDhI9mufqQvgjRy6Sw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这将输出”guess out inner”。对此的解释是，当我们调用`outerFunc`函数并将`innerFunc`函数的返回值分配给变量`x`时，`outerParam`会得到值“out”，即使我们分配新值`outer-2`给`outerVar`变量。因为重新分配是在调用`outer`函数之后发生的，并且在此期间，当我们调用`outerFunc`函数时，它会在作用域链中查找`outerVar`的值，所以`outerVar`会得到值“outer”。现在，当我们调用引用了`innerFunc`的`x`变量时，`innerParam`将得到值`inner`，因为那是我们在调用中传递的值，而`globalVar`变量将获得值“guess”，因为在调用`x`变量之前，我们将分配一个新的值给`globalVar`，并且在调用`x`的时候，`globalVar`在作用域中的值是`guess`。

如果还不能正确理解闭包的问题，可以再看看下面的一个例子：

```javascript
const arrFuncs = [];
for(var i = 0; i < 5; i++){
  arrFuncs.push(function (){
    return i;
  });
}
console.log(i); // i is 5

for (let i = 0; i < arrFuncs.length; i++) {
  console.log(arrFuncs[i]()); // all logs "5"
}
```

代码由于闭包而无法正常运行。

`var`关键字创建了一个全局变量，当我们推送一个函数时，我们返回全局变量`i`。因此，当我们在循环后调用数组中的一个函数时，它会输出`5`，因为我们得到的`i`的当前值为5，并且由于它是全局变量，因此我们可以对其进行访问。因为闭包在创建变量时会保留该变量的引用而不是其值，所以我们可以使用IIFES或将`var`关键字改为`let`，用于块级作用域来解决这个问题。

## 21. JavaScript中的虚假值是什么？

```javascript
const falsyValues = ['', 0, null, undefined, NaN, false];
```

虚假值是当转换为布尔值时变为false的值。

## 22. 如何检查值是否是虚假值？

使用`Boolean`函数或双非运算符`!!`即可。

## 23. ”use strict”有什么作用？

`use strict`是JavaScript中的一个ES5功能，可使我们的代码在函数或整个脚本中进入严格模式。严格模式可以帮助我们避免代码早期出现的错误，并为其添加限制。

**严格模式给我们的限制**

分配或访问未声明的变量：

```javascript
function returnY() {
    "use strict";
    y = 123;
    return y;
}
```

将值分配给只读或不可写的全局变量：

```javascript
"use strict";
var NaN = NaN;
var undefined = undefined;
var Infinity = "and beyond";
```

删除不可删除的属性：

```javascript
"use strict";
const obj = {};

Object.defineProperty(obj, 'x', {
    value: '1'
});

delete obj.x;
```

重复参数名称：

```javascript
"use strict";

function someFunc(a, b, b, c) {

}
```

使用`eval`函数创建变量：

```javascript
"use strict";

eval("var x = 1;");

console.log(x); //Throws a Reference Error x is not defined
```

`this`的默认值为`undefined`：

```javascript
"use strict";

function showMeThis() {
    return this;
}

showMeThis(); //returns undefined
```

当然，严格模式下的限制不仅仅这些，其他还有很多。

## 24 .JavaScript中this的值是什么？

基本上，this指当前正在执行或正在调用函数的对象的值。我之所以要说“当前”，是因为this的值会根据我们使用它的上下文以及我们在哪里使用它而改变。

```javascript
const carDetails = {
    name: "Ford Mustang",
    yearBought: 2005,
    getName() {
        return this.name;
    },
    isRegistered: true
};

console.log(carDetails.getName()); // logs Ford Mustang
```

通常这就是我们所期望的，因为`getName`方法返回`this.name`，this在此上下文中指向的对象是`carDetails`对象，该对象当前是执行函数的“所有者”对象。

好的，让我们添加一些代码使其变得复杂起来。在`console.log`语句下面，添加以下三行代码：

```javascript
var name = "Ford Ranger";
var getCarName = carDetails.getName;

console.log(getCarName()); // logs Ford Ranger
```

第二个`console.log`语句输出单词`Ford Ranger`，这很奇怪，因为在我们的第一个`console.log`语句中，它输出的是`Ford Mustang`。原因就是`getCarName`方法有了一个不同的“所有者”对象，即`window`对象。在全局作用域中使用`var`关键字声明变量会将属性附加到与变量名称相同的`window`对象中。请记住，当不使用`use strict`时，this在全局作用域中引用的是`window`对象。

```javascript
  console.log(getCarName === window.getCarName); //logs true
  console.log(getCarName === this.getCarName); // logs true
```

在此示例中，`this`和`window`引用的是相同的对象。

解决此问题的一种方法是在函数中使用`apply`和`call`方法。

```javascript
console.log(getCarName.apply(carDetails)); //logs Ford Mustang
console.log(getCarName.call(carDetails));  //logs Ford Mustang
```

`apply`和`call`方法期望第一个参数是一个对象，该对象将是该函数内部this的值。

IIFE或Immediately Invoked Function Expression，在全局作用域内声明的函数，对象内部方法中的匿名函数和内部函数的this默认值均指向`window`对象。

```javascript
(function () {
    console.log(this);
})(); //logs the "window" object

function iHateThis() {
    console.log(this);
}

iHateThis(); //logs the "window" object  

const myFavoriteObj = {
    guessThis() {
        function getThis() {
            console.log(this);
        }
        getThis();
    },
    name: 'Marko Polo',
    thisIsAnnoying(callback) {
        callback();
    }
};


myFavoriteObj.guessThis(); //logs the "window" object
myFavoriteObj.thisIsAnnoying(function () {
    console.log(this); //logs the "window" object
});
```

如果要获取`myFavoriteObj`对象中的`name`属性的值，即`Marko Polo`，可以有两种方法解决此问题。

首先，我们将`this`的值保存在一个变量中。

```javascript
const myFavoriteObj = {
    guessThis() {
        const self = this; //saves the this value to the "self" variable
        function getName() {
            console.log(self.name);
        }
        getName();
    },
    name: 'Marko Polo',
    thisIsAnnoying(callback) {
        callback();
    }
};
```

我们保存了`this`的值，此处也就是`myFavoriteObj`对象。因此，我们可以在内部函数`getName`的内部中访问它。

其次，我们使用ES6箭头函数。

```javascript
const myFavoriteObj = {
    guessThis() {
        const getName = () => {
            //copies the value of "this" outside of this arrow function
            console.log(this.name);
        }
        getName();
    },
    name: 'Marko Polo',
    thisIsAnnoying(callback) {
        callback();
    }
};
```

箭头函数没有它自己的this。它复制了封闭作用域中this的值，或者在此示例中，复制到了内部函数`getName`之外this的值，即`myFavoriteObj`对象。我们还可以根据函数的调用方式确定this的值。

## 25. 何为对象的prototype？

用最简单的术语来说，prototype是对象的模型。如果属性和方法不存在于当前对象中，那么prototype被用作属性和方法的备选方案。这是在对象之间共享属性和方法的方式。这是围绕JavaScript原型继承的核心概念。

```javascript
const o = {};
console.log(o.toString()); // logs [object Object] 
```

即使`o`对象中不存在`o.toString`方法，它也不会抛出错误，而是返回字符串`[object Object]`。当属性确实不存在于对象中时，程序将查看其原型，并且如果仍然不存在，则将查看原型的原型，依此类推，直到在原型链中找到具有相同属性的属性为止。原型链的末尾是`Object.prototype`。

```javascript
console.log(o.toString === Object.prototype.toString); // logs true
// which means we we're looking up the Prototype Chain and it reached 
// the Object.prototype and used the "toString" method.
```

## 26. IIFE是什么，以及它的用途？

IIFE或Immediately Invoked Function Expression是在创建或声明后将被调用或执行的函数。创建IIFE的语法是，将`function (){}`包装在圆括号`()`或分组运算符中，以便于可以将函数视为表达式，然后再用另一对圆括号`()`调用它。因此，IIFE看起来是这样的`(function(){})()`。

```javascript
(function () {

}());

(function () {

})();

(function named(params) {

})();

(() => {

})();

(function (global) {

})(window);

const utility = (function () {
    return {
        //utilities
    };
})();
```

这些示例都是有效的IIFE。第二个到最后一个示例显示我们可以将参数传递给IIFE函数。最后一个示例表明，我们可以将IIFE的结果保存到变量中，以便稍后引用。

IIFE的最佳用途是进行初始化设置功能，并避免与全局作用域内的其他变量命名冲突或污染全局名称空间。让我们举个例子。

```html
<script src="https://cdnurl.com/somelibrary.js"></script>
```

假设我们有一个指向库`somelibrary.js`的链接，该库公开了一些我们可以在代码中使用的全局函数，但是该库有两个我们不使用的方法，`createGraph`和`drawGraph`方法，因为这些方法中有bug。而我们想要实现我们自己的`createGraph`和`drawGraph`方法。

解决此问题的一种方法是更改脚本的结构：

```html
<script src="https://cdnurl.com/somelibrary.js"></script>
<script>
   function createGraph() {
      // createGraph logic here
   }
   function drawGraph() {
      // drawGraph logic here
   }
</script>
```

当我们使用此解决方案时，我们将覆盖该库提供给我们的那两个方法。

解决此问题的另一种方法是更改我们自己的辅助函数的名称：

```html
<script src="https://cdnurl.com/somelibrary.js"></script>
<script>
   function myCreateGraph() {
      // createGraph logic here
   }
   function myDrawGraph() {
      // drawGraph logic here
   }
</script>
```

当使用此解决方案时，我们也要更改那些函数调用为新的函数名称。

还有一种方法是使用IIFE：

```html
<script src="https://cdnurl.com/somelibrary.js"></script>
<script>
   const graphUtility = (function () {
      function createGraph() {
         // createGraph logic here
      }
      function drawGraph() {
         // drawGraph logic here
      }
      return {
         createGraph,
         drawGraph
      }
   })();
</script>
```

在此解决方案中，我们将创建一个utility程序变量，该变量是IIFE的结果，将返回一个包含两个方法`createGraph`和`drawGraph`的对象。

在此示例中，IIFE解决的另一个问题是：

```javascript
var li = document.querySelectorAll('.list-group > li');
for (var i = 0, len = li.length; i < len; i++) {
   li[i].addEventListener('click', function (e) {
      console.log(i);
   })
}
```

假设我们有一个带有`list-group`类的`ul`元素，并且它有5个`li`子元素。当我们单击单个`li`元素时，我们想要`console.log(i)`的值。

但是我们在此代码中却行不通。无论单击哪个`li`元素，输出都为`5`。这个问题归因于闭包的工作方式。闭包只是函数的一个的功能，用于记住其当前作用域、其父函数作用域上和全局作用域中的变量引用。当我们在全局作用域内使用`var`关键字声明变量时，显然，会创建全局变量`i`。因此，当我们单击`li`元素时，它将输出5，因为这是稍后在回调函数中引用它时`i`的值。

有一个解决方案是IIFE：

```javascript
var li = document.querySelectorAll('.list-group > li');
for (var i = 0, len = li.length; i < len; i++) {
   (function (currentIndex) {
      li[currentIndex].addEventListener('click', function (e) {
         console.log(currentIndex);
      })
   })(i);
}
```

此解决方案之所以有效，是因为IIFE会为每次迭代创建一个新的作用域，并且我们捕获`i`的值并将其传递给`currentIndex`参数，因此，当我们调用IIFE时，每次迭代的`currentIndex`值都是不同的。

## 27. Function.prototype.apply方法的用途是什么？

apply在调用时会调用一个函数，用来指定`this`或此函数的“所有者”对象。

```javascript
const details = {
  message: 'Hello World!'
};

function getMessage(){
  return this.message;
}

getMessage.apply(details); // returns 'Hello World!'
```

此方法的功能类似于`Function.prototype.call`，唯一的区别是传递参数的方式。在`apply`中，我们将参数作为数组传递。

```javascript
const person = {
  name: "Marko Polo"
};

function greeting(greetingMessage) {
  return `${greetingMessage} ${this.name}`;
}

greeting.apply(person, ['Hello']); // returns "Hello Marko Polo!"
```

## 28. Function.prototype.call方法的用途是什么？

`call`在调用时会调用一个函数，指定this或此函数的“所有者”对象。

```javascript
const details = {
  message: 'Hello World!'
};

function getMessage(){
  return this.message;
}

getMessage.call(details); // returns 'Hello World!'
```

此方法的功能类似于`Function.prototype.apply`，唯一的区别是传递参数的方式。在`call`中，我们直接传递用逗号`,`来分隔的参数，对每一个参数都是如此。

```javascript
const person = {
  name: "Marko Polo"
};

function greeting(greetingMessage) {
  return `${greetingMessage} ${this.name}`;
}

greeting.call(person, 'Hello'); // returns "Hello Marko Polo!"
```

## 29. Function.prototype.apply和Function.prototype.call有什么区别？

`apply`和`call`之间的唯一区别是我们在被调用的函数中传递参数的方式。在`apply`中，我们将参数作为数组传递，而在`call`中，我们将参数直接传递到参数列表中。

```javascript
const obj1 = {
 result:0
};

const obj2 = {
 result:0
};

function reduceAdd(){
   let result = 0;
   for(let i = 0, len = arguments.length; i < len; i++){
     result += arguments[i];
   }
   this.result = result;
}

reduceAdd.apply(obj1, [1, 2, 3, 4, 5]); // returns 15
reduceAdd.call(obj2, 1, 2, 3, 4, 5); // returns 15
```

## 30. Function.prototype.bind的用法是什么？

`bind`方法返回一个被绑定到特定`this`值或“所有者”对象的新函数，以便稍后可以在代码中使用。`call`，`apply`方法立即调用函数，而不是像`bind`方法那样返回新的函数。

```javascript
import React from 'react';

class MyComponent extends React.Component {
     constructor(props){
          super(props); 
          this.state = {
             value : ""
          }  
          this.handleChange = this.handleChange.bind(this); 
          // Binds the "handleChange" method to the "MyComponent" component
     }

     handleChange(e){
       //do something amazing here
     }

     render(){
        return (
              <>
                <input type={this.props.type}
                        value={this.state.value}
                     onChange={this.handleChange}                      
                  />
              </>
        )
     }
}
```

## 31. 什么是函数式编程？JavaScript的哪些功能使其成为函数式语言的代表？

函数式编程是一种声明性编程范例或模式，说明了我们如何使用表达式来构建带有函数的应用程序，这些表达式可以计算值而无需改变传递给它的参数。

JavaScript数组具有`map`，`filter`，`reduce`方法，这些方法之所以成为函数式编程世界中最著名的函数，不但是因为它们的用处，而且也是因为它们不会改变使这些函数成为纯函数的数组，并且JavaScript支持闭包和高阶函数。所有这些正是函数式编程语言的特征。

`map`方法创建一个新数组，新数组的元素是原数组中每个元素调用指定回调函数后所得的值。

```javascript
const words = ["Functional", "Procedural", "Object-Oriented"];

const wordsLength = words.map(word => word.length);
```

`filter`方法创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

```javascript
const data = [
  { name: 'Mark', isRegistered: true },
  { name: 'Mary', isRegistered: false },
  { name: 'Mae', isRegistered: true }
];

const registeredUsers = data.filter(user => user.isRegistered);
```

`reduce`方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。

```javascript
const strs = ["I", " ", "am", " ", "Iron", " ", "Man"];
const result = strs.reduce((acc, currentStr) => acc + currentStr, "");
```

## 32. 何为高阶函数？

高阶函数是可以返回函数，或接收多个函数作为参数的函数。

```javascript
function higherOrderFunction(param,callback){
    return callback(param);
}
```

## 33. 为什么将函数称为一级对象？

JavaScript中的函数是一级对象，因为它们能被当作编程语言中的任何其他值。因此，可以将它分配给变量；可以成为对象的属性，也称为方法；可以作为数组中的项；可以作为参数传递给函数；还可以作为函数的值返回。函数与JavaScript中任何其他值之间的唯一区别是函数可以被调用。

## 34. 手动实现Array.prototype.map方法。

```javascript
function map(arr, mapCallback) {
  // First, we check if the parameters passed are right.
  if (!Array.isArray(arr) || !arr.length || typeof mapCallback !== 'function') { 
    return [];
  } else {
    let result = [];
    // We're making a results array every time we call this function
    // because we don't want to mutate the original array.
    for (let i = 0, len = arr.length; i < len; i++) {
      result.push(mapCallback(arr[i], i, arr)); 
      // push the result of the mapCallback in the 'result' array
    }
    return result; // return the result array
  }
}
```

正如MDN中对`Array.prototype.map`方法的描述：

> `map`方法创建一个新数组，新数组的元素是原数组中每个元素调用指定回调函数后所得的值。

## 35. 手动实现Array.prototype.filter方法。

```javascript
function filter(arr, filterCallback) {
  // First, we check if the parameters passed are right.
  if (!Array.isArray(arr) || !arr.length || typeof filterCallback !== 'function') 
  {
    return [];
  } else {
    let result = [];
    // We're making a results array every time we call this function
    // because we don't want to mutate the original array.
    for (let i = 0, len = arr.length; i < len; i++) {
      // check if the return value of the filterCallback is true or "truthy"
      if (filterCallback(arr[i], i, arr)) { 
      // push the current item in the 'result' array if the condition is true
        result.push(arr[i]);
      }
    }
    return result; // return the result array
  }
}
```

正如MDN中对`Array.prototype.filter`方法的描述：

> `filter`方法创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

## 36. 手动实现Array.prototype.reduce方法。

```javascript
function reduce(arr, reduceCallback, initialValue) {
  // First, we check if the parameters passed are right.
  if (!Array.isArray(arr) || !arr.length || typeof reduceCallback !== 'function') 
  {
    return [];
  } else {
    // If no initialValue has been passed to the function we're gonna use the 
    let hasInitialValue = initialValue !== undefined;
    let value = hasInitialValue ? initialValue : arr[0];
    // first array item as the initialValue

    // Then we're gonna start looping at index 1 if there is no 
    // initialValue has been passed to the function else we start at 0 if 
    // there is an initialValue.
    for (let i = hasInitialValue ? 0 : 1, len = arr.length; i < len; i++) {
      // Then for every iteration we assign the result of the 
      // reduceCallback to the variable value.
      value = reduceCallback(value, arr[i], i, arr); 
    }
    return value;
  }
}
```

正如MDN中对`Array.prototype.reduce`方法的描述：

> `reduce`方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。

## 37. 参数对象是什么？

参数对象是函数中传递的参数值的集合。这是一个类似`Array`的对象，因为它具有`length`属性，并且我们可以使用数组索引符号`arguments[1]`访问单个值，但在数组中没有`forEach`，`reduce`，`filter`和`map`的内置方法。

这可以帮助我们了解函数中传递的参数数量。

我们可以使用`Array.prototype.slice`将`arguments`对象转换为数组。

```javascript
function one() {
  return Array.prototype.slice.call(arguments);
}
```

注意：`arguments`对象不适用于ES6箭头函数。

```javascript
function one() {
  return arguments;
}
const two = function () {
  return arguments;
}
const three = function three() {
  return arguments;
}

const four = () => arguments;

four(); // Throws an error  - arguments is not defined
```

当我们调用函数`four`时，它将抛出`ReferenceError：arguments is not defined`错误。如果你的环境支持rest语法，那么可以解决此问题。

```javascript
const four = (...args) => args;
```

这会将所有参数值自动放入数组中。

## 38. 如何创建没有原型的对象？

我们可以使用`Object.create`方法创建没有原型的对象。

```javascript
const o1 = {};
console.log(o1.toString());
// logs [object Object] get this method to the Object.prototype 

const o2 = Object.create(null);
// the first parameter is the prototype of the object "o2" which in this
// case will be null specifying we don't want any prototype
console.log(o2.toString());
// throws an error o2.toString is not a function
```

## 39. 为什么在调用下面函数时代码中的b变成了全局变量？

```javascript
function myFunc() {
  let a = b = 0;
}

myFunc();
```

原因在于赋值运算符`=`遵从从右到左的求值顺序。这意味着当多个赋值运算符出现在单个表达式中时，程序会从右到左求值。这样我们的代码就变成了这样。

```javascript
function myFunc() {
  let a = (b = 0);
}

myFunc();
```

首先，表达式`b = 0`求值，并且在此示例中未声明`b`。因此，在表达式`b = 0`的返回值为0并使用`let`关键字将其分配给新的局部变量`a`之后，JS引擎在此函数外部创建了一个全局变量`b`。

我们可以通过在给变量赋值之前先声明变量来解决此问题。

```javascript
function myFunc() {
  let a,b;
  a = b = 0;
}
myFunc();
```

## 40. 什么是ECMAScript？

ECMAScript是制作脚本语言的标准，意味着JavaScript遵循ECMAScript标准中的规范，因为它是JavaScript的蓝图（blueprint）。

## 41. ES6或ECMAScript 2015中有哪些新功能？

- 箭头函数
- 类
- 模板字符串
- 增强对象文字
- 对象解构
- Promises
- 生成器
- 模组
- 符号
- 代理
- Sets
- 默认功能参数
- Rest和Spread
- 使用`let`和`const`的块作用域

## 42. `var`，`let`和`const`关键字有什么区别？

用`var`关键字声明的变量是函数作用域的。这意味着即使我们在块内声明变量，也可以在整个函数访问该变量。

```javascript
function giveMeX(showX) {
  if (showX) {
    var x = 5;
  }
  return x;
}

console.log(giveMeX(false));
console.log(giveMeX(true));
```

第一个`console.log`语句打印`undefined`，而第二个打印`5`。我们可以访问`x`变量，是因为它被提升到了函数作用域的顶部。所以代码可以解析为这样：

```javascript
function giveMeX(showX) {
  var x; // has a default value of undefined
  if (showX) {
    x = 5;
  }
  return x;
}
```

如果你想知道为什么程序在第一个`console.log`语句中打印`undefined`，请记住，声明的变量没有初始值的话，其默认值为`undefined`。

用`let`和`const`关键字声明的变量是块级作用域的。这意味着变量只能在被声明的那个块`{}`中访问。

```javascript
function giveMeX(showX) {
  if (showX) {
    let x = 5;
  }
  return x;
}


function giveMeY(showY) {
  if (showY) {
    let y = 5;
  }
  return y;
}
```

如果我们传入参数`false`调用此函数，则会抛出`Reference Error`，因为我们无法访问该块外部的`x`和`y`变量，并且这些变量不会被提升。

`let`和`const`之间也有区别，我们可以使用`let`分配新的值，但不能使用`const`，但是`const`是可变的。这意味着如果我们分配给`const`的值是一个对象，那么我们只能更改对象属性的值，但不能重新分配新的值给此变量。

## 43. 何为箭头函数？

箭头函数是在JavaScript中创建函数的新方法。用箭头函数创建函数花费的时间很少，并且语法比函数表达式更简洁，因为我们在创建函数时省略了`function`关键字。

```javascript
//ES5 Version
var getCurrentDate = function (){
  return new Date();
}

//ES6 Version
const getCurrentDate = () => new Date();
```

在此示例中，ES5版本中，分别需要`function(){}`声明和`return`关键字以创建函数和返回值。在箭头函数版本中，我们只需要括号`()`，且无需`return`语句——因为如果只返回一个表达式或值，则箭头函数会隐式返回。

```javascript
//ES5 Version
function greet(name) {
  return 'Hello ' + name + '!';
}

//ES6 Version
const greet = (name) => `Hello ${name}`;
const greet2 = name => `Hello ${name}`;
```

箭头函数中的参数可以与函数表达式和函数声明相同。如果箭头函数中只有一个参数，那么我们可以省略括号，所以上面这两种写法是等效的。

```javascript
const getArgs = () => arguments

const getArgs2 = (...rest) => rest
```

箭头函数无权访问`arguments`对象。因此，调用第一个`getArgs`函数将引发错误。相反，我们可以使用`rest`参数来获取箭头函数中传递的所有参数。

```javascript
const data = {
  result: 0,
  nums: [1, 2, 3, 4, 5],
  computeResult() {
    // "this" here refers to the "data" object
    const addAll = () => {
      // arrow functions "copies" the "this" value of 
      // the lexical enclosing function
      return this.nums.reduce((total, cur) => total + cur, 0)
    };
    this.result = addAll();
  }
};
```

箭头函数没有自己的`this`值。它捕获和获取的是闭包函数的`this`值，或者在此示例中，`addAll`函数复制`executeResult`方法的`this`值，并且如果我们在全局作用域内声明箭头函数，则`this`的值将为`window`对象。

## 44. 什么是类？

类是用JavaScript编写构造函数的一个新方法。它是使用构造函数的语法糖，且仍然在底层使用原型和基于原型的继承。

```javascript
//ES5 Version
function Person(firstName, lastName, age, address) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
    this.address = address;
}

Person.self = function () {
    return this;
}

Person.prototype.toString = function () {
    return "[object Person]";
}

Person.prototype.getFullName = function () {
    return this.firstName + " " + this.lastName;
}

//ES6 Version
class Person {
    constructor(firstName, lastName, age, address) {
        this.lastName = lastName;
        this.firstName = firstName;
        this.age = age;
        this.address = address;
    }

    static self() {
        return this;
    }

    toString() {
        return "[object Person]";
    }

    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    }
}
```

重写方法并从另一个类继承。

```javascript
//ES5 Version
Employee.prototype = Object.create(Person.prototype);

function Employee(firstName, lastName, age, address, jobTitle, yearStarted) {
  Person.call(this, firstName, lastName, age, address);
  this.jobTitle = jobTitle;
  this.yearStarted = yearStarted;
}

Employee.prototype.describe = function () {
  return `I am ${this.getFullName()} and I have a position of ${this.jobTitle} and I started at ${this.yearStarted}`;
}

Employee.prototype.toString = function () {
  return "[object Employee]";
}

//ES6 Version
class Employee extends Person { //Inherits from "Person" class
  constructor(firstName, lastName, age, address, jobTitle, yearStarted) {
    super(firstName, lastName, age, address);
    this.jobTitle = jobTitle;
    this.yearStarted = yearStarted;
  }

  describe() {
    return `I am ${this.getFullName()} and I have a position of ${this.jobTitle} and I started at ${this.yearStarted}`;
  }

  toString() { // Overriding the "toString" method of "Person"
    return "[object Employee]";
  }
}
```

那么我们怎么知道程序在底层使用原型呢？

```javascript
class Something {

}

function AnotherSomething() {

}
const as = new AnotherSomething();
const s = new Something();

console.log(typeof Something); // logs "function"
console.log(typeof AnotherSomething); // logs "function"
console.log(as.toString()); // logs "[object Object]"
console.log(as.toString()); // logs "[object Object]"
console.log(as.toString === Object.prototype.toString);
console.log(s.toString === Object.prototype.toString);
// both logs return true indicating that we are still using 
// prototypes under the hoods because the Object.prototype is
// the last part of the Prototype Chain and "Something"
// and "AnotherSomething" both inherit from Object.prototype
```

## 45. 什么是模板字面量？

模板字面量是使用JavaScript创建字符串的新方法。我们可以使用反引号或后引号实现模板字面量。

```javascript
//ES5 Version
var greet = 'Hi I\'m Mark';

//ES6 Version
let greet = `Hi I'm Mark`;
```

在ES5版本中，我们需要使用 `\` 转义 `'` 以转义该符号的正常功能，而在这种情况下，即完成该字符串值。在模板字面量中，我们不需要这样做。

```javascript
//ES5 Version
var lastWords = '\n'
  + '   I  \n'
  + '   Am  \n'
  + 'Iron Man \n';


//ES6 Version
let lastWords = `
    I
    Am
  Iron Man   
`;
```

在ES5版本中，我们需要添加`\n`以在字符串中添加新的行。而在模板字面量中，我们也不需要这样做。

```javascript
//ES5 Version
function greet(name) {
  return 'Hello ' + name + '!';
}


//ES6 Version
const greet = name => {
  return `Hello ${name} !`;
}
```

在ES5版本中，如果需要在字符串中添加表达式或值，则需要使用`+`或字符串串联运算符。在模板字面量中，我们可以使用`${expr}`嵌入一个表达式，这使其比ES5版本更整洁。

## 46. 什么是对象解构？

对象解构是一种从对象或数组中获取或提取值的新方法。

假设我们有一个这样的对象。

```javascript
const employee = {
  firstName: "Marko",
  lastName: "Polo",
  position: "Software Developer",
  yearHired: 2017
};
```

从对象获取属性的老方法是我们创建一个与对象属性同名的变量。这种方式很麻烦，因为我们要为每个属性都创建一个新变量。假设我们有一个具有许多属性的大型对象，那么使用这种方法来提取属性的方法将会令人厌烦。

```javascript
var firstName = employee.firstName;
var lastName = employee.lastName;
var position = employee.position;
var yearHired = employee.yearHired;
```

如果我们使用对象解构，代码就会干净多了，并且比老的方法所需要的时间少一些。对象解构的语法是，如果要在对象中获取属性，则使用`{}`，在`{}`中，指定需要提取的属性，而如果要从数组中获取数据，则使用`[]`。

```javascript
let { firstName, lastName, position, yearHired } = employee;
```

如果要更改需要提取的变量名，则使用`propertyName:newName`语法。在此示例中，`fName`变量的值将保留`firstName`属性的值，而`lName`变量将保留`lastName`属性的值。

```javascript
let { firstName: fName, lastName: lName, position, yearHired } = employee;
```

在解构时我们也可以使用默认值。在此示例中，如果`firstName`属性在对象中包含值`undefined`，则当我们解构时，`firstName`变量将保留默认值”`Mark`”。

```javascript
let { firstName = "Mark", lastName: lName, position, yearHired } = employee;
```

## 47. 什么是ES6模块？

模块使得我们可以将代码库拆分为多个文件，以实现更高的可维护性——可以避免我们将所有代码都放在一个大文件中。在ES6支持模块之前，曾有两种非常流行的模块系统用于支持JavaScript中的代码可维护性。

- CommonJS-Node.js
- AMD（Asynchronous Module Definition异步模块定义）—浏览器

基本上，使用模块的语法很简单，`import`用于从另一个文件或多个功能或多个值中获取功能，而`export`用于从一个文件或多个功能或多个值中导出功能。

**在文件中导出功能或Named Exports**

使用ES5（CommonJS）

```javascript
// Using ES5 CommonJS - helpers.js
exports.isNull = function (val) {
  return val === null;
}

exports.isUndefined = function (val) {
  return val === undefined;
}

exports.isNullOrUndefined = function (val) {
  return exports.isNull(val) || exports.isUndefined(val);
}
```

使用ES6模块

```javascript
// Using ES6 Modules - helpers.js
export function isNull(val){
  return val === null;
}

export function isUndefined(val) {
  return val === undefined;
}

export function isNullOrUndefined(val) {
  return isNull(val) || isUndefined(val);
}
```

**在另一个文件中导入功能**

```javascript
// Using ES5 (CommonJS) - index.js
const helpers = require('./helpers.js'); // helpers is an object
const isNull = helpers.isNull;
const isUndefined = helpers.isUndefined;
const isNullOrUndefined = helpers.isNullOrUndefined;

// or if your environment supports Destructuring
const { isNull, isUndefined, isNullOrUndefined } = require('./helpers.js');
// ES6 Modules - index.js
import * as helpers from './helpers.js'; // helpers is an object

// or 

import { isNull, isUndefined, isNullOrUndefined as isValid } from './helpers.js';

// using "as" for renaming named exports
```

**在一个文件中导出单个功能或Default Exports**

使用ES5（CommonJS）

```javascript
// Using ES5 (CommonJS) - index.js
class Helpers {
  static isNull(val) {
    return val === null;
  }

  static isUndefined(val) {
    return val === undefined;
  }

  static isNullOrUndefined(val) {
    return this.isNull(val) || this.isUndefined(val);
  }
}

module.exports = Helpers;
```

使用ES6模块

```javascript
// Using ES6 Modules - helpers.js
class Helpers {
  static isNull(val) {
    return val === null;
  }

  static isUndefined(val) {
    return val === undefined;
  }

  static isNullOrUndefined(val) {
    return this.isNull(val) || this.isUndefined(val);
  }
}

export default Helpers
```

**从另一个文件导入单个功能**

使用ES5（CommonJS）

```javascript
// Using ES5 (CommonJS) - index.js
const Helpers = require('./helpers.js'); 
console.log(Helpers.isNull(null));
```

使用ES6模块

```javascript
import Helpers from '.helpers.js'
console.log(Helpers.isNull(null));
```

这是使用ES6模块的基础。有关模块的内容就不多说了，因为这是一个广泛的话题，再扯开去我怕大家嫌文章太长不肯继续看下去了。

## 48. 什么是Set对象，以及其工作原理？

Set对象是一个ES6功能，可用于存储唯一值、基元或对象引用。集合中的值只能出现一次。使用`SameValueZero`算法来检查集合对象中是否存在某个值。

我们可以使用Set构造函数创建Set实例，并且可以选择性地传递`Iterable`用作初始值。

```javascript
const set1 = new Set();
const set2 = new Set(["a","b","c","d","d","e"]);
```

我们可以使用`add`方法将一个新的值添加到Set实例中，并且由于`add`返回Set对象，因此我们可以链接`add`调用。如果Set对象中已经存在了这个值，则不会再次添加该值。

```javascript
set2.add("f");
set2.add("g").add("h").add("i").add("j").add("k").add("k");
// the last "k" will not be added to the set object because it already exists
```

我们可以使用`delete`方法从Set实例中删除一个值，如果Set对象中已经存在这个值，则此方法返回一个`true`，同样道理`false`则表明该值不存在。

```javascript
set2.delete("k") // returns true because "k" exists in the set object
set2.delete("z") // returns false because "z" does not exists in the set object
```

我们可以使用`has`方法来检查Set实例中是否存在某个特定值。

```javascript
set2.has("a") // returns true because "a" exists in the set object
set2.has("z") // returns false because "z" does not exists in the set object
```

我们可以使用`size`属性获取Set实例的长度。

```javascript
set2.size // returns 10
```

我们可以使用`clear`方法来删除或移除Set实例中的所有元素。

```javascript
set2.clear(); // clears the set data
```

我们可以使用Set对象删除数组中的重复元素。

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 6, 7, 8, 8, 5];
const uniqueNums = [...new Set(numbers)]; // has a value of [1,2,3,4,5,6,7,8]
```

## 49. 什么是回调函数？

回调函数是将在以后的某个时间点被调用的函数。

```javascript
const btnAdd = document.getElementById('btnAdd');

btnAdd.addEventListener('click', function clickCallback(e) {
    // do something useless
});
```

在此示例中，我们等待ID为`btnAdd`的元素中的`click event`，如果`clicked`，则将执行`clickCallback`函数。回调函数添加了一些功能到某些数据或事件。数组中的`reduce`，`filter`和`map`方法期望将回调函数作为参数。如果对回调打比方就是，当你call某个人时，如果对方没有应答，那么你留下一条消息希望对方callback。Call某人或留下消息的行为就是事件或数据，而callback就是你期望之后发生的行为。

## 50. 什么是Promises？

Promises是JavaScript中处理异步操作的一种方法。它代表异步操作的值。在使用回调之前，我们用Promises来解决执行和处理异步代码的问题。

```javascript
fs.readFile('somefile.txt', function (e, data) {
  if (e) {
    console.log(e);
  }
  console.log(data);
});
```

如果我们在回调内部有另一个异步操作，则此方法会出现问题。代码将变得混乱且不可读。此时的代码被称为“回调地狱”。

```javascript
//Callback Hell yucksss
fs.readFile('somefile.txt', function (e, data) {
  //your code here
  fs.readdir('directory', function (e, files) {
    //your code here
    fs.mkdir('directory', function (e) {
      //your code here
    })
  })
})
```

如果我们在此代码中使用promises，则代码会更具可读性、更易于理解和维护。

```javascript
promReadFile('file/path')
  .then(data => {
    return promReaddir('directory');
  })
  .then(data => {
    return promMkdir('directory');
  })
  .catch(e => {
    console.log(e);
  })
```

Promises有4个不同的状态。

- **Pending**——promises的初始状态。由于操作尚未完成，因此尚未得知promises的结果。
- **Fulfilled**——异步操作已完成，并成功生成结果值。
- **Rejected**——异步操作已失败，并给出失败原因。
- **Settled**——说明promises已处于Fulfilled状态或Rejected状态两者之一。

Promise构造函数有两个参数，分别是函数resolve和reject。

如果异步操作已完成且没有错误，则调用`resolve`函数来解决Promise，而如果发生错误，则调用`reject`函数并将错误或原因传递给它。

我们可以使用`.then`方法来访问fulfilled promise的结果，我们可以用`.catch`方法中捕获错误。我们可以在`.then`方法中链接多个异步promise操作，因为`.then`方法返回Promise，就像上面的示例一样。

```javascript
const myPromiseAsync = (...args) => {
  return new Promise((resolve, reject) => {
    doSomeAsync(...args, (error, data) => {
      if (error) {
        reject(error);
      } else {
        resolve(data);
      }
    })
  })
}

myPromiseAsync()
  .then(result => {
    console.log(result);
  })
  .catch(reason => {
    console.log(reason);
  })
```

我们可以创建一个辅助函数，让该函数将带有回调的异步操作转换为Promise。它的工作原理类似于从节点核心模块`util promisify`实用程序函数。

```javascript
const toPromise = (asyncFuncWithCallback) => {
  return (...args) => {
    return new Promise((res, rej) => {
      asyncFuncWithCallback(...args, (e, result) => {
        return e ? rej(e) : res(result);
      });
    });
  }
}

const promReadFile = toPromise(fs.readFile);

promReadFile('file/path')
  .then((data) => {
    console.log(data);
  })
  .catch(e => console.log(e));
```

## 51. 什么是async/await以及工作原理？

`async/await`是用JavaScript语言编写异步或非阻塞代码的新方法。它是在Promises的基础上构建的。它可以帮助我们编写更具可读性和更简洁的异步代码，比使用Promises和Callbacks简单多了。

但是，在使用此功能之前，我们必须先学习Promises的基础知识，因为正如我之前所说，`async/await`是基于Promises构建的，这意味着在底层使用的仍然是Promises。

使用Promises：

```javascript
function callApi() {
  return fetch("url/to/api/endpoint")
    .then(resp => resp.json())
    .then(data => {
      //do something with "data"
    }).catch(err => {
      //do something with "err"
    });
}
```

使用`async/await`

**注意**：我们正在使用旧的`try / catch`语句来捕获try语句内所有异步操作发生的任意错误。

```javascript
async function callApi() {
  try {
    const resp = await fetch("url/to/api/endpoint");
    const data = await resp.json();
    //do something with "data"
  } catch (e) {
    //do something with "err"
  }
}
```

**注意**：函数声明之前的`async`关键字使函数隐式返回Promise。

```javascript
const giveMeOne = async () => 1;

giveMeOne()
  .then((num) => {
    console.log(num); // logs 1
  });
```

**注意**：`await`关键字只能在异步函数内使用。在不是异步函数的任何其他函数中使用`await`关键字将引发错误。在执行下一行代码之前，关键字`await`会等候右侧表达式（可能是Promise）返回。

```javascript
const giveMeOne = async () => 1;

function getOne() {
  try {
    const num = await giveMeOne();
    console.log(num);
  } catch (e) {
    console.log(e);
  }
}

//Throws a Compile-Time Error = Uncaught SyntaxError: await is only valid in an async function

async function getTwo() {
  try {
    const num1 = await giveMeOne(); //finishes this async operation first before going to
    const num2 = await giveMeOne(); //this line
    return num1 + num2;
  } catch (e) {
    console.log(e);
  }
}

await getTwo(); // returns 2
```

## 52. Spread扩展运算符和Rest剩余运算符有什么区别？

Spread运算符和Rest参数的运算符有相同的操作符`...`，而它们之间的区别是Spread运算符功能是把数组或类数组对象展开成一系列用逗号隔开的值，而Rest运算符功能与扩展运算符恰好相反，它会把逗号隔开的值序列组合成一个数组。

```javascript
function add(a, b) {
  return a + b;
};

const nums = [5, 6];
const sum = add(...nums);
console.log(sum);
```

在上面这个示例中，在我们调用`add`函数时，使用Spread运算符将nums数组分割成多个参数，因此参数a的值为5，b的值为6。因此总和为11。

```javascript
function add(...rest) {
  return rest.reduce((total,current) => total + current);
};

console.log(add(1, 2)); // logs 3
console.log(add(1, 2, 3, 4, 5)); // logs 15
```

在上面这个示例中，函数`add`接受任意数量的参数并将其全部加起来并返回总数。

```javascript
const [first, ...others] = [1, 2, 3, 4, 5];
console.log(first); //logs 1
console.log(others); //logs [2,3,4,5]
```

在这上面这个示例中，我们使用Rest运算符提取所有剩余的数组值，并将它们放在除第一项之外的other数组中。

## 53. 什么是默认参数？

默认参数是JavaScript中定义默认变量的新方法，它在ES6或ECMAScript 2015版本中可用。

```javascript
//ES5 Version
function add(a,b){
  a = a || 0;
  b = b || 0;
  return a + b;
}

//ES6 Version
function add(a = 0, b = 0){
  return a + b;
}
//If we don't pass any argument for 'a' or 'b' then 
// it's gonna use the "default parameter" value which is 0
add(1); // returns 1 
```

我们可以在默认参数中使用解构：

```javascript
function getFirst([first, ...rest] = [0, 1]) {
  return first;
}

getFirst();  // returns 0
getFirst([10,20,30]);  // returns 10

function getArr({ nums } = { nums: [1, 2, 3, 4] }){
    return nums;
}

getArr(); // returns [1, 2, 3, 4]
getArr({nums:[5,4,3,2,1]}); // returns [5,4,3,2,1]
```

我们还可以使用先定义的参数再定义它们之后的参数。

```javascript
function doSomethingWithValue(value = "Hello World", callback = () => { console.log(value) }) {
  callback();
}
doSomethingWithValue(); //logs "Hello World"
```

## 54. 什么是包装对象？

原始类型，例如`string`，`number`和`boolean`，除了`null`和`undefined`之外，都具有属性和方法，即使它们并不是`objects`。

```javascript
let name = "marko";

console.log(typeof name); // logs  "string"
console.log(name.toUpperCase()); // logs  "MARKO"
```

`name`是一个没有属性和方法的原始字符串值，但在此示例中，我们将调用`toUpperCase()`方法，该方法不会引发错误，但会返回`MARKO`。

原因是原始类型的值被临时转换或强制转换为`object`，因此`name`变量的行为类似于`object`。除`null`和`undefined`之外的每个原始类型都有包装对象。包装对象是`String`，`Number`，`Boolean`，`Symbol`和`BigInt`。在本示例中，调用`name.toUpperCase()`在后台是这样的：

```javascript
console.log(new String(name).toUpperCase()); // logs  "MARKO"
```

在访问属性或调用方法之后，新创建的对象将立即被丢弃。

## 55. 隐性转换Implicit和显式转换Explicit之间有什么区别？

隐性转换是一种无需我们直接或手动执行就将值转换为另一种类型的方法。

假设有下面这么一个例子：

```javascript
console.log(1 + '6');
console.log(false + true);
console.log(6 * '2');
```

第一个`console.log`语句输出`16`。在其他语言中，这将引发编译时错误，但是在JavaScript中，`1`转换为`string`，然后通过`+`运算符连接起来。我们什么也没做，因为这是由JavaScript自动为我们转换的。

第二个`console.log`语句输出`1`，它将`false`转换为`boolean`，结果将为`0`，而`true`则为`1`，因此结果为`1`。

第三个`console.log`语句输出`12`，先将`'2'`转换为`number`，然后再执行乘法`6 * 2`，因此结果`12`。

显式转换是将值转换为另一种类型（我们显式执行）的方法。

```javascript
console.log(1 + parseInt('6'));
```

在此示例中，我们使用`parseInt`函数将`'6'`转换为number，然后使用`+`运算符将`1`和`6`相加。

## 56. 什么是NaN？如何检查值是否为NaN？

`NaN`表示“Not A Number”，是JavaScript中的一个值，也就是通过将数字转换为非数字值或对其执行运算后从而得到的结果，因此为`NaN`。

```javascript
let a;

console.log(parseInt('abc'));
console.log(parseInt(null));
console.log(parseInt(undefined));
console.log(parseInt(++a));
console.log(parseInt({} * 10));
console.log(parseInt('abc' - 2));
console.log(parseInt(0 / 0));
console.log(parseInt('10a' * 10));
```

JavaScript具有内置方法`isNaN`，该方法可以测试值是否为`NaN`值。但是此功能有一个奇怪的行为。

```javascript
console.log(isNaN()); //logs true
console.log(isNaN(undefined)); //logs true
console.log(isNaN({})); //logs true
console.log(isNaN(String('a'))); //logs true
console.log(isNaN(() => { })); //logs true
```

所有这些`console.log`语句都返回true，即使我们传递的值不是`NaN`。

在ES6或ECMAScript 2015中，建议我们使用`Number.isNaN`方法，因为它实际上会检查该值是否确实是`NaN`，或者我们可以用我们自己的辅助函数来检查，因为在JavaScript中，`NaN`是唯一不等于自身的值。

```javascript
function checkIfNaN(value) {
  return value !== value;
}
```

## 57. 如何检查值是否是数组？

我们可以使用Array全局对象中的`Array.isArray`方法检查值是否为数组。当传递给它的参数是数组时，它返回true，否则返回false。

```javascript
console.log(Array.isArray(5));  //logs false
console.log(Array.isArray("")); //logs false
console.log(Array.isArray()); //logs false
console.log(Array.isArray(null)); //logs false
console.log(Array.isArray({ length: 5 })); //logs false

console.log(Array.isArray([])); //logs true
```

如果你的环境不支持此方法，则可以使用`polyfill`实现。

```javascript
function isArray(value){
   return Object.prototype.toString.call(value) === "[object Array]"
}
```

## 58. 如何在不使用取模运算符(％)的情况下检查数字是否为偶数？

对于这个问题，我们可以使用按位与运算符`&`。`&`对其操作数进行运算，并将其视为二进制值，执行AND运算。

```javascript
function isEven(num) {
  if (num & 1) {
    return false;
  } else {
    return true;
  }
};
```

二进制中的0为000。

二进制中的1为001。

二进制中的2为010。

二进制中的3为011。

二进制中的4为100。

二进制中的5为101。

二进制中的6为110。

二进制中的7为111。

等等...

| a    | b    | a & b |
| :--- | :--- | :---- |
| 0    | 0    | 0     |
| 0    | 1    | 0     |
| 1    | 0    | 0     |
| 1    | 1    | 1     |

因此，当我们调用`console.log(5&1)`时，它返回1。好吧，首先`&`运算符将两个数字都转换为二进制，因此5转换为101，而1成了001。

然后，它使用按位运算符比较每个位（0和1）。101&001。从表中可以看到，如果a AND b为1，则结果只能为1。

| 101 & 001 |
| :-------- |
| 101       |
| 001       |
| **001**   |

- 因此，我们首先比较的是最左边的位`1 & 0`，结果应为0。
- 然后，我们比较中间位`0 & 0`，结果是0。
- 接着，我们比较最后一位`1 & 1`，结果为1。
- 然后将二进制结果001转换为十进制数字，结果为1。

如果我们调用console.log(4 & 1)，则返回0。因为4的最后一位是0，而`0 & 1`为0。如果你对此感觉理解不了，那么也可以使用递归函数来解决这个问题。

```javascript
function isEven(num) {
  if (num < 0 || num === 1) return false;
  if (num == 0) return true;
  return isEven(num - 2);
}
```

## 59. 如何检查对象中是否存在某个属性？

有三种方法可以检查对象中是否存在着某个属性。

首先，使用`in`运算符。使用`in`运算符的语法类似于`propertyname in object`。如果属性存在，则返回true，否则返回false。

```javascript
const o = { 
  "prop" : "bwahahah",
  "prop2" : "hweasa"
};

console.log("prop" in o); //This logs true indicating the property "prop" is in "o" object
console.log("prop1" in o); //This logs false indicating the property "prop" is not in  "o" object
```

其次，在对象中使用`hasOwnProperty`方法。此方法可用于JavaScript中的所有对象。如果属性存在，则返回true，否则返回false。

```javascript
//Still using the o object in the first example.
console.log(o.hasOwnProperty("prop2")); // This logs true
console.log(o.hasOwnProperty("prop1")); // This logs false
```

第三，使用括号表示法`obj["prop"]`。如果属性存在，则返回该属性的值，否则将返回`undefined`。

```javascript
//Still using the o object in the first example.
console.log(o["prop"]); // This logs "bwahahah"
console.log(o["prop1"]); // This logs undefined
```

## 60. 什么是AJAX？

AJAX代表Asynchronous JavaScript and XML。它是一组用于异步显示数据的相关技术。这意味着我们可以在不重新加载网页的情况下将数据发送到服务器以及从服务器获取数据。

AJAX使用的技术：

- HTML——网页结构
- CSS——网页样式
- JavaScript——网页行为和DOM更新
- XMLHttpRequest API——用于从服务器发送和检索数据
- PHP，Python，Nodejs——某些服务器端语言

## 61. JavaScript中创建对象的方式有哪些？

使用对象字面量：

```javascript
const o = {
    name: "Mark",
    greeting() {
        return `Hi, I'm ${this.name}`;
    }
};

o.greeting(); //returns "Hi, I'm Mark"
```

使用构造函数：

```javascript
function Person(name) {
   this.name = name;
}

Person.prototype.greeting = function () {
   return `Hi, I'm ${this.name}`;
}

const mark = new Person("Mark");

mark.greeting(); //returns "Hi, I'm Mark"
```

使用`Object.create`方法：

```javascript
const n = {
   greeting() {
      return `Hi, I'm ${this.name}`;
   }
};

const o = Object.create(n); // sets the prototype of "o" to be "n"

o.name = "Mark";

console.log(o.greeting()); // logs "Hi, I'm Mark"
```

## 62. Object.seal和Object.freeze方法有什么区别？

这两种方法的区别在于，当我们对对象使用`Object.freeze`方法时，该对象的属性是不可变的，这意味着我们无法更改或编辑这些属性的值。在`Object.seal`方法中，我们可以更改那些现有属性。

## 63. 对象中的in运算符和hasOwnProperty方法有什么区别？

正如你所知道的，这两个功能都可以检查对象中是否存在某个属性。它将根据结果返回true和false。它们之间的区别在于，如果在当前对象中找不到属性，则`in`运算符还会检查对象的原型链，而`hasOwnProperty`方法只会在当前对象中检查是否存在该属性，忽略其原型链。

```javascript
// We'll still use the object in the previous question.
console.log("prop" in o); // This logs true;
console.log("toString" in o); // This logs true, the toString method is available in this object's prototype which is the Object.prototype


console.log(o.hasOwnProperty("prop")); // This logs true
console.log(o.hasOwnProperty("toString")); // This logs false, does not check the object's prototype
```

## 64. 用JavasScript处理异步代码的方法有哪些？

- Callbacks
- Promises
- Async/await
- 如`async.js`，`bluebird`，`q`，`co`之类的库

## 65. 函数表达式和函数声明有什么区别？

假设有下面这么一个例子：

```javascript
hoistedFunc();
notHoistedFunc();

function hoistedFunc(){
  console.log("I am hoisted");
}

var notHoistedFunc = function(){
  console.log("I will not be hoisted!");
}
```

`notHoistedFunc`调用会引发错误，而`hoistedFunc`调用则不会，是因为`hoistedFunc`被提升了，而`notHoistedFunc`则没有。

关于变量提升，我们在之前的面试集合中有详细解读。70 个 JavaScript 面试题及答案集锦（二）。

## 66. 可以用多少种方式调用一个函数？

JavaScript中有4种方法可以调用函数。调用时将确定this的值或函数“owner”对象的值。

**作为函数调用**——如果一个函数不是作为方法、构造函数或通过`apply`，`call`方法而调用，那么此函数就是作为函数被调用的。此函数的“所有者”对象将是window对象。

```javascript
//Global Scope

function add(a, b) {
    console.log(this);
    return a + b;
}

add(1, 5); // logs the "window" object and returns 6

const o = {
    method(callback) {
        callback();
    }
}

o.method(function () {
    console.log(this); // logs the "window" object
});
```

**作为方法调用**——如果对象的属性具有函数的值，我们将其称为方法。调用该方法时，该方法的this值将是该对象。

```javascript
const details = {
    name: "Marko",
    getName() {
        return this.name;
    }
}

details.getName(); // returns Marko
// the "this" value inside "getName" method will be the "details" object 
```

**作为构造函数调用**——如果在函数之前使用`new`关键字调用了函数，则将其称为`function constructor`。这将创建一个空对象，而this将指向该对象。

```javascript
function Employee(name, position, yearHired) {
  // creates an empty object {}
  // then assigns the empty object to the "this" keyword
  // this = {};
  this.name = name;
  this.position = position;
  this.yearHired = yearHired;
  // inherits from Employee.prototype
  // returns the "this" value implicitly if no 
  // explicit return statement is specified
};

const emp = new Employee("Marko Polo", "Software Developer", 2017);
```

**通过`apply`和`call`方法调用**——如果我们要显式地指定this值或函数的“所有者”对象，则可以使用这些方法。这些方法可用于所有函数。

```javascript
const obj1 = {
 result:0
};

const obj2 = {
 result:0
};


function reduceAdd(){
   let result = 0;
   for(let i = 0, len = arguments.length; i < len; i++){
     result += arguments[i];
   }
   this.result = result;
}


reduceAdd.apply(obj1, [1, 2, 3, 4, 5]);  //the "this" object inside the "reduceAdd" function will be "obj1"
reduceAdd.call(obj2, 1, 2, 3, 4, 5); //the "this" object inside the "reduceAdd" function will be "obj2"
```

## 67. 什么是memoization，它有什么用？

`memoization`是构建一个函数 —— 能够记住先前计算的结果或值 —— 的过程。

`memoization`函数的用处是，避免了函数的计算，如果该函数在上一次已执行相同参数的计算。这样可以节省时间，但缺点是，我们要花费更多的内存来保存以前的结果。

## 68.实现memoization辅助函数

```javascript
function memoize(fn) {
  const cache = {};
  return function (param) {
    if (cache[param]) {
      console.log('cached');
      return cache[param];
    } else {
      let result = fn(param);
      cache[param] = result;
      console.log(`not cached`);
      return result;
    }
  }
}

const toUpper = (str ="")=> str.toUpperCase();

const toUpperMemoized = memoize(toUpper);

toUpperMemoized("abcdef");
toUpperMemoized("abcdef");
```

此`memoize`辅助函数仅在接受一个参数的函数上起作用。我们需要制作一个接受多个参数的`memoize`辅助函数。

```javascript
const slice = Array.prototype.slice;
function memoize(fn) {
  const cache = {};
  return (...args) => {
    const params = slice.call(args);
    console.log(params);
    if (cache[params]) {
      console.log('cached');
      return cache[params];
    } else {
      let result = fn(...args);
      cache[params] = result;
      console.log(`not cached`);
      return result;
    }
  }
}
const makeFullName = (fName, lName) => `${fName} ${lName}`;
const reduceAdd = (numbers, startingValue = 0) => numbers.reduce((total, cur) => total + cur, startingValue);

const memoizedMakeFullName = memoize(makeFullName);
const memoizedReduceAdd = memoize(reduceAdd);

memoizedMakeFullName("Marko", "Polo");
memoizedMakeFullName("Marko", "Polo");

memoizedReduceAdd([1, 2, 3, 4, 5], 5);
memoizedReduceAdd([1, 2, 3, 4, 5], 5);
```

## 69. 为什么typeof null返回object？如何检查一个值是否为null？

`typeof null == 'object'`将始终返回true，这是由于自JavaScript诞生以来null的实现。也曾提出过补丁，就是将`typeof null == 'object'`更改为`typeof null == 'null'`，但由于导致了更多bug而被否决。

我们可以使用`===`严格相等运算符来检查值是否为null。

```javascript
function isNull(value) {
    return value === null;
}
```

## 70. new关键字有什么作用？

`new`关键字与构造函数一起使用，用来在JavaScript中创建对象。

假设有下面的示例代码。

```javascript
function Employee(name, position, yearHired) {
  this.name = name;
  this.position = position;
  this.yearHired = yearHired;
};

const emp = new Employee("Marko Polo", "Software Developer", 2017);
```

new关键字做了4件事。

- 创建一个空对象。
- 将该空对象分配给this值。
- 该函数将继承自`functionName.prototype`。
- 如果不使用明确的return语句，则返回this。

上面的代码将首先创建一个空对象`{}`，然后将this值指定到此空对象`this = {}`，并向此this对象添加属性。因为我们没有显式的return语句，所以程序会自动返回this。