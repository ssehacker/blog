# 从javascript中的原型谈起

### 问题
作为一门面向对象编程的语言，javascript一直表现的令人难以理解，尤其是先学了java或者c++之后。下面，深入的总结一下js是如何实现面向对象编程的。

### 创建对象
在javascript中，如果需要创建一个对象，可以如下实现。
```javascript
var o1 = {};
//or
var o2 = new Object();//这个和上面那种字面量方式等价。
```
那么，上面的new操作做了那些事情呢？举个例子：
```javascript
function Person(name){
	this.name = name;
	this.say = function (){
		console.log('I\'m '+this.name);
	}
}

var p1 = new Person('xiaoming');

//这里的new操作做了如下事情：
function newPolyfill(constructor, args){
	var temp = {};
	constructor.call(temp, args);
	temp.prototype = constructor.prototype; //prototype的作用后文会详细叙述
	return temp;
}

var p2 = newPolyfill(Person, ['xiaohong']);

```
理解了new的作用，在谈prototype之前，我们先谈谈javascript中的实例变量，类变量，实例函数，类函数。

### 实例变量，实例函数
学过java或者c++等面向对象编程语言的人应该对实例变量，类变量，实例函数，类函数这几个概念不会陌生，其实，javascript中也有等价的概念。
还是用上面的Person函数来叙述， 其中的 this.name 就是实例变量，this.say就是实例方法，这两者都是仅在实例对象中独享的，p1.name 和 p2.name彼此相互独立，不会互相干扰。
下面来看看类变量和类函数：
```javascript
function Person(name){
	this.name = name;
	this.say = function(){
		console.log('I\'m '+this.name);
	}
}
Person.prototype.age = 11;
Person.prototype.protoMethod = function(){
	console.log('this is prototype method.');
}

var p1 = new Person('xiaohong');
var p2 = new Person('xiaoming');

console.log(p1.say === p2.say); //false,因为say是实例函数，每个实例对象中独享的。
console.log(p1.protoMethod === p2.protoMethod); // true, 相当于类中的静态变量，每个实例对象中共享，所以，p1中的改变会影响到p2.
p1.age = 44;
console.log(p2.age); //输出44


```
prototype就是原型，我们创建的每个函数都有一个prototype属性，这个属性是一个指针，指向一个对象，这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。
对应的，每个实例对象中都有一个对应的对象：\__proto__指向类的prototype。按照标准，\__proto__是不对外公开的。但在ie和firefox中可以访问。


prototype的好处：
避免了同一份实例函数在内存中的多份拷贝(如果需求就是需要拷贝的，则不能用prototype，直接在构造函数中赋值即可)。

### 原型链
原型链：当从一个对象那里调取属性或方法时，如果该对象自身不存在这样的属性或方法，就会去自己关联的prototype对象那里寻找，如果prototype没有，就会去prototype关联的前辈prototype那里寻找，如果再没有则继续查找Prototype.Prototype引用的对象，依次类推，直到Prototype.….Prototype为undefined（Object的Prototype就是undefined）从而形成了所谓的“原型链”。

原型链的好处：
1.避免了重复定义,对象中的方法可以从父类中继承而来。这也是面向对象语言中继承的好处。
例如，person.say().如果在person中没有say方法，那么它会在它的prototype中寻找，如果还没找到，则会在prototype的prototype中寻找，依次类推。

### 总结
通过prototype， javascript可以解决面向对象编程中实例对象中的变量共享问题,同时也使得对象和对象之间不会彼此孤立（为继承做准备）。
下一节，将讲述javascript中是如何实现继承的。



