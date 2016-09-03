# JS常用技巧

### 创建实例
这样可以使`new Clazz()`和` Clazz()`都返回一个新的实例。

```js
function Clazz(opts){
    if(!(this instanceof Clazz)){
        return new Clazz(opts);
    }
    //do something here
    this.opts = opts;
}
```

### 单例模式

可以直接像下面这样，每次返回的都是同一个对象：
```js
var Singleton = {};
//do something here
Singleton.name = 'foo';

```

或者用ES6的class：

```js
class Singleton{
    constructor(props){
        if(!Singleton.instance ){
            //do something here
            this.props = props;
            //...
            Singleton.instance = this;
        }
        return Singleton.instance;
    }
}

console.log(new Singleton('brave man') === new Singletom('zhouyong')); //true
```
