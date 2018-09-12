---
title: Element-UI踩坑记录
date: 2018-09-12
categories: "Notes"
Author: 李赛飞
---

# ELement-UI 使用注意

## 1.带输入建议的输入框`el-autocomplete`

`el-autocomplete`是一个可带输入建议的输入框组件。[官方示例](http://element-cn.eleme.io/#/zh-CN/component/input)

使用时，在显示建议数据的时候要注意，数据接口的属性名必须要为`value`,否则无法显示数据。