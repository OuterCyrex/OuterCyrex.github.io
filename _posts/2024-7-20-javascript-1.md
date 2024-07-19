---
layout:       post
title:        "前端基础学习之JavaScript part one"
author:       "OuterCyrex"
header-style: text
catalog:      true
tags:
    - 前端
    - JavaScript




---

# 一.变量、数组和流程控制

## 一.变量

在`JavaScript`中变量的定义常常使用下述三种：

| 变量类型 | 特点                                                         |
| -------- | ------------------------------------------------------------ |
| var      | 函数作用域，作用域提升，且全局变量可以被挂载到window上(不建议使用) |
| let      | 块级作用域，存在暂时性死区                                   |
| const    | 常量，声明就需要初始化 通常用于声明数组和对象                |

下边对上述内容进行解释：

**暂时性死区**即从**代码块开始**到**变量声明语句执行完毕**之间的区域内，该变量不可被使用。

```javascript
console.log(name)
{
    var name = "Outer"
}
console.log(name)
```

`var`的作用域是全局的，不存在**暂时性死区**，即在声明变量的语句之前即可使用变量，且作用域是全局的。

实际开发中常常不使用`var`来声明变量。

而`let`则存在**暂时性死区**，且作用域是**块级**的

```javascript
{
    let x = "Outer"
    console.log(x)
}
console.log(x)
```

上述代码中，第一个`log`会输出，第二个由于不在`let`的作用域内因此不会输出。

`const`定义常量，一旦声明便不可被修改

```javascript
const a = 12
a = 18
```

`Javascript`是一种动态弱类型语言，因此对变量的类型要求不高。在`JS`中存在两种等号

| 等号类型 | 作用                     |
| -------- | ------------------------ |
| ==       | 判断数值是否相同         |
| ===      | 判断数值和类型是否都相同 |

如：

```javascript
let a = 1
let b = '1'
console.log(a==b)
console.log(a===b)
//输出
true
false
```

## 二.数据类型

`Javascript`中的数据类型有8种：

| 类型      | 作用                          |
| --------- | ----------------------------- |
| number    | 数值类型，范围为 `± 2^53 - 1` |
| string    | 字符串类型                    |
| boolean   | 布尔类型                      |
| null      | 空值                          |
| undefined | 未定义                        |
| object    | 对象(类似结构体)              |
| symbol    | 表示唯一标识                  |
| bigInt    | 安全地存储和操作大整数        |

### 1.number和bigInt

`number`类型的数据范围是`± 2^53 - 1`，可以存储小数但存在一定**精度误差**。且注意作为一个函数时`Number()`的首字母`N`要大写

```javascript
let a = 1
let b = Number(2)
console.log(a,typeof a)
console.log(b,typeof b)
//输出结果
1 'number'
2 'number'
```

且对于**其他进制**的数字，存入`number`类型的变量后会变为对应的**十进制**

```javascript
const a2 = 0b110 //输出6
console.log(a2)
const a3 = 0o11 //输出9
console.log(a3)
const a4 = 0xa1 //输出161
console.log(a4)
```

其中对于`Number`类型的变量存在一些**方法**来实现一些效果：

| 方法或函数         | 作用                                       |
| ------------------ | ------------------------------------------ |
| toFixed(n)         | 是方法，保留n为小数                        |
| Number.parseInt()  | 是函数，取整数部分，但一般是针对字符串类型 |
| Number.isInteger() | 判断一个数是否为整数                       |
| Math.round()       | 四舍五入保留整数                           |
| Math.floor()       | 上取整保留整数                             |
| Math.ceil()        | 下取整保留整数                             |

注意，`Number.parseInt()`常常是针对字符串而言，因此对`Number`类型转换时编译会出现**警告**，但`javascript`是**弱类型语言**，因此仍然可以进行转换。

```javascript
let a = 3.1415926
console.log(a.toFixed(2))
console.log(Number.parseInt(a))
console.log(Number.isInteger(a))
//输出
3.14
3
false
```

此外，`javascript`的`Math`库还具有**取整**，**上取整**和**下取整**：

```javascript
console.log(Math.round(3.5)) // 四舍五入 4
console.log(Math.round(3.4)) // 四舍五入 3
console.log(Math.ceil(1.0000001)) // 向上取整 2
console.log(Math.ceil(1.9)) // 向上取整 2
console.log(Math.floor(1.9)) // 向下取整 1
console.log(Math.floor(1.001)) // 向下取整 1
```

与`Number`不同的是，`BigInt`只能存储**整数**，且其字节大小是动态变化的，数值越大字节数越大，且**`BigInt`类型不能直接与`Number`进行运算。**

### 2.String

在`javascript`中，`""`与`''`是完全等价的，不需要做区分。

`javascript`中的`String`类型与`Go`中的有异曲同工之处。

```javascript
const x="Outer"
console.log(x,typeof x)
const y=String("Cyrex")
console.log(y,typeof y)
//输出结果
Outer string
Cyrex string
```

与`go`中的字符串相似，可以直接进行下标的索引。

```javascript
const x="Outer"
console.log(x,typeof x)
console.log(x[1],typeof x[1])
```

与`go`不同的一点是，`javascript`支持依据字符内容来查询下标，若未查询到则输出`-1`

```javascript
const x="Outer"
console.log(x.indexOf(`t`))
console.log(x.indexOf(`z`))
//输出结果
2
-1
```

对于字符串类型，常见的**方法**包括：

| 方法     | 作用                                       |
| -------- | ------------------------------------------ |
| indexOf  | 可以根据内部的字符串来检索对应的下标       |
| split    | 根据某个特殊标识符来分割字符串，并返回数组 |
| slice    | 可以根据下标获取对应的切片                 |
| includes | 判断字符串内是否有匹配的子串               |
| trim     | 去除字符串中的空格                         |

对上述的一些方法进行简要的介绍，首先介绍`split`

```javascript
const x="Outer,Cyrex"
const a=x.split(',')
console.log(a[0])
//输出结果
Outer
```

其次是`slice`，会根据输入的下标进行切片，如果输入负数，则意味着**从字符串末尾开始计数**，即`负数代表的下标 = 字符串长度 + 负数`

```javascript
const x="OuterCyrex"
const a = x.slice(5,10)
console.log(a)
//输出结果
Cyrex
```

### 3.Boolean

**布尔值**只有两种情况：`true`和`false`

除了上述两种情况外，也可以将特定的值强转为**布尔值**。

```javascript
console.log(Boolean(0))
console.log(Boolean(''))
console.log(Boolean(null))
console.log(Boolean(undefined))
console.log(Boolean(false))
console.log(Boolean(NaN))
//注意区分，上述均为false，下方的为true
console.log(Boolean(' '))
```

### 4.object和symbol

`javascript`中的`object`与**结构体**相似但差别也较大，更像是一个映射组。

```javascript
let Obj={
    name:"Outer",
    age:19,
}
console.log(Obj)
```

也可以为其**字段名**加上**双引号或单引号**。

对于内容的检索存在两种方法，一种是类似**结构体**的方案，即`对象名.字段名`，另一种是类似**映射**的方案，即`对象名["字段名"]`。

```javascript
console.log(Obj.name,Obj["name"])
//输出结果
Outer Outer
```

对**对象的字段**进行增删改的操作如下：

在`javascript`中，由于是**弱类型语言**，其并不严格要求对象的字段信息，定义之后仍然可以进行增删字段。

增改的操作是一样的，只需要赋值即可。

若字段不存在则**增加新的字段**，若存在则**对已有的字段进行更改**。

```javascript
let Obj={
    name:"Outer",
    age:19,
}
Obj.sex="male"
Obj.name="Cyrex"
console.log(Obj)
```

删除操作则需要使用`delete`函数。

```javascript
let Obj={
    name:"Outer",
    age:19,
}
delete Obj.age
console.log(Obj)
```

此外，我们如果想让某个变量的内容作为字段名，可以使用`[]`来实现

```javascript
const x = "name"
const obj = {
    [x]: "Outer"
}
console.log(obj)
```

上述代码便实现了将`x`的内容`name`作为字段名的效果。

`javascript`中存在一种`symbol`类型，可以使字段名重复存在

```javascript
const x = Symbol("name")
const y = Symbol("name")
let Obj = {
    [x]:"Outer",
    [y]:"Cyrex",
}
console.log(Obj)
//输出结果
{Symbol(name): 'Outer', Symbol(name): 'Cyrex'}
```

可以理解为`Symbol`与`string`不同点在于`Symbol`给每个字符串一个**特殊的标识**来区分内容一样的字符串。

### 5.null和undefined

`null`值类似于`go`语言中的`nil`，即**空值**。**空值**是**确切存在的一个值**，但是其内容为空。

注意，`null`的数据类型为`Object`，类似与`go`中的**空接口**`interface{}`

与`null`不同的是，`undefined`表示值根本不存在或违背定义。

出现`undefined`的情况包括：

- 变量被声明了，但没有赋值时，就等于`undefined`
- 调用函数时，应该提供的参数没有提供，该参数等于`undefined`
- 对象没有赋值的属性，该属性的值为`undefined`
- 函数没有返回值时，默认返回`undefined`

比较有意思的是，`null`和`undefined`被认为是数值相等的。

```javascript
console.log(null == undefined)
console.log(null === undefined)
//输出结果
true
false
```

## 三.数组

### 1.定义数组

在`javascript`中创建数组有两种方式：一种是直接创建，另一种是通过`Array`函数来创建

```javascript
const a = ["Outer","Cyrex"]
console.log(a)
const b = Array("Outer","Cyrex")
console.log(b)
//输出结果
Array(2)
0: "Outer"
1: "Cyrex"
length: 2
```

### 2.遍历数组

在`javascript`中，有两种数组遍历方式：

```javascript
let a = Array(1,2,3,4,5,6,7,8,9,10)
for(let i=0;i<a.length;i++){
    console.log(a[i])
}
for(const value of a){
    console.log(value)
}
```

其中，`for of`方法类似于`go`语言中的`for range`方法，但其不能获取索引，因此存在一定弊端。

### 3.数组方法

首先介绍一个函数：

 在`javascript`中存在很多**判断类型**的函数，数组对应的**判断类型**函数便是`isArray`。也可以使用`instanceof`来实现判断。

**注意**，`typeof`获取的数组类型实际是`Object`，这也就是为什么数组并不是八大数据类型之一，其本质是一种以下标为`key`的对象。

```javascript
console.log(Array.isArray(a1))
console.log(Array.isArray(1))
console.log(a1 instanceof Array)
//输出结果
true
false
true
```

下边介绍`javascript`中的数组方法：

| 方法      | 作用                                                         |
| --------- | ------------------------------------------------------------ |
| push()    | 在数组的末端添加一个或多个元素，并返回添加之后数组的长度     |
| pop()     | 移除数组末端的元素并返回其值                                 |
| shift()   | 从数组的开头移除元素，并返回其值                             |
| unshift() | 从数组的开头插入新元素，并返回添加之后数组的长度             |
| join()    | 与`split`方法互逆，添加指定的字符串内容到数组元素之间，并返回插入之后的字符串 |
| sort()    | 将数组进行升序排列，通过`reverse()`方法反转可以获得降序      |
| splice()  | 在数组中截取一个切片，并返回被截取的数组内容                 |
| forEach() | 迭代每一个元素，不可被终止且无返回值                         |
| map()     | 迭代每一个元素并进行指定从操作，返回值为进行操作后的数组     |
| filter()  | 迭代每一个元素，获取满足函数内条件的元素并组建一个新数组，返回该新数组 |
| every()   | 判断数组是否每一个元素都满足条件                             |
| some()    | 判断数组是否存在某一元素满足条件                             |
| find()    | 获取数组中满足条件的数据，如果有 就是满足条件的第一个数据；如果没有就是undefined |

前四个方法是基于**数据结构**中的**栈**和**队列**来使用的：

```javascript
let a = Array(1,2,3,4,5,6,7,8,9,10)
let last = a.pop()
console.log(last)
a.push(11)
console.log(a)
let first = a.shift()
console.log(first)
a.unshift(0)
console.log(a)
//输出结果
last:10
push后的数组：[1, 2, 3, 4, 5, 6, 7, 8, 9, 11]
first:1
unshift后的数组：[0, 2, 3, 4, 5, 6, 7, 8, 9, 11]
```

之后是`join`的用途，用于在数组的各个元素之间插入指定的字符：

```javascript
let a = Array(1,2,3,4,5,6,7,8,9,10)
let str = a.join(',')
console.log(str)
//输出结果
1,2,3,4,5,6,7,8,9,10
```

`sort`用于将元素按升序进行排列，但在`JavaScript`中，`Array.prototype.sort()` 方法默认是将所有元素转换为字符串，然后按照字符串的`Unicode`码点进行排序。

```javascript
let a = Array('c','w','a','f','g','h','v','c','w')
a.sort()
console.log(a)
//输出结果
['a', 'c', 'c', 'f', 'g', 'h', 'v', 'w', 'w']
```

为了解决这种情况，实现**按照数字大小来排序**，则需要再`sort()`内传入一个函数。

当`sort()`内函数传回的是**负值**，则将元素进行调换，若为**正值或0**则不进行调换。

```javascript
let numbers = [3, 1, 4, 1, 5, 9, 2, 6]
numbers.sort(function(a, b) {  
  return a - b
})
console.log(numbers)
//输出结果
[1, 1, 2, 3, 4, 5, 6, 9]
```

此时`sort()`会将每相邻两个元素放入函数`function(a, b) {return a - b}`中，若传回负值则调换顺序，否则不变。以此来实现**按数字大小排序**。

```javascript
let numbers = [3, 1, 4, 1, 5, 9, 2, 6,41,55]
b = numbers.splice(4,7)
console.log(b,numbers)
//输出结果
[5, 9, 2, 6, 41, 55]
[3, 1, 4, 1]
```

接下来进行介绍的是`ES6`版新增的方法

首先是`forEach()`，其参数为一个**函数**(很神奇的是，和`go`一样，`javascript`中函数也是第一公民)，可以对数组的**所有元素**进行**迭代**并放入该**函数**中，且**不可终止**。

```javascript
numbers.forEach(function(value, index,array){
    numbers.pop()
    console.log(value,index,array)
})
//输出结果
3 0 (9) [3, 1, 4, 1, 5, 9, 2, 6, 41]
1 1 (8) [3, 1, 4, 1, 5, 9, 2, 6]
4 2 (7) [3, 1, 4, 1, 5, 9, 2]
1 3 (6) [3, 1, 4, 1, 5, 9]
5 4 (5) [3, 1, 4, 1, 5]
```

这个函数可以选择三个参数，分别是元素的值`value`，下标`index`和当前迭代操作完成之后的数组`array`。

注意，上述代码进行到`5`结束是由于`pop()`导致数组最后**无法继续迭代**。

之后是`map`，其与`forEach`不同的是，他会返回操作后的新数组，而并不是在原数组上进行修改

```javascript
let numbers = [3, 1, 4, 1, 5, 9, 2, 6,41,55]
let newArray = numbers.map(function(value,index,array){
    return value - index
})
console.log(newArray)
//输出结果
[3, 0, 2, -2, 1, 4, -4, -1, 33, 46]
```

`filter`方法会筛选出满足函数内条件的元素，将这些元素组建出新的数组并返回，函数内`return ture`的元素会被纳入数组

```javascript
let numbers = [3, 1, 4, 1, 5, 9, 2, 6,41,55]
let newArray = numbers.filter(function(value){
    if(value > 10){
        return true
    }
})
console.log(newArray)
//输出结果
[4, 5, 9, 6, 41, 55]
```

`every()`和`some()`的返回值都是布尔值`Boolean`，用于判断数组内元素是否满足函数内条件，`every()`只有在均满足时返回`true`，而`some()`则是存在满足的元素即返回`true`

```javascript
let numbers = [3, 1, 4, 1, 5, 9, 2, 6,41,55]
let output = numbers.every(function(value){
    if(value > 3){
        return true
    }
})
console.log(output)
output = numbers.some(function(value){
    if(value > 3){
        return true
    }
})
console.log(output)
//输出结果
false
true
```

`find`则类似`ORM`框架中的`take`，会获取第一个满足条件的元素的值

```javascript
let numbers = [3, 1, 4, 1, 5, 9, 2, 6,41,55]
let output = numbers.find(function(value,index){
    if(value > 3){
        return true
    }
})
console.log(output)
//输出结果
4
```

也可以使用`findIndex`来返回第一个满足条件的元素的下标，若没找到对应元素则返回`-1`

```javascript
let numbers = [3, 1, 4, 1, 5, 9, 2, 6,41,55]
let index = numbers.findIndex(function(value,index){
    if(value > 3){
        return true
    }
})
console.log(index)
//输出结果
2
```

## 四.流程控制

### 1.逻辑运算符

在`javascript`中的的**与、或、非**分别为`&&`、`||`、`!`

```javascript
console.log(true && false)
console.log(true || false)
console.log(!true)
```

在`javascript`中，存在一种特殊的机制，**逻辑短路**，即若已经能确定该逻辑语句的值，`javascript`便不会读取之后的内容

- `&&`： 如果前面的条件是`false`，那就不会去判断后面的条件了

- `||`：如果前端的条件是`true`，那就不会去判断后面的条件了

```javascript
function t(){
    console.log("t")
    return true
}
console.log(false && t())
console.log(true || t())
//输出结果
false
true
```

其中若函数被运行时会输出`'t'`，以此来检测是否会运行该函数。

### 2.流程控制语句

由于`javascript`中的大部分流程控制语句与`C`语言的内容一致，因此不过多讲解

选择语句仍然是`if ... else ...`

```javascript
if (condition)  
    console.log("条件为真");  
else  
    console.log("条件为假");
```

`switch`语句也与`C`语言中的完全一致。如若无`break`便会继续运行其余分支。

```javascript
switch (表达式或变量) {
    case  值1:
        代码块;
        break;
    case  值2:
        代码块;
        break;
    ....
    default:
        以上条件都不成立，执行此代码块
}
```

`javascript`中的特色在于其`for`循环语句，在`javascript`中有三种`for`循环的方法：

| for循环语句                     | 运用情景                     |
| ------------------------------- | ---------------------------- |
| for(初始条件;循环条件;迭代条件) | 最基础最传统的`for`循环语句  |
| `for in`语句                    | 用来遍历对象的可枚举属性列表 |
| `for of`语句                    | 用于快速的遍历数组和对象     |

传统的`for`循环与`C`语言基本一致，在此不作说明。

`for ... in`是用来检索对象中的`key`值(或称为**字段名**)，对于数组而言便是输出**下标**

```javascript
const obj = {
    name:"Outer",
    age: 18
}
for (const objKey in obj) {
    console.log(objKey)
}
a = Array(1,2)
  for (const index in a) {
      console.log(index)
  }
//输出结果
name,age
0,1
```

`for of`在之前的**数组**知识里已经进行过介绍，在此不作重复介绍。

`javascript`中的`while`、`do ... while`、`continue`和`break`也与`C`语言基本一致，不再介绍。

```javascript
while (条件){
    循环体
}

do {
	循环体
} while (条件表达式)
```

`do ... while`相较于`while`而言会先运行一次循环题体。



