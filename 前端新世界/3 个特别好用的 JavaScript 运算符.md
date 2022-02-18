## 3 个特别好用的 JavaScript 运算符

大家好，本教程将介绍大多数初学者都不知道的3种出色的JS运算符，真的非常有用又省时哦。

这3个运算符是：

1. 空值合并运算符(`??`)
2. 逻辑空赋值(`??=`)
3. 可选链运算符(`?.`)

为了更好地帮助大家理解，我会逐个讨论这些运算符，并举例说明。

## 1. 空值合并运算符(??)

语法：`a??b`

- 如果定义了`a`，则输出为`a`
- 如果`a`未定义/Nullish（NULL或UNDEFINED），则输出为`b`

换句话说，如果第一个参数不是null或未定义，则空值合并运算符(`??`)返回第一个参数，否则返回第二个参数。

举例：

```javascript
let a=NULL
console.log(a??50) //50
console.log(a) //NULL
let a=10
let c=30
console.log(a??b??c??d) //10
//gives output, the first defined value
```

## 2. 逻辑空赋值(??=)

语法：`a??=b`

上面的语法等效于`a ?? (a=b)`

- 如果`a`不是NULLISH（Null或Undefined），则输出为`a`
- 如果`a`是NULLISH，则输出为`b`，并且`b`的值分配给`a`

举例：

```javascript
let a=NULL
console.log(a??=50) //50
console.log(a) //50
//compare the output of a from the previous example.
```

## 3. 可选链运算符(?.)

语法：`obj ?. prop`

- 如果`obj`的值存在，则类似于`obj.prop`
- 否则，若`obj`的值未定义或是`null`，则返回`undefined`。

通过使用`?.`带对象的运算符，而不是仅使用点`.`运算符，JavaScript会在尝试访问`obj.prop`之前隐式检查以确保`obj`不为`null`或未定义。

注意：`obj`也可以嵌套，例如`obj.name?.firstname`。

举例：

```javascript
let obj= {
   person:{
       firstName:"John",
       lastName:"Doe"
   },

   occupation: {
       compony:'capscode',
       position:'developer'
   },
   fullName: function(){
       console.log("Full Name is: "+ this.person.firstName+" >"+this.person.lastName)
  }
}
console.log(obj . person . firstName) //John
console.log(obj . human . award) //TypeError: Cannot read property 'award' of undefined
console.log(obj ?. human . award) //TypeError: Cannot read property 'award' of undefined
console.log(obj . human ?. award) //undefined
delete obj?.firstName;  // delete obj.firstName if obj exists
obj . fullName ?. () //logs John Doe
obj ?. fullName() //logs John Doe
obj . fullDetails() // TypeError: obj.fullDetails is not a function
obj ?. fullDetails() // TypeError: obj.fullDetails is not a function
obj.fullDetails ?. () //undefined
```

总结一下，可选链`?.`语法有三种形式：

1. `obj?.prop`——如果`obj`存在，则返回`obj.prop`，否则为`undefined`。
2. `obj?.[prop]`——如果`obj`存在，则返回`obj[prop]`，否则返回`undefined`。
3. `·obj.method?.()`——如果`obj.method`存在，则调用`obj.method()`，否则返回`undefined`。

希望这篇文章能对大家有所帮助。感谢阅读