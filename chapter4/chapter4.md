# Chapter4  JS基础知识（下1）-- 异步和单线程

#### 这是我自己的学习过程总结，如果有什么写的不清楚的地方，欢迎大家交流、批评、提问。可以在这里评论，也可以在github上提问，谢谢大家

## 题目

* 同步和异步的区别是什么？分别举一个同步和异步的例子
* 一个关于setTimeout的笔试题
* 前端使用异步的场景有哪些

## 知识点

* 什么是异步（对比同步）
* 前端使用异步的场景
* 异步和单线程

### 什么是异步

``` js
console.log(100)
setTimeout(function () {
  console.log(200)
}, 1000)
console.log(300)
// 100 300 200
```

### 前端使用异步的场景

**在可能等待的情况，等待过程中不能像alert一样阻塞程序运行，因此，所有的“等待的情况”都需要异步**

* 定时任务：setTimeout,setInverval
* 网络请求：ajax请求，动态< img >加载
* 事件绑定

**ajax请求代码示例**

``` js
console.log('start')
$.get('./data1.json', function(data1) {
  console.log(data1)
})
console.log('end)
```

**< img >加载示例**

``` js
console.log('start')
var img = document.createElement('img')
img.onload = function () {
  console.log('loaded')
}
img.src = '/xxx.png'
console.log('end')
```

**事件绑定示例**

``` js
console.log('start')
decument.getElementById('btn1').addEventListener('click', function () {
  alert('clicked')
})
console.log('end')
```

### 异步和单线程

**一次只能干一个事，暂存起来的函数会根据自己的代码判断什么时候解封**

* 执行第一行，打印100
* 执行setTimeout后，传入setTimeout的函数会被暂存起来，不会立即执行（单线程的特点，不能同时干两件事）
* 执行最后一行，打印300
* 待所有程序执行完，处于空闲状态时，会立马看有没有暂存起来的要执行
* 发现暂存起来的setTimeout中的函数无需等待时间,就立即拿过来执行

## 解答

* 同步和异步的区别是什么？分别举一个同步和异步的例子
  * 同步会阻塞代码执行，而异步不会
  * alert是同步，setTimeout是异步

* 一个关于setTimeout的笔试题

  ``` js
  console.log(1)
  setTimeout(function () {
    console.log(2)
  }, 0)
  console.log(3)
  setTimeout(function () {
    console.log(4)
  }, 1000)
  console.log(5)
  // 1 3 5 2 4
  ```

* 前端使用异步的场景
  * 定时任务：setTimeout,setInverval
  * 网络请求：ajax请求，动态< img >加载
  * 事件绑定

## gitbub地址 持续更新 可下载
[https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning](https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning)