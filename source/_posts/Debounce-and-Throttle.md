---
title: Debounce and Throttle
date: 2018-06-22 16:41:29
tags: [JS,网站优化]
---

# 防抖动与节流

> [DEMO](http://yangfan2016.github.io/myweb2016/demo/%E5%89%8D%E7%AB%AF%E4%BC%98%E5%8C%96/%E9%98%B2%E6%8A%96%E4%B8%8E%E8%8A%82%E6%B5%81/)

### 防抖动
```js
var debounce = function (fn, delay, isImmediate) {
    var timer = null;
    // 默认不立即触发
    isImmediate = typeof isImmediate === "undefined" ? false : isImmediate;

    return function () {
        var ctx = this, // 保存作用域
            args = arguments; // 保存参数
        // 初始化清空所有定时器
        if (timer) {
            clearTimeout(timer);
        }
        // 如果是立即触发
        if (isImmediate) {
            if (!timer) { // timer为空时触发操作
                fn.apply(ctx, args);
            }
            // delay时间后置空timer
            timer = setTimeout(_ => {
                timer = null;
            }, delay);
        } else { // delay时间后触发操作
            timer = setTimeout(_ => {
                fn.apply(ctx, args);
            }, delay);
        }
    };
};
```

防抖动立即触发
![debounce-immediate.png](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/debounce-immediate.png)

防抖动
![debounce.png](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/debounce.png)

### 节流
```js
var throttle = function (fn, delay, isImmediate) {
    var timer = null;
    // 默认立即触发
    isImmediate = typeof isImmediate === "undefined" ? true : isImmediate;

    return function () {
        var ctx = this, // 保存作用域
            args = arguments; // 保存参数
        if (!timer) { // timer为空时
            if (isImmediate) fn.apply(ctx, args); // 立即触发
            timer = setTimeout(function () {
                clearTimeout(timer);
                timer = null;
                if (!isImmediate) fn.apply(ctx, args); // delay时间后触发操作
            }, delay);
        }
    };
};

```
节流立即触发
![throttle-immediate.png](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/throttle-immediate.png)
节流
![throttle.png](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/throttle.png)
### 总结

- 防抖动：将多个操作合并为一个操作（例如，键盘输入关键字搜索内容），在规定延时时间后触发，如果在定时器时间范围内触发，则会清楚定时器，重新计时
- 节流：在给定的延时时间后触发一次操作，在此时间范围内的操作均不触发（例如，图片懒加载、向下无限滚动获取新数据）