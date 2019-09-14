# **js类和继承**

# JavaScript版本号对比

```
/*
 * 版本号比较方法
 * 传入两个字符串，当前版本号：curV；比较版本号：reqV
 * 调用方法举例：compare("1.1","1.2")，将返回false
 */
function compare(curV,reqV){
   if(curV && reqV){
      //将两个版本号拆成数字
      var arr1 = curV.split('.'),
          arr2 = reqV.split('.');
      var minLength=Math.min(arr1.length,arr2.length),
          position=0,
          diff=0;
      while(position<minLength && ((diff=parseInt(arr1[position])-parseInt(arr2[position]))==0)){
          position++;
      }
      diff=(diff!=0)?diff:(arr1.length-arr2.length);
      //若curV大于reqV，则返回true
      return diff>0;
   }else{
      //输入为空
      console.log("版本号不能为空");
      return false;
   }
}
```

[]()

[]: https://github.com/ziyi2/js/blob/master/JS%E7%B1%BB%E5%92%8C%E7%BB%A7%E6%89%BF.md	"js类和继承"



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



### `bind`的源码实现

```
Function.prototype.myCall = function (obj) {
  let context = obj || window
  obj.fn = this
  let args = [...arguments].splice(1)
  let result = obj.fn(...args)
  delete obj.fn
  return result
}

Function.prototype.myApply = function (obj) {
  let context = obj || window
  obj.fn = this
  let args = arguments[1]
  let result
  if (args) {
    result = obj.fn(...args)
  } else {
    result = obj.fn()
  }

  delete obj.fn

  return result
}

Function.prototype.myBind = function (obj) {
  let context = obj || window
  let _this = this
  let _args = [...arguments].splice(1)

  return function () {
    let args = arguments
    // 产生副作用
    // return obj.fn(..._args, ...args)
    return _this.apply(context, [..._args, ...args])
  }
}

function myFun (argumentA, argumentB) {
  console.log(this.value)
  console.log(argumentA)
  console.log(argumentB)
  return this.value
}

let obj = {
  value: 'ziyi2'
}
console.log(myFun.myCall(obj, 11, 22))
console.log(myFun.myApply(obj, [11, 22]))
console.log(myFun.myBind(obj, 33)(11, 22))
```



### 谈谈你对作用域链的理解

当代码在一个环境中创建时，会创建变量对象的一个作用域链（scope chain）来保证对执行环境有权访问的变量和函数。作用域第一个对象始终是当前执行代码所在环境的变量对象（VO）。如果是函数执行阶段，那么将其activation object（AO）作为作用域链第一个对象，第二个对象是上级函数的执行上下文AO，下一个对象依次类推。

在《JavaScript深入之变量对象》中讲到，当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

### Event Loop 是什么

> JavaScript的事件分两种，**宏任务(macro-task)**和**微任务(micro-task)**

- **宏任务**：包括整体代码script，setTimeout，setInterval
- **微任务**：Promise.then(非new Promise)，process.nextTick(node中)
- 事件的执行顺序，`是先执行宏任务，然后执行微任务`，这个是基础，任务可以有同步任务和异步任务，同步的进入主线程，异步的进入Event Table并注册函数，异步事件完成后，会将回调函数放入Event Queue中(`宏任务和微任务是不同的Event Queue`)，同步任务执行完成后，会从Event Queue中读取事件放入主线程执行，回调函数中可能还会包含不同的任务，因此会循环执行上述操作。

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

# this指向问题

this 的指向，始终坚持一个原理：**this 永远指向最后调用它的那个对象**。

```
var name = "windowName";
function a(){
	var name ="Cherry";
	console.log(this.name);//windowName
	console.log("inner:"+this); //window
}
a();
console.log("outer:"+this);//window
```

```
var name = "windowName";
var a ={
	name:"Cherry",
	fn: function(){
		console.log(this.name);//"Cherry"
	}
}
a.fn();
```

```
var f =a.fn;
f();//window
```

```
var name = "windowName";
function fn(){
	var name = 'Cherry';
	innerFunction();
	function innerFunction(){
		console.log(this.name);
	}
}
fn();//windowName
```

```
var name = "windowName";
var a ={
	name:'Cherry',
	func1:function(){
		console.log(this.name);
	},
	func2:function(){
		setTimeout(function(){
			this.func1();
		},100);
	}
};
a.func2();//VM5360:9 Uncaught TypeError: this.func1 is not a function
```

```
var name = "windowName";
var a ={
	name:'Cherry',
	func1:function(){
		console.log(this.name);
	},
	func2:function(){
		setTimeout(function(){
			console.log(this.name);
		},100);
	}
};
a.func2();//windowName;
```

```
var name = "windowName";
var a ={
	name:'Cherry',
	func1:function(){
		console.log(this.name);
	},
	func2:function(){
			console.log(this.name);
	}
};
a.func2();//'Cherry'
```

###### 如何改变this指向？

##### 1、使用 ES6 的箭头函数

**箭头函数的 this 始终指向函数定义时的 this，而非执行时。**箭头函数需要记着这句话：“**箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值**，**如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined”。**

```var name = "windowName";
var a={
	name:"Cherry",
	func1:function(){
		console.log(this.name);
	},
	func2:function(){
		setTimeout(()=>{
			this.func1();
		},100)
	}
}
a.func2();//Cherry
```

##### 2、在函数内部使用 _this = this

```var a={
	name:"Cherry",
	func1:function(){
		console.log(this.name);
	},
	func2:function(){
		var _this =this;
		setTimeout(function(){
			_this.func1();
		},100)
	}
}
a.func2();//Cherry
```

在函数内部使用 _this = this

这个例子中，在 func2 中，首先设置 var _this = this;，这里的 this 是调用 func2 的对象 a，为了防止在 func2 中的 setTimeout 被 window 调用而导致的在 setTimeout 中的 this 为 window。我们将 this(指向变量 a) 赋值给一个变量 _this，这样，在 func2 中我们使用 _this 就是指向对象 a 了。

##### 3、使用 apply、call、bind

fun.apply(thisArg, [argsArray])

- thisArg：在 fun 函数运行时指定的 this 值。需要注意的是，指定的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的自动包装对象。
- argsArray：一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 fun 函数。如果该参数的值为null 或 undefined，则表示不需要传入任何参数。

##### 4、new 实例化一个对象

- New 实例化一个对象
  如果函数调用前使用了 new 关键字, 则是调用了构造函数。
  这看起来就像创建了新的函数，但实际上 JavaScript 函数是重新创建的对象：

##### Apply、call、bind的区别以及实现

apply传递数组，call传递参数，而bind改变this指向的时候返回的是一个函数。而apply、call是立即执行。

###### 1、call模拟实现

```
Function.prototype.call =function(context){
    context = context? Object(context): window;
    context.fn = this;//this也就是调用call的函数
    let args = [...argments].slice(1);
    let r = context.fn(...args);
    delete context.fn;
    return r;
}
```

###### 2、apply模拟实现

```
Function.prototype.apply = function(context,args){
        context = context? Object(context): window;//不传递context默认为window
        context.fn = this;
        if(!args){
            return context.fn();
        }
        let r = context.fn(...args);
        delete context.fn;
        return r;
}
```

###### 3.手写bind

```
Function.prototype.bind = function(context){
    const _this = this;
    const argus = Array.prototype.slice.apply(argments,[1]) //拿到除了context之外的预置参数序列
    return function(){
        _this.apply(context,argus.concat(Array.prototype.slice.call(argments)))
        //绑定this同时将调用时传递的序列和预置序列进行合并
    }
}
```

# 简单的类型对比、数据类型的判断?怎样判断“值”属于哪种类型?typeof 是否能正确判断类型？instanceof 呢？instanceof 有什么作用？内部逻辑是如何实现的？

##### 1、严格相等 ===

全等操作符比较两个值是否相等，两个被比较的值在比较前都**不进行隐式转换**。如果两个被比较的值具有不同的类型，这两个值是不全等的。否则，如果两个被比较的值类型相同，值也相同，并且都不是 number 类型时，两个值全等。最后，如果两个值都是 number 类型，当两个都不是 NaN，并且数值相同，或是两个值分别为 +0 和 -0 时，两个值被认为是全等的。

##### 2、非严格相等 ==

相等操作符比较两个值是否相等，**在比较前将两个被比较的值转换为相同类型**。在转换后（等式的一边或两边都可能被转换），最终的比较方式等同于全等操作符 === 的比较方式。 相等操作符满足交换律。

![image-20190904004111430](/Users/caiyifan/Library/Application Support/typora-user-images/image-20190904004111430.png)

同值相等由 Object.is 方法提供。

```
Object.is =function(x,y){
    if(x === y){
        // +0,-0情况处理
        return x !==0 || 1/x ===1/y;
    }else{
        //NaN
        return x !== x && y!==y;
    }
}
```

##### 3、instanceof

instanceof运算符可以**用来判断某个构造函数的prototype属性是否存在于另外一个要检测对象的原型链上。** 
object instanceof constructor

```
[]  instanceof Object; // true
```

###### 手写instanceOf

```
function _instanceOf(left,right){
    let prototype =right.prototype;
    obj = left.__proto__;
    while(true){
        if(obj === null) return false;
        if(obj === prototype) return true;
        obj = obj.__proto__;
    }
}
function Person(){
    this.name = "first";
}
let person = new Person();
console.log(_instanceOf(person,Person));
```

##### 4、Object.prototype.toString.call()

每一个继承 Object 的对象都有 toString 方法，如果 toString 方法没有重写的话，会返回 [Object type]，其中 type 为对象的类型。但当除了 Object 类型的对象外，其他类型直接使用 toString 方法时，会直接返回都是内容的字符串，所以我们需要使用call或者apply方法来改变toString方法的执行上下文。

```
const an = ['Hello','An'];
an.toString(); // "Hello,An"
Object.prototype.toString.call(an); // "[object Array]"
Object.prototype.toString.call('An') // "[object String]"
Object.prototype.toString.call(1) // "[object Number]"
Object.prototype.toString.call(Symbol(1)) // "[object Symbol]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(function(){}) // "[object Function]"
Object.prototype.toString.call({name: 'An'}) // "[object Object]"
```

Object.prototype.toString.call() 常用于判断浏览器内置对象时。

##### 5、typeof一元运算符

typeof一元运算符，用来返回操作数类型的字符串。
typeof只有一个实际应用场景，就是**用来检测一个对象是否已经定义或者是否已经赋值。**

##### 6、Array.isArray()

- 功能：用来判断对象是否为数组

- instanceof 与 isArray
  当检测Array实例时，Array.isArray 优于 instanceof ，因为 Array.isArray 可以检测出 iframes

  ```
  var iframe = document.createElement('iframe');
  document.body.appendChild(iframe);
  xArray = window.frames[window.frames.length-1].Array;
  var arr = new xArray(1,2,3); // [1,2,3]
  // Correctly checking for Array
  Array.isArray(arr);  // true
  Object.prototype.toString.call(arr); // true
  // Considered harmful, because doesn't work though iframes
  arr instanceof Array; // false
  ```

  Array.isArray() 与 Object.prototype.toString.call()
  Array.isArray()是ES5新增的方法，当不存在 Array.isArray() ，可以用 Object.prototype.toString.call() 实现。

  ```
  if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
  }
  ```

# JS函数柯里化

  柯里化是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。
  Currying有哪些好处呢？

- 1、参数复用

  ```
  function check(reg,txt){
      return reg.test(txt);
  }
  check(/\d+/g,'test'); //false
  check*(/[a-z]+/g,'test'); //true
  
  function curryingCheck(reg){
      return function(txt){
          return reg.test(txt);
      }
  }
  var hasNumber = curryingCheck(/\d+/g);
  var hasLetter = curryingCheck(/[a-z]+/g);
  
  hasNumber('test1');//true
  hasNumber('testtest');//false
  hasLetter('212222')//flase
  ```

  2、延迟运行

  ```
  function.prototype.bing = function(context){
      var _this = this;
      var args = Array.prototype.slice.call(arguments,1)
  return function(){
      return _this.apply(context,args);
    }
  }
  ```

  柯里化常见题

  ```
  function add(){
      //第一次执行时，定义一个数组专门用来存储所有的参数
      var _args = Array.prototype.slice.call(arguments);
      //在内部声明一个函数，利用闭包的特性保存_args并收集所有的参数值。
      var _adder = function(){
          _args.push(...arguments);
          return _adder;
      };
      //利用toString隐式转换的特性，当最后执行隐式转换，并计算最终返回的值
      _adder.toString = function(){
          return _args.reduce(function(a,b){
              return a+b;
          });
      }
      return _adder;
  }
  
  add(1)(2)(3);
  add(1,2,3)(4);
  add(1)(2)(3)(4)(5);
  ```

# 手写flat（）

```
//方法一

    function ArrayFlat(arr){
    let a = [];
    arr.forEach(item){
        if(Array.isArray(item)){
            ArrayFlat(item);
        }else{
            a.push(item);
        }
    }
    return a;
}

//方法二

var a = arr.toString.split(',');
for(var i=0;i<a.length;i++){
    a[i] =eval(arr[i]);
}
```

# babel

Babel 的三个主要处理步骤分别是： 解析（parse），转换（transform），生成（generate）。

- 解析
  将代码解析成抽象语法树（AST），每个js引擎（比如Chrome浏览器中的V8引擎）都有自己的AST解析器，而Babel是通过Babylon实现的。在解析过程中有两个阶段：词法分析和语法分析，词法分析阶段把字符串形式的代码转换为令牌（tokens）流，令牌类似于AST中节点；而语法分析阶段则会把一个令牌流转换成 AST的形式，同时这个阶段会把令牌中的信息转换成AST的表述结构。
- 转换
  在这个阶段，Babel接受得到AST并通过babel-traverse对其进行深度优先遍历，在此过程中对节点进行添加、更新及移除操作。这部分也是Babel插件介入工作的部分。
- 生成
  将经过转换的AST通过babel-generator再转换成js代码，过程就是深度优先遍历整个AST，然后构建可以表示转换后代码的字符串。

# NaN 是什么？有什么特别之处？

NaN 属性是代表非数字值的特殊值。该属性用于指示某个值不是数字。可以把 Number 对象设置为该值，来指示其不是数字值。

# 谈谈你对 JS 执行上下文栈和作用域链的理解？

执行上下文就是当前 JavaScript 代码被解析和执行时所在环境, JS执行上下文栈可以认为是一个存储函数调用的栈结构，遵循先进后出的原则。

- JavaScript执行在单线程上，所有的代码都是排队执行。
- 一开始浏览器执行全局的代码时，首先创建全局的执行上下文，压入执行栈的顶部。
- 每当进入一个函数的执行就会创建函数的执行上下文，并且把它压入执行栈的顶部。当前函数执行-完成后，当前函数的执行上下文出栈，并等待垃圾回收。
- 浏览器的JS执行引擎总是访问栈顶的执行上下文。
- 全局上下文只有唯一的一个，它在浏览器关闭时出栈。
  作用域链: 无论是 LHS 还是 RHS 查询，都会在当前的作用域开始查找，如果没有找到，就会向上级作用域继续查找目标标识符，每次上升一个作用域，一直到全局作用域为止。
- 如果查找的目的是对变量进行赋值，那么就会使用LHS 查询；如果目的是获取变量的值，就会使用RHS 查询

```
  		RHS 查询与简单地查找某个变量的值别无二致，而LHS 查询则是试图找到变量的容器本身，从而可以对其赋值。
  		LHS查询指的是找到变量的容器本身，从而可以对其进行赋值。也就是找到赋值操作的目标。
    	LHS查询的时候会沿着作用域链进行查询，找到的话就会将值赋值给这个变量，如果到达作用域顶端仍然找不到，就会在作用域链顶端创建这个变量。
```

# 去重

```
Array.prototype.unique = function(){
    const newArray = [];
    let isRepeat;
    for(let i=0;i<this.length;i++){
        isRepeat =false;
        for(let j= 0;j<newArray.length;j++){
            if(this[i]===newArray[j]){
                isRepeat = true;
                break;
            }
        }
        if(!isRepeat){
            newArray.push(this[i]);
        }
    }
    return newArray;
}
```

```
Array.prototype.unique =function(){
    const newArray=[];
    let isRepeat;
    for(let i=0;i<this.length;i++){
        isRepeat =false;
        for(let j=i+1;i<this.length;j++){
            if(this[i]===this[j]){
                isRepeat = true;
                break;
            }
        }
        if(!isRepeat){
            newArray.push(this[i]);
        }
    }
    return newArray;
}
```

```
let arr =[1,2,3,22,233,22,2,233,'a',3,'b','a'];
Array.prototype.unique = function(){
    const newArray = [];
    this.forEach(item =>{
        if(newArray.indexOf(item)===-1){
            newArray.push(item);
        }
    });
    return newArray;
}
```

```
Array.prototype.unique = function(){
    const newArray = [];
    this.sort();
    for(let i=0;i<this.length;i++){
        if(this[i] !== newArray[newArray.length -1]){
            newArray.push(this[i]);
        }
    }
    return newArray;
}
```

```
Array.prototype.unique = function(){
    const newArray = [];
    this.sort();
    for(let i=0;i<this.length;i++){
        if(this[i] !== newArray[newArray.length -1]){
            newArray.push(this[i]);
        }
    }
    return newArray;
}
```

```
Array.prototype.unique = function(){
    return [...new Set(this)];
}
```

# 实现一个 reduce 函数，作用和原生的 reduce 类似下面的例子。

```
function reduce(param,callback ,initVal){
    var hasInitVal = initVal !== void 0;
    var acc =hasInitVal ? initVal: param[0];
    each(hasInitVal? param:Array.prototype.slice.call(param,1),function(v,k,o){
        acc = callback(acc,v,k,o);
    });
    return acc;
}
```

# new 的原理是什么？通过 new 的方式创建对象和通过字面量创建有什么区别？Object.create 有什么作用？

new:

1. 创建一个新对象。

2. 这个新对象会被执行[[原型]]连接。

3. 属性和方法被加入到 this 引用的对象中。并执行了构造函数中的方法.

4. 如果函数没有返回其他对象，那么this指向这个新对象，否则this指向构造函数中返回的对象。

   ```
   function new(func){
       let target ={};
       target.__proto__ = func.prototype;
       let res = func.call(target);
       if(res &&typeof(res) =="object" || typeof(res) =="function"){
           return res;
       }
       return target;
   }
   ```

字面量创建对象，不会调用 Object构造函数, 简洁且性能更好;
new Object() 方式创建对象本质上是方法调用，涉及到在proto链中遍历该方法，当找到该方法后，又会生产方法调用必须的堆栈信息，方法调用结束后，还要释放该堆栈，性能不如字面量的方式。
通过对象字面量定义对象时，不会调用Object构造函数。

# Promise.all()和race()、finally的实现

promise构造函数是同步执行的，then方法是异步执行的

```
all(list) {
        return new Promise((resolve, reject) => {
            let resValues = [];
            let counts = 0;
            for (let [i, p] of list) {
                resolve(p).then(res => {
                    counts++;
                    resValues[i] = res;
                    if (counts === list.length) {
                        resolve(resValues)
                    }
                }, err => {
                    reject(err)
                })
            }
        })
    }
    
Promise._race = promises => new Promise((resolve, reject) => {
        promises.forEach(promise => {
                promise.then(resolve, reject)
        })
})

Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```



# setTimeout、Promise、Async/Await 的区别

1. setTimeout

   ```
   console.log('script start')    //1. 打印 script start
   setTimeout(function(){
     console.log('settimeout')   // 4. 打印 settimeout
   })      // 2. 调用 setTimeout 函数，并定义其完成后执行的回调函数
   console.log('script end')       //3. 打印 script start
   // 输出顺序：script start->script end->settimeout
   ```

   

2. Promise
   Promise本身是同步的立即执行函数， 当在executor中执行resolve或者reject的时候, 此时是异步操作， 会先执行then/catch等，当主栈完成后，才会去调用resolve/reject中存放的方法执行，打印p的时候，是打印的返回结果，一个Promise实例。

   ```
   console.log('script start')
   let promise1 = new Promise(function (resolve) {
     console.log('promise1')
     resolve()
     console.log('promise1 end')
   }).then(function () {
     console.log('promise2')
   })
   setTimeout(function(){
     console.log('settimeout')
   })
   console.log('script end')
   // 输出顺序: script start->promise1->promise1 end->script end->promise2->settimeout
   ```

   当JS主线程执行到Promise对象时，

   当JS主线程执行到Promise对象时，

- promise1.then() 的回调就是一个 task
- promise1 是 resolved或rejected: 那这个 task 就会放入当前事件循环回合的 microtask queue
- promise1 是 pending: 这个 task 就会放入 事件循环的未来的某个(可能下一个)回合的 microtask queue 中
- setTimeout 的回调也是个 task ，它会被放入 macrotask queue 即使是 0ms 的情况

3. async/await

   ```
   async function async1(){
    console.log('async1 start');
     await async2();
     console.log('async1 end')
   }
   async function async2(){
     console.log('async2')
   }
   console.log('script start');
   async1();
   console.log('script end')
   ```

   

   // 输出顺序：script start->async1 start->async2->script end->async1 end
   async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了 async 函数体。
   举个例子：

   ```
   async function func1() {
     return 1
   }
   console.log(func1())
   ```

   

![image-20190904010428300](/Users/caiyifan/Library/Application Support/typora-user-images/image-20190904010428300.png)

很显然，func1的运行结果其实就是一个Promise对象。因此我们也可以使用then来处理后续逻辑。
func1().then(res => {
  console.log(res);  // 30
})
await的含义为等待，也就是 async 函数需要等待await后的函数执行完成并且有了返回结果（Promise对象）之后，才能继续执行下面的代码。await通过返回一个Promise对象来实现同步的效果。
我觉得这题主要是考察这三者在事件循环中的区别，事件循环中分为宏任务队列和微任务队列。
其中settimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行；
promise.then里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行；async函数表示函数里面可能会有异步方法，await后面跟一个表达式，async方法执行时，遇到await会立即执行表达式，然后把表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行。

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
