---
layout:       post
title:        "前端基础学习之JavaScript part two"
author:       "OuterCyrex"
header-style: text
catalog:      true
tags:
    - 前端
    - JavaScript





---

# 二.函数、对象与时间库

## 一.函数

### 1.函数的声明

在`javascript`中，函数的声明与`go`类似，其关键字为`function`

```javascript
function hello() {
    console.log("Hello")
};
hello()
//输出结果
hello
```

同样，在`javascript`中也存在**匿名函数**，在`javascript`和`go`中，**函数**被视为**一等公民**，可以进行**赋值给参数**等多种操作。

与`go`相同的地方是，`javascript`中可以直接使用匿名函数，被称为**立即执行函数**。也可以将其给赋值给一个变量，然后再调用该变量。

```javascript
const hello = function(){
    console.log("Hello")
};
(function(){
    console.log("World")
})();
hello()
//输出结果
hello
World
```

此处注意，一般而言要在函数末尾书写`;`，否则可能出现**语法错误**。

除了上述的声明方法外，`ES6`规范还新增了**箭头函数**，即可以通过以下的方法来声明一个函数：

```javascript
const calc = (a,b)=>{
    console.log(a+b)
};
calc(1,2)
```

同为动态语言，`javascript`的函数形式与`python`相似，其在`function`的括号内填入参数且**无需标明数据类型**，返回值则在函数体内用`return`返回。

```javascript
const calc = function(a,b){
    return a + b
};
console.log(calc(1,2))
```

**注意**，若在函数内没有`return`语句，则默认`return undefined`。 

### 2.缺省值

在定义函数时，我们可以选择让其返回**默认值**。

在`javascript`中，若**未向函数传参**，则该参数在函数内部会被定义为`undefined`。

```javascript
const JudgeName = function(name){
    return name === undefined;
};
console.log(JudgeName())
//输出结果
true
```

通过这个特性，我们便可以实现**设置默认值**的效果。

```javascript
const JudgeName = function(name){
    if(name === undefined){
        return "Outer"
    }else{
        return name
    }
};
console.log(JudgeName())
console.log(JudgeName("Cyrex"))
//输出结果
Outer
Cyrex
```

除此之外，我们可以将其简化：

```javascript
const JudgeName = function(name){
    return name || "Outer"
};
console.log(JudgeName())
console.log(JudgeName("Cyrex"))
```

通过`return result || default`的方式，仍然是可以实现**默认值**效果的，其原理是：

```javascript
Default = "我是缺省值"
console.log(false || Default)
//输出结果
我是缺省值
```

除了上述的方法外，我们也可以直接在定义函数时**给参数赋值**

```javascript
const JudgeName = function(name = "Outer"){
    return name
};
console.log(JudgeName())
console.log(JudgeName("Cyrex"))
```

其原理是，在定义函数时已经给参数赋了值。若传参则新值会代替其默认值，若未传参则不变。

### 3.参数列表

在需要传入多个参数时，可以通过**参数列表**来全部接收，其语法与`go`类似：

```javascript
const sum = function(...list){
    let total = 0
    for (let a of list){
        total += a
    }
    return total
}
console.log(sum(1,2,3,4,5,6,7))
//输出结果
28
```

`...变量名`会将传入的数值转化为对应的**数组**，我们只需要在函数中调用这个**数组**即可。

此外，`javascript`提供了`arguments`，其指代的是输入的**所有参数**组成的**参数列表**。

```javascript
const sum = function(){
    console.log(typeof arguments)
    console.log(Array.isArray(arguments))
    let total = 0
    for (let a of arguments){
        total += a
    }
    return total
}
console.log(sum(1,2,3,4,5,6,7))
//输出结果
Object
false
28
```

上述代码同样实现了将输入的所有参数进行求和。

且我们可以观察到`arguments`的数据类型为`Object`且其`isArray`值为`false`，证明其**不是数组**。

### 4.定义方法

如果在**对象**中**声明函数**，则该**函数**会变成对应**对象**的**方法**。

```javascript
let obj = {
    name : "Outer",
    hello : function(){
        console.log(`Hello,${this.name}`)
    }
}
obj.hello()
//输出结果
Hello,Outer
```

上述函数实际是将函数定义给了对应的`key`，当我们调用这个`key`的时候，只需要加上`()`即可实现对应**方法**的调用。

与**函数的声明**一样，我们可以通过多种方式来实现**方法的声明**。

我们可以直接给函数起名，而不使用**匿名函数**。

```javascript
let obj = {
    name : "Outer",
    hello(){
        console.log(`Hello,${this.name}`)
    }
}
obj.hello()
```

也可以使用之前的**箭头函数**，但**注意**，一旦使用**箭头函数**，其内部不可以使用`this`，之后讲到`this`时会再做解释。

```javascript
let obj = {
    name : "Outer",
    hello:()=>{
        console.log("Hello!")
    }
}
obj.hello()
```

由于不能使用`this`的特性，一般不建议使用**箭头函数**来声明方法。

### 5.this

在**方法的声明**中内我们使用了`this`，接下来将更加详细的介绍`this`

首先，一段`javascript`的所有代码都被囊括在一个称为`window`的**对象**中，其类似于`HTML`的`body`，是`javascript`运行的主体。

```javascript
window.alert("alert")
var MyName = "Outer"
console.log(window.MyName)
```

- 如果我们在**方法外**调用`this`，则其指代的对象便是`window`。

- 如果在**方法内**使用`this`，则`this`指代的是**拥有该方法的对象**。

- 如果在**箭头函数内**使用`this`，则指向的是`window`

如果我们在**对象外**定义了一个**函数**，则可以在该**函数**内设置`this`，并手动将其指向我们**指定的对象**。

常见的方法有三种：`call、apply、bind`

| 关键字 | 用法                                            |
| ------ | ----------------------------------------------- |
| call   | `function.call(this指向谁, 参数1,参数2...)`     |
| apply  | `function.apply(this指向谁, [参数1, 参数2...])` |
| bind   | `function.bind(指向,参数1,参数2,...)`           |

首先使用的是**函数类型**的`call`方法，他可以将**函数**转化为**指定对象**的**方法**。

```javascript
let newFunc = function(){
    console.log(`Hello,${this.name}`)
}
let Obj = {
    name:"Outer"
}
newFunc.call(Obj)
```

其传参的方法为，在`call`内先填入要**指向的对象**，之后再填入**参数**

```javascript
let newFunc = function(age){
    this.age = age
}
let Obj = {
    name:"Outer",
    age:undefined,
}
newFunc.call(Obj,19)
console.log(Obj)
//输出结果
{name: 'Outer', age: 19}
```

其他两种方法的用法已经在表格内列举，其中`apply`在传参时需要传入数组。

```javascript
newFunc.apply(Obj,[19])
```

此外，`bind`不同于`call`的地方在于，其并不是直接调用方法，而是返回**对应的方法**，需要我们**自行调用**

```javascript
let newFunc = function(age){
    this.age = age
    console.log(this)
}
let Obj = {
    name:"Outer",
    age:undefined,
}
newFunc.bind(Obj,19)()
newFunc.bind(Obj)(19)
```

其中，两种传参的方法都是正确的，可以在**绑定对象**时传参，也可以**绑定完之后**再传参。

**注意**，箭头函数没办法修改`this`的指向。

## 二.对象

### 1.创建对象

 在`javascript`中，一共有4中创建对象的方式

其分别为，**直接定义对象**、**`new`关键字**、**原型对象**以及**`create`方法**。

之前在数据类型那一章节介绍的便是**直接定义对象**

```javascript
const Obj = Object({
    name:"Outer",
})
```

除了**直接定义**外，我们也可以通过**`new`关键字**来生成一个新的对象：

```javascript
const Obj = new Object({
    name:"Outer",
})
```

------

注意，`new`的用法是与**数据类型**连用，即`new Type()`，其不仅可以定义`Object`类型，也可以用来定义函数等：

```javascript
const func = new function(){
    console.log("Hello")
}
```

另外，`new`是用于为变量**实例化**的，因此其后边接的一定是**实例化**的数据。

------

**原型对象**即通过定义一个函数，并引用该**函数**作为**原型**来**创建新的对象**，且该**函数**被称为**构造函数**，有点类似`python`中的`__init__`方法

```javascript
function Student(name,age){
    this.name = name
    this.age = age
}
Student.prototype.Say = function(){
    console.log(`${this.name} Says He is ${this.age}-Year-Old`)
}
const user = new Student("Outer",19);
user.Say()
//输出结果
Outer Says He is 19-Year-Old
```

上述代码中，我们创建了一个**构造函数**`Student`，并在之后将其**实例化**，将**实例化**的内容赋值给了`user`

我们也可以使用**匿名函数**来实现这种效果，但一般不建议使用这种**立即执行函数**

```javascript
const func = new function(name = "Outer"){
    this.name = name
}
console.log(func)
//返回值
{name: 'Outer'}
```

`prototype`可以**为对象增加方法**，如上方的`Student.prototype.Say`便为`Student`增加了`Say`方法。

最后一种方法是`create`方法，其并不是直接创建一个**对象**，而是将内容传给一个**空的对象**的`prototype (原型)`字段内

```javascript
const user = Object.create({
    name :"Outer",
    age : 19,
})
console.log(user)
//输出结果
{}
prototype:{
    name:"Outer",
    age:19,
}
```

**原型**顾名思义，即可以理解为当前创建的这个**对象**是`prototype`的副本。

如果没有内容，该**对象**就会继承其**原型**的内容。

### 2.更改属性

此处说的**属性**，即**字段**、**键**。**字段**是在将对象看做**结构体**而言的，**键**是在将对象看做**键值对**而言的。

一个**对象**的属性被分为两类，一类是继承而来的，被称为`prototype`型，另一类是该对象自身的属性，是非`prototype`的，可以是**可枚举属性**，也可以是**不可枚举属性**。

| 关键字         | 作用                                               |
| -------------- | -------------------------------------------------- |
| delete         | 删除自身属性，不能删除`prototype`型属性            |
| in             | 可以判断自己是否拥有该属性，可以为`prototype`型    |
| hasOwnProperty | 只能检测**非`prototype`属性**，不包含`prototype`型 |
| for in         | 枚举属性，包括**非`prototype`属性**和`prototype`型 |
| Object.keys()  | 只能枚举**非`prototype`属性**                      |

在之前介绍过`delete`可以**删除**自身的某个字段，但不能**删除**`prototype`型字段。

`in`关键字可以查询对象中是否存在指定字段。

```javascript
const user = Object.create({
    name :"Outer",
    age : 19,
});
user.sex = "Male";
console.log("age" in user)
console.log("sex" in user)
//输出结果
true
true
```

可以看到，无论是`create`创建的`prototype`属性，还是自身定义的属性，都可以被`in`检测。

与之不同的是，`hasOwnProperty`不会检测`prototype`属性

```javascript
const user = Object.create({
    name :"Outer",
    age : 19,
});
user.sex = "Male";
console.log(user.hasOwnProperty("Outer"))
console.log(user.hasOwnProperty("sex"))
//输出结果
false
true
```

`for in`在之前介绍数组时已经说过，可以用来枚举对象的属性。

```javascript
const user = Object.create({
    name :"Outer",
    age : 19,
});
user.sex = "Male";
for(let index in user){
    console.log(index)
}
//输出结果
name
age
sex
```

与之区分的是`Object.Keys()`不会返回`prototype`属性，且其返回值是一个`string`类型的**数组**。

```javascript
const user = Object.create({
    name :"Outer",
    age : 19,
});
user.sex = "Male";
console.log(Object.keys(user))
//输出结果
['sex']
```

上述所有的可以列举`prototype`型的方法都能检测**对象里的方法**。

比如下边用`for in`来测试：

```javascript
const user = Object.create({
    name :"Outer",
    age : 19,
    Say:function(){
        console.log("Hello！")
    }
});
user.sex = "Male";
for(let index in user){
    console.log(index)
}
//输出结果
name
age
sex
Say
```

### 3.API

介绍一下`Obejct`上的常用`API`。`API`即一些**预先定义的函数**，目的是提供**访问一组例程的能力**，而又**无需访问源码**，或**理解内部工作机制的细节**。

先前介绍数组时的`Array.isArray`实际上就是一种`API`，只是那时暂时被称为**函数**。

| API                          | 作用                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| Object.assign()              | 浅拷贝，只能拷贝**非`prototype`属性**                        |
| Object.keys()                | 获取**可枚举**的**非`prototype`属性**的字段名并返回一个数组  |
| Object.getOwnPropertyNames() | 获取自身拥有的**枚举或不可枚举的非`prototype`属性**的字段名并返回一个数组 |
| Object.defineProperty()      | 可以对对象的属性调整设置。                                   |

关键介绍一下浅拷贝`Object.assign()`，**浅拷贝**的**拷贝**体现在他可以将某个对象的内容拷贝进**目标空对象**内。其语法为：

```javascript
Object.assign(target,...sources);
```

如：

```javascript
const obj = {}
Object.assign(obj,{"name":"Outer"})
console.log(obj)
//输出结果
{"name":"Outer"}
```

`Object.assign`有几个特点：

- 只能拷贝**非`prototype`属性**到目标空对象
- 如果目标对象已有字段，则拷贝会将**目标对象**原有的**同名属性**覆盖，而不重名的不会被删除。
- 如果**源对象**内含有一个**子对象**，且被拷贝给了**目标对象**，这个过程实际是给**目标对象**拷贝了这个**子对象**的**引用**。（注意：**数组**也是一种对象，因此也受此影响）

如，目标方法已有`name`字段，再次拷贝时便会将**同名属性**覆盖。

```javascript
const obj = {"name":"Outer"}
Object.assign(obj,{"name":"Cyrex"})
console.log(obj)
//输出结果
Cyrex
```

第三个特点字面不易理解，通过例子来解释：

```javascript
const obj = {}
const source = {
    name:"Outer",
    subObj:{
        age:19,
    }
}
Object.assign(obj,source)
console.log(obj)
source.subObj.age = 20
console.log(obj)
//输出结果
name: "Outer"
subObj: {age: 20}
name: "Outer"
subObj: {age: 20}
```

在上述代码中，我们将带有子对象`subObj`的源对象`source`拷贝给了`obj`，我们会发现，尽管更改的是`source`的属性，`obj`的`subObj.age`属性同样发生了更改。这便是传**引用**导致的。

且该效果是**全局**的，如上述代码中第一个`console.log`在更改之前，输出结果也仍然是更改之后的数据。

之后的`Object.keys()`和`Object.getOwnPropertyNames()`不再过多介绍，前者在之前已经介绍过，后者与前者不同的是其可以获取**不可枚举属性**。

至于不可枚举属性的设置，则需要`Object.defineProperty()`

```javascript
const o1 = {name: "Outer"}

Object.defineProperty(o1, "age", {
    value: 21,
    writable: true,
    enumerable: false,
    configurable: true,
})

console.log("Object.keys", Object.keys(o1))
console.log("Object.getOwnPropertyNames", Object.getOwnPropertyNames(o1))
//输出结果
['name']
['name','age']
```

其中，`defineProperty`的内容分别为

| 属性         | 作用           |
| ------------ | -------------- |
| value        | 设置该字段的值 |
| writable     | 是否可更改     |
| enumerable   | 是否可枚举     |
| configurable | 是否可删除     |

### 4.原型与原型链

#### a.原型

在`javascript`中，每个**对象**都有一个隐藏的`[[prototype]]`字段（除了`Null`和`Object.prototype`）。

| 关键字          | 作用                                                         |
| --------------- | ------------------------------------------------------------ |
| `[[prototype]]` | 一个非`null`且非`Object`的对象的**原型**字段名               |
| `prototype`     | 指向一个**构造函数**抽象出的虚拟**对象**，即该**构造函数**实例化后的**对象**的**原型** |
| `__proto__`     | 直接指向一个对象的**原型**                                   |

首先要区分`[[prototype]]`与`prototype`，前者是一个对象自带的内部属性，而后者是函数对象的属性，该对象被用作通过**该函数创建的实例的原型**。

```javascript
const source = {
        name:"Outer"
    }
const obj = Object.create(source)
console.log(Object.getPrototypeOf(obj) === source);
//输出结果
true
```

此处`Object.getPrototypeOf()`的`API`会获取对应对象的`[[prototype]]`字段的内容。

```javascript
const obj = {}
console.log(Object.getPrototypeOf(obj) === Object.prototype)
//输出结果
true
```

可以看出，如果在创建时未指定其`[[prototype]]`的指向，则会默认指向`Object.prototype`

而`prototype`通常是针对原型函数而言的，`.prototype`会获取以**原型函数**实例化的对象的原型。

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}
var Outer = new Person("Outer", 19);
console.log(Object.getPrototypeOf(Outer) === Person.prototype);
//输出结果
true
```

上述代码中，`Person.prototype`实际是就是以**原型函数**`Person`来实例化的对象`Outer`的原型，类似于将该**原型函数**给抽象成一个虚拟的**对象**。

```javascript
Object:constructor: ƒ Person(name, age)
```

上述代码便是所谓**抽象出来的对象**。

实际上，如果想访问一个对象的`[[prototype]]`，应该通过`__proto__`取检索访问

```javascript
const source = {
        name:"Outer"
    }
const obj = Object.create(source)
console.log((obj.__proto__)===source)
//输出结果
true
```

总而言之，`.prototype`能够索引到构造函数抽象出的虚拟**对象**，而不能获取任意对象的`[[prototype]]`字段。能够获取任意对象的`[[prototype]]`字段的是`__proto__`属性。

#### b.原型链

每个非空非`Object`的对象都有其对应的原型，这些原型组成一条链表，便是**原型链**。原型链的末尾是`Null`

```javascript
const source = {
        name:"Outer"
    }
const obj = Object.create(source)
```

如上述代码构建的原型链即为：

```javascript
obj -> source -> Object -> Null
```

#### c.原型更改

我们可以通过`Object.create()`的`API`来为某个对象添加**方法**。

```javascript
const obj = Object.create({
    Say:function(){
        console.log("Hello!")
    }
})
obj.Say()
```

除此之外，我们可以为其原型**增添或更改**其**字段**或**方法**。

```javascript
const obj = {}
Object.prototype.Say = function(){
    console.log("Hello!")
}
obj.Say()
```

或者通过`__proto__`来访问一个**对象**的**原型**，并进行更改

```javascript
const source = {
    name:"Outer"
}
const obj = Object.create(source)
obj.__proto__.name = "Cyrex"
console.log(obj)
//输出结果
obj -> source{
    name:"Cyrex",
} //这里进行了抽象表达来展现原型链
```

***

有了上述知识，我们便可以解释`new`的过程了：

1. 创建一个新的对象
2. 把该对象的`__proto__`属性设置为构造函数的`prototype`属性，即完成**原型链**
3. 执行构造函数中的代码，构造函数中的`this`指向该对象
4. 返回该对象obj

***

## 三.时间库

### 1.时间戳

在`javascript`中获取时间戳需要使用`new Date().getTime()`

其起始时间是`1970-01-01 00:00:00`，即`Unix`纪元

```javascript
console.log(new Date().getTime())
//输出结果
1721745613633
```

注：这个时间戳是**毫秒级别的13位时间戳**

时间戳与时间的转换可以通过`Date()`来实现

```javascript
const timer = new Date(1721745613633)
console.log(timer)
//输出结果
Tue Jul 23 2024 22:40:13 GMT+0800 (中国标准时间)
```

也可以直接通过`new Date()`来获得标准时间

```javascript
console.log(new Date())
//输出结果
Tue Jul 23 2024 22:42:00 GMT+0800 (中国标准时间)
```

### 2.日期对象

与其他语言的**时间库**一样，我们可以将`Date()`的时间进行拆分

```javascript
const date = new Date()
console.log(date.getFullYear())
console.log(date.getMonth()+1)
console.log(date.getDate())
console.log(date.getHours())
console.log(date.getMinutes())
console.log(date.getSeconds())
//输出结果
2024
7
23
22
58
17
```

其中要注意的是，`data.getMonth()`获取的月份是从0开始的，因此一般要进行`date.getMonth()+1`来获取正确的月份。

### 3.格式化输出

通过`javascript`的字符串**格式化**，我们可以轻松实现**格式化输出**

```javascript
const nowTime = function(){
    let timer = new Date()
    let y = timer.getFullYear()
    let M = timer.getMonth() + 1
    let d = timer.getDay()
    let h = timer.getHours()
    let m = timer.getMinutes()
    let s = timer.getSeconds()
    return `${y}-${M}-${d} ${h}:${m}:${s}`
}
console.log(nowTime())
//输出结果
2024-7-2 23:8:12
```

但这样不符合我们常规的时间表达方式，通常而言我们要在不足两位时补足0，而不是直接写1位。

```javascript
const nowTime = function(){
    let timer = new Date()
    const y = timer.getFullYear().toString()
    const M = (timer.getMonth() + 1).toString().padStart(2, "0")
    const d = timer.getDate().toString().padStart(2, "0")
    const h = timer.getHours().toString().padStart(2, "0")
    const m = timer.getMinutes().toString().padStart(2, "0")
    const s = timer.getSeconds().toString().padStart(2, "0")
    return `${y}-${M}-${d} ${h}:${m}:${s}`
}
console.log(nowTime())
//输出结果
2024-07-23 23:18:48
```

在`javascript`中，存在`padStart()`方法可以将字符串进行补零处理，但其必须要在**字符串**后，因此需要`toString()`将`Date()`返回的数字转化为字符串，然后再进行补零处理。

```javascript
padStart(num,"ch")
```

其中`num`是需要补的位数，如果不足`num`则会在前边补`ch`直至该**字符串**的长度等于`num`

### 4.时间计算

通过`new Date()`类型自带的方法`set`可以自定义其时间

与`get`类似，`set`需要加上对应的时间名

```javascript
let timer = new Date()
timer.setFullYear(timer.getFullYear() - 1)
timer.setMonth(timer.getMonth())
timer.setDate(timer.getDate() - 1)
timer.setHours(timer.getHours() - 1)
timer.setMinutes(timer.getMinutes() - 1)
timer.setSeconds(timer.getSeconds() - 1)
const nowTime = function(timer){
    const y = timer.getFullYear().toString()
    const M = (timer.getMonth() + 1).toString().padStart(2, "0")
    const d = timer.getDate().toString().padStart(2, "0")
    const h = timer.getHours().toString().padStart(2, "0")
    const m = timer.getMinutes().toString().padStart(2, "0")
    const s = timer.getSeconds().toString().padStart(2, "0")
    return `${y}-${M}-${d} ${h}:${m}:${s}`
}
console.log(nowTime(timer))
```

如上，我们便将每一个时间单位都进行了减一处理。

除了这种计算外，如果我们有两个不同的**时间戳**，我们可以对其进行相减获得相差的**毫秒数**

```javascript
let Timestr = new Date().getTime() + 2313141241

let now = new Date()
let then = new Date(Timestr) //假定Timestr是一个时间戳
let diff = then - now
let s = diff / 1000
let m = s / 60
let h = m / 60
console.log(h)
//输出结果
642.5392336111111
```

