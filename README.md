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

央视m3u8流 http://ivi.bupt.edu.cn/hls/cctv1hd.m3u8
type: 'application/x-mpegURL'


## 6. 提示添加到主屏幕  A2HS

经过调研 只有安装chrome 支持此主动提示
iphone里 只有safir是真浏览器 

[添加到主屏幕](https://developer.mozilla.org/zh-CN/docs/Web/Progressive_web_apps/%E6%B7%BB%E5%8A%A0%E5%88%B0%E4%B8%BB%E5%B1%8F%E5%B9%95)
[示例](https://mdn.github.io/pwa-examples/a2hs/)
[相关资料](https://love2dev.com/pwa/add-to-homescreen-library/)




## 7.  安卓软键盘遮挡输入框 以及 ios 软键盘顶起页面导致页面上移问题 （老问题了，只能根据页面来选择性优化）


介绍： 
    页面是固定一屏高度 专家列表以及聊天室是动态高度根据不同屏幕来自适应高度来实现一屏幕效果。


[事故页面](wx.catjc.com/#/walkman)

安卓：把输入框定位属性改为 `fixed` 即可 安卓键盘像是把页面渲染高度缩小了。
ios： ios软键盘则是直接在把整体页面顶起 并且页面变为可滚动（可监听到页面滚动）
ios环境下 输入框聚焦时 监听滚动条滚动距离x，然后将x赋值到输入框的 `bottom:x +'px'` 即可。 缺点：页面还是可滚动。
 

 ## 8. ipx以上在webview的底部导航条兼容问题



meta-viewport 添加 viewport-fit=cover
以及 媒体查询 @supports
```
<meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1, user-scalable=no,viewport-fit=cover" />

@supports (bottom: constant(safe-area-inset-bottom)) or (bottom: env(safe-area-inset-bottom)) { 
    
    .tabbar{
        padding-bottom: constant( safe-area-inset-bottom); /* 兼容 iOS < 11.2 */
        padding-bottom: env( safe-area-inset-bottom); /* 兼容 iOS >= 11.2 */
    }
}

```

## 8. react 某元素 xxx.addEventListener('xxx')  绑定`原生事件`无法阻止冒泡

* 场景: 

    一个页面 父组件监听了 `touchStart touchMove touchEnd`  

   子组件弹窗时 不想触发父组件的 `原生事件` 


* 注: 

     阻止原生事件,只能通过判断标签
    父组件所监听的`touch`系列事件 基本需要大部分元素有效
    

* 解决办法 旧 存在不兼容 阻止ios滑屏事件:

    将父组件的`原生事件`改为`react的合成事件`

    子组件注册`react合成事件` 并调用 `e.stopPropagation()`

     ```
    //父组件
    div.addEventListener('touchstart', this.touchStart);
    改为
    <div  onTouchStart={this.touchStart}></div>

    //子组件
    <div  onTouchStart={e => e.stopPropagation()}></div>

    ```
* 解决办法 新: 

    父组件继续使用`原生事件`,子组件也使用`原生事件`
    
    ```
    div.addEventListener('touchstart', this.touchStart,{passive: false});
    div.addEventListener('touchmove', this.touchMove,{passive: false});
    div.addEventListener('touchend', this.touchEnd,{passive: false});

    //离开页面记得卸载 xxx.removeEventListener(....)


    //子组件 只需要阻止移动 事件即可
    div.addEventListener('touchmove', this.touchmove,{passive: false});

    touchmove =  e => {
        //阻止浏览器滚动
        e.preventDefault();
        //阻止冒泡
        e.stopPropagation();
    }

    ```


