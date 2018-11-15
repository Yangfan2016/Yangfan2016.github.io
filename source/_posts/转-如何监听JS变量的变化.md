---
title: '[转]如何监听JS变量的变化'
date: 2017-09-20 14:04:08
tags:
---



# 如何监听JS变量的变化[转]

## 读前必看

Object.defineProperty()  [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)


## 如何监听 js 中变量的变化?

我现在有这样一个需求，需要监控js的某个变量的改变，如果该变量发生变化，则触发一些事件，不能使用timeinterval之类的定时去监控的方法，不知道有比较好的解决方案么？

这个问题问的很好。

流行的MVVM的JS库/框架都有共同的特点就是数据绑定，在数据变更后响应式的自动进行相关计算并变更DOM展现。所以这个问题也可以理解为如何实现MVVM库/框架的数据绑定。

常见的数据绑定的实现有脏值检测，基于ES5的getter和setter，以及ES已被废弃的Object.observe，和ES6中添加的Proxy。

## 脏值检测

angular使用的就是脏值检测，原理是比较新值和旧值，当值真的发生改变时再去更改DOM，所以angular中有一个$digest。那么为什么在像ng-click这样的内置指令在触发后会自动变更呢？原理也很简单，在ng-click这样的内置指令中最后追加了$digest。

简易的实现一个脏值检测：


```html

<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>two-way binding</title>
    </head>
    <body onload="init()">
        <button ng-click="inc">
            Increase
        </button>
        <button ng-click="reset">
            Reset
        </button>
        <span style="color:red" ng-bind="counter"></span>
        <span style="color:blue" ng-bind="counter"></span>
        <span style="color:green" ng-bind="counter"></span>
        <script type="text/javascript">
            /* 数据模型区开始 */
            var counter = 0;
            function inc() {
                counter++;
            }
            function reset() {
                counter = 0;
            }
            /* 数据模型区结束 */
            /* 绑定关系区开始 */
            function init() {
                bind();
            }
            function bind() {
                var list = document.querySelectorAll("[ng-click]");
                for (var i=0; i<list.length; i++) {
                    list[i].onclick = (function(index) {
                        return function() {
                            window[list[index].getAttribute("ng-click")]();
                            apply();
                        };
                    })(i);
                }
            }
            function apply() {
                var list = document.querySelectorAll("[ng-bind='counter']");
                for (var i=0; i<list.length; i++) {
                    if (list[i].innerHTML != counter) {
                        list[i].innerHTML = counter;
                    }
                }
            }
            /* 绑定关系区结束 */
        </script>
    </body>
</html>

```

这样做的坏处是自己变更数据后，是无法自动改变DOM的，必须要想办法触发apply()，所以只能借助ng-click的包装，在ng-click中包含真实的click事件监听并追加脏值检测以判断是否要更新DOM。

另外一个坏处是如果不注意，每次脏值检测会检测大量的数据，而很多数据是没有检测的必要的，容易影响性能。

关于如何实现一个和angular一样的脏值检测，知道原理后还有很多工作要去做，以及如何优化等等。如果有兴趣可以看看民工叔曾经推荐的《Build Your Own Angular.js》，第一章Scope便讲了如何实现angular的作用域和脏值检测。对了，上面的例子也是从民工叔的博客稍加修改来的，建议最后去看下原文，链接在参考资料中。

ES5的getter与setter
在ES5中新增了一个Object.defineProperty，直接在一个对象上定义一个新属性，或者修改一个已经存在的属性， 并返回这个对象。


```js

Object.defineProperty(obj, prop, descriptor)

```

其接受的第三个参数可以取get和set并各自对应一个getter和setter方法：

```js

var a = { zhihu:0 };
Object.defineProperty(a, 'zhihu', {
  get: function() {
    console.log('get：' + zhihu);
    return zhihu;
  },
  set: function(value) {
    zhihu = value;
    console.log('set:' + zhihu);
  }
});
a.zhihu = 2; // set:2
console.log(a.zhihu); // get：2
                      // 2
```

基于ES5的getter和setter可以说几乎完美符合了要求。为什么要说几乎呢？

首先IE8及更低版本IE是无法使用的，而且这个特性是没有polyfill的，无法在不支持的平台实现，
这也是基于ES5getter和setter的Vue.js不支持IE8及更低版本IE的原因。也许有人会提到avalon，avalon在低版本IE借助vbscript一些黑魔法实现了类似的功能。

除此之外，还有一个问题就是修改数组的length，直接用索引设置元素如items[0] = {}，以及数组的push等变异方法是无法触发setter的。
如果想要解决这个问题可以参考Vue的做法，在Vue的observer/array.js中，Vue直接修改了数组的原型方法：

```js

const arrayProto = Array.prototype
export const arrayMethods = Object.create(arrayProto)
/**
 * Intercept mutating methods and emit events
 */
;[
  'push',
  'pop',
  'shift',
  'unshift',
  'splice',
  'sort',
  'reverse'
]
.forEach(function (method) {
  // cache original method
  var original = arrayProto[method]
  def(arrayMethods, method, function mutator () {
    // avoid leaking arguments:
    // http://jsperf.com/closure-with-arguments
    var i = arguments.length
    var args = new Array(i)
    while (i--) {
      args[i] = arguments[i]
    }
    var result = original.apply(this, args)
    var ob = this.__ob__
    var inserted
    switch (method) {
      case 'push':
        inserted = args
        break
      case 'unshift':
        inserted = args
        break
      case 'splice':
        inserted = args.slice(2)
        break
    }
    if (inserted) ob.observeArray(inserted)
    // notify change
    ob.dep.notify()
    return result
  })
})

```

这样重写了原型方法，在执行数组变异方法后依然能够触发视图的更新。

但是这样还是不能解决修改数组的length和直接用索引设置元素如items[0] = {}的问题，想要解决依然可以参考Vue的做法：
前一个问题可以直接用新的数组代替旧的数组；后一个问题可以为数组拓展一个$set方法，在执行修改后顺便触发视图的更新。

已被废弃的Object.observe
Object.observe曾在ES7的草案中，并在提议中进展到stage2，最终依然被废弃。
这里只举一个MDN上的例子：

```js

// 一个数据模型
var user = {
  id: 0,
  name: 'Brendan Eich',
  title: 'Mr.'
};
// 创建用户的greeting
function updateGreeting() {
  user.greeting = 'Hello, ' + user.title + ' ' + user.name + '!';
}
updateGreeting();
Object.observe(user, function(changes) {
  changes.forEach(function(change) {
    // 当name或title属性改变时, 更新greeting
    if (change.name === 'name' || change.name === 'title') {
      updateGreeting();
    }
  });
});

```

由于是已经废弃了的特性，Chrome虽然曾经支持但也已经废弃了支持，这里不再讲更多，有兴趣可以搜一搜以前的文章，这曾经是一个被看好的特性（Object.observe()带来的数据绑定变革）。
当然关于它也有一些替代品Polymer/observe-js。

ES6带来的Proxy
人如其名，类似HTTP中的代理：

```js

var p = new Proxy(target, handler);
target为目标对象，可以是任意类型的对象，比如数组，函数，甚至是另外一个代理对象。
handler为处理器对象，包含了一组代理方法，分别控制所生成代理对象的各种行为。

```

举个例子：

```js

let a = new Proxy({}, {
  set: function(obj, prop, value) {
    obj[prop] = value;
    if (prop === 'zhihu') {
      console.log("set " + prop + ": " + obj[prop]);
    }
    return true;
  }
});
a.zhihu = 100;

```

当然，Proxy的能力远不止此，还可以实现代理转发等等。

但是要注意的是目前浏览器中只有Firefox18支持这个特性，而babel官方也表明不支持这个特性：

> Unsupported feature   
Due to the limitations of ES5, Proxies cannot be transpiled or polyfilled.


目前已经有babel插件可以实现，但是据说实现的比较复杂。
如果是Node的话升级到目前的最新版本应该就可以使用了，上面的例子测试环境为Node v6.4.0。


原文：[https://blog.daraw.cn/2016/08/17/how-to-monitor-changes-of-js-variable/](https://blog.daraw.cn/2016/08/17/how-to-monitor-changes-of-js-variable/)
