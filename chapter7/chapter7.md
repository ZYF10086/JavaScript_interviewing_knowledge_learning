# Chapter7  JS-Web-API下 --- 事件、Ajax、存储

#### 这是我自己的学习过程总结，如果有什么写的不清楚的地方，欢迎大家交流、批评、提问。可以在这里评论，也可以在github上提问，谢谢大家

## 7.1  事件

### 题目

* 编写一个通用的事件监听函数
* 描述事件冒泡流程
* 对于一个无限下拉加载图片的页面，如何给每个图片都绑定事件

### 知识点

* 通用事件绑定
* 事件冒泡
* 代理

#### 通用事件绑定

``` js
// 常用事件绑定
var btn = document.getElementById('btn1')
btn.addEventListener('click', function (event) {
  console.log('clicked')
})

// 简单的封装事件绑定
function bindEvent(elem, type, fn) {
  elem.addEventListener(type, fn)
}
var a = document.getElementById('link1')
bindEvent(a, 'click', function(e) {
  e.preventDefault() // 阻止默认行为
  alert('clicked')
})

// 完善通用绑定事件的函数
function bindEvent(elem, type, selector, fn) {
  if(fn == null) {
    fn = selector
    selector = null
  }
  elem.addEventListener(type, function(e) {
    var target
    if(selector) {
      target = e.target
      if (target.matches(selector)) {
        fn.call(target, e)
      }
    } else {
       fn(e)
    }
  })
}

//使用代理
var div1 = document.getElementById('div1')
bindEvent(div1, 'click', 'a', function (e) {
  console.log(this.innerHTML)
})

// 不使用代理
var a = document.getElementById('a1')
bindEvent(div1, 'click', function (e) {
  console.log(a.innerHTML)
})
```

关于IE低版本的兼容性

* IE低版本使用attachEvent绑定事件，和W3C标准不一样
* IE低版本使用量以非常少，很多网站都早已不支持
* 建议对IE低版本的兼容性：了解即可，无需深究
* 如果遇到对IE低版本要求苛刻的面试，果断放弃

#### 事件冒泡

底层节点会一层一层触发父节点的事件

``` html
<!-- 要求：点击激活弹出激活，点击取消弹出取消 -->
<body>
  <div id = "div1">
    <p id="p1">激活</p>
    <p id="p2">取消</p>
    <p id="p3">取消</p>
    <p id="p4">取消</p>
  </div>
  <div id = "div2">
    <p id="p5">取消</p>
    <p id="p6">取消</p>
  </div>
 </body>
```

``` js
var p1 = document.getElementById('p1')
var body = document.body
bindEvent(p1, 'click', function (e) {
  e.stopPropatation()  //取消冒泡
  alert('激活')
})
bindEvent(body, 'click', function(e) {
  alert('取消')
})
```

#### 代理

其实就是对于事件冒泡的应用

``` html
<!-- 要求：点击不同的a标签，弹出对应a标签的内容 -->
<body>
  <div id = "div1">
    <a href="#">a1</a>
    <a href="#">a2</a>
    <a href="#">a3</a>
    <a href="#">a4</a>
    <!-- 会随时新增更多的 a 标签 -->
  </div>
 </body>
```

``` js
// 直接把事件绑定在div1上
var div1 = document.getElementById('div1')
div1.addEventListener('click', function (e) {
  var target = e.target  // target会知道点击事件从哪触发的
  if (target.nodeName === 'A') {
    alert(target.innerHTML)
  }
})
```

代理的好处

* 代码简洁
* 减少浏览器内存占用

### 解答

* 编写一个通用的事件监听函数
  * 参看上面
* 描述事件冒泡的流程
  * DOM树型结构
  * 事件冒泡
  * 阻止冒泡
  * 冒泡的应用
* 对于一个无限下拉加载图片的页面，如何给每个图片都绑定事件
  * 使用代理
  * 知道代理的两个优点

## 7.2  Ajax

### 题目

* 手动编写一个ajax，不依赖第三方库
* 跨域的几种实现方式

### 知识点

* XMLHttpRequest
* 状态码说明
* 跨域

#### XMLHttpRequest

``` js
var xhr = new XMLHttpRequest()
xhr.open("GET", "/api", false)
xhr.onreadystatechange = function () {
  // 这里的函数异步执行，可参考之前的chapter4的异步模块
  if (xhr.readyState == 4) {
    if (xhr.status == 200) {
      alert(xhr.responseText)
    }
  }
}
xhr.send(null)
```

IE兼容性问题

* IE低版本使用ActiveXObject

#### 状态码说明

* readyState
  * 0 - （未初始化）还没有调用send()方法
  * 1 - （载入）已调用send()方法，正在发送请求
  * 2 - （载入完成）send()方法执行完成，已经接收到全部响应内容
  * 3 - （交互）正在解析响应内容
  * 4 - （完成）响应内容解析完成，可以在客户端调用了

* status
  * 2xx - 表示成功处理请求。如200
  * 3xx - 需要重定向，浏览器直接跳转
  * 4xx - 客户端请求错误，如404
  * 5xx - 服务器端错误

#### 跨域

* 什么是跨域
  * 浏览器有同源策略，不允许ajax访问其他域接口
  * 跨域条件：协议、域名、端口，有一个不同就算跨域
  * 可以跨域的三个标签
    * < img src=xxx> 用于打点统计，统计网站可能是其他域
    * < link href=xxxx> 可以使用CDN，CDN的也是其他域
    * < script src=xxx> 可以用于JSONP
  * 跨域注意事项
    * 所有的跨域请求都必须经过信息提供方允许
    * 如果未经允许即可获取，那是浏览器同源策略出现漏洞
* JSONP

  ``` js html
  <script>
  window.callback = function (data) {
    // 这是我们跨域得到的信息
    console.log(data)
  }
  </script>
  <script src="http://baidu.com/api.js"></script>
  ```

* 服务器端设置http header
  * 另外一个解决跨域的简洁方法，需要服务器端来做
  * 但是作为交互方，我们必须知道这个方法
  * 是将来解决跨域问题的一个趋势

### 解答

* 手动编写一个ajax，不依赖第三方库
  * 见上面
* 跨域的几种实现方式
  * JSONP
  * http header

## 7.3  存储

### 题目

* 请描述一个cookie，sessionStorage和localStorage的区别？

### 知识点

* cookie
* localStorage 和 sessionStorage

#### cookie

* 本身用于客户端和服务器端通信
* 但是它有本地存储的功能，于是就被“借用”
* 使用 document.cookie = ... 获取和修改即可
* cookie用于存储的缺点
  * 存储量太小,只有4KB
  * 所有http请求都带着，会影响获取资源的效率
  * API简单，需要封装才能用 document.cookie

#### localStorage 和 sessionStorage

* HTML5专门为存储而设计，最大容量5M
* API简单易用
* localStorage.setItem(key, value); localStorage.getItem(key);
* iOS safari 隐藏模式下，localStorage.getItem会报错

### 解答

* 容量
* 是否会携带到ajax中
* API易用性

## gitbub地址 持续更新 可下载
[https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning](https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning)