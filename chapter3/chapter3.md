# Chapter3 JS基础知识（中）---作用域和闭包

---

#### 这是我自己的学习过程总结，如果有什么写的不清楚的地方，欢迎大家交流、批评、提问。可以在这里评论，也可以在github上提问，谢谢大家

## 题目

1. 说一下对变量提升的理解
2. 说明this几种不同的使用场景
3. 创建10个< a >标签，点击的时候弹出来对应的序号
4. 如何理解作用域
5. 实际开发中闭包的应用

## 知识点

* 执行上下文
* this
* 作用域
* 作用域链
* 闭包

### 执行上下文

``` js
console.log(a)  //undefined
var a = 100

fn('zhangsan')  // 'zhangsan' 20
function fn(name) {
  age = 20
  console.log(name, age)
  var age
}
```

在一段script或者一个函数中，在第一行之前都会先生成一个执行上下文

* 范围：一段< script >或者一个函数
* 全局：变量定义、函数声明
* 函数：变量定义、函数声明、this、arguments

**注意：** 提前的是函数声明，函数表达式只会当作变量定义提前

``` js
fn()  //不会报错
function fn () {
  //函数声明
}

//相当于 提前了 var fn1 = undefined
fn1()  // 会报错
var fn1 = function () {
  // 函数表达式
}
```

### this

**this要在执行时才会确定值，定义时无法确认**

``` js
var a = {
  name: 'A',
  fn: function () {
    console.log(this.name)
  }
}
a.fn()  // this === a
a.fn.call({name: 'B'})  // this === {name: 'B'}
var fn1 = a.fn
fn1()  // this === window
```

* 作为构造函数执行
  ``` js
  function Foo(name) {
    this = {}
    this.name = name
    return this
  }
  var f = new Foo('zhangsan')
  ```
* 作为对象属性执行
  ``` js
  var obj = {
    name : 'A',
    printName: function () {
      console.log(this.name)
    }
  }
  obj.printName()  // 'A'
  ```
* 作为普通函数执行
  ``` js
  function fn() {
    console.log(this)  //this === window
  }
  fn()
  ```
* call apply bind
  ``` js
  // 不知道call、apply、bind语法的，先去看看语法
  function fn1(name) {
    alert(name)
    console.log(this)
  }
  fn1.call({x:100}, 'zhangsan')
  // apply和bind差不多
  var fn2 = function (name,age) {
    alert(name)
    console.log(this)
  }.bind({y:200})
  fn2('zhangsan',20)
  ```

### 作用域

* 没有块级作用域
  ``` js
  if(true) {
    var name = 'zhangsan'
  }
  console.log(name) // 'zhangsan'
  ```
* 只有函数和全局作用域
  ``` js
  var a = 100
  function fn() {
    var a = 200
    console.log('fn', a)
  }
  console.log('global', a) // 100
  fn() //200
  ```

### 作用域链

``` js
var a = 100
function fn() {
  var b = 200

  // 当前作用域没有定义的变量，即“自由变量”
  // 去定义！！的时候父级作用域寻找a
  console.log(a)  //100

  console.log(b) //200
}
fn()
```

``` js
var a = 100
function F1() {
  var b = 200
  function F2() {
    var c = 300
    console.log(a)
    console.log(b)
    console.log(c)
  }
  F2()
}
F1()  //100 200 300
```

### 闭包

``` js
function F1() {
  var a = 100

  // 返回一个函数（函数作为返回值）
  return function() {
    console.log(a)  //自由变量，父作用域寻找
  }
}
// f1 得到一个函数
var f1 = F1()
var a = 200
f1()  // 100
```

#### 闭包的使用场景

* 函数作为返回值（上一个demo）
* 函数作为参数传递
  ``` js
  function F1() {
    var a = 100
    return function () {
      console.log(a)
    }
  }
  var f1 = F1()

  function F2(fn) {
    var a = 200
    fn()
  }
  F2(f1)  //100
  ```

### 解题

1. 说一下对变量提升的理解
   * 主要就是对执行上下文的理解
   * 变量定义
   * 函数声明（注意和函数表达式的区别）
2. 说明this几种不同的使用场景
   * 作为构造函数执行
   * 作为对象属性执行
   * 作为普通函数执行
   * call apply bind
3. 创建10个< a >标签，点击的时候弹出来对应的序号
   ``` js
   // 官方写法  我自己的写法在github中的chapter1中的chapter1-04.html
   var i
   for (i = 0; i < 10; i++) {
     (function (i) {
       var a = document.createElement('a')
       a.innerHTML = i +'<br>'
       a.addEventListener('click', function (e) {
         e.preventDefault()
         alert(i)
       })
       document.body.appendChild(a)
     })(i)
   }
   ```
4. 如何理解作用域
   * 自由变量
   * 作用域链，即自由变量的查找
   * 闭包的两个场景
5. 实际开发中闭包的应用
   ``` js
   // 闭包实际应用中主要用于封装变量，收敛权限
   function isFirstLoad() {
     var _list = []
     return function (id) {
       id(_list.indexOf(id) >= 0) {
         return false
       } else {
         _list.push(id)
         return true
       }
     }
   }
   // 使用
   var firstLoad = isFirstLoad()
   firstLoad(10) //true
   firstLoad(10) //false
   firstLoad(20) //true
   ```

## gitbub地址 持续更新 可下载
[https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning](https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning)