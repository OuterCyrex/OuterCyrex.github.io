---
layout:       post
title:        "前端基础学习之JavaScript part three"
author:       "OuterCyrex"
header-style: text
catalog:      true
tags:
    - 前端
    - JavaScript
---

# 三.类、模块化与BOM

## 一.类

### 1.创建类

**类 **(`class`) 这一概念源于**面对对象编程**，是**对象**的抽象体现。**实例化**的**类**便是我们所知的**对象**。

此前的`javascript`是没有**类**这一概念的，而是使用`prototype`**原型**来实现**面对对象**的编程效果。

在之后的更新中，`javascript`加入了**类**，来减少通过**构造函数**等**原型**来构建**对象**的麻烦。

如此前我们要通过**构造函数**来定义一个**对象**：

```javascript
const init = function(name,age){
    this.name = name;
    this.age = age;
}
init.prototype.Say = function(){
    console.log("Hello,I'm " + this.name)
}
const obj = new init("Outer",19)
console.log(obj)
obj.Say()
//输出结果，此处将整个原型链输出
init {name: 'Outer', age: 19}
age: 19
name: "Outer"

[[Prototype]]: Object
Say: ƒ ()
constructor: ƒ (name,age)

[[Prototype]]: Object
```

可能是意识到这样构建**对象**的过程过于冗杂，`javascript`为我们提供了**语法糖**

```javascript
class init{
    name
    age
    constructor(name,age) {
        this.name = name
        this.age = age
    }
    Say(){
        console.log("Hello,I'm " + this.name)
    }
}
const obj = new init("Outer",19)
console.log(obj)
obj.Say()
//输出结果，此处将整个原型链输出
init {name: 'Outer', age: 19}
age: 19
name: "Outer"

[[Prototype]]: Object
Say: ƒ Say()
constructor: class init
    
[[Prototype]]: Object
```

可以看到，此处的`[[prototype]]`内的`constructor`字段是`class init`，即我们此前定义的**类**，而非**构造函数**。

一个**类**常常由三个成分构成，分别是**成员变量，构造函数`constructor`和方法**。以上方示例中的**类**为例来介绍：

```javascript
class 类名 {
    name
    age
    constructor(name,age){
        this.name = name
        this.age = age
    }
    Say(){
        console.log("Hello,I'm " + this.name)
    }
}
```

其中`name`与`age`被称为**成员变量**，是一个**类**的基本构成内容。

`constructor`类似`python`中的`__init__`，即此前创建对象用的**构造函数**。

在类中直接声明**函数**字段，实例化该**类**后该函数便成为对应对象的**方法**。

### 2.继承

若不引入类，而是通过`javascript`自身的**原型**来定义，其过程较为冗杂。

```javascript
const parent = function(name,age){
    this.name = name;
    this.age = age;
}
parent.prototype.Say = function(){
    console.log("Hello,I'm " + this.name)
}
const sub = function(){
    this.sex = "Male"
}
sub.prototype = new parent("Outer",19)
console.log(new sub())
```

这个过程实际是**原型链**的对接，但需要重复定义多个原型，过于麻烦。

在引入**类**后，我们可以轻松实现**类的继承**。

```javascript
class parent{
    name
    age
    constructor(name,age) {
        this.name = name
        this.age = age
    }
    Hello(){
        console.log("Hello!")
    }
}
class sub extends parent{
    job
    constructor(name,age,job) {
        super(name,age);
        this.job = job
    }
    Say(){
        console.log("My Job is " + this.job)
    }
}

const obj = new sub("Outer",19,"Programmer")
console.log(obj)
obj.Say()

//输出结果
sub {name: 'Outer', age: 19, job: 'Programmer'}
age: 19
job: "Programmer"
name: "Outer"

[[Prototype]]: parent
Say: ƒ Say()
constructor: class sub

[[Prototype]]: Object
Hello: ƒ Hello()
constructor: class parent
    
[[Prototype]]: Object
//Console.log的输出
My Job is Programmer
```

如上述的代码所示，我们需要`extend`来在**父类**的**原型链**上接入**子类**。`super`则可以将**父类**的成员变量直接传给**子类**。

通过上述的继承过程，我们获得了这样一条**原型链**：

```javascript
sub -> class sub -> class parent -> Object -> Null
```

### 3.静态属性和方法

**静态属性与静态方法**类似于类中的常量，一般只能被**调用**。

首先是**静态方法**，**静态方法**类似于一个封装好的**函数**，这一点于`java`是如出一辙的

```javascript
class Math{
    static power(a,b){
        let total = 1;
        for(let i = 0;i < b;i++){
            total *= a
        }
        return total
    }
}
console.log(Math.power(2,5))
//输出结果
32
```

通过`static`属性，可以让该**方法**与**类的实例**无关，其不能被**类的实例**调用，而只能通过**类本身**来调用。

某种程度上，可以认为**静态方法**就是一个**函数**，只是这个函数被封装在了其**类**里，这是**面对对象编程**的常见概念。

之后是**静态属性**，与**静态方法**一样，其不需要被**实例化**，而是直接访问**类**来获取。

```javascript
class Math{
    static Pi = 3.1415926535
    static Area(r){
        return r * r * Math.Pi
    }
}
console.log(Math.Pi)
console.log(Math.Area(2))
```

总而言之，通过**静态属性与静态方法**，我们可以实现对**特定变量和方法**的**封装**，方便更好的调用。

## 二.模块化

**模块化**是指将一个复杂的程序依据一定的规范封装成几个块，是项目开发中的重要内容，可以帮助我们更好的**分割开发任务**，去除冗余内容，避免重复写入。

### 1.引入外部文件

在`HTML`中引入一段`javascript`文件只需要在`<script>`标签内写入文件路径即可

```html
<script src="./export.js" type="module"></script>
<!--export.js的内容-->
console.log("Hello World!")
```

注意，此处加`type = "module"`是`ES6`的新规范，如果涉及到`ES6`的新特性而未加这段代码则会报错。因此一般建议将其加上。

### 2.引入部分内容

通常情况下，如果引入的`javascript`文件内容过多，仍然会导致冗余和编写效率下降。我们可以选择只引入特定的几个变量或函数，来减少引用内容。

```javascript
<script type="module">
    import {obj,Say,Pi} from './testJS.js'
    console.log(obj)
    Say()
    console.log(Pi)
</script>

//testJS.js的内容
export const obj = {
    name:"Outer",
    age : 19,
}
export const Say = function(){
    console.log("Hello!")
}
export const Pi = 3.1415926535

//输出结果
{name: 'Outer', age: 19}
Hello!
3.1415926535
```

这里需要通过`import`来导入对应的变量或函数，其格式为

```javascript
import {...list} from '文件路径'
```

其中`...list`即要引入的变量或函数的**变量列表**。

而在对应的`javascript`文件中，我们需要将要某个变量变为`public`即**公有**，或被称为**导出**，只需要在变量或函数前加上`export`关键字即可。

```javascript
export const obj = {
    name:"Outer",
    age : 19,
}
export const Say = function(){
    console.log("Hello!")
}
export const Pi = 3.1415926535
```

### 3.设置导出默认值

我们可以设置`import`的**默认值**，这样当导入值**未声明**时，便会导入**默认值**。

```javascript
<script type="module">
    import func from './export.js'
    console.log(func(5,6))
</script>

//export.js的内容
export default function sum(a,b){
    return a + b
}

//输出结果
11
```

我们可以发现，通过`import`语句，我们将`function sum(a,b){return a + b}`的值赋值给了`func`变量，因此我们可以在后边调用它。

注意，只有我们在**没有指定调用变量**时，才会获取默认值`default`。

但我们可以同时调用这两种`import`，其之间是不会产生冲突的。

```javascript
<script type="module">
    import {Say} from './export.js'
    Say()
    import func from './export.js'
    console.log(func(5,6))
</script>
```

### 4.IIFE

在`javascript`中存在一种语法，即可以在**立即执行函数**类封装一定的代码，有点类似于`go`语言的**闭包**。这样的语法被称为**立即执行的函数表达式**，缩写为`IIFE`。

```javascript
const Calc = (function (){
    const func = function(a, b){
        return a+b
    }
    return {
        add: func,
    }
}())

console.log(Calc.add(1,2))
```

上述代码我们在**立即执行函数**`calc`内定义了`func`函数，并让其返回一个对象，该对象只有一个方法`func`。

这样之后，我们每次调用`calc`，实际是调用其返回的对象`{add:func}`，通过检索对应的字段来获取**立即执行函数**内的函数`func`。

## 三.BOM

**BOM**即`Browser Object Model`，**浏览器对象模型**。**BOM**的主要对象即`window`。

`window`即**浏览器**内置的一个对象，包含着一系列操作**浏览器**的`API`。

| 关键字   | 作用                               |
| -------- | ---------------------------------- |
| History  | 用于记录浏览器访问的页面，便于跳转 |
| Location | 记录浏览器地址信息                 |
| Alert    | 弹出对话框                         |
| confirm  | 弹出询问框                         |
| prompt   | 弹出输入框                         |

### 1.关键字

接下来对上述几个关键字进行说明

首先是`History`关键字：

通过`History`的对应`API`，我们可以实现页面的**后退**和**前进**。

```javascript
<button onclick="forward()">前进</button>
<button onclick="back()">后退</button>
<button onclick="go(1)">前进1页</button>
<button onclick="go(-1)">后退1页</button>

<script>
   function forward(){
      history.forward()
  }
  function back(){
    history.back()
   }
  function go(n){
      history.go(n)
  }
</script>
```

如上述代码所示，`history.forward()`可以前进一页，`history.back()`则是后退一页。

而`history.go()`则可以根据传入的参数指定跳转几页。

之后是`Location`关键字：

以**URL**：`http://outercyrex.github.io:3000/login/user?name=Outer`为例。

| 属性              | 值                                 | 说明                                                         |
| ----------------- | ---------------------------------- | ------------------------------------------------------------ |
| location.portocol | http:                              | 页面使用的协议，通常是`http:`或`https:`                      |
| location.hostname | OuterCyrex.github.io               | 服务器域名                                                   |
| location.port     | 3000                               | 请求的端口号                                                 |
| location.host     | outercyrex.github.io:3000          | 服务器名及端口号                                             |
| location.origin   | `http://outercyrex.github.io:3000` | url的源地址，只读                                            |
| location.href     | 完整的url地址                      | 等价于`window.location`                                      |
| location.pathname | `/login/user?name=Outer`           | url中的路径和文件名，不会返回hash和search后面的内容，只有当打开的页面是一个文件时才会生效 |
| location.search   | `?name=Outer`                      | 查询参数                                                     |

可以对`location`的参数进行更改来进行页面的跳转。

此外，`location`还具有两个方法

| 方法               | 作用                                         |
| ------------------ | -------------------------------------------- |
| location.reload()  | 刷新当前页面                                 |
| location.replace() | 跳转至replace()内的URL页面且不会保存历史记录 |

### 2.对话框

如之前的表格所示，常见的对话框有三种，分别是**提示框**、**询问框**和**输入框**。

`alert`可以实现**提示框**，其内部是一个字符串，可以进行更改。

```javascript
alert("Hello,World!")
//输出结果
localhost:63342显示：Hello,World!
    
let name = "Outer"
alert(`${name},Hello,World!`)
//输出结果
localhost:63342显示：Outer,Hello,World!
```

然后是**询问框**`confirm`，其会根据用户操作来返回对应的`boolean`值。

```javascript
let name = "Outer"
console.log(confirm(`你是${name}吗？`))
//点击“是”之后的输出结果
true
```

最后是**输入框**`prompt`，会返回用户输入的值，也可以设置默认值。

```javascript
let name = "Outer"
console.log(prompt(`你是${name}吗？`))

//输入"是的，我是"后的输出结果
是的，我是

//设置默认值
prompt(`你是${name}吗？`,"感觉你是")
```

若点击取消则会返回`null`值。

注意：`confirm`和`prompt`都会**阻塞代码**，即若用户不进行操作，接下来的代码就不会运行。

### 3.计时器

在`javascript`中有两种计时器，分别是`setTimeout`和`setInterval`

| 计时器      |                                                              |
| ----------- | ------------------------------------------------------------ |
| setTimeout  | 倒计时执行函数，单位是毫秒，返回值为当前计时器的序号，且可以用`clearTimeout`来关闭 |
| setInterval | 间隔多少时间来执行函数，会一直执行，需要`clearInterval`来关闭。 |

分别介绍两个计时器：

首先是`setTimeout`，其单位为**毫秒**，且**1秒 = 1000毫秒**。

其语法为`setTimeout(function,time)`，`function`为要执行的函数，`time`为时间间隔。

```javascript
function handler(){
    console.log("中间件已执行！")
}
setTimeout(handler,3000)
//3秒后的输出结果
中间件已执行！
```

且`setTimeout`存在返回值，其返回值为当前计时器是文件内第几个。

```javascript
function handler(){
    console.log("中间件已执行！")
}
setTimeout(handler,3000)
console.log(setTimeout(handler,5000))
//输出结果
2
中间件已执行！
```

请**注意**，`setTimeout`的返回值是**立刻返回**的，而并不是等待计时器倒计时结束才返回的。

`clearTimeout`的 传入的参数应为一个`number`类型，也即`setTimeout`的返回值，

```javascript
function handler(){
    console.log("中间件已执行！")
}
setTimeout(handler,3000)
clearTimeout(1)
```

由于设置了clearTimeout，计时器关闭，无内容输出。

此后是`setInterval`，其语法为`setInterval(function,time)`，其中`function`为要调用的函数，`time`为间隔的时间。

```javascript
setInterval(()=>{
    let timer = new Date().toLocaleString()
    console.log(timer)
},3000)
//输出结果
2024/7/27 10:16:14
2024/7/27 10:16:17
2024/7/27 10:16:20
2024/7/27 10:16:23
2024/7/27 10:16:26
2024/7/27 10:16:29
......
```

`setInterval`的返回值也是其在当前文件中是第几个计时器，**注意**，此处的计时器包括`setTimeout`与`setInterval`

通过`clearInterval`来关闭计算器，若不关闭则会一直进行下去。

```javascript
let countdown = 3
setInterval(()=>{
    let timer = new Date().toLocaleString()
    console.log(timer)
    countdown--;
    if(countdown === 0)clearInterval(1)
},1000)
//输出结果
2024/7/27 10:21:07
2024/7/27 10:21:08
2024/7/27 10:21:09
```

### 4.防抖和节流

在项目开发中，要考虑到可能存在的由于用户多次点击导致某个函数**重复执行**的事件。

这里便需要两个函数，分别是`debounce`和`throttle`

**防抖**，即若用户在短时间内重复执行某个函数时，只计入最后一次点击的效果。

```javascript
<button onclick="handler('Outer')">按钮</button>

const out = function(name){
    console.log(name)
}
function debounce(fn,delay){
    let timer = null
    return function(){
        clearTimeout(timer)
        timer = setTimeout(()=>{
            fn.apply(this,arguments)
        },delay)
    }
}
let handler = debounce(out,1000)
```

对上述的`debounce`函数进行分析。

其传入值为要传入的函数`fn`和时间间隔`delay`，之后我们设置了`timer = null`，如果此前该函数已被调用，则`timer`的值会更改为`setTimeout`的返回值，这样我们便可以通过`clearTimeout`直接关闭其进程。（因为`clearTimeout`是全局性的，即使他在程序中位于`setTimeout`之前）

`fn.apply(this,arguments)`中的`this`指代的是`fn`本身，而`arguments`则是`javascript`的关键字，代表传入该函数的**参数列表**。在一起则相当于`fn(...arguments)`，等于调用了一遍函数。

**节流**，即在设定的一段时间内只能进行一次交互，交互一次后必须等待该间隔才能再次进行交互。如果重复交互则会在**间隔**完成后立即执行。

下面先设计一个简单的**节流函数**，其实现了在间隔内重复交互无效的效果。

```javascript
const out = function(name){
    console.log(name)
}
function throttle(fn,delay){
    let st = new Date().getTime()
    return function(){
        let time = new Date().getTime()
        if(time - st > delay){
            out.apply(this,arguments)
            st = new Date().getTime()
        }
    }
}
let handler = throttle(out,1000)
```

为了完善刚才的效果，我们需要加入`setTimeout`来实现**交互队列**

```javascript
function throttle(fn,delay){
    let st = new Date().getTime()
    let timer = null
    return function(){
        let time = new Date().getTime()
        clearTimeout(timer)
        if(time - st > delay){
            out.apply(this,arguments)
            st = new Date().getTime()
        }else{
            timer = setTimeout(()=>{
                out.apply(this,arguments)
            },delay - (time - st))
        }
    }
}
```

其思路很简单，如果我们在间隔时间内重复交互，会进入`else`选择项，其会设置将剩余的`delay`间隔进行完成后再交互。如果过多此的重复交互则会触发`clearTimeout`将多余的交互清除。

### 5.滚动

`window`内有两个关于滚动的参数，分别是`ScrollX`和`ScrollY`

```javascript
console.log(window.scrollY)
console.log(window.scrollX)
```

其分别对应了**水平滚动距离**和**垂直滚动距离**。且由于是`window`的参数，因此只对`body`有效。

对于滚动的操作，存在**相对滚动**和**绝对滚动**。**相对滚动**的参照是当前**滚动条**的位置，而**绝对滚动**的参照是整个`body`。

| 语句     | 参照                               |
| -------- | ---------------------------------- |
| scrollBy | 相对滚动，参照的是当前的滚动条位置 |
| scrollTo | 绝对滚动，参照的是`body`           |

其参数分别对应的是**x轴**和**y轴**。

```javascript
window.scrollTo(0,1000)
window.scrollBy(0,1000)
```

更重要的是，二者内部均可以传入**对象**。

```javascript
window.scrollTo({top: 1200, behavior: "smooth"})
```

如上述代码传入了一个对象，实现了向下滚动1000px，且是**平滑滚动**。**平滑滚动**为**滚动**添加了**过渡**，让其更加自然，在实际开发中经常使用。

------

在页面中有三个很重要的概念：**屏幕、窗口和视口**。

| 页面内容 | 作用                                           |
| -------- | ---------------------------------------------- |
| 屏幕     | 整个电脑显示的屏幕                             |
| 窗口     | 浏览器的操作页面，包括标签页和其他内容以及视口 |
| 视口     | 网站页面内部显示的内容，且不包括滚动条         |

```javascript
console.log("视口",innerWidth,innerHeight)
console.log("窗口",outerWidth,outerHeight)
console.log("屏幕",screen.width,screen.height)
//输出结果
视口 849 903
窗口 1420 1032
屏幕 1920 1080
```

**屏幕**值一般是固定的。**窗口**和**视口**会根据实际大小而变化。

------

### 6.浏览器信息

通过`navigator`字段，我们可以获取当前用户的浏览器信息，以便根据用户环境更改页面内容。

| 属性                 | 描述                     |
| -------------------- | ------------------------ |
| navigator.userAgent  | 获取浏览器的整体信息     |
| navigator.appName    | 获取浏览器名称           |
| navigator.appVersion | 获取浏览器的版本号       |
| navigator.platform   | 获取当前计算机的操作系统 |

```javascript
console.log(navigator.userAgent)
console.log(navigator.appName)
console.log(navigator.appVersion)
console.log(navigator.platform)
//输出结果
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36

Netscape

5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36

Win32
```

