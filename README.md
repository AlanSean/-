遇到的兼容问题记录


[TOC]



### 1.下拉刷新的问题

绑定事件时preventDefault 无效 

解决办法:

[设置passive为false](<https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener>)



### 2.跨域给localStorage带来的问题  (2020-5-7)

和pp体育合作的项目，点击轮播图跳转到其他模块时，网页导航栏被刘海(苹果特色)遮住了。

解决办法: 

1.  先定位是不是计算的手机状态栏高度的问题(无问题)
2. 看看是不是本地存储，再次取出时有没有问题
    (站内路由跳转无问题，点击轮播图是直接使用location跳转，发现跳转后本地的值没了)
3. 先查看代码删除localStorage逻辑(无问题)
4. 猜测值还在，可能是跨域了所以取不到了
    经过核实 发现跳转链接是http  网站是https 协议不同所以是跨域了
5. 找后台让她将写死的协议http:// 改为 // 

