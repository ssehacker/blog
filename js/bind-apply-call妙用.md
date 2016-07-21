## bind,call,apply的妙用

#### bind注意事项
关于bind,call,apply的用法这里就不提了，很多地方都有介绍，值得注意的一点是，当在构造函数上调用`bind`， 则bind的第一个参数将会被忽略。 举个例子：
```js
function Foo(name){
    this.name = name;
}
var that = {};
var Foo2 = Foo.bind(that, 'zhangsan');
var f = new Foo2('zhangsan2');
console.log(f.name === 'zhangsan'); //这里的f.name 不是zhangsan2,因为bind会把它后面跟的参数列表前置到函数的形参中。

console.log(that.name ); //undefined，Foo构造函数时，bind的第一个参数被忽略了
Foo2('zhangsan3');
console.log(that.name); //'zhangsan3'， 此时的Foo不是作为一个构造函数

```
#### ArrayLike 转化为 Array
javascript中，数组就是通过对象的方式实现的， 如果像下面这样定义一个对象， 你会发现在大多数场景可以像数组一样使用它。
```js
var arrayLike = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3
};
for(var i=0; i<arrayLike.length; i++){
    console.log(aa[i]);
}

```

谈到ArrayLike，最先想到的就是每个函数中内置的arguments变量， 
我们可以通过`Array.prototype.slice.call` 函数将一个伪数组转化为数组：
```js
var arr = Array.prototype.slice.call(arguments);
//同样，上面例子中的arrayLike变量也可以被转化成数组：
var realArr = Array.prototype.slice.call(arrayLike);
console.log(Object.prototype.toString.call(realArr));
```

>上面我们用到了 `Object.prototype.toString.call(realArr) ==='[object Array]'`来判断realArr是否为数组， 因为`typeof arr`会被判断为一个对象
> 
> 实际上，typeof除了对number, string, undefined, boolean, 这几个数据类型能精准的判断外， 其他的都会被识别为[object Object],包括null）。
> 
> 其次， 我们也可以通过 `arr instanceof Array ===true` 来判断
> 

再举个比较实用的例子：
```js
var arr = [5, 45 , 420 , -21];
Math.max.apply(null, arr); //420
//虽然直接用Math.max也可以求出最大值，但是Math.max不接受一个数组作为参数，所以用这个方法可以很方便。
```

#### bind.call 调用
这个比较拗口， 我举个例子吧， 比如`Date`这个构造函数，我们现在想写一个函数，要达到如下的效果：
```js
//要求返回值时Date类型
function convertDate(num1, num2...){
    //do something
}
convertDate(1993,5,1); // 得到 1993-5-1
convertDate(1993,5,1, 2,22,22); //得到时间： 1993-5-1 2:22:22
```

这个怎么实现呢？这个时候bind就派上用场了。
```js
//错误的写法
function convertDate(){
    //很多人可能会这样写, 其实仔细想想就会发现不对了，想想这样用的返回值是什么？而且apply的第一个参数是指代this变量的。
    return Date.apply(Date，arguments);
}

//启蒙的写法，但同样错误
function convertDate(){
    //bind函数的传入形式是(_this, arg1, arg2,arg3...),
    //所以我们很自然的想到了call或者apply
    //这里说明一下，这里的bind的第一个参数会被忽略，因为bind被构造函数调用了，见上面__bind的注意事项__
    return new Date.bind(null, arguments);
}

//正确的写法

function convertDate(){
    //这里的null是传给bind的第一个参数，因为会被忽略，所以null在这里也可以替换为任何值
    var bindArgs = [null].concat(Array.prototype.slice.call(arguments));
    //为了不引起误会，这里的Date.bind被替换成了Function.bind，事实上，Function.bind === Date.bind，它们都是指向同一个地址的
    return new (Function.bind.apply(Date, bindArgs));
}

convertDate(1993, 5, 1); //等价于 new Date(1993, 5, 1)
convertDate(1993, 5, 1, 2, 22, 22); //等价于 new Date(1993, 5, 1, 2, 22, 22)
```

当然，针对这个问题还有一些其他的方法， 比如我前面介绍过的`Reflect.construct`。

总而言之， 当ArrayLike和Array参数形式需要互相转化的时候，第一时间应该想到apply和call。


