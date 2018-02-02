# Chapter8  JS开发环境

#### 这是我自己的学习过程总结，如果有什么写的不清楚的地方，欢迎大家交流、批评、提问。可以在这里评论，也可以在github上提问，谢谢大家

1. IDE（写代码的效率）
2. git（代码版本管理，多人协作开发）
3. JS模块化
4. 打包工具
5. 上线回滚的流程

## 8.1  IDE

* webstorm
* sublime
* vscode
* atom
* 插件

## 8.2  Git

* 正式项目都需要代码版本管理
* 大型项目需要多人协作开发
* Git 和 linux 是一个作者
* 网络Git服务器如 coding.net  github.com gitee.com
* 一般公司代码非开源，都有自己的Git服务器

### 常用Git命令

* git add .  // 增加某个或全部文件
* git checkout xxx // 取消某个文件
* git commit -m "xxx" //提交本地仓库 后面是备注
* git push origin master // 提交远程仓库的master分支
* git pull origin master // 获取
* git branch // 当前分支
* git checkout -b xxx / git checkout xxx // 新建分支，切换分支
* git merge xxx // 合并分支代码

## 8.3  JS模块化

### 不实用模块化

层层引用

* util.js getFormatDate函数
* a.util.js aGetFormatDate函数 使用getFormatDate函数
* a.js aGetFormatDate

``` js
// util.js
function getFormatDate(date, type) {
  // type === 1 返回 2017-06-15
  // type === 2 返回 2017年6月15日
  // ...
}

// a-util.js
function aGetFormatDate(date) {
  // 要求返回 2017年6月15日 格式
  return GetFormatDate(date, 2)
}

// a.js
var dt = new Date()
console.log(aGetFormatDate(dt))
```

``` html
<!-- 使用  -->
<script src="util.js"></script>
<script src="a-util.js"></script>
<script src="a.js"></script>

<!-- 1. 这些代码中的函数必须是全局变量，才能暴漏给使用方，全局变量污染 -->
<!-- 2. a.js 知道要引用 a-util.js，但是他知道还需要依赖与util.js吗？  -->
```

### 使用模块化

``` js
//util.js
export {
  getFormatDate: function (date, type) {
  // type === 1 返回 2017-06-15
  // type === 2 返回 2017年6月15日
  // ...
  }
}

// a-util.js
var getFormatDate = require('util.js')
export {
  aGetFormatDate: function (date) {
  // 要求返回 2017年6月15日 格式
  return GetFormatDate(date, 2)
  }
}

// a.js
var aGetFormatDate = require('a-util.js')
var dt = new Date()
console.log(aGetFormatDate(dt))

// 使用直接`<script src="a.js"></script>`，其他的根据依赖关系自动引入
// 那两个函数没必要做成全局变量，不会带来污染和覆盖
```

#### AMD(Asynchronous Module Definition 异步模块定义)

* require.js  requirejs.org/
* 全局 define 函数
* 全局 require 函数
* 依赖JS会自动、异步加载

#### CommonJS

nodejs 模块化规范，现在被大量用于前端开发的原因：

* 前端开发依赖的插件和库，都可以从npm中获取
* 构建工具的高度自动化，使得使用npm的成本非常低
* CommonJS不会异步加载JS，而是同步一次性加载出来

### 打包工具

#### webpack

* 参看专门说明webpack的文章

### 上线和回滚

#### 1.上线和回滚的基本流程

* 上线流程的要点
  * 将测试完成的代码提交到git版本库的master分支
  * 将当前服务器的代码全部打包并记录版本号，备份
  * 将master分支的代码提交覆盖到线上服务器，生成新版本号
* 回滚流程要点
  * 将当前服务器的代码打包并记录版本号，备份
  * 将备份的上一个版本号解压，覆盖到线上服务器，并生成新的版本号

#### 2. linux基本命令

``` cmd
mkdir a
ls
ll
cd a
pwd
cd ../
rm -rf a
vi a.js
cp a.js a1.js
mkdir src
mv a1.js src/a1.js
rm a.js
cat a.js
head -n 1 a.js
tail -n 2 a.js
grep '2' a.js
```

## gitbub地址 持续更新 可下载
[https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning](https://github.com/zust-hh/JavaScript_interviewing_knowledge_learning)