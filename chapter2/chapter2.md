# Chapter 2  JS基础知识（上）

## 2.1 变量类型和计算

### 题目

1. JS中使用typeof能得到哪些类型
2. 何时使用 === 何时使用 ==
3. JS中有哪些内置函数
4. JS变量按照储存方式区分为哪些类型，并描述其特点
5. 如何理解JSON

### 知识点

* 变量类型
* 变量计算

#### 变量类型

1. 值类型 vs 引用类型

值类型：赋值创建两个对象

``` javascript
    var a = 100
    var b = a
    a = 200
    console.log(b)   //100
```

引用类型：对象、数组、函数，赋值只是指针的引用

``` javascript
    var a = {age:20}
    var b = a
    b.age = 21
    console.log(a.age)   //21
```

2. typeof 运算符详解 

只能区分值类型的详细，除了function

``` js
    typeof undefined  //undefined
    typeof  'abc'  //string
    typeof 123  //number
    typeof true  //boolean
    typeof {}  //object
    typeof []  //object
    typeof null  //object
    typeof console.log  //function
```

3. 变量计算 - 强制类型转换

* 字符串拼接

``` js
    var a = 100 + 10  // 110
    var b = 100 + '10'  // '10010'
```

* == 运算符 会 尽量转化为相同

``` js
    100  ==  '100'  // true
    0 == ''  //true
    null == undefined  //true
```

* if语句

``` js
    var a  =  true
    if (a) {   }
    var b  = 100   //true
    if (b) {   }
    var c  =  ''   //false
    if (c) {   }
```

* 逻辑运算符

``` js
    console.log(10 && 0)  //0
    console.log('' || 'abc')  //'abc'
    console.log(!window.abc)  //true

    //判断一个变量会被当做true还是false
    var a = 100
    console.log(!!a)
```

### 题目解答

1. string number boolean object undefined function
2. 全部使用 === ，除了以下这种情况

``` js
    if(obj.a == null) {
      // 这里相当于obj.a === null || obj.a === undefined ，简写形式
      // 这里是jquery 源码中推荐的写法
    }
```

3.  Object Array Boolean Number String Function Date RegExp Error
4.  值类型和引用类型
5.  JSON只不过是一个 JS 对象而已，也可以说是一种数据格式

``` js
    JSON.stringify({a:10, b:20})
    JSON.parse('{"a":10,"b":20}')
```

## 2.2  原型和原型链

### 题目

1. 如何准确判断一个变量是数组类型（xxx instanceof Array）
2. 写一个原型链继承的例子
3. 描述 new 一个对象的过程
4. zepto（或其他框架）源码中如何使用原型链

### 知识点

* 构造函数
* 构造函数 - 拓展
* 原型规则和示例
* 原型链
* instanceof

#### 构造函数(首字母大写)

``` js
    function Foo(name, age) {
      this.name = name
      this.age = age
      this.class = 'class-1'
      // return this // 默认有这一行
    }
    var f = new Foo('zhangsan', 20) // 相当于赋值this
    // var f1 = new Foo('lisi', 22)
```

#### 构造函数 - 拓展

* var a = {}其实是 var a = new Object() 的语法糖
* var a = []其实是 var a  = new Array() 的语法糖
* function Foo() {...} 其实是 var Foo = new Function(...)
* 使用 instanceof 判断一个函数是否是一个变量的构造函数

#### 原型规则和示例

* 所有的引用类型 (数组、对象、函数)，都具有对象特性，即可自由扩展属性（除了“null”意外）

``` js
    var obj = {}; obj.a = 100;
    var arr = []; arr.a = 100;
    function fn () {}
    fn.a = 100;

    console.log(obj.__proto__);
    console.log(arr.__proto__);
    console.log(fn.__proto__);

    console.log(fn.prototype)

    console.log(obj.__proto__ === Object.prototype )
```

* 所有的引用类型（数组、对象、函数），都有一个__proto__（隐式原型）属性，属性值是一个普通的对象
* 所有的函数，都有一个prototype（显式原型）属性，属性值也是一个普通的对象
* 所有的引用类型（数组、对象、函数），proto属性值指向它的构造函数的" prototype "属性值
* 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的 __proto__（即它的构造函数的prototype）中寻找

``` js
    function Foo(name, age) {
      this.name = name
    }
    Foo.prototype.alertNmae = function () {
      alert(this.name)
    }
    // 创建示例
    var f = new Foo('zhangsan')
    f.printName = function () {
      console.log(this.name)
    }
    // 测试
    f.printName()
    f.alertName()
```

**循环对象本身的属性的方法**

``` js
    var item
    for(item in f) {
      // 高级浏览器已经在 for in 中屏蔽了来自原型的属性
      // 但是这里建议大家还是加上这个判断，保证程序的健壮性
      if(f.hasOwnProperty(item)) {
        console.log(item)
      }
    }
```

#### 原型链

``` js
    function Foo(name, age) {
      this.name = name
    }
    Foo.prototype.alertNmae = function () {
      alert(this.name)
    }
    // 创建示例
    var f = new Foo('zhangsan')
    f.printName = function () {
      console.log(this.name)
    }
    // 测试
    f.printName()
    f.alertName()
    f.toString()  //要去 f.__proto__.__proto__中查找
```

#### instanceof

用于判断应用类型属于哪个构造函数的方法

* f instanceof Foo 的判断逻辑是：
* f 的__proto__一层一层往上，能否对应到Foo.prototype
* 再试着判断 f instanceof Object

### 解题

* 如何准确判断一个变量是数组类型

``` js
    var arr = []
    arr instanceof Array  //true
    typeof arr  // Object，typeof 是无法判断是否是数组的
```

* 写一个原型链继承的例子

这个例子作为理解，最好面试别写这个

``` js
    // 动物
    function Animal() {
      this.eat = function () {
        console.log('animal eat')
      }
    }
    // 狗
    function Dog() {
      this.bark = function () {
        console.log('dog bark')
      }
    }
    Dog.prototype = new Animal()
    // 哈士奇
    var hashiqi = new Dog()
```

这是个封装DOM查询的例子

``` js
    function Elem(id) {
      this.elem = document.getElementById(id)
    }

    Elem.prototype.html = function (val) {
      var elem = this.elem
      if (val) {
        elem.innerHTML = val
        return this  // 链式操作
      } else {
        return elem.innerHTML
      }
    }

    Elem.prototype.on = function (type, fn) {
      var elem = this.elem
      elem.addEventListener(type, fn)
      return this
    }

    var div1 = new Elem('div1') //测试时改为自己想要测试id
    // console.log(div1.html())
    // div1.html('<p>hello world</p>')
    // div1.on('click',function () {
    //   alert('clicked')
    // })
    div1.html('<p>hello world</p>').on('click',function () {
      alert('clicked')
    })
    // 因为返回了this，所以可以改成这样

```

* 描述 new 一个对象的过程
  * 创建一个新对象
  * this 指向这个新对象
  * 执行代码，即对 this 赋值
  * 返回 this

* zepto（或其他框架）源码中如何使用原型链
  * 阅读源码
  * 参考慕课网教程zepto设计与源码解读