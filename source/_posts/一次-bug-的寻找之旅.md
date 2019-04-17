---
title: 一次 bug 的寻找之旅
date: 2019-04-17 14:17:36
tags:
---

## 一次 bug 的寻找之旅

### 前言

在最近的一个项目中，用到了 ant-design 的 轮播图组件 `Carousel`，发现无法按照预期给每个 slide 加样式（内联样式），后来发现是 ant-design 的轮播图组件引用的三方库 `react-slick`，通过源码查找，终于发现是代码把内联样式重写了

### 情景再现

按照 ant-design 的官方示例，把demo拷贝过来

```jsx
import { Carousel } from 'antd';

function onChange(a, b, c) {
  console.log(a, b, c);
}

ReactDOM.render(
  <Carousel afterChange={onChange}>
    <div><h3>1</h3></div>
    <div><h3>2</h3></div>
    <div><h3>3</h3></div>
    <div><h3>4</h3></div>
  </Carousel>,
  mountNode
);
```

一切正常。然后按业务改动代码，加入背景图


```jsx
import urlBg001  from "./images/urlBg001.png";
import urlBg002 from "./images/urlBg002.png";
import urlBg003 from "./images/urlBg003.png";

ReactDOM.render(
  <Carousel afterChange={onChange}>
    <div style={{ backgroundImage: `url(${urlBg001})` }}><h3>1</h3></div>
    <div style={{ backgroundImage: `url(${urlBg002})` }}><h3>2</h3></div>
    <div style={{ backgroundImage: `url(${urlBg003})` }}><h3>3</h3></div>
  </Carousel>,
  mountNode
);
```

然而，没有任何效果（不显示背景图）

### 思考问题所在

为何会这样呢

首先，我以为是图片路径的问题，做了个验证，发现图片正常显示

```jsx

ReactDOM.render(
  <Carousel afterChange={onChange}>
    <div><img src={urlBg001} /></div>
  </Carousel>,
  mountNode
);

```

然后，我就怀疑是不是 `Carousel` 这个组件对插槽做了限制，进行了进一步的验证

```jsx

ReactDOM.render(
  <Carousel afterChange={onChange}>
    <div id="test-slide" style={
        { 
            backgroundImage: `url(${urlBg001})`,
            color:"#f02",
            width:"50px",
        }}>1</div>
  </Carousel>,
  mountNode
);

```

结果，没有任何效果，打开控制台，找到 id 是 `test-slide` 的那个元素，发现渲染的结果是下面这样：

```html
<div id="test-slide" tabindex="-1" style="width: 100%; display: inline-block;">1</div>
```

`style` 属性被重写了，我自己加的 `style` 不见了，更加确信了是 `Carousel` 组件内部搞的鬼

打开 `github` 扒 ant-design 源码，没有发现对 `style` 属性做任何更改，然后在头部找到该组件其实是引入一个 `react-slick`，然后包装了下

```tsx
// https://github.com/ant-design/ant-design/blob/master/components/carousel/index.tsx#L23

const SlickCarousel = require("react-slick").default;

export default class Carousel extends React.Component<CarouselProps, {}> {
  // 部分代码省略 ...
  renderCarousel = ({ getPrefixCls }: ConfigConsumerProps) => {
    // 部分代码省略 ...
    return (
      <div className={className}>
        <SlickCarousel ref={this.saveSlick} {...props} />
      </div>
    );
  };

  render() {
    return <ConfigConsumer>{this.renderCarousel}</ConfigConsumer>;
  }
}
```

真相慢慢的付出了水面，再次扒 `react-slick` 的源码，终于功夫不负有心人，在这个文件里发现了问题的源头

```jsx
// https://github.com/akiran/react-slick/blob/master/src/slider.js#L184

React.cloneElement(children[k], {
  key: 100 * i + 10 * j + k,
  tabIndex: -1,
  style: {
    width: `${100 / settings.slidesPerRow}%`,
    display: "inline-block"
  }
})

```
直接将子元素`children` 的 `style` 属性覆盖了

最后在 `react-slick` 找到了相关 [issue](https://github.com/akiran/react-slick/issues/1244)，然额这个 issue 仍然是 open 状态

在 PR 中找到 [pull#1372](https://github.com/akiran/react-slick/pull/1372)，有人尝试修复过，但是 PR 又关了，不知道什么原因

### 解决方案

由于不知道源码作者为何要这么设置，我只能先找其他方法解决

- 方法一
多嵌套一层，只要不是在第一层元素上操作就行

```jsx
ReactDOM.render(
  <Carousel>
    <div>
      <div style={{ backgroundImage: `url(${logo})` }}>1</div>
    </div>
  </Carousel>,
  mountNode
);
```

- 方法二

用其他方式达成目的

```jsx
ReactDOM.render(
  <Carousel>
    <img src={logo} alt="001"/>
  </Carousel>,
  mountNode
);
```

### 总结

- 找这个 bug 还是挺费时间的，不过，通过这次 bug 之旅，学到一定要用科学的方法，精准定位问题来源，多做空白试验参照，进行对比，这样才能更快的解决问题
- 一定要多看源码，可以学到很多技巧

### 备注

可以点击下面的链接查看 “情景重现” 和组件源码

- “情景重现”  
demo https://codesandbox.io/s/92n1x0w1my  
- 源码  
ant-design https://github.com/ant-design/ant-design/blob/master/components/carousel/index.tsx#L23    
react-slick https://github.com/akiran/react-slick/blob/master/src/slider.js#L185  


