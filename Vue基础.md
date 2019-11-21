Vue基础

#### 前端框架与库的区别?

- jquery 库 -> DOM(操作DOM) + 请求
- art-template 库 -> 模板引擎
- 框架 = 全方位功能齐全

#### 插值表达式 

- {{ 表达式 }}
  - 对象 (不要连续3个{{ {name:'jack'} }})
  - 字符串 {{ 'xxx' }}
  - 判断后的布尔值  {{  true }}
  - 三元表达式  {{ true?'是正确':'错误'  }}
- 可以用于页面中简单粗暴的调试

- 注意: 必须在data这个函数中返回的对象中声明

#### vue中常用的v-指令演示

- v-text:元素的InnerText属性,必须是双标签 跟{{ }}效果是一样的 使用较少
- v-html: 元素的innerHTML
- v-if : 判断是否插入这个元素,相当于对元素的销毁和创建
- v-else-if
- v-else
- v-show 隐藏元素  如果确定要隐藏,   会给元素的style加上display:none。是基于css样式的切换

```javascript
 v-text 只能用在双标签中
 v-text 其实就是给元素的innerText赋值
 v-html 其实就是给元素的innerHTML赋值
 v-if 如果值为false,会留下一个<!---->作为标记，万一未来v-if的值是true了，就在这里插入元素
 如果有if和else就不需要单独留坑了
 如果全用上  v-if 相邻v-else-if 相邻 v-else 否则 v-else-if可以不用
 v-if和v-else-if都有等于对应的值，而v-else直接写
 v-if家族都是对元素进行插入和移除的操作
 v-show是显示与否的问题
 注意: 指令其实就是利用属性作为标识符,简化DOM操作,
  看：v-model="xxx"
  v-model 代表要做什么  xxx代表针对的js内存对象
  写在那个元素上，就对哪个元素操作

```

#### v-if和v-show的区别 (官网解释)

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

#### v-bind使用

- 给元素的属性赋值  <div id="{{变量}}"></div>

  - 可以给已经存在的属性赋值 input value
  - 也可以给自定义属性赋值 mydata

- 语法 在元素上 `v-bind:属性名="常量||变量名"`

- 简写形式 `:属性名="变量名"`

- ```html
  <div v-bind:原属性名="变量"></div>
  <div :属性名="变量">
      
  </div>
  ```

#### v-on的使用

- 处理自定义原生事件的,给按钮添加click并让使用变量的样式改变
- 普通使用   ```v-on:事件名="表达式||函数名"```
- 简写方式  ``` @事件名="表达式"```

#### v-model

- 双向数据流（绑定）
  - 页面改变影响内存(js)
  - 内存(js)改变影响页面

原生input其实只是一个语法糖，:bind="value"与[@change](https://github.com/change)="value = $event.target.value"的结合。

自定义组件的时候的v-model默认监听change事件和绑定value 的prop。

#### v-bind 和 v-model 的区别?

- `input v-model="name"`
  - 双向数据绑定  页面对于input的value改变，能影响内存中name变量
  - 内存js改变name的值，会影响页面重新渲染最新值
- `input :value="name"`
  - 单向数据绑定 内存改变影响页面改变
- v-model: 其的改变影响其他  v-bind: 其的改变不影响其他
- v-bind就是对属性的简单赋值,当内存中值改变，还是会触发重新渲染

#### v-for的使用

- 基本语法 `v-for="item in arr"`
- 对象的操作 `v-for="item in obj"`
- 如果是数组没有id
  - `v-for="(item,index) in arr" :class="index" `
- 各中v-for的属性顺序(了解)
  - 数组 item,index
  - 对象 value,key,index

#### 关于对象内的this

- vue已经把以前this是window或者事件对象的问题搞定了
- methods和data本身是在同一个对象中的,所以在该对象中可以通过this.随意取
- this.xxx 取data中的值, this.xxxMethod调methods中的函数

#### 局部组件的使用

​	渲染组件-父使用子组件

- 1: 创建子组件(对象)
  - `var Header = { template:'模板' , data是一个函数,methods:功能,components:子组件们 } `
- 2: 在父组件中声明,根属性`components:{ 组件名:组件对象 }`
- 3: 在父组件要用的地方使用 `<组件名></组件名>`
  - 在不同框架中,有的不支持大写字母，用的时候
    - 组件名 MyHeader
    - 使用 my-header
- 总结: 有父，声子，挂子，用子

#### 总结父传子

* 父用子     先声子,挂子,用子
* 父传子     父传子(属性),子声明(收),子直接用(就是自己的一样)



#### 注册全局组件

* 应用场景: 多出使用的公共性功能组件，就可以注册成全局组件,减少冗余代码

* 全局API `Vue.component('组件名',组件对象);

```
	<script type="text/javascript">


		// 全局组件

		// 第一个参数是组件的名字，第二个参数是options

		// 1.在父组件中  先绑定  :自定义的属性名 = posts
		// 2.子要声明 props:['自定义的属性名']  来接收
		// 3.收到了就是自己 你可以任意使用




		// bootstrap  下拉菜单  html+css+js
		Vue.component('Vbtn',{
			template:`
				<button>按钮<[表情]tton>
			`
		});

		var Vcontent = {
	       	template:`
				<div class='content'>我是内容组件
					<ul>
						<li v-for = '(item,index) in posts'>
							<h3>{{item.title}}</h3>
							<h4>{{item.content}}</h4>
						<[表情]>

					</ul>
				</div>
			`,
		props:['posts']
	       }

	       var Vaside = {
	       	template:`
				<div class='aside'>我是侧边栏组件
					
				</div>
			`
	       };


		// 局部组件：声子  挂子 用子
		var Vheader  = {
			template:`
				<div class='head'>
					我是头部组件
					
				</div>
			`
		};


		// 1.声明局部组件 App入口组件
		var App  = {
			template:`
				<div class='main'>
					<Vheader />
					<div class = 'main2'>
						<Vaside />
						<Vcontent  :posts = 'posts'/>
					</div>
				</div>
			`,
			data(){
				return {
					posts:[
					{id:1,title:"我的第一篇博客",content:'天王该帝王'},
					{id:2,title:"我的第二篇博客",content:'小鸡炖蘑菇'},
					{id:3,title:"我的第三篇博客",content:'宝塔镇河妖'}


					]

				}
			},
			components:{
				Vheader,
				Vaside,
				Vcontent
			}
		};

		new Vue({
			el:'#app',
			// 3.使用
			template:'<App></App>',
			data(){
				return {

				}
			},
			// 2.挂载组件
			components:{
				App
			}
		});

	</script>
```

#### 子传父

- 1.子要绑定原生事件，在原生事件函数中通过this.$emit(‘自定义的事件名’,arg1);触发父组件中子组件自定义的事件名
- 2.父组件中的子组件```v-bind:自定义事件的名字 = 'fn'```绑定自定义的事件

- 3.父组件 就可以触发fn的函数 数据就可以从子组件中传过来了

```
	<script type="text/javascript">


		// 全局组件

		// 第一个参数是组件的名字，第二个参数是options

		// 1.在父组件中  先绑定  :自定义的属性名 = posts
		// 2.子要声明 props:['自定义的属性名']  来接收
		// 3.收到了就是自己 你可以任意使用




		// bootstrap  下拉菜单  html+css+js
		Vue.component('Vbtn',{
			template:`
				<button>按钮<[表情]tton>
			`
		});

		var Vcontent = {
	       	template:`
				<div class='content'>我是内容组件
					<ul>
						<li v-for = '(item,index) in posts'>
							<h3>{{item.title}}</h3>
							<h4>{{item.content}}</h4>
						<[表情]>

					</ul>
				</div>
			`,
		props:['posts']
	       }

	       var Vaside = {
	       	template:`
				<div class='aside'>我是侧边栏组件
					
				</div>
			`
	       };


		// 局部组件：声子  挂子 用子
		var Vheader  = {
			template:`
				<div class='head'>
					我是头部组件
					<button @click = 'changeFontSize'>字体变大<[表情]tton>
					
				</div>
			`,
			methods:{
				changeFontSize(){

					// 触发父组件 中声明的自定义事件   vue $emit()
					// 第一个参数是触发自定义事件的名字 第二个参数就是传进去的值
					this.$emit('change')
				}
			}
		};


		// 1.声明局部组件 App入口组件
		var App  = {
			template:`
				<div class='main' :style = '{fontSize:postFontSize+"em"}'>
					<Vheader @change = 'changeHandler'/>
					<div class = 'main2'>
						<Vaside />
						<Vcontent  :posts = 'posts'/>
					</div>
				</div>
			`,
			methods:{
				changeHandler(){
					this.postFontSize+=.1;
				}
			},
			data(){
				return {
					posts:[
					{id:1,title:"我的第一篇博客",content:'天王该帝王'},
					{id:2,title:"我的第二篇博客",content:'小鸡炖蘑菇'},
					{id:3,title:"我的第三篇博客",content:'宝塔镇河妖'}


					],
					postFontSize:1

				}
			},
			components:{
				Vheader,
				Vaside,
				Vcontent
			}
		};

		new Vue({
			el:'#app',
			// 3.使用
			template:'<App></App>',
			data(){
				return {

				}
			},
			// 2.挂载组件
			components:{
				App
			}
		});

	</script>
```

####  注册全局组件

1.应用场景: 多出使用的公共性功能组件，就可以注册成全局组件,减少冗余代码
2.全局API `Vue.component('组件名',组件对象);`

#### 具名插槽

```
	<script type="text/javascript">

		Vue.component('mySlot',{
			template:`
				<li>
					预留的第一个坑
					<slot name = 'two'></slot>
					预留的第二个坑
					<slot name = 'one'></slot>
				<[表情]>

			`
		});
		
		var App = {
			template:`
				<div>
					<ul>
						<mySlot>
							<h2 slot = 'one'>我是第一个坑</h2>
							<h3 slot = 'two'>我是第二个坑</h3>
						</mySlot>
						

					</ul>
				</div>
			`
		}

		new Vue({
			el:'#app',
			components:{
				App
			},
			template:'<App />'
		});
	</script>
```



#### `$emit和$on`进行组件之间的传值

> 注意：$emit和$on的事件必须在一个公共的实例上，才能够触发



需求：

​	1.有A，B，C三个组件，同时挂载到入口组件中

​	2.将A组件中的数据传递到C组件，再将B组件中的数据传递到C组件

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Vue2-单一事件管理组件通信</title>

  
</head>
<body>
  <div id="app">
    <dom-a></dom-a>
    <dom-b></dom-b>
    <dom-c></dom-c>   
  </div>
  <script src="vue.js"></script>
  <script>
    

  //准备一个空的实例对象
  var Event = new Vue();
  console.log(Event);
 
  //组件A
  var A = {
    template: `
      <div>
        <span>我是A组件的数据->{{a}}</span>
        <input type="button" value="把A数据传给C" @click = "send">
      </div>
    `,
    methods: {
      send () {
        alert(1);
        console.log(this);
        Event.$emit("a-msg", this.a);
      }
    },
    data () {
      return {
        a: "我是a组件中数据"
      }
    }
  };
  //组件B
  var B = {
    template: `
      <div>
        <span>我是B组件的数据->{{a}}</span>
        <input type="button" value="把B数据传给C" @click = "send">
      </div>
    `,
    methods: {
      send () {
        Event.$emit("b-msg", this.a);
      }
    },
    data () {
      return {
        a: "我是b组件中数据"
      }
    }
  };
  //组件C
  var C = {
    template: `
      <div>
        <h3>我是C组件</h3>
        <span>接收过来A的数据为: {{a}}</span>
        <br>
        <span>接收过来B的数据为: {{b}}</span>
      </div>
    `,
    mounted () {
      alert(2);
      //接收A组件的数据
      Event.$on("a-msg", (a)=> {
        this.a = a;
      });
 
      //接收B组件的数据
      Event.$on("b-msg",  (b)=> {
        this.b = b;
      });
    },
    data () {
      return {
        a: "",
        b: ""
      }
    }
  };

    new Vue({
      el: "#app",
      components: {
         'dom-a':A,
         'dom-b':B,
         'dom-c':C
      }
    });
  </script>
 
</body>
</html>

```



#### 附加功能:过滤器&监视改动

* filters

  * 将数据进行添油加醋的操作

  * 过滤器分为两种

    * 1:组件内的过滤器(组件内有效)

    * 2:全局过滤器(所有组件共享)

      ##### 局部过滤器的使用

      * 先注册,后使用
      * 组件内 `filters:{ 过滤器名:过滤器fn }` 最终fn内通过return产出最终的数据
      * 使用方式是 `{{ 原有数据 | 过滤器名  }}`
      * 需求：
        * 页面input框输入钱数, 另一边显示其前面都加上¥
      
      ```
      template:`
    				<div>
      					<input type="number" name = 'price' v-model = 'price'/>
    					<h3>{{price | myCurrentcy}}</h3>
      					<h4>{{ msg  | myReverse('hehe')}}</h4>
    				</div>
      			`,
      			filters:{
      
      				myCurrentcy:function(value) {
      					return  '¥' + value;
      				}
      			}
      ```
      
      * 过滤器fn:
        * 声明`function(data,argv1,argv2...){}`
        * 使用`{{ 数据 | 过滤器名(参数1,参数2) }}`
    
    #####        全局过滤器的使用
    
    ##### 		   语法：```Vue.component('过滤器的名字',fn)```
    
    ##### 		调用：跟局部组件调用方式一样

* watch 监视单个属性

  ```javascript
  watch:{
  
                 
          		//监视复杂类型,无法监视的原因是因为监视的是对象的地址 
          		// obj:function(newV,oldV) {
          		// 	console.log(newV,oldV);
          		// },
          		// key是属于data属性的属性名，value是监视后的行为 fn中的参数（新值，旧值）
          		msg:function (newV,oldV) {
          			console.log(newV,oldV);
  
          			if (newV==='alex') {
          				console.log('sb');
          			}
          		},
                  // 深度监视 ：object ||array
          		stus:{
          			deep:true,//深度监视
          			handler:function (newV,oldV) {
          				console.log(newV[0].name)
          			}
          		}
  ```

  ##### 	小结：  基本数据类型 简单监视，复杂数据类型深度监视   

  

* computed 同时监视多个属性

  	默认computed只有getter方法





#### 生命周期

##### 定义：

 每个 Vue 实例在被创建时都要经过从创建倒挂载再到更新、卸载的一系列过程，同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，可以让我们用自己注册的js方法控制整个大局，在这些事件响应方法中的this直接指向的是vue的实例。



##### 钩子函数的执行时间

beforeCreate

在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。

created

实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

beforeMount

在挂载开始之前被调用：相关的 render 函数首次被调用。

mounted

el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。

beforeUpdate

数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。

updated

由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

beforeDestroy

实例销毁之前调用。在这一步，实例仍然完全可用。

destroyed

Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

 还有两个特殊的（配合使用keep-alive）：[activated](https://cn.vuejs.org/v2/api/#activated)、[deactivated](https://cn.vuejs.org/v2/api/#deactivated) 

activated

keep-alive组件被激活时调用

deactivated 

keep-alive组件被停用时调用

##### 钩子函数中该做的事情

created

实例已经创建完成，因为它是最早触发的原因可以进行一些数据，资源的请求。

mounted

实例已经挂载完成，可以进行一些DOM操作

beforeUpdate

可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

updated

可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。

该钩子在服务器端渲染期间不被调用。

destroyed

可以执行一些优化操作

keep-alive 

在使用vue-router时有时需要使用<keep-alive></keep-alive>来缓存组件状态，这个时候created钩子就不会被重复调用了，如果我们的子组件需要在每次加载的时候进行某些操作，可以使用activated钩子触发。 

#### 模块化

* webpack命令
  `npm init -y`
  `npm install webpack@2.2.1 --save-dev --registry https://registry.npm.taobao.org`
* package.json文件
  `"scripts": {
    "test": "webpack ./main.js ./build.js"
  },`
* 命令行运行 `npm run test`

#### ES6模块

* 导入和导出只能存在顶级作用域
* require引入是代码执行的时候才加载
* import 和export 都是提前加载 ，加载在代码执行之前

#### 箭头函数和function

* 一方面箭头函数是种简写形式
* 应用场景: 由于箭头函数本身没有this和arguments，通常用在事件类的回调函数上，让其向上级function绑定this，而非事件对象
* 箭头函数不可以作为构造函数

#### ES6函数简写

* 用在对象的属性中

```javascript
fn3() { //干掉了:function,用在对象的属性中
				console.log(this);
},
```

#### key

* 使用子组件循环输出一堆数据
* 不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。
* 建议v-for就写，提升性能，避免vue运算,key就是记录元素与dom间的位置关系

```html
<son v-for="(item,index) in persons" :key="index" ></son>
```

#### v-for中key的作用

大家要知道，不仅只是vue中，react中在执行列表渲染时也会要求给每个组件添加key这个属性

如果想知道key的作用，不得我们得聊一下虚拟DOM的Diff算法



所谓虚拟DOM的诞生，使我们可以不直接操作DOM元素，只操作数据便可以重新渲染页面。而隐藏在背后的原理便是其高效的Diff算法，它的核心是基于两个简单的假设：

1. ###### 两个相同的组件产生类似的DOM结构，不同的组件产生不同的DOM结构

2. ###### 同一个层级的一组节点，他们可以通过唯一的id进行区分



下面这张图是react的Diff示意图：

![image-20180820211848710](/var/folders/7b/sz6pt5jd3y3b51q1ngm_8pph0000gn/T/abnerworks.Typora/image-20180820211848710.png)

由此图我们可以看出：

当页面的数据发生变化时，Diff算法只会比较同一层级的节点：

###### 如果节点类型不同，直接干掉前面的节点，再创建并插入新的节点，不会再比较这个节点以后的子节点了

###### 如果节点类型相同，则会重新设置该节点的属性，从而实现节点的更新

当某一层有很多相同的界定啊时，也就是列表节点，Diff算法的更新过程默认情况下也是遵循以上原则

比如下面这个情况

![image-20180820212221368](/var/folders/7b/sz6pt5jd3y3b51q1ngm_8pph0000gn/T/abnerworks.Typora/image-20180820212221368.png)

我们希望可以在B和C之间加一个F,Diff算法默认 执行起来是这样的：

![image-20180820212311533](/var/folders/7b/sz6pt5jd3y3b51q1ngm_8pph0000gn/T/abnerworks.Typora/image-20180820212311533.png)

既把C更新成F,D更新成C,E更新成D,最后再插入E,是不是很没有效率？



所有我们***需要使用key来给每个节点做一个唯一的标识，Diff算法就可以正确的识别此节点，找到正确的位置区插入新的节点***

![image-20180820212507408](/var/folders/7b/sz6pt5jd3y3b51q1ngm_8pph0000gn/T/abnerworks.Typora/image-20180820212507408.png)

所以一句话，***key的作用主要是为了高效的更新虚拟DOM***。另外vue的在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分他们，否则vue只会替换其内部属性而不会触发过渡效果。

#### slot

* slot就是子组件里给DOM留下的坑
* <子组件>DOM</子组件>
* slot动态的DOM、props是动态的数据

#### 组件生命周期

* 需要频繁的创建和销毁组件
  - 比如页面中部分内容显示与隐藏，但是用的是v-if
* 组件缓存
  - 内置组件中<keep-alive>
  - 被其包裹的组件，在v-if=false的时候，不会销毁，而是停用
  - v-if="true" 不会创建,而是激活
  - 避免频繁创建组件对象的性能损耗
* 成对比较
  - created 和 beforeCreate 
    + A 可以操作数据 B 数据没有初始化
  - mounted 和 beforeMount
    + A 可以操作DOM B 还未生成DOM
  - updated 和 beforeUpdate
    + A 可以获取最终数据 B 可以二次修改
  - 频繁销毁创建的组件使用内置组件<keep-alive></keep-alive>包裹

```javascript
	activated(){  //激活的 keep-alive v-if="true"
		console.log('activated')
	},
	deactivated(){  //停用的 keep-alive v-if="false"
		console.log('deactivated')
	},
	beforeDestroy(){ //销毁前 v-if="false"
		console.log('beforeDestroy')
	},
	destroyed(){//销毁后 v-if="false"
		console.log('destroyed')
	},
```

#### 获取DOM元素

* 救命稻草, document.querySelector
* 1: 在template中标识元素 ref="xxxx"
* 2: 在要获取的时候, this.$refs.xxxx 获取元素
  - 创建组件,装载DOM,用户点击按钮

* ref在DOM上获取的是原生DOM对象
* ref在组件上获取的是组件对象
  - $el 是拿其DOM

- 这个对象就相当于我们平时玩的this,也可以直接调用函数

`$nextTick 是在DOM更新循环结束之后执行延迟回调，在修改数据之后使用$nextTick 可以在回调中获取到更新后的DOM`

#### vue 在 v-for 时给每项元素绑定事件需要用事件代理吗？为什么？

同时对比 3 个图中监听器的数量以及我以往阅读 vue 源码的过程中，并没有发现 vue 会自动做事件代理，但是一般给 v-for 绑定事件时，都会让节点指向同一个事件处理程序（第二种情况可以运行，但是 eslint 会警告），一定程度上比每生成一个节点都绑定一个不同的事件处理程序性能好，但是监听器的数量仍不会变，所以使用事件代理会更好一点。

#### vue双向数据绑定

手写一个数据绑定：

```
<input id="input" type="text" />

<div id="text"></div>

let input = document.getElementById("input");
let text = document.getElementById("text");
let data = { value: "" };
Object.defineProperty(data, "value", {
  set: function(val) {
    text.innerHTML = val;
    input.value = val;
  },
  get: function() {
    return input.value;
  }
});
input.onkeyup = function(e) {
  data.value = e.target.value;
};
```



#### Virtual DOM 真的比操作原生 DOM 快吗？谈谈你的想法。

这是一个性能 vs. 可维护性的取舍。框架的意义在于为你掩盖底层的 DOM 操作，让你用更声明式的方式来描述你的目的，从而让你的代码更容易维护。没有任何框架可以比纯手动的优化 DOM 操作更快，因为框架的 DOM 操作层需要应对任何上层 API 可能产生的操作，它的实现必须是普适的。



#### Vue生命周期

在谈到Vue的生命周期的时候，我们首先需要创建一个实例，也就是在 new Vue ( ) 的对象过程当中，首先执行了init（init是vue组件里面默认去执行的），在init的过程当中首先调用了beforeCreate，然后在injections（注射）和reactivity（反应性）的时候，它会再去调用created。所以在init的时候，事件已经调用了，我们在beforeCreate的时候千万不要去修改data里面赋值的数据，最早也要放在created里面去做（添加一些行为）。

当created完成之后，它会去判断instance（实例）里面是否含有“el”option（选项），如果没有的话，它会调用vm.$mount(el)这个方法，然后执行下一步；如果有的话，直接执行下一步。紧接着会判断是否含有“template”这个选项，如果有的话，它会把template解析成一个render function ，这是一个template编译的过程，结果是解析成了render函数：

```
render (h) {
  return h('div', {}, this.text)
}
```

解释一下，render函数里面的传参h就是Vue里面的createElement方法，return返回一个createElement方法，其中要传3个参数，第一个参数就是创建的div标签；第二个参数传了一个对象，对象里面可以是我们组件上面的props，或者是事件之类的东西；第三个参数就是div标签里面的内容，这里我们指向了data里面的text。

使用render函数的结果和我们之前使用template解析出来的结果是一样的。render函数是发生在beforeMount和mounted之间的，这也从侧面说明了，在beforeMount的时候，$el还只是我们在HTML里面写的节点，然后到mounted的时候，它就把渲染出来的内容挂载到了DOM节点上。这中间的过程其实是执行了render function的内容。

在使用.vue文件开发的过程当中，我们在里面写了template模板，在经过了vue-loader的处理之后，就变成了render function，最终放到了vue-loader解析过的文件里面。这样做有什么好处呢？原因是由于在解析template变成render function的过程，是一个非常耗时的过程，vue-loader帮我们处理了这些内容之后，当我们在页面上执行vue代码的时候，效率会变得更高。

beforeMount在有了render function的时候才会执行，当执行完render function之后，就会调用mounted这个钩子，在mounted挂载完毕之后，这个实例就算是走完流程了。

后续的钩子函数执行的过程都是需要外部的触发才会执行。比如说有数据的变化，会调用beforeUpdate，然后经过Virtual DOM，最后updated更新完毕。当组件被销毁的时候，它会调用beforeDestory，以及destoryed。

这就是vue实例从新建到销毁的一个完整流程，以及在这个过程中它会触发哪些生命周期的钩子函数。那说到这儿，可能很多童鞋会问，钩子函数是什么意思？

钩子函数，其实和回调是一个概念，当系统执行到某处时，检查是否有hook，有则回调。说的更直白一点，每个组件都有属性，方法和事件。所有的生命周期都归于事件，在某个时刻自动执行。

#### Vue 组件中data为什么返回的是个函数呢

因为组件是用来复用的，且在JS中的对象之间是引用关系，那如果组件中的data是一个对象，那么这样作用域没有隔离，子组件中的data属性值会相互影响，相反如果组件中的data是个函数，那么每个实例可以维护一份被返回对象的独立的拷贝，那么这样组件实例之间的data属性值就不会互相影响了。



#### 虚拟DOM

虚拟 dom 是相对于浏览器所渲染出来的真实 dom 的，在react，vue等技术出现之前，我们要改变页面展示的内容只能通过遍历查询 dom 树的方式找到需要修改的 dom 然后修改样式行为或者结构，来达到更新 ui 的目的。

这种方式相当消耗计算资源，因为每次查询 dom 几乎都需要遍历整颗 dom 树，如果建立一个与 dom 树对应的虚拟 dom 对象（ js 对象），以对象嵌套的方式来表示 dom 树，那么每次 dom 的更改就变成了 js 对象的属性的更改，这样一来就能查找 js 对象的属性变化要比查询 dom 树的性能开销小。

#### Vue中直接给数组项赋值，为什么视图不更新呢？说说原因

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
1）当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
2）当你修改数组的长度时，例如：vm.items.length = newLength
 也就是说，直接设置数组的某一项的值，虽然改变了数组的值，但视图上显示的仍为数组之前的值，数据的响应失效了。出现这种现象的根本原因是什么呢？

首先我们先来了解vue数据响应的原理。官方文档的解释：

当你把一个普通的 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 把这些属性全部转为 getter/setter。
也就是说当改变data中属性的值时会触发其相应setter的调用，从而实现响应的操作。但getter和setter是有局限性的。我们先来看下面的这个例子：

var person = {};
Object.defineProperties( person, { 
      age: { 
        defaultValue: 11, 
        get: function () {
          return this.defaultValue;
        }, 
        set: function (val) { 
          this.defaultValue = val; 
          console.log("触发了set") 
        }
      } 
    });

// 修改属性的值时能够触发set
person.age = 12    // 触发了set
->触发了set
->12

person.age
->12

// 将属性的值设置为一个数组，当通过索引值修改数组的某一项或使用数组的某些方法修改数组时不能触发set
person.age = [2,3,4]    // 触发了set
->触发了set
->(3) [2, 3, 4]

person.age[2] = 5      // 未触发set
->5

person.age
->(3) [2, 3, 5]

person.age.push(5)     // 未触发set
->4

person.age
->(4) [2, 3, 4, 5]


// 将属性的值设置为一个对象，当修改对象中某属性的值时无法触发set
person.age = { first: 1 }
->触发了set
->{first: 1}

person.age.first = 2      // 未触发set
->2
通过上述例子可以观察得出：

当该属性的值为一个数组时，通过索引修改数组某一项的值或使用数组的某些方法修改数组并不能触发set；当属性的值为一对象时，直接修改对象中属性的值时也无法触发set。

有问题就相应的有解决方案。官方文档给出的解决方案如下：

为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果，同时也将触发状态更新：

// Vue.set
Vue.set(vm.items, indexOfItem, newValue)

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

你也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名：

vm.$set(vm.items, indexOfItem, newValue)


为了解决第二类问题，你可以使用 splice：

vm.items.splice(newLength)
而对于对象，当修改对象的属性或为对象添加属性时应该使用以下方法：

Vue.set(vm.userProfile, 'age', 27)

vm.$set(vm.userProfile, 'age', 27)
有时你可能需要为已有对象赋予多个新属性，比如使用 Object.assign() 或 _.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，则使用后一种方法来替代前一种方法：

// 不要使用此方法
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})

// 应该使用此方法
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
额外的：由于数据响应原理机制， Vue 不允许动态添加根级响应式属性，所以你必须在初始化实例前声明所有可能用到的根级响应式属性，且为这些属性都设一个初值，哪怕只是一个空值。

#### $nextTick 的使用？

**当数据更新了，在dom中渲染后，自动执行该函数**

nextTick 可以让我们在下次 DOM 更新循环结束之后执行延迟回调，用于获得更新后的 DOM。

在 Vue 2.4 之前都是使用的 microtasks，但是 microtasks 的优先级过高，在某些情况下可能会出现比事件冒泡更快的情况，但如果都使用 macrotasks 又可能会出现渲染的性能问题。所以在新版本中，会默认使用 microtasks，但在特殊情况下会使用 macrotasks，比如 v-on。

对于实现 macrotasks ，会先判断是否能使用 setImmediate ，不能的话降级为 MessageChannel ，以上都不行的话就使用 setTimeout

```
if (typeof setImmediate !== 'undefined' && isNative(setImmediate)) {
  macroTimerFunc = () => {
    setImmediate(flushCallbacks)
  }
} else if (
  typeof MessageChannel !== 'undefined' &&
  (isNative(MessageChannel) ||
    // PhantomJS
    MessageChannel.toString() === '[object MessageChannelConstructor]')
) {
  const channel = new MessageChannel()
  const port = channel.port2
  channel.port1.onmessage = flushCallbacks
  macroTimerFunc = () => {
    port.postMessage(1)
  }
} else {
  /* istanbul ignore next */
  macroTimerFunc = () => {
    setTimeout(flushCallbacks, 0)
  }
}
nextTick 同时也支持 Promise 的使用，会判断是否实现了 Promise

export function nextTick(cb?: Function, ctx?: Object) {
  let _resolve
  // 将回调函数整合进一个数组中
  callbacks.push(() => {
    if (cb) {
      try {
        cb.call(ctx)
      } catch (e) {
        handleError(e, ctx, 'nextTick')
      }
    } else if (_resolve) {
      _resolve(ctx)
    }
  })
  if (!pending) {
    pending = true
    if (useMacroTask) {
      macroTimerFunc()
    } else {
      microTimerFunc()
    }
  }
  // 判断是否可以使用 Promise
  // 可以的话给 _resolve 赋值
  // 这样回调函数就能以 promise 的方式调用
  if (!cb && typeof Promise !== 'undefined') {
    return new Promise(resolve => {
      _resolve = resolve
    })
  }
}
```



#### vue自定义指令

```
Vue.direcctive(‘focus’,{
		inserted:function(el){
			el.focus()
		}
})
new Vue({
		el:"app"
})

指令定义函数提供了几个钩子函数（可选）：

bind: 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。
inserted: 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。
update: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新（详细的钩子函数参数见下）。
componentUpdated: 被绑定元素所在模板完成一次更新周期时调用。
unbind: 只调用一次， 指令与元素解绑时调用。

接下来我们来看一下钩子函数的参数 (包括 el，binding，vnode，oldVnode) 。
```

#### Mvvm定义

MVVM是Model-View-ViewModel的简写。即模型-视图-视图模型。【模型】指的是后端传递的数据。【视图】指的是所看到的页面。【视图模型】mvvm模式的核心，它是连接view和model的桥梁。它有两个方向：一是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的页面。实现的方式是：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转化成后端的数据。实现的方式是：DOM 事件监听。这两个方向都实现的，我们称之为数据的双向绑定。

#### **跟keep-alive有关的生命周期是哪些？描述下这些生命周期**

activated和deactivated

keep-alive的生命周期
1.activated： 页面第一次进入的时候，钩子触发的顺序是created->mounted->activated
2.deactivated: 页面退出的时候会触发deactivated，当再次前进或者后退的时候只触发activated

#### vue使用v-for遍历对象时，是按什么顺序遍历的？如何保证顺序？

在遍历对象时，会按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下都一致。

#### prop验证的type类型有哪几种？

Number, String, Boolean, Array, Function, Object

#### 说说你对单向数据流和双向数据流的理解

单向数据流：所有状态的改变可记录、可跟踪，源头易追溯；所有数据只有一份，组件数据只有唯一的入口和出口，使得程序更直观更容易理解，有利于应用的可维护性；一旦数据变化，就去更新页面(data-页面)，但是没有(页面-data)；如果用户在页面上做了变动，那么就手动收集起来(双向是自动)，合并到原有的数据中。
双向数据流：无论数据改变，或是用户操作，都能带来互相的变动，自动更新。

#### vue在created和mounted这两个生命周期中请求数据有什么区别呢？

1. created阶段的ajax请求与mounted请求的区别：前者页面视图未出现，如果请求信息过多，页面会长时间处于白屏状态
2. 在created的时候，视图中的html并没有渲染出来，所以此时如果直接去操作html的dom节点，一定找不到相关的元素，而在mounted中，由于此时html已经渲染出来了，所以可以直接操作dom节点，（此时document.getelementById 即可生效了）。

#### computed和watch的区别

##### computed特性

1.是计算值
2.应用：就是简化tempalte里面{{}}计算和处理props或$emit的传值
3.具有缓存性，页面重新渲染值不变化,计算属性会立即返回之前的计算结果，而不必再次执行函数

#####  watch特性

1.是观察的动作
2.应用：监听props，$emit或本组件的值执行异步操作
3.无缓存性，页面重新渲染时值不变化也会执行

#### hash /a#/b； history /a/b。vue如何检测前端路由变化（vue中方法）

watch

```
watch: {
$route(to, from){
console.log(to, from)
}
}
```

#### vuex action和mutation的区别

action：简单来说就是异步操作数据

mutation：把处理数据逻辑方法全部放在mutation里面使数据和视图分离（vuex中store数据改变唯一的方法就是mutation）

#### Vue自定义组件

```
<template>
  <div>
    <div>实现自定义组件</div>
  </div>
</template>
<script>
export default{
  data () {
    return {
      msg: 'hello vue'
    }
  },
  components: {}
}
</script>
<style>
</style>
```



















