---
title: 快速构建一个 vue 插件
date: 2019-01-10 17:05:29
tags:
---


### 碎碎念

上一篇[文章](https://juejin.im/post/5c2f248b51882525030dc50b)，我们介绍了如何构建一个 react 插件，今天我们说说如何构建 vue 插件

### 准备工作

由于与上一篇 react 插件文章使用的是相同的结构，代码测试、持续集成及发布 npm 包也都是一个套路，这里就不再敖述。
下面主要说下不同的地方，let's start  😊

1. 开发依赖包

```js
{
    "devDependencies": {
        "@babel/core": "^7.0.0",
        "@babel/preset-env": "^7.0.0",
        "babel-loader": "^8.0.2",
        "chai": "^4.2.0",
        "coveralls": "^3.0.2",
        "css-loader": "^1.0.0",
        "jest": "^23.6.0",
        "style-loader": "^0.23.1",
        "vue": "^2.5.21",
        "vue-loader": "^15.5.0", // 解析 SFC 文件
        "vue-style-loader": "^4.1.2",
        "vue-template-compiler": "^2.5.21", // vue-loader 的同步依赖 
        "webpack": "^4.17.2",
        "webpack-cli": "^3.1.0"
    },
}
```

2. webpack 配置

```js
const path = require('path');
const { VueLoaderPlugin } = require("vue-loader");

module.exports = {
    mode: "production", // 生产模式
    entry: { // 入口
        "YanProgress": path.resolve(__dirname, './src/index.js')
    },
    output: { // 出口
        path: path.resolve(__dirname, './dist'),
        filename: '[name].min.js',
        publicPath: "./dist/",
        libraryTarget: 'umd', // 按 UMD 模式打包
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader',
                options: {
                    // 模板编译过程中，编译器可以将某些特性转换为 require 调用
                    transformAssetUrls: {
                        video: ['src', 'poster'],
                        source: 'src',
                        img: 'src',
                        image: 'xlink:href' // SVG
                    }
                },
                // 只命中src目录里的js文件，加快 Webpack 搜索速度
                include: path.resolve(__dirname, "./src"),
            },
            {
                test: /\.js$/,
                use: [
                    {
                        loader: 'babel-loader',
                        options: {
                            presets: ['@babel/preset-env']
                        }
                    },
                ],
                // 只命中src目录里的js文件，加快 Webpack 搜索速度
                include: path.resolve(__dirname, "./src/"),
            },
            {

                test: /\.css$/,
                loader: "style-loader!css-loader"
            }
        ]
    },
    plugins: [
        // vue-loader **这个插件是必须的！**它的职责是将你定义过的其它规则复制并应用到 .vue 文件里相应语言的块。
        new VueLoaderPlugin
    ],
    resolve: { // 省略文件后缀时，默认按下面的配置取
        extensions: ['.js', '.vue'],
    },
    externals: { // 不把 vue 打包进去
        vue: 'vue',
    }
};
```

### 编写插件

写 vue 插件稍微复杂一点 😢，根据[官网](https://cn.vuejs.org/v2/guide/plugins.html#%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6)的案例，我们需要提供一个包含 install 方法的对象或者一个函数（[传送门](https://cn.vuejs.org/v2/api/#Vue-use)），供 Vue.use 调用注册你的插件

- 写法一

```js
import Component from './YanProgress.vue'; // 这个就是你平时写的 SFC 组件

// 这里要导出一个包含 install 方法的对象
let plugin = { // 这里要导出一个 install 方法
    install(Vue,options) { 
        // 这里写你的代码，你可以全局注册组件，也可以写全局指令，也可以扩展 Vue 的方法
        // 1. 全局组件
        Vue.component('yan-progress',Component); 
        // 2. 全局方法或属性
        Vue.myGlobalMethod = function () {
            // 逻辑...
        }
        // 3. 全局指令
        Vue.directive('my-directive', {
            bind (el, binding, vnode, oldVnode) {
                // 逻辑...
            }
        })
        // 4. 注入组件
        Vue.mixin({
            created: function () {
                // 逻辑...
            }
        })
        // 5. 添加实例方法
        Vue.prototype.$myMethod = function (methodOptions) {
            // 逻辑...
        }
    }
};

if (window && window.Vue) { // 如果是渐进式开发（script 引入简单粗暴的开发方式），需要自动注册你的插件
    window.Vue.use(plugin);
}

export default plugin;

```

- 写法二

```js
import Component from './YanProgress.vue'; // 这个就是你平时写的 SFC 组件

// 或者这里也可以写成函数
function plugin(Vue,options) { 
        // 这里写你的代码，你可以全局注册组件，也可以写全局指令，也可以扩展 Vue 的方法
        Vue.component('yan-progress',Component); 
    }
};

if (window && window.Vue) { // 如果是渐进式开发（script 引入简单粗暴的开发方式），需要自动注册你的插件
    window.Vue.use(plugin);
}

export default plugin;
```

这样写的原因是，下面[源码](https://github.com/vuejs/vue/blob/dev/src/core/global-api/use.js)伺候😄

```ts
export function initUse (Vue: GlobalAPI) {
  Vue.use = function (plugin: Function | Object) { // 在这里哦，可以传对象，也可以传函数
    const installedPlugins = (this._installedPlugins || (this._installedPlugins = []))
    if (installedPlugins.indexOf(plugin) > -1) { // 避免重复注册插件
      return this
    }

    // additional parameters
    const args = toArray(arguments, 1)
    args.unshift(this)
    if (typeof plugin.install === 'function') { // 如果是带有 install 方法的对象
      plugin.install.apply(plugin, args) // 不改变插件的 this（这里的 this 还是指向插件对象本身）
    } else if (typeof plugin === 'function') { // 如果是函数
      plugin.apply(null, args) // 不改变插件的 this（这里应该是指向window，在浏览器非严格模式下）
    }
    installedPlugins.push(plugin)
    return this
  }
}
```

### 开源贡献

拥抱开源，这样才能让社区，乃至行业发展更有动力，哎，似曾相识的赶脚，😂
> 注：例如，你的 star 是对我最大的鼓励，是支持我继续开源的动力，😄

- awesome-vue 社区 [awesome-vue](https://github.com/vuejs/awesome-vue)
- 其他社区，可以到 `Github` 探索

### 完结撒花🎉

👏 再次欢迎大家一起和我搞 ji 由（[Github](https://github.com/Yangfan2016)）😊 

- 项目地址 https://github.com/Yangfan2016/vue-yan-progress   
- 博客原文 https://yangfan2016.github.io  
- react-yan-progress https://github.com/Yangfan2016/react-yan-progress  
- vue-yan-progress https://github.com/Yangfan2016/vue-yan-progress  

