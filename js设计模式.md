## **1、说一下设计模式？**

单例：

```
class CreateUser{
    constructor(name){
        this.name = name;
        this.getName();
    }
    getName(){
        return this.name;
    }
}
var  ProxyMode = (function(){
    var instance = null;
    return function(name){
        if(!instance){
            instance = new CreateUser(name);
        }
        return instance;
    }
})();
var a= new ProxyMode('aaa');
var b= new ProxyMode('bbb');
```

发布者、订阅者模式

```
function Publish(){
    this.s =[];
    this.add(sMember){
        let isExist = this.s.some(fucntion(item){
            if(item == sMember){
                return true;
            }else{
                return false;
            }
        })
        if(!isExist){
            s.push(sMember);
        }
    }
    this.send = function(data){
        this.s.forEach(function(fn){
            fn(data);
        })
        return this;
    }
}

let a = function(data){
    console.log(data);
}
let p = new Publish();
p.add(a);
p.send('sadasdsa');
```

观察者模式

```
// 创建对象
var targetObj = {
    name:'小李'
}
var targetObj2 = {
    name:'小李'
}
// 定义值改变时的处理函数（观察者）
function observer(oldVal, newVal) {
    // 其他处理逻辑...
    targetObj2.name = newVal
    console.info('targetObj2的name属性的值改变为 ' + newVal);
}

// 定义name属性及其set和get方法（name属性为被观察者）
Object.defineProperty(targetObj, 'name', {
    enumerable: true,
    configurable: true,
    get: function() {
        return name;
    },
    set: function(val) {
        //调用处理函数
        observer(name, val)
        name = val
    }
});

targetObj.name = '张三';
targetObj.name = '李四';
console.log(targetObj2.name)
```