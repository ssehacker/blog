# 将callback/事件形式转化成Promise

### 为什么要用Promise
在写JS代码的时候，最痛苦的莫过于`callback hell`(回调坑)，试想一下，如果好几个回调一层一层的叠在一起，那是一件多么痛苦的事情！

```js
function(arg1, callback1){
    //do something
    callback1(arg2, function(arg3, callback2){
        //do something
        //...
    });

}
```
像这样的代码大大降低了代码的可读性和可维护性，应该是要极力避免的。在社区的推动下，Promise孕育而生，关于Promise的用法这里就不详细描述了，网上有很多Promise的实现库。比如：
    - 鼎鼎大名的[kriskowal/q](https://github.com/kriskowal/q)
    - [es6-promise](https://github.com/stefanpenner/es6-promise)
虽然在ES6规范里面已经内置了Promise，但是低版本的浏览器还是不兼容, 对于这种情况只能用上面所说的库了。

### 如何转化
好了，回到主题，那么我们应该如何转化呢？举个例子就很好理解啦:
```js
//callback
new Promise(function(resolve, reject){
    //do something
    asyncFunc(arg1, function(err,data){
        if(err) reject(err);
        resolve(data);
    });
});

//event
new Promise(function(resolve, reject){
    handle.on('end', function(err, data){
        if(err) reject(err);
        resolve(data);
    });
});

```

像上面这样的写法，我们就可以实现callback/event 到promise的转化啦，然后配合`yo`，`async`，用起来跟写同步的代码一样爽。

