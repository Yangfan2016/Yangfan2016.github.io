---
title: 快速构建一个 react 插件
date: 2019-01-04 15:21:36
tags:
---

### 前言
一般情况下，我们写 React 项目，用 create-react-app 脚手架开发比较方便，但是如果要写一个插件的话，用三方脚手架就显得有点臃肿了，我们可以自己配置一个符合我们开发的简单工具，本文教你如何一步一步开发一个
React 插件 ，let's start  😊

### 准备工作

工欲善其事，必先利其器，我们来选型

- Typescript  

为了方便开发，我们选用 Typescript 作为开发语言，可以即时类型检查，顺便还能装逼（巨硬大法好），
> 注：
Typescript 可选的，你也可以选择 js 刀耕火种，不过最好还是用 Typescript 写吧，毕竟9102年了，骚年 😄

- webpack + babel

这里先用我们熟悉的 webpack 作为打包工具（之后会尝试改成 rollup 作为打包工具）

- jest + travis + coveralls

jest 作为我们代码测试的工具,这里选用 travis ，一个在线持续集成的工具（帮助你打包、构建、运行scripts命令、代码测试等）
选用 coveralls 可以根据 travis 代码测试后生成的代码覆盖率生成 badge（Github 好多项目都有的）  
![Build Status](https://user-gold-cdn.xitu.io/2019/1/8/1682db589e80f715?w=90&h=20&f=svg&s=724)

### 目录结构

下面我们来大体组织下目录结构
```
react-yan-progress
├── build                                 // 打包目录
│   └── YanProgress.min.js
├── src                                   // 源码
│   ├── index.css
│   └── index.tsx
├── test                                  // 测试文件
│   └── YanProgress.test.js
├── index.d.ts                            // 声明文件（ts）
├── jest.config.js                        // jest 测试配置文件
├── webpack.config.js                     // webpack 配置文件
├── tsconfig.json                         // ts 配置文件
├── package.json
├── .travis.yml                           // travis 配置文件
├── LICENSE 
└── README.md
```

### 开发者选项

所有的依赖的包如下

```js
{
    "devDependencies": {
	"@babel/core": "^7.1.6",
	"@babel/preset-env": "^7.1.6", 
        "@babel/preset-react": "^7.0.0", // for react
        "@types/react": "^16.7.18", // 声明文件
        "@types/react-dom": "^16.0.11", // 声明文件
        "babel-loader": "^8.0.4",
        "chai": "^4.2.0", // 测试断言库
        "coveralls": "^3.0.2", // 代码覆盖率
        "css-loader": "^1.0.1", 
        "jest": "^23.6.0", // 测试工具
        "react": "^16.7.0",
        "react-dom": "^16.7.0",
        "style-loader": "^0.23.1",
        "ts-loader": "^5.3.2", // 解析 ts 
        "typescript": "^3.2.2", // 解析 ts
        "webpack": "^4.25.1",
        "webpack-cli": "^3.1.2"
    }      
}
```

命令配置如下，详情 `package.json`
```json
{
    "scripts": {
        "build": "webpack --config webpack.config.js --progress --colors",
        "test": "jest ./test/YanProgress.test.js",
        "coveralls": "cat ./coverage/lcov.info | coveralls"
    },
}

```

### webpack 配置

我们采用 webpack4 ,具体配置请看官网，[传送门](https://webpack.js.org/concepts/)
```js
const path = require('path');

module.exports = {
	mode: "production", // 生产模式
	entry: { // 入口
		"YanProgress": path.resolve(__dirname, './src/index.tsx')
	},
	output: { // 出口
		path: path.resolve(__dirname, './build'),
		filename: '[name].min.js',
		publicPath: "./build/",
		libraryTarget: 'commonjs2', // 注意这里按 commonjs2 模块规范打包
	},
	module: {
		rules: [
			{
				test: /\.tsx?$/,
				use: [
					{
						loader: 'babel-loader',
						options: {
							presets: ['@babel/preset-env', "@babel/preset-react"]
						}
					},
					{
						loader: 'ts-loader', // 解析 ts
					}
				],
				include: path.resolve(__dirname, "./src/"), // 只解析 src 目录下的文件
			},
			{

				test: /\.css$/,
				loader: "style-loader!css-loader?modules&localIdentName=[hash:8]", // css_modules 配置详情  http://www.ruanyifeng.com/blog/2016/06/css_modules.html
				include: path.resolve(__dirname, "./src/"),
			}
		]
	},
	resolve: { // 省略文件后缀时，默认按下面的配置取
		extensions: ['.ts', '.tsx', '.js'],
	},
	externals: { // 不把 react 打包进去
		react: 'react'
	}
};
```

### 【选读】Typescript 配置

由于我们要在 ts 文件中 引入 css 模块，但是 ts 不认识，所以我们需要进行如下配置

在项目的根目录下新建一个 `index.d.ts` ts 声明文件

```ts
declare module '*.css';
```

### 开始编写插件

这里就是与平常的开发组件一样，举个例子

```js
// jsx
import React from 'react';

class YanProgress extends React.Component{
	render() {
		return (
			<div>
				骚年，写代码快乐吗，看我干嘛 😄，赶快滚去写代码啊，别忘了点个 star 😂
			</div>
		);
	}
}

export default YanProgress; // 记得导出啊，骚年
```

你可以直接看我写好的代码（然后 ctrl+c,ctrl+v），源码在这里，[点我](https://github.com/Yangfan2016/react-yan-progress/tree/master)


### 安装依赖及代码压缩打包

webpack4 默认会压缩代码，so 我们直接执行刚才 package.json 配置好的 scripts 的命令


```bash
$ yarn
$ yarn run build
```

### 代码测试
- 单元测试  
	可以在 `test` 目录下新建一个 `xxx.test.js` 的测试文件，写好测试用例（这里使用的是 chai 断言库的 expect 风格），执行如下命令

	```bash
	$ yarn run test
	```

- 包测试  
	如果你也想以 npm 包的形式引入（`import YanProgress from 'react-yan-progress'`），测试的话，可以执行如下命令  

	在你的项目根目录下，打开终端运行如下命令，建立链接
	```bash
	$ yarn link
	```
	在你要测试的 demo 项目根目录下，执行如下，然后你就可以这样使用了 `import YanProgress from 'react-yan-progress'`
	```bash
	$ yarn link react-yan-progress
	```

### 持续集成

这里用到比较方便简单的 travis 在线测试工具，和测试代码覆盖率工具 coveralls，网址如下：  
持续集成 https://travis-ci.org  
代码覆盖率  https://coveralls.io  

注册使用过程就略过了，毕竟已经有很多教程了（面向谷歌编程 😂）

在项目根目录下新建一个 `.travis.yml` 文件，配置如下

```yml
language: node_js # 运行环境
node_js:
  - "10.6.0" # 版本
branches:
  only:
  - master # 只有主支可以
before_install:
    - export TZ='Asia/Shanghai' # 如果你的项目里涉及到时间处理，这里需要设置时区
install: yarn install # 安装 npm 包
script: # 执行命令
  - yarn run build # 打包
  - yarn run test # 测试
after_success: # 成功之后执行如下命令
  - yarn run coveralls # 测试代码覆盖率
```

### 发布 npm 包

注册 npm 账号，注册过程略  
> 注意之前，先去 npm 官网找一下，你的包名有木有被抢先占用了

执行如下命令进行发布
```bash
$ npm publish
```

升级包  
执行如下命令（x.x.x  -> major.minor.patch）
```bash
$ npm version patch
```

### 开源贡献

拥抱开源，这样才能让社区，乃至行业发展更有动力
> 注：例如，你的 star 是对我最大的鼓励，是支持我继续开源的动力

- React-componets 社区 [awesome-react-components](https://github.com/brillout/awesome-react-components)
- 其他社区，可以到 `Github` 探索

### 完结撒花🎉

👏 欢迎大家一起和我搞 ji（[Github](https://github.com/Yangfan2016)）😊 

- 项目地址 https://github.com/Yangfan2016/react-yan-progress#react-yan-progress  
- 博客原文 https://yangfan2016.github.io