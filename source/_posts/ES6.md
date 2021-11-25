---
title: ES6相关
date: 2018-07-09 20:15:53
tags:
---

ES6 的部分新特性和功能,留作回顾。

![](/images/ES6.jpg)

## 1.const 与 let

### 使用var带来的麻烦:
``` bash
function getClothing(isCold) {
  if (isCold) {
    var freezing = 'Grab a jacket!';
  } else {
    var hot = 'It's a shorts kind of day.';
    console.log(freezing);
  }
}
```
运行getClothing(false)后输出的是undefined,这是因为执行function函数之前,所有变量都会被提升, 提升到函数作用域顶部.

let与const声明的变量解决了这种问题,因为他们是块级作用域, 在代码块(用{}表示)中使用let或const声明变量, 该变量会陷入暂时性死区直到该变量的声明被处理.

``` bash
function getClothing(isCold) {
  if (isCold) {
    const freezing = 'Grab a jacket!';
  } else {
    const hot = 'It's a shorts kind of day.';
    console.log(freezing);
  }
}
```
运行getClothing(false)后输出的是ReferenceError: freezing is not defined,因为 freezing 没有在 else 语句、函数作用域或全局作用域内声明，所以抛出 ReferenceError。

## 2.模板字符串

### 在ES6之前,将字符串连接到一起的方法是+或者concat()方法,如
``` bash
const student = {
  name: 'Richard Kalehoff',
  guardian: 'Mr. Kalehoff'
};

const teacher = {
  name: 'Mrs. Wilson',
  room: 'N231'
}

let message = student.name + ' please see ' + teacher.name + ' in ' + teacher.room + ' to pick up your report card.';
```
模板字符串用倒引号 ( `` )（而不是单引号 ( '' ) 或双引号( "" )）表示，可以包含用 ${expression} 表示的占位符

``` bash
let message = `${student.name} please see ${teacher.name} in ${teacher.room} to pick up your report card.`;
```

## 3.解构赋值

在ES6中,可以使用解构从数组和对象提取值并赋值给独特的变量
解构数组的值:

``` bash
const point = [10, 25, -34];
const [x, y, z] = point;
console.log(x, y, z);
```
``` bash
Prints: 10 25 -34
```
[]表示被解构的数组, x,y,z表示要将数组中的值存储在其中的变量, 在解构数组是, 还可以忽略值, 例如const[x,,z]=point,忽略y坐标.

解构对象中的值:
``` bash
const gemstone = {
  type: 'quartz',
  color: 'rose',
  karat: 21.29
};
const {type, color, karat} = gemstone;
console.log(type, color, karat);
```
花括号 { } 表示被解构的对象，type、color 和 karat 表示要将对象中的属性存储到其中的变量

## 4.箭头函数
箭头函数的三个特点:
不需要 function 关键字来创建函数
省略 return 关键字
继承当前上下文的 this 关键字
``` bash
//例如：
    [1,2,3].map(x => x + 1)
    
//等同于：
    [1,2,3].map((function(x){
        return x + 1
    }).bind(this))
```
当你的函数有且仅有一个参数的时候，是可以省略掉括号的。当你函数返回有且仅有一个表达式的时候可以省略{} 和 return.

## 5.import 和 export
import导入模块、export导出模块
``` bash
//全部导入
import people from './example'

//有一种特殊情况，即允许你将整个模块当作单一对象进行导入
//该模块的所有导出都会作为对象的属性存在
import * as example from "./example.js"
console.log(example.name)
console.log(example.age)
console.log(example.getName())

//导入部分
import {name, age} from './example'

// 导出默认, 有且只有一个默认
export default App

// 部分导出
export class App extend Component {};
```
区别：
``` bash
1.当用export default people导出时，就用 import people 导入（不带大括号）

2.一个文件里，有且只能有一个export default。但可以有多个export。

3.当用export name 时，就用import { name }导入（记得带上大括号）

4.当一个文件里，既有一个export default people, 又有多个export name 或者 export age时，导入就用 import people, { name, age } 

5.当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example
```

## 6.promise
在promise之前代码过多的回调或者嵌套，可读性差、耦合度高、扩展性低。
通过Promise机制，扁平化的代码机构，大大提高了代码可读性；用同步编程的方式来编写异步代码，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。

基本用法
``` bash
 fetch('/api/todos')
      .then(res => res.json())
      .then(data => ({ data }))
      .catch(err => ({ err }));
```