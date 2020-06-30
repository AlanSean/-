遇到的兼容问题记录

[TOC]

## 1.下拉刷新的问题

绑定事件时 preventDefault 无效

解决办法:

[设置 passive 为 false](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)

## 2.跨域给 localStorage 带来的问题 (2020-5-7)

和 pp 体育合作的项目，点击轮播图跳转到其他模块时，网页导航栏被刘海(苹果特色)遮住了。

解决办法:

1.  先定位是不是计算的手机状态栏高度的问题(无问题)
2.  看看是不是本地存储，再次取出时有没有问题
    (站内路由跳转无问题，点击轮播图是直接使用 location 跳转，发现跳转后本地的值没了)
3.  先查看代码删除 localStorage 逻辑(无问题)
4.  猜测值还在，可能是跨域了所以取不到了
    经过核实 发现跳转链接是 http 网站是 https 协议不同所以是跨域了
5.  找后台让她将写死的协议 http:// 改为 //

## 3.python 使用 vscode import 的问题

import 时 总是提示 unresolved import XXX

```
解决办法:
    打开vscode设置 - 搜索jedi - 第一个打勾
    launch.json添加
    "env": {
        "PYTHONPATH": "${workspaceRoot}"
    },
    "envFile": "${workspaceFolder}/.env"
```

## 4. 第三方 mac 环境配置问题

node 版本低 [mac 参考升级](https://www.jianshu.com/p/71c82fc63522)

权限问题 运行下面命令 进行修改权限

`sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}`



## 5. videojs横屏 x5层级问题 2020-6-30

[x5层级问题](https://x5.tencent.com/tbs/guide/video.html)
[videojs横屏插件](https://github.com/prateekrastogi/videojs-landscape-fullscreen)
