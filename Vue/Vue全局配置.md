---
title: Vue全局配置
date: 2021-03-19 11:58:30
tags:
	- Vue
categories:
	- Vue
---

# Vue全局配置

Vue的全局配置指的是`Vue.config`的各项属性，具体如下

+ `silent:`用于取消所有的Vue日志与警告，布尔值

  ```js
  Vue.config.silent = true
  ```

+ `optionMergeStrategies:`

+ `devtools:`是否允许[vue-devtools](https://github.com/vuejs/vue-devtools)检查代码，布尔值，开发版本默认为 `true`，生产版本默认为 `false`。

+ `errorHandler:`指定Vue组件的渲染和观察期间未捕获错误的处理函数，被调用时可获取错误信息和Vue实例

  ```js
  Vue.config.errorHandler = function(error, vm, info){
    // `info` 是 Vue 特定的错误信息，比如错误所在的生命周期钩子
    // 只在 2.2.0+ 可用
  }
  ```

+ `warnHandler:`为Vue运行时的警告自定义一个处理函数，只在开发者环境下生效，生产环境会被忽略

+ `ignoredElements:`须使Vue忽略在Vue之外定义的元素（如使用Web Elements APIs创建的元素），否则会抛出一个Unknown custom element的错误，`Array<string|regExp>`，默认空数组

  ```js
  Vue.config.ignoredElements: [
      'my-custom-web-component',
    	'another-web-component',
    	// 用一个 `RegExp` 忽略所有“ion-”开头的元素
    	// 仅在 2.5+ 支持
    	/^ion-/
  ]
  ```

+ `keyCode:`给`v-on`自定义键位别名，`{[key]: string | Array<number>}`，默认空对象

  ```js
  Vue.config.keyCode = {
   	v: 86,
      f1: 112,
      'media-play-pause': 179,	//不能用驼峰写法mediaPlayPause
      up: [38, 87]
  }
  ```

  ```html
  <input type="text" @keyup.media-play-pause="method">
  ```

+ `performance:`设置为 `true` 以在浏览器开发工具的性能/时间线面板中启用对组件初始化、编译、渲染和打补丁的性能追踪。只适用于开发模式和支持 [`performance.mark`](https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark) API 的浏览器上。布尔值，默认false

+ `productionTip:`设置为 `false` 以阻止 vue 在启动时生成生产提示，布尔值，默认true。