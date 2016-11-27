# js浅拷贝和深拷贝

### 什么是浅拷贝和深拷贝？
举个形象的例子：
```js
var obj = {
    user: {
        name: 'xiaoming',
        age: 17
    }
};
//浅拷贝
var copy1 = {};
copy1.user = obj.user;

//深拷贝
var copy2 = {};
copy2.user = {
    name: 'xiaoming',
    age: 17
};

copy1 === obj; //false
copy2 === obj; //false

copy1.user = obj.user; //true
copy2.user = obj.user; //false
```

copy1中的user和obj中的user引用的是同一个对象，而copy2则不是。也就是说，如果copy1发生了改变，obj也会改变。copy2则不然。
通过上面的例子，我们应该能很清楚的了解深拷贝和前拷贝的概念了。

### 浅拷贝的实现
有很多第三方的库提供了各种各样的实现方法，我们一一解析。

#### Object.assign
仍然以上面的为例：
```js
var copy1 = Object.assign({}, obj);
copy1 === obj; //false
copy1.user === obj.user; //true
```

但是Object.assign不兼容所有版本的IE浏览器，下面是它的详细兼容情况：
PC端：
|Chrome|Firefox (Gecko) | Internet Explorer | Edge | Opera | Safari | 
|-------|----------------|------------------|------|--------|-------|
|45|34 (34)|No support|  (Yes)|   32|  9|

移动端：

|Android|Chrome for Android | Firefox Mobile (Gecko)| IE Mobile | Opera Mobile | Safari Mobile | 
|-------|----------------|------------------|------|--------|-------|
|No support|45|34.0 (34)| No support|   No support| (Yes)|

### 其他方法
其他第三方的库实现有：
- `JQuery` 的 `$.extend` 方法
- Underscore的 `_.clone` 或通过`_.extend({}, obj)`
- Lodash的`_.clone` 或通过 `_.extend({}, obj)`
浅拷贝比较简单，就不详细叙述了，下面重点介绍一下深拷贝。

### 深拷贝
值得思考的是： 深拷贝中应该如何处理下面这些对象？
1. DOM/BOM对象
2. 原型链
3. 正则表达式
4. 函数
好好研究一下lodash的clone 方法。


