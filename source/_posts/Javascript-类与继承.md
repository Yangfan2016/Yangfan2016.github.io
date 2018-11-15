---
title: Javascript 继承
date: 2018-09-06 17:19:54
tags:
---

## 继承
### 构造函数继承

```js
function Parent() {
    this.name='parent';
}

Parent.prototype.run=function () {
    console.log('We can run');
}

function Child() {
    Parent.call(this);
}

let c=new Child();
c.name; // 'parent'
c.run(); // undefined
```

- 缺点  
1. 无法继承父类原型上的方法

### 原型链实现继承

```js
function Parent() {
    this.name='parent';
    this.colors=["red","yellow"];
}

Parent.prototype.run=function () {
    console.log('We can run');
}

function Child() {
    
}

Child.prototype=new Parent();
Child.prototype.constructor=Child;   

var c1=new Child();
var c2=new Child();

c1.colors; // ['red','yellow']
c2.colors; // ['red','yellow']

c1.colors.push('green');

c1.colors; // ['red','yellow','green']
c2.colors; // ['red','yellow','green']
```

- 缺点  
1. 子类共用一个父类的实例，造成数据污染  
1. 创建子类实例时，无法向父类构造函数传参

### 组合式继承

```js
function Parent() {
    this.name='parent';
    this.colors=["red","yellow"];
    console.log("I've executed");
}

Parent.prototype.run=function () {
    console.log('We can run');
}

function Child() {
    Parent.call(this);
}

Child.prototype=new Parent();
Child.prototype.constructor=Child;

var c1=new Child(); // "I've executed" "I've executed"
var c2=new Child(); // "I've executed" "I've executed"

c1.colors; // ['red,yellow']
c2.colors; // ['red,yellow']

c1.colors.push('green');

c1.colors; // ['red,yellow','green']
c2.colors; // ['red,yellow']
```

- 缺点  
1. 每新建一个子类都会调用两次父类构造器

### 寄生组合式继承

```js

function Parent() {
    this.name='parent';
    this.colors=["red","yellow"];
}

Parent.prototype.run=function () {
    console.log('We can run');
}


function Child() {
    Parent.call(this);
}
function Super() {/* noop */}
Super.prototype=Parent.prototype;
Child.prototype=new Super();
Child.prototype.constructor=Child;

```

- 特点  
1. 完美实现继承
2. 利用寄生函数实现父类构造函数只执行一次，将原型上的方法挂载到寄生函数上，最后将寄生函数的实例作为子类的原型对象


### ES6 继承

```js

class Parent {
    constructor() {
        this.name='parent';
        this.colors=["red","yellow"];
    }
    run() {
        console.log('We can run');
    }
}

class Child extends Parent{
    constructor() {
        super();
        this.name='child';
    }
}

```

- 特点
1. 简单明了 
1. 只是一种语法糖而已

- 下面是babel将ES6转成ES5继承的实现

### Babel 编译后的ES5代码

```js
"use strict";

// 创建类
var _createClass = function() {
    function defineProperties(target, props) {
        for (var i = 0; i < props.length; i++) {
            var descriptor = props[i];
            descriptor.enumerable = descriptor.enumerable || false;
            descriptor.configurable = true;
            if ("value"in descriptor)
                descriptor.writable = true;
            Object.defineProperty(target, descriptor.key, descriptor);
        }
    }
    return function(Constructor, protoProps, staticProps) {
        // 定义原型对象的属性、方法
        if (protoProps)
            defineProperties(Constructor.prototype, protoProps);
        // 定义类的静态属性、方法
        if (staticProps)
            defineProperties(Constructor, staticProps);
        return Constructor;
    }
    ;
}();
// 确保子类初始化时调用super
function _possibleConstructorReturn(self, call) {
    if (!self) {
        throw new ReferenceError("this hasn't been initialised - super() hasn't been called");
    }
    return call && (typeof call === "object" || typeof call === "function") ? call : self;
}
// 继承
function _inherits(subClass, superClass) {
    if (typeof superClass !== "function" && superClass !== null) {
        throw new TypeError("Super expression must either be null or a function, not " + typeof superClass);
    }
    // 利用 Object.crete 创建了一个父类原型对象的新实例，并且修正了构造器指向
    subClass.prototype = Object.create(superClass && superClass.prototype, {
        constructor: {
            value: subClass,
            enumerable: false,
            writable: true,
            configurable: true
        }
    });
    // 将子类原型链链到父类上
    if (superClass)
        Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
}
// 安全检测，确保用 'new' 操作的构造函数
function _classCallCheck(instance, Constructor) {
    if (!(instance instanceof Constructor)) {
        throw new TypeError("Cannot call a class as a function");
    }
}

var Parent = function() {
    function Parent() {
        _classCallCheck(this, Parent);

        this.name = 'parent';
        this.colors = ["red", "yellow"];
    }

    _createClass(Parent, [{
        key: "run",
        value: function run() {
            console.log('We can run');
        }
    }]);

    return Parent;
}();

var Child = function(_Parent) {
    _inherits(Child, _Parent);

    function Child() {
        _classCallCheck(this, Child);

        var _this = _possibleConstructorReturn(this, (Child.__proto__ || Object.getPrototypeOf(Child)).call(this));

        _this.name = 'child';
        return _this;
    }

    return Child;
}(Parent);


```
