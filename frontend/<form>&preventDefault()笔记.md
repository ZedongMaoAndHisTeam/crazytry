---
title: <form>和preventDefault()笔记
date: 2018-09-10
categories: "Notes"
Author: 李赛飞
---
Vue官方demo里的SVGTable,有一段代码：

```javascript
add: function (e) {
      // 为什么preventDefault()是必需的？
      e.preventDefault()
      if (!this.newLabel) return
      this.stats.push({
        label: this.newLabel,
        value: 100
      })
      this.newLabel = ''
    }
```

[在线预览](http://jsbin.com/xotonac/edit?html,output)

与add()方法绑定的是一个Button,如果将e.preventDefault()方法移除会发现add()方法失效，原因是：
> `<form>`下第一个没有明确type的`<button>`元素默认为submit,`<form>`的submit会刷新页面.