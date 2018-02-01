# Chapter6  JS-Web-API -- DOM操作和BOM操作

#### 这是我自己的学习过程总结，如果有什么写的不清楚的地方，欢迎大家交流、批评、提问。可以在这里评论，也可以在github上提问，谢谢大家

## 6.1  DOM操作（Document Object Model）

### 题目

* DOM是哪种基本的数据结构？
* DOM操作的常用API有哪些
* DOM节点的attr和property有何区别

### 知识点

* DOM本质
* DOM节点操作
* DOM结构操作

#### DOM的本质

DOM（Document Object Model——文档对象模型）是用来呈现以及与任意 HTML 或 XML 交互的API文档。DOM 是载入到浏览器中的文档模型，它用节点树的形式来表现文档，每个节点代表文档的构成部分（例如： element——页面元素、字符串或注释等等）。
DOM可以理解为浏览器把拿到的HTML代码，结构化为一个浏览器能识别并且js可操作的模型。

BOM（Browser Object Document）即浏览器对象模型。
BOM提供了独立于内容 而与浏览器窗口进行交互的对象；
由于BOM主要用于管理窗口与窗口之间的通讯，因此其核心对象是window；
BOM由一系列相关的对象构成，并且每个对象都提供了很多方法与属性

#### DOM节点操作

* 获取DOM节点

  ``` js
  var div1 = document.getElementById('div1') // 元素
  var divList = document.getElementsByTagName('div') // 集合
  console.log(divList.length)
  console.log(divList[0])

  var containerList = document.getElementsByClassName('.container') // 集合
  var pList = document.querySelectorAll('p') // 集合
  ```

* prototype

  ``` js
  var pList = document.querySelectorAll('p')
  var p = pList[0]
  console.log(p.style.width)  // 获取样式
  p.style.width = '100px'  //修改样式
  console.log(p.className) // 获取class
  p.className = 'p1' // 修改class

  // 获取 nodeName 和nodeType
  console.log(p.nodeName)
  console.log(p.nodeType)
  ```

* Attribute(有关文档的标签属性)
  标签属性，用于扩充HTML标签，可以改变标签行为或提供数据，格式为name=value

  ``` js
  var pList = document.querySelectorAll('p')
  var p = pList[0]
  p.getAttribute('data-name')
  p.setAttribute('data-name', 'hello')
  p.getAttribute('style')
  p.setAttribute('style', 'font-size:30px')
  ```

#### DOM结构操作

* 新增节点

  ```js
  var div1 = document.getElementById('div1')
  // 添加新节点
  var p1 = document.createElement('p')
  p1.innerHTML = 'this is p1'
  div1.appendChild(p1) // 添加新创建的元素
  // 移动已有节点
  var p2 = document.getElementById('p2')
  div1.appendChild(p2)
  ```

* 获取父元素

  ``` js
  var div1 = document.getElementById('div1')
  var parent = div1.parentElement
  ```

* 获取子元素

  ``` js
  var div1 = document.getElementById('div1')
  var child = div1.childNodes
  ```

* 删除节点

  ``` js
   var div1 = document.getElementById('div1')
   div1.removeChild(child[0])
  ```

### 解答

* DOM是哪种基本的数据结构

  树

* DOM操作的常用API有哪些

  * 获取DOM节点，以及节点的property和Attribute
  * 获取父节点，获取子节点
  * 新增节点，删除节点

* DOM节点的Attribute和property有何区别

  * property只是一个JS对象的属性的修改
  * Attribute是对html标签属性的修改

## 6.2  BOM操作（Browser Object Model）

### 题目

* 如何检测浏览器的类型
* 拆解url的各部分

### 知识点

* navigator
* screen
* location
* history

#### navigator & screen

``` js
// navigator
var ua = navigator.userAgent
var isChrome = ua.indexOf('Chrome')
console.log(isChrome)

//screen
console.log(screen.width)
console.log(screen.height)
```

#### location & history

```js
// location
console.log(location.href) //整个URL
console.log(location.protocal) // 'http:' 'https:'
console.log(location.pathname) // '/learn/199'
console.log(location.search) // 问号后面的查询字符串
console.log(location.hash) // 哈希

// history
history.back()
history.forward()
//页面上返回功能
var a = document.createElement('a');
a.innerHTML = '<input type="button" id="btn" name="" value="后退">'
document.body.append(a);
var  btn = document.getElementById('btn');
btn.onclick = function(){
    prev()
}
function  prev(){
    history.back()
}
```

## gitbub地址 持续更新 可下载
[https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning](https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning)