## Redux 和 Vuex 的设计思想

### 共同点

首先两者都是处理全局状态的工具库，大致实现思想都是：全局state保存状态---->dispatch(action)------>reducer(vuex里的mutation)----> 生成newState; 整个状态为同步操作；

### 区别

最大的区别在于处理异步的不同，vuex里面多了一步commit操作，在action之后commit(mutation)之前处理异步，而redux里面则是通过中间件处理


流程

再回顾一下刚才的流程图，尝试走一遍 Redux 流程：

1、用户通过 View 发出 Action：

store.dispatch(action);
2、然后 Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action。 Reducer 会返回新的 State 。

let nextState = xxxReducer(previousState, action);
3、State 一旦有变化，Store 就会调用监听函数。

store.subscribe(listener);
4、listener可以通过 store.getState() 得到当前状态。如果使用的是 React，这时可以触发重新渲染 View。



### VUEX

1、在vue组件里面，通过dispatch来触发actions提交修改数据的操作。

2、然后再通过actions的commit来触发mutations来修改数据。

3、mutations接收到commit的请求，就会自动通过Mutate来修改state（数据中心里面的数据状态）里面的数据。

4、最后由store触发每一个调用它的组件的更新

### Action

到这里又该处理异步这块儿了。mutation 是必须同步的，这个很好理解，和之前的 reducer 类似，不同步修改的话，会很难调试，不知道改变什么时候发生，也很难确定先后顺序，A、B两个 mutation，调用顺序可能是 A -> B，但是最终改变 State 的结果可能是 B -> A。

对比Redux的中间件，Vuex 加入了 Action 这个东西来处理异步，Vuex的想法是把同步和异步拆分开，异步操作想咋搞咋搞，但是不要干扰了同步操作。View 通过 store.dispatch('increment') 来触发某个 Action，Action 里面不管执行多少异步操作，完事之后都通过 store.commit('increment') 来触发 mutation，一个 Action 里面可以触发多个 mutation。所以 Vuex 的Action 类似于一个灵活好用的中间件。

Vuex 把同步和异步操作通过 mutation 和 Action 来分开处理，是一种方式。但不代表是唯一的方式，还有很多方式，比如就不用 Action，而是在应用内部调用异步请求，请求完毕直接 commit mutation，当然也可以。

Vuex 还引入了 Getter，这个可有可无，只不过是方便计算属性的复用。

Vuex 单一状态树并不影响模块化，把 State 拆了，最后组合在一起就行。Vuex 引入了 Module 的概念，每个 Module 有自己的 state、mutation、action、getter，其实就是把一个大的 Store 拆开。

总的来看，Vuex 的方式比较清晰，适合 Vue 的思想，在实际开发中也比较方便。

### 对比Redux

Redux： view——>actions——>reducer——>state变化——>view变化（同步异步一样）

Vuex： view——>commit——>mutations——>state变化——>view变化（同步操作）

 view——>dispatch——>actions——>mutations——>state变化——>view变化（异步操作）

## 为什么 Vuex 的 mutation 和 Redux 的 reducer 中不能做异步操作？

Mutation 必须是同步函数
一条重要的原则就是要记住 mutation 必须是同步函数。为什么？请参考下面的例子：

```
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```


现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的。

在组件中提交 Mutation
你可以在组件中使用 this.$store.commit('xxx') 提交 mutation，或者使用 mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用（需要在根节点注入 store）。

```
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`


  // `mapMutations` 也支持载荷：
  'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
]),
...mapMutations({
  add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
})


  }
}


```

为什么Redux的reducer里不能有异步操作。

先从Redux的设计层面来解释为什么Reducer必须是纯函数
如果你经常用React+Redux开发，那么就应该了解Redux的设计初衷。Redux的设计参考了Flux的模式，作者希望以此来实现时间旅行，保存应用的历史状态，实现应用状态的可预测。所以整个Redux都是函数式编程的范式，要求reducer是纯函数也是自然而然的事情，使用纯函数才能保证相同的输入得到相同的输入，保证状态的可预测。所以Redux有三大原则：

单一数据源，也就是state
state 是只读，Redux并没有暴露出直接修改state的接口，必须通过action来触发修改
使用纯函数来修改state，reducer必须是纯函数
下面在从代码层面来解释为什么reducer必须是纯函数
那么reducer到底干了件什么事，在Redux的源码中只用了一行来表示：

currentState = currentReducer(currentState, action)
这一行简单粗暴的在代码层面解释了为什么currentReducer必须是纯函数。currentReducer就是我们在createStore中传入的reducer（至于为什么会加个current有兴趣的可以自己去看源码），reducer是用来计算state的，所以它的返回值必须是state，也就是我们整个应用的状态，而不能是promise之类的。

要在reducer中加入异步的操作，如果你只是单纯想执行异步操作，不会等待异步的返回，那么在reducer中执行的意义是什么。如果想把异步操作的结果反应在state中，首先整个应用的状态将变的不可预测，违背Redux的设计原则，其次，此时的currentState将会是promise之类而不是我们想要的应用状态，根本是行不通的。



## Vue 的响应式原理中 Object.defineProperty 有什么缺陷？为什么在 Vue3.0 采用了 Proxy，抛弃了 Object.defineProperty？

1. Object.defineProperty无法监控到数组下标的变化，导致通过数组下标添加元素，不能实时响应；
2. Object.defineProperty只能劫持对象的属性，从而需要对每个对象，每个属性进行遍历，如果，属性值是对象，还需要深度遍历。Proxy可以劫持整个对象，并返回一个新的对象。
3. Proxy不仅可以代理对象，还可以代理数组。还可以代理动态增加的属性。

```
//defineProperty实现

var data = { name: 'yck' }
observe(data)
let name = data.name // -> get value
data.name = 'yyy' // -> change value

function observe(obj) {
  // 判断类型
  if (!obj || typeof obj !== 'object') {
    return
  }
  Object.keys(obj).forEach(key => {
    defineReactive(obj, key, obj[key])
  })
}

function defineReactive(obj, key, val) {
  // 递归子属性
  observe(val)
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter() {
      console.log('get value')
      return val
    },
    set: function reactiveSetter(newVal) {
      console.log('change value')
      val = newVal
    }
  })
}

//代理

let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver)
    },
    set(target, property, value, receiver) {
      setBind(value)
      return Reflect.set(target, property, value)
    }
  }
  return new Proxy(obj, handler)
}

let obj = { a: 1 }
let value
let p = onWatch(
  obj,
  v => {
    value = v
  },
  (target, property) => {
    console.log(`Get '${property}' = ${target[property]}`)
  }
)
p.a = 2 // bind `value` to `2`
p.a // -> Get 'a' = 2



//代理

<body>
  hello,world
  <input type="text" id="model">

  <p id="word"></p>

</body>

<script>
  const model = document.getElementById("model")
  const word = document.getElementById("word")
  var obj= {};

  const newObj = new Proxy(obj, {
      get: function(target, key, receiver) {
        console.log(`getting ${key}!`);
        return Reflect.get(target, key, receiver);
      },
      set: function(target, key, value, receiver) {
        console.log('setting',target, key, value, receiver);
        if (key === "text") {
          model.value = value;
          word.innerHTML = value;
        }
        return Reflect.set(target, key, value, receiver);
      }
    });

  model.addEventListener("keyup",function(e){
    newObj.text = e.target.value
  })
</script>
```

#### 说一下vue和react中的路由实现方式？

https://www.jianshu.com/p/8b94f1b98578

## React和Vue的区别

### 区别

**监听数据变化的实现原理不同**

- Vue 通过 getter/setter 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
- React 默认是通过比较引用的方式进行的，如果不优化（PureComponent/shouldComponentUpdate）可能导致大量不必要的VDOM的重新渲染

为什么 React 不精确监听数据变化呢？这是因为 Vue 和 React 设计理念上的区别，Vue 使用的是可变数据，而React更强调数据的不可变。所以应该说没有好坏之分，Vue更加简单，而React构建大型应用的时候更加棒。

**数据流的不同**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190601094122147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1Rva2tpXw==,size_16,color_FFFFFF,t_70)
Vue中默认是支持双向绑定的。在Vue1.0中我们可以实现两种双向绑定：

- 父子组件之间，props 可以双向绑定
- 组件与DOM之间可以通过 v-model 双向绑定

在 Vue2.x 中去掉了第一种，也就是父子组件之间不能双向绑定了（但是提供了一个语法糖自动帮你通过事件的方式修改），并且 Vue2.x 已经不鼓励组件对自己的 props 进行任何修改了。
所以现在我们只有 组件 <–> DOM 之间的双向绑定这一种。

然而 React 从诞生之初就不支持双向绑定，React一直提倡的是单向数据流，他称之为 onChange/setState()模式。

不过由于我们一般都会用 Vuex 以及 Redux 等单向数据流的状态管理框架，因此很多时候我们感受不到这一点的区别了。

**HoC (高阶组件) 和 mixins**

在 Vue 中我们组合不同功能的方式是通过 mixin，而在React中我们通过 HoC (高阶组件）。

React 最早也是使用 mixins 的，不过后来他们觉得这种方式对组件侵入太强会导致很多问题，就弃用了 mixinx 转而使用 HoC，关于mixin究竟哪里不好，可以参考React官方的这篇文章 Mixins Considered Harmful

而 Vue 一直是使用 mixin 来实现的。

为什么 Vue 不采用 HoC 的方式来实现呢？

高阶组件本质就是高阶函数，React 的组件是一个纯粹的函数，所以高阶函数对React来说非常简单。

但是Vue就不行了，Vue中组件是一个被包装的函数，并不简单的就是我们定义组件的时候传入的对象或者函数。比如我们定义的模板怎么被编译的？比如声明的props怎么接收到的？这些都是vue创建组件实例的时候隐式干的事。由于vue默默帮我们做了这么多事，所以我们自己如果直接把组件的声明包装一下，返回一个高阶组件，那么这个被包装的组件就无法正常工作了。

**组件通信的区别**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190601094434921.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1Rva2tpXw==,size_16,color_FFFFFF,t_70)
其实这部分两个比较相似。

在Vue 中有三种方式可以实现组件通信：

- 父组件通过 props 向子组件传递数据或者回调，虽然可以传递回调，但是我们一般只传数据，而通过 事件的机制来处理子组件向父组件的通信
- 子组件通过 事件 向父组件发送消息
- 通过 V2.2.0 中新增的 provide/inject 来实现父组件向子组件注入数据，可以跨越多个层级。

另外有一些比如访问 parent/parent/*p**a**r**e**n**t*/children等比较dirty的方式这里就不讲了。

在 React 中，也有对应的三种方式：

- 父组件通过 props 可以向子组件传递数据或者回调
- 可以通过 context 进行跨层级的通信，这其实和 provide/inject 起到的作用差不多。

可以看到，React 本身并不支持自定义事件，Vue中子组件向父组件传递消息有两种方式：事件和回调函数，而且Vue更倾向于使用事件。但是在 React 中我们都是使用回调函数的，这可能是他们二者最大的区别。

**模板渲染方式的不同**

在表层上， 模板的语法不同

- React 是通过JSX渲染模板
- 而Vue是通过一种拓展的HTML语法进行渲染

但其实这只是表面现象，毕竟React并不必须依赖JSX。
在深层上，模板的原理不同，这才是他们的本质区别：

- React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的
- Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现

对这一点，我个人比较喜欢React的做法，因为他更加纯粹更加原生，而Vue的做法显得有些独特，会把HTML弄得很乱。举个例子，说明React的好处：

**react中render函数是支持闭包特性的，所以我们import的组件在render中可以直接调用。但是在Vue中，由于模板中使用的数据都必须挂在 this 上进行一次中转，所以我们import 一个组件完了之后，还需要在 components 中再声明下，这样显然是很奇怪但又不得不这样的做法。**

**Vuex 和 Redux 的区别**

从表面上来说，store 注入和使用方式有一些区别。

在 Vuex 中，$store 被直接注入到了组件实例中，因此可以比较灵活的使用：

- 使用 dispatch 和 commit 提交更新
- 通过 mapState 或者直接通过 this.$store 来读取数据

在 Redux 中，我们每一个组件都需要显示的用 connect 把需要的 props 和 dispatch 连接起来。

另外 Vuex 更加灵活一些，组件中既可以 dispatch action 也可以 commit updates，而 Redux 中只能进行 dispatch，并不能直接调用 reducer 进行修改。

从实现原理上来说，最大的区别是两点：

- Redux 使用的是不可变数据，而Vuex的数据是可变的。Redux每次都是用新的state替换旧的state，而Vuex是直接修改
- Redux 在检测数据变化的时候，是通过 diff 的方式比较差异的，而Vuex其实和Vue的原理一样，是通过 getter/setter来比较的（如果看Vuex源码会知道，其实他内部直接创建一个Vue实例用来跟踪数据变化）

而这两点的区别，其实也是因为 React 和 Vue的设计理念上的区别。React更偏向于构建稳定大型的应用，非常的科班化。相比之下，Vue更偏向于简单迅速的解决问题，更灵活，不那么严格遵循条条框框。因此也会给人一种大型项目用React，小型项目用 Vue 的感觉。

------

### 各自优点

**React**

1. React速度很快：它并不直接对DOM进行操作，引入了一个叫做虚拟DOM的概念，安插在javascript逻辑和实际的DOM之间，性能好。最大限度减少DOM交互。
2. 跨浏览器兼容：虚拟DOM帮助我们解决了跨浏览器问题，它为我们提供了标准化的API，甚至在IE8中都是没问题的。
3. 一切都是component：代码更加模块化，重用代码更容易，可维护性高。这样当某个或某些组件出现问题是，可以方便地进行隔离。每个组件都可以进行独立的开发和测试，并且它们可以引入其它组件。这等同于提高了代码的可维护性。
4. 单向数据流：Flux是一个用于在JavaScript应用中创建单向数据层的架构，它随着React视图库的开发而被Facebook概念化。减少了重复代码，这也是它为什么比传统数据绑定更简单。
5. 同构、纯粹的javascript：因为搜索引擎的爬虫程序依赖的是服务端响应而不是JavaScript的执行，预渲染你的应用有助于搜索引擎优化。
6. 兼容性好：比如使用RequireJS来加载和打包，而Browserify和Webpack适用于构建大型应用。它们使得那些艰难的任务不再让人望而生畏。

**vue**

1. 性能高效
2. 双向数据绑定
3. 学习难度低，上手简单。

**React 的缺陷**

1. React 只是一个视图库，而不是一个完整的框架。
2. 对于 Web 开发初学者来说，有一个学习曲线。
3. 将 React 集成到传统的 MVC 框架中需要一些额外的配置。
4. 代码复杂性随着内联模板和 JSX 的增加而增加。
5. 如果有太多的小组件可能增加项目的庞大和复杂。