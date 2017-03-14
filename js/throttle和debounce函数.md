### Throttle和Debounce函数
我们前端经常遇到一些场景，如：
1. 鼠标快速点击N次
2. 滚轮滚动短时间内多次触发事件
3. 鼠标移动多次触发事件
4. 窗口resize

这些场景都会遇到一个问题，就是短时间内触发了多次回调函数，而我们事实上只想让起触发一次，这个时候，就需要用这两种中任一种函数解决了。
这种用途的函数称为Throttle函数或者Debounce函数。
#### 原理
函数的原理很简单，我们知道，js是一个单线程的语言，setTimeout函数可以将回调函数（即任务）放到js执行队列中，然后不断的轮询这个队列，执行相应的任务。
所以我们可以利用setTimeout来实现。
直接看代码吧
```js
function throttle(fn, timeout) {
  let timeout = timeout || 500,
  let handler;
  return function(...arg) {
    clearTimeout(handler);
    let ctx = this;
    handler = setTimeout(function() {
      fn.apply(ctx, arg);
    }, timeout);
  }
}
```
以上的代码也可以根据需要将this改成通过debounce传入。

#### 使用举例
```js
window.onresize = throttle(function() {
  // do something.
  // 即使你在500 ms内改变窗口，触发了10次，这里的代码也只会执行一次。
});
```

以上～
