---
layout:       post
title:        "前端基础学习之JavaScript part four"
author:       "OuterCyrex"
header-style: text
catalog:      true
tags:
    - 前端
    - JavaScript

---

# 四.DOM

**DOM**是`Document Object Model`的首字母缩写，即**文档对象模型**。

其允许开发人员进行**获取、添加、修改和移除**页面内特定元素的操作。

## 一.获取元素

下列内容介绍了一些**获取元素**的方法。注意，如果未获取到对应元素，下述方法均会返回`null`值。

### 1.ID获取

通过`ID`获取主要是通过`document`类中的`getElementById`方法来实现，其会返回**该标签的内容**。

```javascript
const dom = document.getElementById("box")
console.log(dom)
//返回值
<div id="box"></div>
```

### 2.Class获取

与ID获取一致，`Class`获取只需要使用`getElementsByClassName`即可，但注意，由于`Class`的**标签并不唯一**，因此该方法会返回一个**数组**。

```javascript
const dom = document.getElementsByClassName("box")
console.log(dom)
//输出结果
HTMLCollection(2) [div.box, div.box]
0: div.box
1: div.box
length: 2
[[Prototype]]: HTMLCollection
```

### 3.其他获取方法

除了上述介绍的两种获取方法外，还有以下几种获取方法：

| 方法                   | 作用                                                         |
| ---------------------- | ------------------------------------------------------------ |
| getElementById         | 根据id来获取对应标签                                         |
| getElementsByClassName | 根据类来获取对应标签，返回值为数组                           |
| getElementsByTagName   | 根据标签名来获取对应标签，返回值为数组                       |
| getElementsByName      | 根据标签的`name`字段来获取对应标签，返回值为数组             |
| getElementsByTagNameNS | 在提供的URL内根据标签离开获取对应标签，返回值为数组，语法为`getElementsByTagNameNS("URL",Tag)` |

### 4.CSS选择器获取

上述的方法虽然多样，但对我们而言自由度不足，如果我们需要寻找非`Id`非`Class`等的标签时，可能会较为麻烦。

在`DOM`中提供了一种根据`CSS`选择器来获取对应标签的方法，确保所有标签都能被查找。

现假定存在下述`HTML`标签：

```html
<div id="ID"></div>
<input name="Outer">
<div class="box"></div>
<div class="box"></div>
<article></article>
```

我们便可以根据其对应的`CSS`选择器进行查找

```javascript
console.log(document.querySelector("#ID"))
console.log(document.querySelector("input[name='Outer']"))
console.log(document.querySelector(".box"))
console.log(document.querySelector("article"))
//返回值
<div id="ID"></div>
<input name="Outer">
<div class="box"></div>
<article></article>
```

注意，这里存在两个方法：

| 方法             | 作用                                               |
| ---------------- | -------------------------------------------------- |
| querySelector    | 返回当前HTML中第一个满足该选择器的标签             |
| querySelectorAll | 获取当前HTML中所有满足该选择器的标签并返回一个数组 |

## 二.元素对象

通过上述方法获取的**标签**可以被称为**元素对象**。在`DOM`其带有一系列属性可以供我们获取。

以`<div id="ID">你好</div>`为例：

```javascript
console.log(document.querySelector("#ID").tagName)
console.log(document.querySelector("#ID").id)
console.log(document.querySelector("#ID").textContent)
//输出结果
DIV
ID
你好
```

除了上述的三种属性，还有很多其他的属性可以调用，在此不做赘述。

### 1.属性操作

获取了标签之后，我们可以对该标签的属性进行**获取、添加与修改**。

假定存在标签`<a href="http://outercyrex.github.io/"></a>`。

我们可以通过`getAttribute`来**获取属性**

```javascript
const dom = document.querySelector("a")
console.log(dom.getAttribute("href"))
//输出结果
http://outercyrex.github.io/
```

通过`setAttribute`来**修改属性**

```javascript
const dom = document.querySelector("a")
dom.setAttribute("href","www.baidu.com")
console.log(document.querySelector("a"))
//输出结果
<a href="www.baidu.com"></a>
```

最后可以通过`removeAttribute`来**删除属性**

```javascript
const dom = document.querySelector("a")
dom.removeAttribute("href")
console.log(document.querySelector("a"))
//输出结果
<a></a>
```

### 2.dataset

如果一个标签的属性过多，可能会存在不易管理的情况，如果我们可以将所有属性按照**对象**的形式更改，可以提升管理效率。`data`关键字可以帮我们实现这种操作。

在一个标签的对应属性上加上`data`，可以将其变为`data`属性

```html
<div data-name="Outer" data-box="newBox"></div>
```

在此以上述的标签为例：

```javascript
const dom = document.getElementById("1")
console.log(dom.dataset)
console.log(dom.dataset.name)
console.log(dom.dataset.box)
console.log(dom.dataset["name"])
//输出结果
DOMStringMap {name: 'Outer', box: 'newBox'}
Outer
newBox
Outer
```

如上述代码的返回结果可知，`dataset`会将对应标签的`data`属性转化为一个**对象**，我们可以通过访问对象的方法对其进行查询、添加、修改和删除。

``` javascript
const dom = document.getElementById("1")
dom.dataset.date = new Date().toString()
console.log(dom)
//输出结果
<div id="1" data-name="Outer" data-box="newBox" data-date="Sun Jul 28 2024 21:33:29 GMT+0800 (中国标准时间)"></div>
```

删除则使用`delete`操作即可，与**对象**的操作一致。

### 3.classList

对于类的操作，`dom`提供了`classList`来快捷的操作一个标签的类属性。

假定有一个`class = "box"`的`<div>`标签，则：

```javascript
const dom = document.getElementsByClassName("box")
const list = dom[0].classList
console.log(list)
//输出结果
DOMTokenList ['box', value: 'box']
```

`classList`为我们提供了多种操作

| 方法                 | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| classList.add()      | 为该标签的`class`属性增加新的值                              |
| classList.contains() | 判断当前标签的`class`属性内是否含有对应的字段，返回布尔值    |
| classList.toggle()   | 若当前标签的`class`属性含有对应的字段则将其删除，若没有则添加该字段。 |
| classList.remove()   | 删除对应标签的`class`属性中对应的字段                        |
| classList.replace()  | 将对应标签的`class`属性中对应的字段修改为指定内容，语法为`replace("旧字段","新字段")` |

### 4.标签内容

首先介绍三个属性：`innerText`、`innerHTML`、`outerHTML`

| 属性      | 作用                              |
| --------- | --------------------------------- |
| innerText | 返回当前标签内部的`text`内容      |
| innerHTML | 返回当前标签内部的全部HTML语句    |
| outerHTML | innerHTML的内容加上当前标签的内容 |

以下述的`HTML`文件为例

```html
<div class="box">
    你好！
    <a href="www.baidu.com"></a>
</div>
```

其对应的属性内容为

```javascript
const dom = document.getElementsByClassName("box")
console.log(dom[0].innerText)
//输出结果
你好！

console.log(dom[0].innerHTML)
//输出结果
你好！
<a href="www.baidu.com"></a>

console.log(dom[0].outerHTML)
//输出结果
<div class="box">
    你好！
    <a href="www.baidu.com"></a>
</div>
```

通过这种方式，我们可以通过更改对应的`innerHTML`等内容来更改对应的`HTML`文件。

### 5.style属性

接下来介绍`style`属性，`dom`为我们提供了`style`属性让我们可以访问当前标签的各类`CSS`属性。

以下述的`html`代码为示例

```html
<div id="box" style="font-size:20px;background-color: blue;border:2px solid red;height:100px;width: 100%"></div>
```

我们可以通过`style`属性来对其各个属性进行更改

```javascript
const dom = document.getElementById("box")
console.log(dom.style.backgroundColor)
console.log(dom.style.fontSize)

dom.style.height = "200px"
//输出结果
blue
20px
```

**注意**，修改时传入的值应为**字符串**。

此外，我们可以通过`getComputedStyle`来获取`style`内的计算结果

```html
<div id="box" style="height:calc(100% - 20px)"></div>
```

输出如下：

```javascript
const dom = document.getElementById("box")
console.log(dom.style.height)
console.log(getComputedStyle(dom).height)
//输出结果
calc(100% - 20px)
980px
```

注意，上述的代码只能获取行内的`CSS`设置，如果是在`<header>`内的`<style>`则是无法通过这种方式进行修改的。

而且`document.style`后的属性是按照**小驼峰命名法**命名的，如`background-color`应为`backgroundColor`。也可以不用**小驼峰命名法**，而是使用`[]`将对应的内容括起来，如`[background-color]`。

## 三.节点操作

这里的节点便是`HTML`中的内容，如**标签**，所说的操作即**增删改查**。

### 1.创建节点

通过`document.createElement`操作，我们可以通过`javascript`来创建标签

```javascript
const dom = document.createElement("a")
dom.setAttribute("href","http://outercyrex.github.io")
dom.innerText = "Outer Blog"
```

通过上述操作，我们创建了一个标签`<a>`，但此时还为将其插入`HTML`内。

存在很多插入的方法，我们以`appendChild`为示例：

```javascript
const dom = document.createElement("a")
dom.setAttribute("href","http://outercyrex.github.io")
dom.innerText = "Outer Blog"

const box = document.getElementById("box")
box.appendChild(dom)
```

### 2.插入子节点

关于插入的操作有很多，首先介绍**插入子节点**，即将当前创建的标签插入某个标签的`innerHTML`中。

| 方法         | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| appendChild  | 在父节点的最后一个子节点后插入一个节点                       |
| append       | 在父节点的最后一个子节点后插入多个不同的节点，即可以传入多个值 |
| insertBefore | 选择在父节点的第几个子节点之前插入节点，语法为`insertBefore(Element,Node)` |

重点介绍一下`insertBefore`，示例如下：

```javascript
const dom = document.createElement("a")
dom.setAttribute("href","http://outercyrex.github.io")
dom.innerText = "Outer Blog"

const box = document.getElementById("box")
box.insertBefore(dom,box.children[2])
```

其输出结果为：

```html
<div id="box">
    <div class="1"></div>
    <div class="2"></div>
    <a href="http://outercyrex.github.io">Outer Blog</a>
    <div class="3"></div>
</div>
```

除此之外，如果第二个值，即`Node`为`null`，则效果与`append`一致，插入**最后一个子节点**之后。

------

此处介绍一下如何获取一个节点的**父节点**和**子节点**

通过`dom`获取的节点具有两个属性：`parent`和`children`

前者会返回**当前节点的父节点**，后者会返回**当前节点的子节点列表**。

```javascript
console.log(box.parent)
console.log(box.children)
//输出结果
undefined
[div.1, a, div.2, div.3]
```

这里由于`box`的父节点为`body`，因此返回了`undefined`

------

### 3.添加兄弟结点

添加兄弟结点的操作涉及两个方法：`before`和`after`

| 方法   | 作用                         |
| ------ | ---------------------------- |
| before | 在对应的子节点之前插入该结点 |
| after  | 在对应的子节点之后插入该结点 |

还是以之前的`box`和`dom`标签为例：

```javascript
const box = document.getElementById("box")
box.children[0].after(dom)
```

输出结果为：

```html
<div id="box">
    <div class="1"></div><a href="http://outercyrex.github.io">Outer Blog</a>
    <div class="2"></div>
    <div class="3"></div>
</div>
```

尤其要注意的是其语法为：

```javascript
对应的兄弟节点.after(要插入的节点)
```

### 4.删除节点

删除节点由两个方法可以进行调用，分别是`removeChild`和`remove`

| 方法          | 作用                                                         |
| ------------- | ------------------------------------------------------------ |
| removeChild() | 移除对应节点的特定子节点，传入的参数为要删除的节点。且该方法会返回被删除的节点 |
| remove()      | 移除使用该方法的结点                                         |

接下来对二者进行示例：

```javascript
const removed = box.removeChild(box.children[0])
console.log(removed)

box.children[2].remove()
//输出结果
<div class="1"></div>
```

对应的`HTML`文件：

```html
<div id="box">
    <div class="2"></div>
</div>
```

## 四.事件

**事件**，即**用户**或**浏览器**本身的某种行为，一般是**用户**对页面的一些动作引起的。

例如，单击某个链接或按钮、在文本框中输入文本、按下键盘上的某个按键、移动鼠标等等

当**事件**发生时，可以使用`javascript`中的**事件监听器**来**检测并执行某些特定的程序**。

一般情况下事件的名称都是以单词`on`开头的，例如**点击事件**`onclick`、**页面加载事件**`onload `等。

### 1.事件绑定

绑定事件有三种方法：

首先是**标签内绑定**，即在标签内写入对应的属性，如下述的`oninput`属性，并让其对应`<script>`中的函数。

```html
<input placeholder="用户名" oninput="input(event)">
<script>
    function input(e){
        console.log(e.target.value)
    }
</script>
```

第二种是**在`dom`中绑定事件**，通过`dom`指向对应的标签，之后进行赋值

```javascript
const btn = document.querySelector("button")
btn.onclick = function(){
    alert("你好")
}
```

最后一种也是最常见的，即**添加事件监听器**

```javascript
const btn = document.querySelector("button")
const alertEvent = function(){
    alert("你好")
}
btn.addEventListener("click",alertEvent)
```

通过这种方法设置事件监听器，我们可以随时对其移除。

```javascript
btn.removeEventListener("click",alertEvent)
```

其中常见的事件见下表

| 事件             | 内容                                                         |
| ---------------- | ------------------------------------------------------------ |
| load             | 加载成功                                                     |
| resize           | 窗口大小变化                                                 |
| scroll           | 滚动事件                                                     |
| **焦点事件**     |                                                              |
| blur             | 失去焦点                                                     |
| focus            | 获得焦点                                                     |
| **鼠标点击事件** |                                                              |
| click            | 用户单击鼠标左键或按下回车键触发                             |
| dbclick          | 用户双击鼠标左键触发                                         |
| mousedown        | 在用户按下了任意鼠标按钮时触发                               |
| mouseenter       | 在鼠标光标从元素外部首次移动到元素范围内时触发，此事件不冒泡 |
| mouseleave       | 元素上方的光标移动到元素范围之外时触发，不冒泡               |
| mousemove        | 光标在元素的内部不断的移动时触发                             |
| mouseover        | 鼠标指针位于一个元素外部，然后用户将首次移动到另一个元素边界之内时触发 |
| mouseout         | 用户将光标从一个元素上方移动到另一个元素时触发               |
| mouseup          | 在用户释放鼠标按钮时触发                                     |
| **键盘事件**     |                                                              |
| keydown          | 当用户按下键盘上的任意键时触发，若按住不放则会重复触发       |
| keypress         | 当用户按下键盘上的字符键时触发，若按住不放则会重复触发       |
| keyup            | 当用户释放键盘上的键时触发                                   |
| textInput        | 这是唯一的文本事件，用意是将文本显示给用户之前更容易拦截文本 |

### 2.事件冒泡

当一个**事件**被触发时，它首先在**最内层的元素**（也称为**目标元素**或**事件源**）上发生

然后，这个事件会向上**冒泡**，依次触发其父元素上的**同一类型事件**，一直**冒泡**到**最外层元素**，通常是`document`**对象**

这种冒泡机制允许我们在**父元素**上设置**事件处理器**，以便在**子元素**的事件发生时执行特定的操作。

```javascript
<div class="p1" style="width: 50%;height:50%;background-color: red">
    <div class="p2" style="width: 50%;height:50%;background-color: blue">
        <div class="p3" style="width: 50%;height:50%;background-color: green">点击将触发事件冒泡</div>
    </div>
</div>

<script>
    const p1 = document.querySelector(".p1")
    const p2 = document.querySelector(".p2")
    const p3 = document.querySelector(".p3")
    p3.onclick = function (e){
        console.log("p3", e.target)
    }
    p2.onclick = function (e){
        console.log("p2", e.target)
    }
    p1.onclick = function (e){
        console.log("p1", e.target)
    }
</script>
//输出结果
p3
p2
p1
```

此处在我们点击`<div class="p3">`之后，控制台共输出了三个结果，这是由于内部的标签在**触发事件**时，会触发其父节点的同种事件。

或者这样想，`click`了`<div class="p3">`的同时其实我们也`click`了其所有父节点。

如果我们想关闭这种**事件冒泡**，可以通过添加下述语句：

```javascript
p3.onclick = function (e){
    console.log("p3", e.target)
    e.stopPropagation()
}
```

即`event`类型带有一个`stopPropagation()`方法，其可以阻止当前**事件监听器向**其**父节点**申请冒泡。

此外还有需要补充的是，`dom`为很多事件设置了默认选项，我们可以选择更改对应的参数来关闭这些默认选项。

```html
<a class="hyperText" href="http://outercyrex.github.io">Outer Blog</a>
```

如上述标签，如果我们点击的话便会直接跳转至对应页面。

我们可以选择增加确定的**对话框**。

```javascript
const a = document.querySelector("a")
a.onclick = function (e){
    const ok = confirm(`你确定要访问 ${e.target.getAttribute("href")}吗`)
    if (!ok){
        e.preventDefault()
    }
}
```

其中的`confirm`会**阻塞代码**，防止直接跳转。若我们选择**否**，则该事件的`preventDefault()`方法会调用，关闭直接跳转功能。

### 3.事件委托

**事件委托**，又叫**事件代理**，即利用**事件冒泡**，只指定一个**事件监听器**，就可以管理某一类型的所有事件。

```javascript
<div class="box" style="width: 50%;height:50%;background-color: #96e6a1">
    <div class="subBox">1</div>
    <div class="subBox">2</div>
    <div class="subBox">3</div>
    <div class="subBox">4</div>
    <div class="subBox">5</div>
    <div class="subBox">6</div>
</div>
<script>
    const box = document.querySelector(".box")
    const click = function(){
        console.log("Hello")
    }
    box.addEventListener("click",click)
</script>
```

如上述代码所示，我们每次点击`.box`的子节点，都会触发其父节点的事件。这样我们其实是实现了**事件委托**，即只设置了**父节点**的**事件监听器**，便可对所有**子节点**也设置同样的属性。

### 4.事件循环

事件循环是一种机制，用于处理**异步事件**和**回调函数**。它是`javascript`运行时环境的一部分，负责**管理事件队列**和**调用栈**。

**事件循环**的核心是一个**事件队列**，所有的事件都被放入这个队列中，然后按照顺序依次执行。如果**队列为空**，`javascript`会等待新的任务加入队列。当`javascript`代码执行时，所有**同步任务**都会被立即执行，而**异步任务**则会被放入事件队列中。

```javascript
console.log(1)
console.log(2)
setTimeout(() => {
    console.log(3)
},0)
console.log(4)
//输出结果
1 2 4 3
```

出现上述结果的原因是`setTimeout`为**异步任务**，会等待所有**同步任务**结束后再进入**事件队列**。

更细致的进行区分的话，**异步任务**被分为了两种类型：**宏任务**和**微任务**。

| 异步任务 | 内容                                 |
| -------- | ------------------------------------ |
| 宏任务   | `setTimeout、setInterval、I/O`操作等 |
| 微任务   | `Promise、MutationObserver`等        |

注意**宏任务**和**微任务**的先后顺序：

当**事件循环**从**事件队列**中取出一个任务时，它会先执行所有**微任务**，然后再执行一个**宏任务**。

```javascript
console.log('1'); 
 
setTimeout(function() { 
    console.log('2'); 
    Promise.resolve().then(function() { 
        console.log('3'); 
    }); 
}, 0); 
 
Promise.resolve().then(function() { 
    console.log('4'); 
}); 
console.log('5'); 
 
// 输出结果
1 5 4 2 3
```

**同步任务**先完成因此`1`和`5`先被执行，之后是**微任务**`Promise`和**宏任务**`setTimeout`，将二者的返回值压入**事件队列**，此时事件队列便有**同步任务**`4`、`2`和**微任务**`3`，依次进行输出。

```javascript
console.log('1'); 
 
setTimeout(function() { 
    console.log('2'); 
    Promise.resolve().then(function() { 
        console.log('3'); 
    }); 
}, 0); 
 
Promise.resolve().then(function() { 
    console.log('4'); 
    setTimeout(function() { 
        console.log('5'); 
    }, 0); 
}); 
 
console.log('6'); 
 
// 输出结果
1 6 4 2 3 5
```

上述代码也同理，便于我们理解**事件队列**的**运作原理**。