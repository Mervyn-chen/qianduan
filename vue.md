# **说一下vue实例的生命周期？**

new Vue() > 初始化事件和生命周期 > beforeCreate > 数据绑定 > created > 如果有el属性，则编译模板 > beforeMount > 添加$el属性，并替换掉挂载的DOM元素 > mounted > 数据更新 > beforeUpdate > 重新编译模板并渲染DOM > updated > 调用$destoryed > beforeDestory > 销毁vue实例 > destroyed

# 虚拟DOM

虚拟 dom 是相对于浏览器所渲染出来的真实 dom 的，在react，vue等技术出现之前，我们要改变页面展示的内容只能通过遍历查询 dom 树的方式找到需要修改的 dom 然后修改样式行为或者结构，来达到更新 ui 的目的。

这种方式相当消耗计算资源，因为每次查询 dom 几乎都需要遍历整颗 dom 树，如果建立一个与 dom 树对应的虚拟 dom 对象（ js 对象），以对象嵌套的方式来表示 dom 树，那么每次 dom 的更改就变成了 js 对象的属性的更改，这样一来就能查找 js 对象的属性变化要比查询 dom 树的性能开销小。

# **react中props和state有什么区别？**

react是单向数据流，props是一个父组件传递给子组件的数据流接口，可以一直的被传递到子孙组件中，在组件内部的props是只读的。而state是组件内部私有的状态，通过修改state会触发组件的重新渲染。

# **Mvvm定义**

MVVM是Model-View-ViewModel的简写。即模型-视图-视图模型。【模型】指的是后端传递的数据。【视图】指的是所看到的页面。【视图模型】mvvm模式的核心，它是连接view和model的桥梁。它有两个方向：一是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的页面。实现的方式是：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转化成后端的数据。实现的方式是：DOM 事件监听。这两个方向都实现的，我们称之为数据的双向绑定。

## vue 父子组件嵌套时，组件内部的各个生命周期钩子触发先后顺序

实例初始化前 beforecreated-------------------->

创建之前--------------------------------->

实例初始化完成 虚拟DOM 是不存在的 create-------------------->

实例创建后--------------------------->

实例挂载前 虚拟dom已经生成beforecreate---------------->



### **VUE组件中 data 里面的数据为什么要return 出来**

因为在JS 中只有函数才存在作用域,data是一个函数时，每个组件实例都有自己的作用域，每个实例相互独立,不会相互影响！！

### 说说Vue中`$nextTick`的实现原理

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

```
// 修改数据
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(function () {
  // DOM 更新了
})

// 作为一个 Promise 使用 (2.1.0 起新增，详见接下来的提示)
Vue.nextTick()
 .then(function () {
  // DOM 更新了
})

2.1.0 起新增：如果没有提供回调且在支持 Promise 的环境中，则返回一个 Promise。请注意 Vue 不自带 Promise 的 polyfill，所以如果你的目标浏览器不原生支持 Promise (IE：你们都看我干嘛)，你得自己提供 polyfill。
0

```



### vue

- **从0到1自己构架一个vue项目，说说有哪些步骤、哪些重要插件、目录结构你会怎么组织**

  要看是vue.js还是vue cli项目，如果是vue.js可以按照普通web项目流程开发，只在代码架构上变为数据和模型分离的模式即可。
  如果是vue cli项目，不仅要分离数据和模型，还要理解vue router, vuex等组件，并且对于组件化的概念要更加透彻，由其是单页面应用，每个子页面如何组件化是必须在项目伊始就想清楚的。

- **你知道vue的模板语法用的是哪个web模板引擎的吗？说说你对这模板引擎的理解**

  Vue使用了Mustache语法，即双大括号的语法。模板引擎的初衷是解决早期的字符模板拼接的问题，也是因这些方面的慢慢的发展壮大。

- **你知道v-model的原理吗？说说看**

  原生input其实只是一个语法糖，:bind="value"与[@change](https://github.com/change)="value = $event.target.value"的结合。
  自定义组件的时候的v-model默认监听change事件和绑定value 的prop。

  ### 父子组件传值

  ```
  <div id="app">   
  <!--父组件，可以在引用子组件的时候，通过属性绑定（v:bind）的形式，把需要传递给子组件的数据    传递到子组件内部-->    
  <com1 :parentmsg="msg"  :parentmsg2="msg2">  
  </com1></div><script>    
  var vm = new Vue({       
  el: '#app',       
  data:{           
  msg: '123父组件中的数据' ,           
  msg2:'124131313'        },      
  methods: {        },       
  components:{           
  'com1':{               
  //子组件中，默认无法访问到父组件中的data和methods                template: '<h1 @click="change"> 这是子组件 {{parentmsg}}、{{parentmsg2}}</h1>',              
  //注意，组件中的所有props中的数据都是通过父组件传递给子组件的       //propes中的数据是只可读               
  props: ['parentmsg','parentmsg2'] ,
  // 把父组件传递过来的parentmsg属性， 数组中，定义一下，这样才能用这个数据,                
  //注意子组件中的data数据，并不是通过父组件传递过来的，而是子组件字有的，比如：子组件通过Ajax请求回来的值，可以放到data中           //dat a中的数据可读可写                
  data(){                   
          return {                       
                  title: '123',                       
                  content: 'qqq'                  
  				}              
  		},              
   methods: {                   
                      change(){                       
                      this.parentmsg='被修改'                  
  		         }                
  	       },          
  		}       
  	}    
  })</script>
  ```

- **你有使用过vue开发多语言项目吗？说说你的做法？**

  所有语言对于信息都命名，存在后端，由restful接口返回，在切换语言时触发dispatch，调用接口，在所有页面里mapState拿到语言配置，渲染上去

  vue-i18n

- **在使用计算属性的时，函数名和data数据源中的数据可以同名吗？**

  不能同名 因为不管是计算属性还是data还是props 都会被挂载在vm实例上，因此 这三个都不能同名

- **vue中data的属性可以和methods中的方法同名吗？为什么？**

  data中的属性和methods方法重名会优先执行data中的属性并且报错

- **怎么给vue定义全局的方法？**

  挂载到Vue的prototype上。把全局方法写到一个文件里面，然后for循环挂载到Vue的prototype上，缺点是调用这个方法的时候没有提示

  ```
   Object.keys(tools).forEach(key => {
        Vue.prototype[key] = tools[key]
   })
  ```

  利用全局混入mixin，因为mixin里面的methods会和创建的每个单文件组件合并。这样做的优点是调用这个方法的时候有提示

  1、通过prototype，这个非常方便。Vue.prototype[method]=method;
  2、通过插件Vue.use(plugin)；
  3、通过mixin，Vue.mixin(mixins);

- **vue2.0不再支持v-html中使用过滤器了怎么办？**

  在method中定义方法

  ```
  htmlFilter(htmlString){
  return htmlString.replace(/+s/g,'')
  }
  ```

  在vue中 `v-html="htmlFilter(htmlString)"`即可

- **怎么解决vue打包后静态资源图片失效的问题？**

  找到config/index.js 配置文件，找build打包对象里的assetsPublicPath属性

  默认值为/，更改为./就好了

  publicPath: '../../'

- **怎么解决vue动态设置img的src不生效的问题？**

  因为动态添加src被当做静态资源处理了，没有进行编译，所以要加上require。

  <img :src="require('../../../assets/images/xxx.png')" />

- **使用vue后怎么针对搜索引擎做SEO优化？**

  - ssr,即单页面后台渲染
  - vue-meta-info 与prerender-spa-plugin 预渲染
  - nuxt
  - phantomjs

- **跟keep-alive有关的生命周期是哪些？描述下这些生命周期**

  activated和deactivated

  - keep-alive的生命周期
    1.activated： 页面第一次进入的时候，钩子触发的顺序是created->mounted->activated
    2.deactivated: 页面退出的时候会触发deactivated，当再次前进或者后退的时候只触发activated

- **如果现在让你从vue/react/angularjs三个中选择一个，你会选哪个？说说你的理由**

  让我选肯定是vue。angularjs没用过。angular倒是用过。挺好用的，但是！！！编译真的很慢，加了热更新还是慢，开发毫无用户体验，按ctrl+s等2秒的绝望，你没用过angular不会明白的。react没在项目中用过，自己玩过几个小项目，使用体验一般般，jsx写起来真的很别扭

- **你知道vue2.0兼容IE哪个版本以上吗？**

  不兼容ie8及以下
  是因为vue的响应式原理是基于es5的Object.defineProperty的,而这个方法不支持ie8及以下

- **使用vue开发一个todo小应用，谈下你的思路**

   输入部分( input )和输出部分( ul )
  逻辑:用户输入之后,通过事件触发拿到用户输入的数据存起来,
  将用户数据集合通过 v-for 渲染到页面上
  当用户点击清单项,通过事件触发移出对应事件

- **你有看过vue推荐的风格指南吗？列举出你知道的几条**

  1、组件名称见名知意，最好加特殊前缀，如：el-Button、el-row等
  2、组件的prop尽可能描述的越详细越好
  3、为组件样式设置scoped作用域
  等等

  1、使用v-for时记得加key，可以快速定位到需要更新的DOM节点，提高效率。
  2、永远不要把 v-if 和 v-for 同时用在同一个元素上。提高渲染效率
  3、优先通过 Vuex 管理全局状态，而不是通过 this.$root 或一个全局事件总线

- **你是从vue哪个版本开始用的？你知道1.x和2.x有什么区别吗？**

  vue1.0的数据绑定完全依赖于数据侦测，使用Object.defineProperty方法使数据去通知相应watch，改变dom结构。vue2.0引入了虚拟dom，只通知到组件，提升了颗粒度

  2.0生命生命周期变化感觉变得更加语义化一点（有规律可寻，更好记了），而且增加了beforeUpdate、updated、activated、deactivated，删除了attached、detached。
  2.0将1.0所有自带的过滤器都删除了，也就是说，在2.0中，要使用过滤器，则需要我们自己编写，以下是一个自定义过滤器示例,

- **你知道vue中key的原理吗？说说你对它的理解**

  便于diff算法的更新，key的唯一性，能让算法更快的找到需要更新的dom，需要注意的是，key要唯一，不然会出现很隐蔽性的更新问题。

- **vue中怎么重置data？**

  使用Object.assign()，vm.$data可以获取当前状态下的data，vm.$options.data可以获取到组件初始化状态下的data，具体可以看[vm.$options](https://cn.vuejs.org/v2/api/#vm-options)。

  ```
  Object.assign(this.$data, this.$options.data())
  ```

- **vue渲染模板时怎么保留模板中的HTML注释呢？**

  ```
  <template comments>
    ...
  </template>
  ```

- **Vue.observable你有了解过吗？说说看**

  类轻量级vuex，用作状态管理。vue2.6发布一个新的API，可以处理一些简单的跨组件共享数据状态的问题。

- **你知道style加scoped属性的用途和原理吗？**

  在标签上绑定了自定义属性，防止css全局污染
  但是很多时候使用ui框架如果加scope就不能覆盖，这个时候一般写sass 会在最外层包裹该组件名的id 就可以不使用scoped 了

- **你期待vue3.0有什么功能或者改进的地方？**

  彻底解决Vuex表单状态管理问题

- **vue边界情况有哪些？**

  组件递归
  组件之间相互引用，A用B，B又用了A

  在errorCaptured中更改组件状态，触发更新后再次报错（未设置短路条件），陷入无限循环

- **如何在子组件中访问父组件的实例？**

   this.$parent拿到父组件实例
   this.$children拿到子组件实例（数组）

   Vue中子组件调用父组件的方法，这里有三种方法提供参考：
   1：直接在子组件中通过this.$parent.event来调用父组件的方法
   2：在子组件里用$emit向父组件触发一个事件，父组件监听这个事件
   3：父组件把方法传入子组件中，在子组件里直接调用这个方法

- **watch的属性用箭头函数定义结果会怎么样？**

   因为箭头函数默绑定父级作用域的上下文，所以不会绑定vue实例，所以 this 是undefind

   this是undefined，要更改的属性会报TypeError错误, Cannot read property 'xxx' of undefined

- **在vue项目中如果methods的方法用箭头函数定义结果会怎么样？**

   在html里面直接用<script>引入vue的，this的默认绑定在window上，而我是用的webpack构建的，默认在严格模式下，this默认绑定为undefined

- **在vue项目中如何配置favicon？**

   html模版加入meta标签，引入favicon

- **你有使用过babel-polyfill模块吗？主要是用来做什么的？**

   ES6的转码。IE的兼容

   babel默认只转换语法,而不转换新的API,如需使用新的API,还需要使用对应的转换插件或者polyfill去模拟这些新特性。

- **说说你对vue的错误处理的了解？**

   分为errorCaptured与errorHandler。
   errorCaptured是组件内部钩子，可捕捉本组件与子孙组件抛出的错误，接收error、vm、info三个参数，return false后可以阻止错误继续向上抛出。
   errorHandler为全局钩子，使用Vue.config.errorHandler配置，接收参数与errorCaptured一致，2.6后可捕捉v-on与promise链的错误，可用于统一错误处理与错误兜底。

- **在vue事件中传入$event，使用e.target和e.currentTarget有什么区别？**

   currentTarget 始终是监听事件者，而 target 是事件的真正发出者

- **在.vue文件中style是必须的吗？那script是必须的吗？为什么？**

   都不是必须的
   如果是普通组件那么只能是一个静态html
   如果是函数式组件， 那么可以直接使用props等函数式组件属性

- **vue怎么实现强制刷新组件？**

   this.$forceUpdate() 强制重新渲染

   强制重新刷新某组件

   ```
   //模版上绑定key
   <SomeComponent :key="theKey"/>
   //选项里绑定data
   data(){
     return{
         theKey:0
     }
   }
   //刷新key达到刷新组件的目的
   theKey++;
   ```

   v-if

- **vue自定义事件中父组件怎么接收子组件的多个参数？**

   this.$emit("eventName",data)
   data为一个对象

- **实际工作中，你总结的vue最佳实践有哪些？**

- **vue给组件绑定自定义事件无效怎么解决？**

   两种方式
   1、组件外部加修饰符.navtive
   2、组件内部声明$emit('自定义事件')

- **vue的属性名称与method的方法名称一样时会发生什么问题？**

   报错 `"Method 'xxx' has already been defined as a data property"`

   键名优先级：props > data > methods

- **vue变量名如果以_、$开头的属性会发生什么问题？怎么访问到它们的值？**

   以 _ 或 $ 开头的属性不会被 Vue 实例代理，因为可能和 Vue 内置的属性、API 方法冲突。可以使用例如 vm.$data._property

- **vue使用v-for遍历对象时，是按什么顺序遍历的？如何保证顺序？**

   在遍历对象时，会按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下都一致。

- **vue如果想扩展某个现有的组件时，怎么做呢？**

   不对原组件进行更改的：

   1. 使用Vue.extend直接扩展

   2. 使用Vue.mixin全局混入

   3. HOC封装

      可以加slot扩展

- **说下attrs和*attrs*和listeners的使用场景**

   组件传值的时候会用到 爷爷在父亲组件传递值，父亲组件会通过$attrs获取到不在父亲props里面的所有属性，父亲组件通过在孙子组件上绑定$attrs 和 $listeners 使孙组件获取爷爷传递的值并且可以调用在爷爷那里定义的方法

- **分析下vue项目本地开发完成后部署到服务器后报404是什么原因呢？**

   1.检查nginx配置，是否正确设置了资源映射条件；
   2.检查vue.config.js中是否配置了publicPath，若有则检查是否和项目资源文件在服务器摆放位置一致。

- **v-once的使用场景有哪些？**

   单次触发的场景

   表单提交。可防止用户在请求未及时响应时，多次提交~

- **说说你对vue的表单修饰符.lazy的理解**

   input标签v-model用lazy修饰之后，vue并不会立即监听input Value的改变，会在input失去焦点之后，才会触发input Value的改变

- **vue为什么要求组件模板只能有一个根元素？**

   

- **EventBus注册在全局上时，路由切换时会重复触发事件，如何解决呢？**

   在组件内的beforeRouteLeave中移除事件监听

   建议在created里注册，在beforeDestory移出

- **怎么修改vue打包后生成文件路径？**

   webpack：output.path
   vue-cli3: outputDir

- **你有使用做过vue与原生app交互吗？说说vue与ap交互的方法**

   app定义一个方法传给我们，根据方法调用

   jsBridge,建立连接，然后相互调用

- **使用vue写一个tab切换**

   v-for循环list，根据索引值设置active的样式和显示内容

   ```
   `<div id="app">
       <ul class="tabs">
           <li class="li-tab" v-for="(item,index) in tabsParam" 
           @click="toggleTabs(index)" 
           :class="index===nowIndex?'active':''">{{item}}</li>
       </ul>
       <div class="divTab" v-show="nowIndex===0">我是tab1</div>
       <div class="divTab" v-show="nowIndex===1">我是tab2</div>
       <div class="divTab" v-show="nowIndex===2">我是tab3</div>
       <div class="divTab" v-show="nowIndex===3">我是tab4</div>
   </div>`
   ```

- **vue中什么是递归组件？举个例子说明下？**

   当前注册一个vue组件定义 name 为 'node-tree' ，在组件 template 内部调用 实现递归。

   用过组件的name属性，调用自身。例如生成树型菜单。

- **怎么访问到子组件的实例或者子元素？**

   this.$children/this.$refs.xxx

- **在子组件中怎么访问到父组件的实例？**

   

- **在组件中怎么访问到根实例？**

   ```
   this.$root
   ```

- **说说你对Object.defineProperty的理解**

   Object.defineProperty定义新属性或修改原有的属性；
   vue的数据双向绑定的原理就是用的Object.defineProperty这个方法，里面定义了setter和getter方法，通过观察者模式（发布订阅模式）来监听数据的变化，从而做相应的逻辑处理。

- **vue组件里写的原生addEventListeners监听事件，要手动去销毁吗？为什么？**

   肯定要，一方面是绑定多次，另一方面是函数没释放会内存溢出

- **vue组件里的定时器要怎么销毁？**

   当生命周期销毁后，并没有将组件中的计时器销毁，虽然页面上看不出来，但是如果在控制台打印的话，会发现计时器还在运行，所以要销毁计时器，避免代码一直执行

- **vue组件会在什么时候下被销毁？**

   v-if=‘false‘

   没有使用keep-alive时的路由切换。

- **使用vue渲染大量数据时应该怎么优化？说下你的思路！**

   1.如果需要响应式，考虑使用虚表（只渲染要显示的数据）；
   2.如果不考虑响应式，变量在beforeCreated或created中声明（Object.freeze会导致列表无法增加数据）

- **在vue中使用this应该注意哪些问题？**

   vue中使用匿名函数，会出现this指针改变。
   解决方法
   1.使用箭头函数
   2.定义变量绑定this至vue对象

- **你有使用过JSX吗？说说你对JSX的理解**

   jsx不是一门新的语言，是一种新的语法糖。让我们在js中可以编写像html一样的代码。
   允许XML语法直接加入到JavaScript代码中，让你能够高效的通过代码而不是模板来定义界面

- **说说组件的命名规范**

   1.kebab-case（短横线分隔命名），引用时必须也采用kebab-case；
   2.PascalCase（首字母大写命名），引用时既可以采用PascalCase也可以使用kebab-case；
   但在DOM中使用只有kebab-case是有效的

- **怎么配置使vue2.0+支持TypeScript写法？**

   使用
   vuejs/vue-class-component

   - 配置ts-loader，tsconfig
   - 增加类型扩展，让ts识别vue文件
   - vue文件中script里面换成ts写法， 需要增加几个ts扩展的package， 比如vue-property-decorator

- **`<template></template>`有什么用？**

   当做一个不可见的包裹元素，减少不必要的DOM元素，整个结构会更加清晰。

- **vue的is这个特性你有用过吗？主要用在哪些方面？**

   动态组件，当你多个组件需要通过 v-if 切换时，可以使用 is 来简化代码

   vue中is的属性引入是为了解决dom结构中对放入html的元素有限制的问题

- **vue的:class和:style有几种表示方式？**

   :class 绑定变量 绑定对象 绑定一个数组 绑定三元表达式
   :style 绑定变量 绑定对象 绑定函数返回值 绑定三元表达式

- **你了解什么是函数式组件吗？**

   需要提供一个render方法， 接受一个参数（createElement函数）， 方法内根据业务逻辑，通过createElement创建vnodes，最后return vnodes

- **vue怎么改变插入模板的分隔符？**

   delimiters ?????

- **组件中写name选项有什么作用？**

   如果用过devTool会有详细感受
   如果你使用keep-alive, component等内置组件时
   如果你使用循环组件时遇到了bug, 你就会知道为什么了

   项目使用keep-alive时，可搭配组件name进行缓存过滤
   DOM做递归组件时需要调用自身name
   vue-devtools调试工具里显示的组见名称是由vue中组件name决定的

- **说说你对provide和inject的理解**

   通过在父组件中inject一些数据然后再所有子组件中都可以通过provide获取使用该参数,

   主要是为了解决一些循环组件比如tree, menu, list等, 传参困难, 并且难以管理的问题, 主要用于组件封装, 常见于一些ui组件库

- **开发过程中有使用过devtools吗？**

   devtools确实是个好东西，大力协助vue项目开发，传参，数据展示，用于调试vue应用

- **说说你对slot的理解有多少？slot使用场景有哪些？**

   slot, 插槽, 在使用组件的时候, 在组建内部插入东西.
   组件封装的时候最常使用到

- **你有使用过动态组件吗？说说你对它的理解**

   

- **prop验证的type类型有哪几种？**

   Number, String, Boolean, Array, Function, Object

   ```
   //七种
   props:{
   	title:String,
   	likes: Number,
   	isPublished: Boolean,
   	commentIds: Array,
   	author: Object,
   	callback: Function,
   	contactsPromise: Promise
   }
   ```

- **prop是怎么做验证的？可以设置默认值吗？**

   单个类型就用Number等基础类型，多个类型用数组，必填的话设置require为true，默认值的话设置default，对象和数组设置默认用工厂函数，自定义验证函数validator。

- **怎么缓存当前打开的路由组件，缓存后想更新当前组件怎么办呢？**

   可以在路由meta中加入参数, 对打开的路由进行keep-alive的判断, 通过钩子active等

- **说说你对vue组件的设计原则的理解**

   第一: 容错处理, 这个要做好, 极端场景要考虑到, 不能我传错了一个参数你就原地爆炸
   第二: 缺省值(默认值)要有, 一般把应用较多的设为缺省值
   第三: 颗粒化, 把组件拆分出来.
   第四: 一切皆可配置, 如有必要, 组件里面使用中文标点符号, 还是英文的标点符号, 都要考虑到
   第五: 场景化, 如一个dialog弹出, 还需要根据不同的状态封装成success, waring, 等
   第六: 有详细的文档/注释和变更历史, 能查到来龙去脉, 新版本加了什么功能是因为什么
   第七: 组件名称, 参数prop, emit, 名称设计要通俗易懂, 最好能做到代码即注释这种程度
   第八: 可拓展性, 前期可能不需要这个功能, 但是后期可能会用上, 要预留什么, 要注意什么, 心里要有逼数
   第九: 规范化,我这个input组件, 叫`on-change`, 我另外一个select组件叫`change`, 信不信老子捶死你
   第十: 分阶段: 不是什么都要一期开发完成看具体业务, 如果一个select, 我只是个简单的select功能, 什么`multi`老子这个版本压根不需要, 别TM瞎折腾! 给自己加戏

- **你了解vue的diff算法吗？**

   

- **vue如何优化首页的加载速度？**

   异步路由和异步加载
   还有分屏加载, 按需加载, 延时加载图片等, cdn, 域名才分

- **vue打包成最终的文件有哪些？**

   vendor.js, app.js, app.css,
   1.xxx.js
   2.xxx.js

   如果有设置到单独提取css的话
   还有
   1.xxx.css
   ......

- **ajax、fetch、axios这三都有什么区别？**

   ajax是概念 异步交换数据的概念
   fetch是浏览器提供的webAPI 原理是基于xmlHttpRequest的封装
   axios是第三方库 基于xmlHttpRequest的封装 使用更便捷

- **vue能监听到数组变化的方法有哪些？为什么这些方法能监听到呢？**

   对中转的数据做了监听

- **vue中是如何使用event对象的？**

   [@click](https://github.com/click)=“func” 默认第一个参数传入event对象
   [@click](https://github.com/click)="func(0, $event)" 如果自己需要传入参数和event对象，则需要使用$event来获取event对象并传入func

- **vue首页白屏是什么问题引起的？如何解决呢？**

   路由没配baseUrl

   在config文件夹中找到index.js打开把
   assetsPublicPath: '/'
   改成
   assetsPublicPath: './'
   再次执行 npm run build 就可以了。

- **说说你对单向数据流和双向数据流的理解**

   单向数据流：所有状态的改变可记录、可跟踪，源头易追溯；所有数据只有一份，组件数据只有唯一的入口和出口，使得程序更直观更容易理解，有利于应用的可维护性；一旦数据变化，就去更新页面(data-页面)，但是没有(页面-data)；如果用户在页面上做了变动，那么就手动收集起来(双向是自动)，合并到原有的数据中。
   双向数据流：无论数据改变，或是用户操作，都能带来互相的变动，自动更新。

- **移动端ui你用的是哪个ui库？有遇到过什么问题吗？**

- **你知道nextTick的原理吗？**

   在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

- **说说你对v-clock和v-pre指令的理解**

   v-cloak指令只是在标签中加入一个v-cloak自定义属性，在HTML还编译完成之后该属性会被删除。
   v-pre可以用来阻止预编译，有v-pre指令的标签内部的内容不会被编译，会原样输出。

- **写出你知道的表单修饰符和事件修饰符**

   事件修饰符.stop .prevent .capture .self .once .passive
   表单修饰符.number .lazy .trim

- **说说你对proxy的理解**

   vue的数据劫持有两个缺点:
   1、无法监听通过索引修改数组的值的变化
   2、无法监听object也就是对象的值的变化
   所以vue2.x中才会有$set属性的存在

   proxy是es6中推出的新api，可以弥补以上两个缺点，所以vue3.x版本用proxy替换object.defineproperty

- **你有自己用vue写过UI组件库吗？**

   这个……全局的theme属性然后做class判断或者加载不同的样式文件。一种是编译时换肤 一种是用户操作换肤。编译时换肤可以通过css in js相关技术修改css预处理器的变量 。用户操作换肤 只能内置一些styleb变量供用户选择了

- **用vue怎么实现一个换肤的功能？**

   

- **有在vue中使用过echarts吗？踩过哪些坑？如何解决的？**

- **如果让你教一个2-3年经验前端经验的同事使用vue，你该怎么教？**

- **vue性能的优化的方法有哪些？**

- **SSR解决了什么问题？有做过SSR吗？你是怎么做的？**

   SSR服务端渲染，解决的SEO问题，首屏加载速度

- **说说你觉得认为的vue开发规范有哪些？**

   

- **vue部署上线前需要做哪些准备工作？**

   主要assetsPublicPath、publicPath 两个

- **vue过渡动画实现的方式有哪些？**

   1.使用vue的transition标签结合css样式完成动画
   2.利用animate.css结合transition实现动画
   3.利用 vue中的钩子函数实现动画

- **vue在created和mounted这两个生命周期中请求数据有什么区别呢？**

   1. created阶段的ajax请求与mounted请求的区别：前者页面视图未出现，如果请求信息过多，页面会长时间处于白屏状态
   2. 在created的时候，视图中的html并没有渲染出来，所以此时如果直接去操作html的dom节点，一定找不到相关的元素
      而在mounted中，由于此时html已经渲染出来了，所以可以直接操作dom节点，（此时document.getelementById 即可生效了）。

- **vue父子组件双向绑定的方法有哪些？**

   1.利用对象的引用关系来实现
   2.父子组件之间的数据传递
   3.使用.sync修饰符

- **vue怎么获取DOM节点？**

   view：v-ref:xxx
   data：this.$ref.xxx 这样就ok啦

- **vue项目有做过单元测试吗？**

   vue+vuex+vue-router+ts+vue-cli demo 实现todolist 并进行单元测试
   vue vuex 与ts的结合部分还不是很友好

- **vue项目有使用过npm run build --report吗？**

   给 process.env 对象添加了一个属性 npm_config_report: "true"，表示开启编译完成后的报告。

- **如何解决vue打包vendor过大的问题？**

   1、在webpack.base.conf.js新增externals配置，表示不需要打包的文件，然后在index.html中通过CDN引入

   ```
   externals: {
       "vue": "Vue",
       "vue-router": "VueRouter",
       "vuex": "Vuex",
       "element-ui": "ELEMENT",
       "BMap": "BMap"
     }
   ```

   2、使用路由懒加载

- **webpack打包vue速度太慢怎么办？**

   升级webpack4,支持多进程

- **vue在开发过程中要同时跟N个不同的后端人员联调接口（请求的url不一样）时你该怎么办？**

   webpack 的devServer配置下代理

- **vue要做权限管理该怎么做？如果控制到按钮级别的权限怎么做？**

   通过获取当前用户的权限去比对路由表，生成当前用户具的权限可访问的路由表，通过 `router.addRoutes` 动态挂载到 `router` 上。

   你可以在后台通过一个 tree 控件或者其它展现形式给每一个页面动态配置权限，之后将这份路由表存储到后端。当用户登录后得到 `roles`，前端根据`roles` 去向后端请求可访问的路由表，从而动态生成可访问页面，之后就是 router.addRoutes 动态挂载到 router 上，你会发现原来是相同的

- **说下你的vue项目的目录结构，如果是大型项目你该怎么划分结构和划分组件呢？**

- **在移动端使用vue，你觉得最佳实践有哪些？**

- **你们项目为什么会选vue而不选择其它的框架呢？**

   Vue.js是一个轻巧，高性能，可组件化的MVVM库，同时拥有非常容易上手的API; VUE是单页面应用，使页面局部刷新，不用每次跳转页面都要请求所有数据和DOM，这样大大加快了访问速度和提升用户体验。而且他的第三方UI库很多节省开发时间。

- **对于即将到来的vue3.0特性你有什么了解的吗？**

- **vue开发过程中你有使用什么辅助工具吗？**

- **vue和微信小程序写法上有什么区别？**

   [https://www.jianshu.com/p/d4a053599572](https://www.jianshu.com/p/d4a053599572)

- **怎么缓存当前的组件？缓存后怎么更新？**

   

- **你了解什么是高阶组件吗？可否举个例子说明下？**

- **为什么我们写组件的时候可以写在.vue里呢？可以是别的文件名后缀吗？**

- **vue-loader是什么？它有什么作用？**

- **说说你对vue的extend（构造器）的理解，它主要是用来做什么的？**

- **如果将axios异步请求同步化处理？**

- **怎么捕获组件vue的错误信息？**

- **为什么vue使用异步更新组件？**

- **如何实现一个虚拟DOM？说说你的思路**

- **写出多种定义组件模板的方法**

- **SPA单页面的实现方式有哪些？**

- **说说你对SPA单页面的理解，它的优缺点分别是什么？**

- **说说你都用vue做过哪些类型的项目？**

- **在vue项目中如何引入第三方库（比如jQuery）？有哪些方法可以做到？**

- **使用vue手写一个过滤器**

- **你有使用过render函数吗？有什么好处？**

   template也会翻译成render，只有一点，template中元素的tag_name是静态的，不可变化，使用createEelment可以生成不同tag_name, 比如h1 ... h6, 可以通过一个number变量控制

- **写出你常用的指令有哪些？**

- **手写一个自定义指令及写出如何调用**

- **组件进来请求接口时你是放在哪个生命周期？为什么？**

- **你有用过事件总线(EventBus)吗？说说你的理解**

- **说说vue的优缺点分别是什么？**

- **DOM渲染在哪个周期中就已经完成了？**

   mounted生命周期

- **第一次加载页面时会触发哪几个钩子？**

- **vue生命周期总共有几个阶段？**

- **vue生命周期的作用是什么？**

- **vue和angular有什么区别呢？**

- **如何引入scss？引入后如何使用？**

   安装scss依赖包：
   `npm install sass-loader --save-dev npm install node-sass --save-dev`
   在build文件夹下修改 webpack.base.conf.js 文件：
   在 module 下的 rules 里添加配置，如下：
   `{ test: /\.scss$/, loaders: ['style', 'css', 'sass'] }`
   应用：
   在vue文件中应用scss时，需要在style样式标签上添加lang="scss"，即`<style lang="scss">。`

- **使用vue开发过程你是怎么做接口管理的？**

   在request.js中对axios请求和响应进行劫持，统一处理，然后在api文件夹中引入request.js后再使用封装后的方法进行请求

- **为何官方推荐使用axios而不用vue-resource？**

- **你了解axios的原理吗？有看过它的源码吗？**

- **你有封装过axios吗？主要是封装哪方面的？**

- **如何中断axios的请求？**

- **axios是什么？怎样使用它？怎么解决跨域的问题？**

- **说说你对vue的template编译的理解？**

- **v-on可以绑定多个方法吗？**

- **vue常用的修饰符有哪些？列举并说明**

- **你认为vue的核心是什么？**

- **v-model是什么？有什么用呢？**

- **说说你对vue的mixin的理解，有什么应用场景？**

- **SPA首屏加载速度慢的怎么解决？**

- **删除数组用delete和Vue.delete有什么区别？**

- **动态给vue的data添加一个新的属性时会发生什么？怎样解决？**

- **组件和插件有什么区别？**

- **说说你使用vue过程中遇到的问题（坑）有哪些，你是怎么解决的？**

- **说说你对选项el,template,render的理解**

- **vue实例挂载的过程是什么？**

- **vue在组件中引入插件的方法有哪些？**

- **v-if和v-for的优先级是什么？如果这两个同时出现时，那应该怎么优化才能得到更好的性能？**

- **分别说说vue能监听到数组或对象变化的场景，还有哪些场景是监听不到的？无法监听时有什么解决方案？**

- **$nextTick有什么作用？**

- **为什么data属性必须声明为返回一个初始数据对应的函数呢？**

- **怎么在watch监听开始之后立即被调用？**

- **watch怎么深度监听对象变化？**

- **watch和计算属性有什么区别？**

- **vue如何监听键盘事件？**

- **v-for循环中key有什么作用？**

- **怎么在vue中使用插件？**

- **你有写过自定义组件吗？**

- **说说你对keep-alive的理解是什么？**

- **怎么使css样式只在当前组件中生效？**

- **你有看过vue的源码吗？如果有那就说说看**

- **你有写过自定义指令吗？自定义指令的生命周期（钩子函数）有哪些？**

- **v-show和v-if有什么区别？使用场景分别是什么？**

- **说说你对MVC、MVP、MVVM模式的理解**

- **说下你对指令的理解？**

- **请描述下vue的生命周期是什么？**

- **vue组件之间的通信都有哪些？**

- **什么是虚拟DOM？**

- **什么是双向绑定？原理是什么？**

- **vue和react有什么不同？使用场景是什么？**

- **说说vue的优缺点**

- **有使用过vue吗？说说你对vue的理解**



### vue-cli

- vue-cli提供了的哪几种脚手架模板？
- vue-cli工程中常用的npm命令有哪些？
- 在使用vue-cli开发vue项目时，自动刷新页面的原理你了解吗？
- vue-cli3插件有写过吗？怎么写一个代码生成插件？
- vue-cli生成的项目可以使用es6、es7的语法吗？为什么？
- vue-cli怎么解决跨域的问题？
- vue-cli中你经常的加载器有哪些？
- 你知道什么是脚手架吗？
- 说下你了解的vue-cli原理？你可以自己实现个类vue-cli吗？
- 怎么使用vue-cli3创建一个项目？
- vue-cli3你有使用过吗？它和2.x版本有什么区别？
- vue-cli默认是单页面的，那要弄成多页面该怎么办呢？
- 不用vue-cli，你自己有搭建过vue的开发环境吗？流程是什么？



### vue-router

- vue-router怎么重定向页面？
- vue-router怎么配置404页面？
- 切换路由时，需要保存草稿的功能，怎么实现呢？
- vue-router路由有几种模式？说说它们的区别？
- vue-router有哪几种导航钩子（ 导航守卫 ）？
- 说说你对router-link的了解
- vue-router如何响应路由参数的变化？
- 你有看过vue-router的源码吗？说说看
- 切换到新路由时，页面要滚动到顶部或保持原先的滚动位置怎么做呢？
- 在什么场景下会用到嵌套路由？
- 如何获取路由传过来的参数？
- 说说active-class是哪个组件的属性？
- 在vue组件中怎么获取到当前的路由信息？
- vur-router怎么重定向？
- 怎样动态加载路由？
- 怎么实现路由懒加载呢？
- 如果让你从零开始写一个vue路由，说说你的思路
- 说说vue-router完整的导航解析流程是什么？
- 路由之间是怎么跳转的？有哪些方式？
- 如果vue-router使用history模式，部署时要注意什么？
- route和router有什么区别？
- vue-router钩子函数有哪些？都有哪些参数？
- vue-router是用来做什么的？它有哪些组件？



### vuex

- 你有写过vuex中store的插件吗？
- 你有使用过vuex的module吗？主要是在什么场景下使用？
- vuex中actions和mutations有什么区别？
- vuex使用actions时不支持多参数传递怎么办？
- 你觉得vuex有什么缺点？
- 你觉得要是不用vuex的话会带来哪些问题？
- vuex怎么知道state是通过mutation修改还是外部直接修改的？
- 请求数据是写在组件的methods中还是在vuex的action中？
- 怎么监听vuex数据的变化？
- vuex的action和mutation的特性是什么？有什么区别？
- 页面刷新后vuex的state数据丢失怎么解决？
- vuex的state、getter、mutation、action、module特性分别是什么？
- vuex的store有几个属性值？分别讲讲它们的作用是什么？
- 你理解的vuex是什么呢？哪些场景会用到？不用会有问题吗？有哪些特性？
- 使用vuex的优势是什么？
- 有用过vuex吗？它主要解决的是什么问题？推荐在哪些场景用？



### ElementUI

- ElementUI是怎么做表单验证的？在循环里对每个input验证怎么做呢？
- 你有二次封装过ElementUI组件吗？
- ElementUI怎么修改组件的默认样式？
- ElementUI的穿梭组件如果数据量大会变卡怎么解决不卡的问题呢？
- ElementUI表格组件如何实现动态表头？
- ElementUI使用表格组件时有遇到过问题吗？
- 有阅读过ElementUI的源码吗？
- 项目中有使用过ElementUI吗？有遇到过哪些问题？它的使用场景主要是哪些？
- 有用过哪些vue的ui？说说它们的优缺点？



### mint-ui

- mint-ui使用过程中有没有遇到什么坑？怎么解决的？
- 说出几个mint-ui常用的组件
- mint-ui是什么？你有使用过吗？