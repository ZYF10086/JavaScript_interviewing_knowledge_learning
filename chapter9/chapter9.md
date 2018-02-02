# Chapter9  JS运行环境

#### 这是我自己的学习过程总结，如果有什么写的不清楚的地方，欢迎大家交流、批评、提问。可以在这里评论，也可以在github上提问，谢谢大家

## 知识点

* 页面加载过程
* 性能优化
* 安全性

### 1.  页面加载

#### 题目

* 从输入url到得到html的详细过程
* window.onload 和 DOMContentLoaded 的区别

#### 页面加载知识点

* 加载资源的形式
  * 输入url（或跳转页面）加载html
  * 加载html中的静态资源
* 加载一个资源的过程
  * 浏览器根据DNS服务器得到域名的IP地址
  * 向这个IP的及其发送 http 请求
  * 服务器收到、处理并返回http请求
  * 浏览器得到返回内容
* 浏览器渲染页面的过程
  * 根据HTML结构生成 DOM Tree
  * 根据 CSS 生成后 CSSOM
  * 将 DOM和CSSOM 整合形成 RenderTree
  * 根据RenderTree开始渲染和展示
  * 遇到 < script >时，会执行并阻塞渲染

#### 解答

* 从输入url到得到html的详细过程
  * 浏览器根据DNS服务器得到域名的IP地址
  * 向这个IP的及其发送 http 请求
  * 服务器收到、处理并返回http请求
  * 浏览器得到返回内容
* window.onload 和 DOMContentLoaded 的区别
  * window.onload 页面的全部资源加载完才会执行，包括图片、视频等
  * DOM 渲染完即可执行，此时图片、视频还没加载完

### 性能优化

这是一个综合性的问题

#### 原则

* 多使用内存、缓存或者其他方法
* 减少 CPU 计算、减少网络请求

#### 入手点

* 加载页面和静态资源
* 页面渲染

##### 加载资源优化

* 静态资源的压缩合并

  ``` js
  <script src="a.js"></script>
  <script src="b.js"></script>
  <script src="c.js"></script>

  <script src="abc.js"></script>
  ```

* 静态资源缓存
  * 通过链接名称控制缓存，< script >标签只要内容不变，链接名称才变
* 使用CDN 让资源加载更快
  * http://www.bootcdn.cn/
* 使用SSR 后端渲染，数据直接输出到HTML中
  * 现在 Vue React 提出了这样的概念
  * 其实 jsp php asp 都属于后端渲染

##### 渲染优化

* CSS放前面，JS放后面
* 懒加载（图片懒加载、下拉加载更多）

  ``` html
  <!-- 先显示小图片，必要时显示real图片 -->
  <img id="img1" src="preview.png" data-realsrc="abc.png">
  <script>
    var img1 = document.getElementById('img1')
    img1.src = img.getAttribute('data-realsrc')
  </script>
  ```

* 减少DOM查询，对DOM查询做缓存

  ``` js
  // 未缓存 DOM 查询
  var i
  for (i = 0; i < document.getElementsByTagName('p'.length; i++) {
    // todo
  })

  // 缓存了DOM 查询
  var pList = document.getElementsByTagName('p')
  var i
  for (i = 0; i< pList.length; i++) {
    //todo
  }
  ```

* 减少DOM操作，多个操作尽量合并在一起执行

  ``` js
  var listNode = document.getElementById('list')

  // 要插入 10 个 li 标签
  // 先定义片段，对片段进行插入
  var frag = document.createDocumentFragment();
  var x, li;
  for(x = 0; x < 10; x++) {
    li = document.createElement("li");
    li.innerHTML = "List item " + x;
    frag.appendChild(li);
  }

  listNode.appendChild(frag);
  ```

* 事件节流

  ``` js
  var textarea = document.getElementById('text')
  var timeoutId
  textarea.addEvemtListener('keyup', function () {
    if (timeoutId) {
      clearTimeout(timeoutId)
    }
    timeoutId = setTimeout(function () {
      // 触发 change 事件
    }, 100)
  })
  ```

* 尽早执行操作（如DOMContentLoaded）

### 安全性

#### 安全性知识点

* XSS 跨站请求攻击
* XSRF 跨站请求伪造

##### XSS

* 在新浪博客写一篇文章，同时偷偷插入一段< script >
* 攻击代码中，获取cookie，发送到自己的服务器

解决方法

* 前端替换关键字，例如替换 < 为 & lt; 
* 后端替换

##### XSRF

* 你已登录一个购物网站，正在浏览商品
* 该网站付费接口是 xxx.com/pay?id=100 但是没有任何验证
* 然后你收到一封邮件，隐藏着< img src= xxx.com/pay?100>
* 你查看邮件的时候，就已经悄悄的付费购买了

解决方法

* 增加验证流程，如输入指纹、密码、短信验证码

## gitbub地址 持续更新 可下载
[https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning](https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning)