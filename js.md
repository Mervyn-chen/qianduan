##### **闭包是什么？有什么用？**

闭包是指有权访问另一函数作用域中变量的函数，创建闭包的常用方式就是在一个函数内部定义另一个函数。
闭包可以创建私有变量和方法，为setTimeout等方法传递参数等
闭包的缺点是，其让变量一直保存在内存中，容易造成内存泄露



使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。

 闭包有三个特性：

 1.函数嵌套函数  

 2.函数内部可以引用外部的参数和变量 

 3.参数和变量不会被垃圾回收机制回收 



##### 说说你对this的理解？

在JavaScript中，this通常指向的是我们正在执行的函数本身，或者是，指向该函数所属的对象。

全局的this → 指向的是Window
函数中的this → 指向的是函数所在的对象
对象中的this → 指向其本身

##### **图像懒加载的原理是什么？**

页面加载时，将图片加载的链接，保存在img标签的自定义属性中，src属性为空，并监听窗口的scroll事件。当img标签出现在视口中时，利用js将图片加载编写填写至src属性中，实现动态加载图片。

##### **实现一个防抖函数和节流函数？**

函数节流: 频繁触发,但只在特定的时间内才执行一次代码
函数防抖: 频繁触发,但只在特定的时间内没有触发执行条件才执行一次代码

```
/防抖函数
function debounce(fn, time){
	return function(){
		var context = this;
		clearTimeout(timeId);
		timeId = setTimeout(function(){
			fn.apply(context, arguements);
		}, time);
	};
}

//节流函数
function throttle(fn,time){
	var last = 0;
	return function(){
		var context = this;
		var now = Date.now();
		if (now - last >= time){
			fn.apply(this, arguments);
			last = now;
		}
	};
}

```

##### Js中常见的对象继承方式？

```
//原型链继承
SubType.prototype = new SuperType();

//构造函数继承
function SubType(){
	SuperType.call(this);
}
```

原型链继承的缺点在于，当父类存在引用型属性时，该属性会被所有实例共享，因此修改某个实例的该属性，会影响到其他实例。而构造函数的缺点在于，每次调用构造函数，对象方法都会被创建一次，无法达到函数复用。因此在实际工作中，是结合两者进行使用，原型链继承方法，而构造函数继承属性。

##### 什么是javascript 中的垃圾回收机制？

在Javascript中，如果一个对象不再被引用，那么这个对象就会被GC回收。

如果两个对象互相引用，而不再被第3者所引用，那么这两个互相引用的对象也会被回收。因为函数a被b引用，b又被a外的c引用，这就是为什么函数a执行后不会被回收的原因  

##### new操作符做了哪些事情？

```
const a = new Foo();
//以下为new 操作符干的事情
var o = new Object();  //新建一个空对象
o.__proto__ = Foo.prototype;   //将该空对象的原型指向构造函数的原型对象
Foo.call(o);    //在空对象上调用构造函数
a = o;    //赋值给变量
```

##### **说一说Js事件循环？**

##### **promise的原理是什么？**

`promise`可以有三种状态，分别是`pedding` 、`Fulfilled`、 `Rejected`。

`Pending Promise`对象实例创建时候的初始状态
`Fulfilled` 可以理解为成功的状态
`Rejected`可以理解为失败的状态

`Promise`上还有`then`方法，`then` 方法就是用来指定`Promise` 对象的状态改变时确定执行的操作，`resolve` 时执行第一个函数（onFulfilled），`reject`时执行第二个函数（onRejected）

当状态变为`resolve`时便不能再变为`reject`，反之同理。

```
class Promise {
    constructor(executor) {
        // 设置属性 status value resolveCbs rejectCbs
    }
    then(onResolved, onRejected) {}
    catch (cb) {
        return this.then(null, cb)
    }
}
```



##### **CommonJS，AMD，CMD以及ES6 import的区别是什么？**

CommonJS是运行时加载，ES6是编译时输出接口
CommonJS是输出的一个值的复制，而ES6输出的是值的引用
AMD 异步模块定义，通过define函数声明依赖模块（数组）和回调函数，特点是依赖前置，其加载模块完成后就会执行该模块，所有模块都加载执行完后会进入回调函数，执行主逻辑，这样的效果就是依赖模块的执行顺序和书写顺序不一定一致，看网络速度，哪个先下载下来，哪个先执行，但是主逻辑一定在所有依赖加载完成后才执行。
CMD 同样为异步加载，区别在于其为就近依赖，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，其加载完某个依赖模块后并不执行，只是下载而已，在所有依赖模块加载完成后进入主逻辑，遇到require语句的时候才执行对应的模块，这样模块的执行顺序和书写顺序是完全一致的。


##### **高阶函数是什么？有什么用？**

一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。
高阶函数广泛应用于js中，例如map、sort、setTimeout等

##### **js对象的深拷贝？**

```
function deepClone(obj){
	if (obj instanceof Object){
		var newObj = Array.isArray(obj)? []:{};
		for (var key in obj){
			if (obj[key] instanceof Object){
				newObj[key] = deepClone(obj[key]);
			}else{
				newObj[key] = obj[key];
			}
		}
	}else{
		var newObj = obj;
	}
	return newObjl
}

```

##### 原生js读取和修改cookie？

```
function getCookie(name){
	var cookies = document.cookie.split(";");
	var value = null;
	for (var i =0; i<cookies.length;i++){
		if (cookies[i].indexOf(name+'=') > 0){
			value = cookies[i].split("=")[1];
		}
	}
	return value;
}

function setCookie(name, value, expires, path, domain, secure){
	var cookieText = name + '=' + value + ';';
	if (expires){
		cookieText += "expires= " + expires.toGMTString() + ';';
	}
	if (path){
		cookieText += "path= " + path + ';';
	}
	if (domain){
		cookieText += "domain= " + domain + ';';
	}
	if (secure){
		cookieText += "secure";
	}
	document.cookie = cookieText;
}

function delCookie(name,path,domain,secure){
	setCookie(name, "", new Date(0), path, domain, secure);
}
```

##### **call、apply、bind方法的区别是什么？**

三种方法都能在运行时改变this的指向，区别在于call，apply用于执行函数，bind用于返回一个新函数。call方法的参数项需要一个个列出来，而apply参数项则是一个列表，bind后面接的参数用于函数柯里化。

##### **ES6 map和对象的区别？**

map的键可以是任意数据结构，而对象只能是字符串。
map可以通过size属性，无需遍历，即可获取键值对的个数。

##### **js中null和undefined的区别是什么？**

Undefined类型只有一个值，即`undefined`。当声明的变量还未被初始化时，变量的默认值为undefined。
Null类型也只有一个值，即`null`。null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象。从逻辑角度看，null值表示一个空对象指针

##### **请列举几种确定js数据类型的方法？**

1、typeof 可能的值为number, boolean, string, undefined, object, function
2、instanceof 后接可能的数据类型，如 Number，String，Object，Array，Date，Function，RegExp
3、使用constructor属性，例如obj.constructor === Array

4、使用prototype，例如Object.prototype.toString.call(a) === '[object String]')

##### let var const

- **let**: 允许你声明一个作用域被限制在块级中的变量、语句或者表达式 let 绑定不受变量提升的约束，这意味着let声明不会被提升到当前，该变量处于从块开始到初始化处理的"暂存死区"。

- **var**: 声明变量的作用域限制在其声明位置的上下文中，而非声明变量总是全局的, 由于变量声明（以及其他声明）总是在任意代码执行之前处理的，所以在代码中的任意位置声明变量总是等效于在代码开头声明。

- const 声明创建一个值的只读引用 (即指针)，这里就要介绍下 JS 常用类型: String、Number、Boolean、Array、Object、Null、Undefined。其中基本类型有 Undefined、Null、Boolean、Number、String，保存在栈中；复合类型 有 Array、Object ，保存在堆中； 基本数据当值发生改变时，那么其对应的指针也将发生改变，故造成 const申明基本数据类型时，再将其值改变时，将会造成报错， 例如 const a = 3 ; a = 5 时 将会报错；但是如果是复合类型时，如果只改变复合类型的其中某个Value项时， 将还是正常使用；

  

##### 箭头函数

语法比函数表达式更短，并且不绑定自己的 this，arguments，super 或 new.target。这些函数表达式最适合用于非方法函数，并且它们不能用作构造函数。

# es6中的class使用

```
//传统生成实例对象通过构造函数
function Point(x, y) {
  this.x = x;
  this.y = y;
}
Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

\>生成实例对象，构造函数的另一种写法

​      \>类的数据类型是函数，本身指向构造函数

​      \>类的方法都定义在prototype上面，Object.assign方法可以一次向类添加多个方法

# **es6 class 与es5的面向对象的区别：**

 1. 写法不同，使用关键字class

 2.当new一个实例，默认有一个constructor方法，且默认返回实例对象（this）,也可以返回另一对象

 3.类的所有方法都在prototype属性上，但是不可枚举，且每方法结束不能使用分号

 4.类的调用必须通过new 一个实例,且类的内部默认使用严格模式

 5.不存在变量提升，必须先声明，再调用

 6.class的this 默认指向当前类

 7.class 的静态方法，使用关键字static,不需new,直接通过类来调用

 8. 实例属性和静态属性的写法，实例属性在类的内部直接使用等式（=）写法，也可以写在constructor 方法里，静态属性只需在实例属性前加一个关键字static即可 

9.类的继承使用关键字extends,继承机制与es5完全不同，

　　es5的继承原理：先new子类的实例对象this,再通过将父类的方法和属性添加到子类的this上(parents.call(this))。

　　Es6的继承原理：先创造父类的实例对象this,所以要构造函数constructor（）访问父类的属性使用this,必须先调用super()方法；再通过子类的constructor()来修改this

10．类的继承可以继承原生的构造函数，es5不可以