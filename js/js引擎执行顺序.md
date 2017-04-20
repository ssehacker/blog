### 从浏览器的构造说起
一般浏览器主要由以下几部分构成：
1. 用户界面
2. 浏览器引擎
3. 渲染引擎
4. 网络
5. UI后端
6. JS解释器
7. 数据存储
如下图所示：
![示意图](https://github.com/ssehacker/blog/blob/master/images/bowser.jpg?raw=true)
以Chrome为例，渲染引擎用的是webkit的，js执行引擎用的就是大名鼎鼎的V8.

### 浏览器执行js的顺序
浏览器下载完HTML之后，自上而下解析，然后异步下载相关资源，如image，css，js，svg等， 但是当遇到script标签的时候，如果标签未添加`async`或`defer`，则js会暂停解析，然后去下载js资源，当下载完成时，立即执行js,完成之后，则继续解析HTML。如下图所示：
![不带async和defer的情况](http://www.growingwiththeweb.com/images/2014/02/26/script.svg)

如果`script`加了`async`属性， 则下载js时异步操作的，只有执行js的时候，才会暂停HTML的解析，那么这里就有个风险，因为这里的js如果有dom操作，那么不一定会有效果。

![带async的情况](http://www.growingwiththeweb.com/images/2014/02/26/script-async.svg)

如果`script`加了`defer`属性，则js会在所有的dom解析完成之后再执行，当然，下载也是异步的。如图：

~[带defer的情况](http://www.growingwiththeweb.com/images/2014/02/26/script-async.svg)

> 注意， `defer`在IE9及其以下浏览器有兼容性问题， 并且以上属性都要求引入的是外部script，即有src属性。

### 关于setTimeout
js引擎会严格按照script在html中的先后顺序依次执行js，但是，当遇到如下情况会怎么样呢？
```js
<script>
setTimeout(function() {
  console.log(11);
}, 0)
</script>

<script>
  console.log(22);
  for (var i=0; i<1000000000; i++) {
    ;
  }
</script>

<script>
  console.log(33);
</script>
```
浏览器渲染引擎首先遇到第一个script，然后把代码块交由给js执行引擎去执行，如果代码块中由setTimeout，那么setTimeout里面的代码块将在html所有的script都执行完成之后才会被执行。注意script是有序的，即使第二段js（假设是外部js）先下载完成，也需要等待之前的js下载并执行完成之后再执行。所以上述代码输出结果为：

```
22
33
11
```
其中，由于那个for循环，`33`会在`22`输出一会之后再输出。
那么问题来了， `setTimeout`的代码块和`defer`对应的代码块哪个先执行呢？ 我猜测应该是`defer`。

### script间变量声明
值得注意的是，`var`声明的变量是不会跨`function`，更不会跨`script`标签进行变量提升。


相关连接：
[http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html#script-async](http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html#script-async)
