遇到的兼容问题记录


[TOC]



###1.下拉刷新的问题

绑定事件时preventDefault 无效 

解决办法:

[	设置passive为false](<https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener>)

