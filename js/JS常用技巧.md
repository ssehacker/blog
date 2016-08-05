# JS常用技巧

### 构造单例模式
js构造单例有很多种方法，这里举个个人比较喜欢的方式，这种方式的好处是`new Singleton()`和` Singleton()`得到的是同一个引用。

```js
function Singleton(opts){
    if(!(this instanceof Singleton){
        return new Singleton(opts);
    }
    //do something here

}
```

