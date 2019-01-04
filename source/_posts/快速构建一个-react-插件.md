---
title: 快速构建一个 react 插件
date: 2019-01-04 15:21:36
tags:
---

## 快速构建一个 react 插件

### 技术栈

- React + Typescript  
- webpack4 + jest
- travis + coveralls

### 目录结构
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

### 开发依赖包

由于用到了 `Typescript` ，依赖的包如下  

```js
{
    "devDependencies": {
        "@babel/core": "^7.1.6", // 腻子脚本，兼容浏览器
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

### pkg 配置

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

### 配置声明文件

由于我们要在 ts 文件中 引入 css 文件，但是 ts 不认识 css 文件，需要进行如下配置

在跟目录下新建一个 `index.d.ts` 文件

```ts
declare module '*.css';
```

### 开始编写插件

可以去 `src` 下参考代码，源码在这里，[点我](https://github.com/Yangfan2016/react-yan-progress/tree/master)


### 构建打包

```bash
$ yarn run build
```

### 代码测试
- 单元测试  
	可以在 `test` 目录下新建一个 `xxx.test.js` 的测试文件，写好测试用例，执行如下命令

	```bash
	$ yarn run test
	```

- npm 包测试  
	在你的项目下，运行如下命令，建立链接
	```bash
	$ yarn link
	```
	在你要测试的 demo 项目下，执行如下，使用你刚才写好的插件
	```bash
	$ yarn link react-yan-progrss
	```

### 持续集成

这里用到比较方便简单的 travis 在线测试工具  
持续集成 https://travis-ci.org  
代码覆盖率  https://coveralls.io  
注册使用过程略

在项目下新建一个 `.travis.yml` 文件，配置如下

```yml
language: node_js # 运行环境
node_js:
  - "10.6.0" # 版本
branches:
  only:
  - master # 只有 主支
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

注册 npm 账号，注册过程，使用注意事项略

执行如下命令进行发布
```bash
$ npm publish
```

### 开源贡献

- 社区1 [awesome-react-components](https://github.com/brillout/awesome-react-components)
- 其他社区，可以到 `Github` 探索

### 注
- 项目地址 https://github.com/Yangfan2016/react-yan-progress#react-yan-progress  
- 欢迎 star ，提 issues