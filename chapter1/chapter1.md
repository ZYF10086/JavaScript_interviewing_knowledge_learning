# Chapter 1

##几道面试题思考

()内为自我思考
1. JS中使用typeof能得到哪些类型(String number Array Object Null )
2. 何时使用 === 何时使用 == (===完全一样，地址也得一样)
3. window.onload和DOMContentLoaded的区别（window.onload表示页面全部加载出来后触发，DOMContentLoaded是DOM元素全部加载出来后触发，样式、图片等还没有加载）
4. 用JS创建10个< a >标签，点击的时候弹出来对应的序号(见chapter1-04.html)
5. 简述如何实现一个模块加载器，实现类似require.js的基本功能（模块加载器会根据所加载module的内容不同，对同一个对象进行修改，这个对象应该有export用于外界调用，构建不同的对象，再返回给调用module，）
6. 实现数组的随机排序

####考点

1. JS中对象类型
2. 强制类型转换
3. 浏览器的渲染过程
4. 作用域
5. JS模块化
6. JS基础算法
