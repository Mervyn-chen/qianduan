#### react中props和state有什么区别？

react是单向数据流，props是一个父组件传递给子组件的数据流接口，可以一直的被传递到子孙组件中，在组件内部的props是只读的。而state是组件内部私有的状态，通过修改state会触发组件的重新渲染。

#### **pure component介绍（类比纯函数介绍）**

原理
首先我们上PureComponent的源代码

if (this._compositeType === CompositeTypes.PureClass) {
  shouldUpdate = !shallowEqual(prevProps, nextProps)
  || !shallowEqual(inst.state, nextStat e);
}
return shouldUpdate;
shallowEqual从字面意思讲是"浅比较"，那么什么事浅比较呢？
就是比较一个旧的props和新props的或者旧的state和新的state的长度是否一致，key是是否相同，以及它们对应的引用是否发生改变。仅仅做了一层的比较。所以这才叫做浅比较。

因此我们可以看到，当props或者state发生改变时，并不是直接去渲染，而是做了一层比较，才决定是否重新渲染该组件。

使用
PureComponent的使用需要注意一些地方，因为它做了shallowEqual操作，所以在使用PureComponent的时候需要
注意props和state的引用是否发生改变，如果引用没有发生改变，当你在setState的时候也不会重新渲染

举个栗子

List组件继承PureComponent

import React from 'react'

class List extends React.PureComponent{
    render(){
        const {list} = this.props;
        return (
            <ul>
                {
                    list.map((item,idx)=>{
                        return (
                            <li key={idx}>
                                {item}
                            </li>
                        )
                    })
                }
            </ul>
​        )
​    }
}
export default List;
App组件继承自PureComponent

import List from './List'
class App extends React.PureComponent{
    state = {
        array:[1,2,3]
    }
    click(){
        const {array} = this.state;
        array.push(4);
        this.setState({array:array})
    }
    render(){
        const {array} = this.state;
        return (
            <div>
                <List list={array} />
                <button onClick={this.click.bind(this)}>测试</button>
            </div>
​        )
​    }
}
这时候点击button，你会发现页面没有任何改变。原因就是array数组的引用没有变化。
如果App继承自Component,当你调用setState的时候，是不管三七二十一，组件都会去重新渲染一遍。

这时候把click事件做修改

click(){
        const {array} = this.state;
        array.push(4);
        const newArray = [].concat(array)
        this.setState({array:newArray})
    }
这时候点击button，你会发现页面发生了改变。原因就是array数组的引用发生变化。

总结
从上面我们发现PureComponent并不是不发生重新渲染，而是在满足不了的条件下才会去帮我们重新渲染。
所以PureComponent真正起到优化的地方是在纯组件上，也就是仅仅用来展示的组件。这样会避免展示的组件不必要的多次重复渲染。在一些复杂组件上用了也没有什么关系，只是在浅比较这一层就过不了，但是一定要记得props和state不可以是同一个引用。

#### **pure component和React生命周期函数shouldComponentUpdate的联系**

state作为react的一个重要的部分，固然自动渲染给我们省了很多的麻烦，然而并不是我们想让他不渲染就很容易做到的
一些时候，state的重新渲染机制导致了不必要的渲染，所以研究一下用来一些特定的情况下也是很有必要的

一. shouldComponentUpdate
（shouldComponentUpdate可能会导致bug，并不建议使用）
关于shouldComponentUpdate，React.js官网是这么解释的：

Use shouldComponentUpdate() to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.
shouldComponentUpdate() is invoked before rendering when new props or state are being received. Defaults to true. This method is not called for the initial render or when forceUpdate() is used.

React默认在每一次state或者props改变之后进行渲染，会在每一次新的props或者state接受之前被调用，但是页面第一次渲染或者forceUpdate()的时候不会被调用.
如果对shouldComponentUpdate进行设置，多半格式是这样的，返回true或者false：

 shouldComponentUpdate(nextProps, nextState) {
   if (this.props.count !== nextProps.count) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }
1
2
3
4
5
6
7
8
9
什么是不必要的组件渲染，并不是state或者props的值不改变，组件就不会重新渲染
e.g

this.setState({
       Number:this.state.Count
})
1
2
3
虽然Count的值并没有改变，但是React会对组件重新渲染，就会造成组件不必要的重新渲染；另外没有状态交换的父组件的重新渲染也会导致自组件没有必要的重新渲染。但是即使返回false，组件依然有可能会重新渲染.

另外，shouldComponentUpdate不可以直接比较Object，会直接true阻止渲染，因为不同的引用指向的其实是对内存中的同一个对象

nextProps.countObject.count == this.props.countObject.count
1
如何解决这一问题：

ES6:Object.assign({}) 浅拷贝(克隆)、对象属性的合并
var Obj = Object.assign({},obj1,obj2); //{}叫目标对象，obj1和obj2是源对象。开源对象的数量为一个或者多个。Object.assign 拷贝的是属性值（相当于当前数据的快照），会跳过那些值为 null 或 undefined 的源对象。

对象属性合并：将obj1和obj2里面的属性添加到{}中去，若obj1和obj2属性名有冲突，后面的属性将会覆盖前面的属性

JSON.parse(JSON.stringify(data)) 深拷贝（相当于原有数据的拷贝）
immutable.js
React.PureComponent：对道具和状态进行浅层次的比较，并减少必要的机会
二. React.PureComponent
PureComponent对道具和状态进行浅层次的比较，并不能定制shouldComponentUpdate()，并减少必要的渲染。
如果props和state的数据结构复杂，则无法正确渲染，只有在几种情况注意：

有相同的props和state，pureComponent的渲染结果相同
使用forceUpdate()深层的数据改变，pureComponent值进行浅层数据比较，可能因为深层数据改变但是渲染没有更新而产生错误的渲染判断
使用不可变的对象来加速nested数据的快速比较

#### **React Hook有看过吗**

https://juejin.im/post/5c99a75af265da60ef635898

#### React延伸，time slice、浏览器为什么60帧，为什么React引入time slice可以如此丝滑

#### React Fiber调度

#### React diff算法

#### SCU、PureComponent

#### setState是同步的还是异步的

#### react的纯函数组件和class组件的区别

#### 修改页面上绑定的变量值，比如一段文章的title，下一行代码是否能立即获取到title的变化（不能，需要nextTick）

#### react组件之间的传递数据

#### web components

#### 词法分析变成抽象语法书的原因 

#### react结合immutable.js为什么能提高渲染效率

