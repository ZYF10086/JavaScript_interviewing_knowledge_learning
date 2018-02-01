# Chapter5  JS基础知识（下2） ---  其他知识

#### 这是我自己的学习过程总结，如果有什么写的不清楚的地方，欢迎大家交流、批评、提问。可以在这里评论，也可以在github上提问，谢谢大家

## 题目

* 获取 2017-06-10 格式的日期
* 获取随机数，要求是长度一致的字符串格式
* 写一个能遍历对象和数组的通用 forEach 函数

## 知识点

* 日期
* Math
* 数组API
* 对象API

### 日期

``` js
Date.now() // 获取当前时间毫米数 1970年至今
var dt = new Date()
dt.getTime() //获取毫米数
dt.getFullYear() //年
dt.getMonth() //月 （0-11）
dt.getDay() // 星期几 （0-6）星期日-星期六
dt.getDate() //日 （1-31）
dt.getHours() // 小时 （0-23）
dt.getMinutes() // 分钟（0-59）
dt.getSeconds()  //秒 （0-59）
```

### Math

* 获取随机数 Math.random() （放在url后面，用于清除缓存）

### 数组API

* forEach 遍历所有元素

  ``` js
  var arr = [1,2,3]
  arr.forEach(function (item, index) {  // item是每一个元素，index是数组中的顺序
    console.log(index, item)
  })
  ```

* every 判断所有元素是否都符合条件

  ``` js
  var arr = [1,2,3]
  var result = arr.every(function (item, index) {
    if(item < 4) {
      return true
    }
  })
  console.log(result)  //true
  ```

* some 判断是否至少一个元素符合条件

  ``` js
  var arr = [1,2,3]
  var result = arr.some(function (item, index) {
    if(item < 2) {
      return true
    }
  })
  console.log(result)  //true
  ```

* sort 排序

  ``` js
  var arr = [1,4,2,3,5]
  var arr2 = arr.sort(function (a, b) {
    // 从小到大排序
    return a - b
    // 从大到小排序
    // return b - a
  })
  console.log(arr2)
  ```

* map 对元素重新组装，生成新数组

  ``` js
  var arr = [1,2,3,4]
  var arr2 = arr.map(function(item, index) {
    // 将元素重新组装，并返回
    return '<b>' + item + '</b>'
  })
  console.log(arr2)
  ```

* filter 过滤符合条件的元素

  ``` js
  var arr = [1,2,3]
  var arr2 = arr.filter(function (item, index) {
    // 通过某条件过滤数组
    if (item >= 2) {
      return true
    }
  })
  console.log(arr2) // [2,3]
  ```

### 对象API

* for in

  ``` js
  var obj = {
    x: 100,
    y: 200,
    z: 300
  }
  var key
  for (key in obj) {
    // 注意这里的hasOwnProperty，在讲原型链的时候讲过了
    if (obj.hasOwnProperty(key)) {
      console.log(key, obj[key])
    }
  }
  // x 100
  // y 200
  // z 300
  ```

## 解答

* 获取2017-06-10格式的日期

  ``` js
  function formatDate(dt) {
    if(!dt) {
      dt = new Date()
    }
    var year = dt.getFullYear()
    var month = dt.getMonth() + 1
    var date = dt.getDate()
    if(month < 10) {
      month = '0' + month
    }
    if(date < 10) {
      date = '0' + date
    }
    return year +  '-'  + month + '-' + date
  }
  var dt = new Date()
  var formatDate = formatDate(dt)
  console.log(formatDate)
  ```

* 获取随机数，要求是长度一致的字符串格式

  ```js
  var random = Math.random()
  var random = random + '0000000000'  // 后面加上10个零
  var random = random.slice(0, 10)
  console.log(random)
  ```

* 写一个能遍历对象和数组的通用forEach函数

  ``` js
  function forEach(obj, fn) {
    var key
    if (obj instanceof Array) {
      obj.forEach(function (item, index) {
        fn(index, item)
      })
    } else {
      for (key in obj) {
        fn(key, obj[key])
      }
    }
  }
  // 使用
  var arr = [1,2,3]
  forEach(arr, function (index, item) {
    console.log(index, item)
  })

  var obj = {x: 100, y: 200}
  forEach(obj, function(key, value) {
    console.log(key, value)
  })
  ```

## gitbub地址 持续更新 可下载
[https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning](https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning)