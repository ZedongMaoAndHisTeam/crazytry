---
title: 使用Vue监听窗口大小变化的方法
date: 2018-08-28
categories: "Notes"
Author: 李赛飞
---
#### 项目需求：
根据窗口大小的不同自适应显示列表。
#### 解决思路：
获取窗口大小，根据窗口大小来动态调整列表显示数目的变量`pageSize`。
#### 难点
Vue在监听window事件上的先天不足，如何解决对window.resize事件的监听？
#### 解决方案
在生命周期钩子函数`mounted`中添加事件监听。
```javascript
    mounted: function() {
      this.$nextTick(
        window.addEventListener('resize', this.handleResize)
      )
    },
    beforeDestroy: function() {
      window.removeEventListener('resize', this.handleResize)
    },
```