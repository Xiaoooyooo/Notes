# Node.js 中的事件循环

首先，Node.js中的JavaScript与浏览器中的JavaScript事件循环机制是不一样的。

在浏览器中，（此处省略不知道多少字...）

而在Node.js（下文简称Node）中，请看下文。

在Node中，一个事件循环可以分为六个部分：

+ **timers阶段：**在这个阶段执行定时器相关（setTimeout、setInterval）的回调；
+  **I/O callbacks 阶段：**在这个阶段执行一些系统调用错误，比如网络通信的错误回调；
+  **idle, prepare 阶段：**仅node内部使用；
+  **poll 阶段：** 检索新的 I/O 事件;执行与 I/O 相关的回调（几乎所有情况下，除了关闭的回调函数，它们由计时器和 `setImmediate()` 排定的之外），其余情况 node 将在此处阻塞；
+  **check 阶段：**执行 `setImmediate()` 的回调；
+  **close callbacks 阶段**：执行 `socket` 的 `close` 事件回调。

对于上面六个阶段，每一个阶段都有属于它们自己的一个事件队列，我们只需要知道`timers`、`poll`、`check`这三个阶段就好，因为我们绝大多数的异步任务都是在这三个阶段完成的。

## timers阶段

这是Node事件循环的第一个阶段，在这个阶段Node会去检查有没有已经过期的定时器，并将已经过期的定时器回调函数添加至当前阶段的任务队列，然后执行。

## poll阶段

这是Node事件循环的第四个阶段，主要有两个功能：

1. 计算应该阻塞和轮询 I/O 的时间；
2. 然后，处理 **轮询** 队列里的事件。



## check阶段



## 参考资料

+ [Node.js 事件循环，定时器和 process.nextTick()](https://nodejs.org/zh-cn/docs/guides/event-loop-timers-and-nexttick/)

+ [深入理解js事件循环机制（Node.js篇）](http://lynnelv.github.io/js-event-loop-nodejs)