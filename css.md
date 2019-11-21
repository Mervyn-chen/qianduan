**CSS：**

## **css 盒子模型**

w3c盒模型：width不包含border和padding

IE盒模型：width包含border和padding

可以通过css去设置盒模型，通过box-sizing去设置，content-box为w3c盒模型，border-box为IE盒模型。



## **css position有哪些属性， 说说 absolute 的具体了解**

static: 默认值，位于文档流之中，正常布局

relative：位于文档流之中，可以使用top和left等属性，使其相对于原位置进行偏移

absolute：绝对定位，元素脱离文档流，相对于其包含块定位（第一个非static值的父元素）

fixed：与absolute类似，不过其包含块为页面。

inherit：从父元素那继承position属性

initial：默认样式

sticky：粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。

## **css 居中有几种方法**

##### 水平

1) 若是行内元素, 给其父元素设置 text-align:center,即可实现行内元素水平居中.

2) 若是块级元素, 该元素设置 margin:0 auto即可.

3) 使用flex 2012年版本布局, 可以轻松的实现水平居中, 子元素设置如下:

```
.son{ 
    display: flex; 
    justify-content: center; 
} 
```

4) 使用CSS3中新增的transform属性, 子元素设置如下: 

```
son{ 
    position:absolute; 
    left:50%; 
    transform:translate(-50%,0); 
} 
```

5) 使用绝对定位方式, 以及负值的margin-left, 子元素设置如下:

```
.son{ 
    position:absolute; 
    width:固定; 
    left:50%; 
    margin-left:-0.5宽度; 
} 
```



##### **垂直居中** 

1) 若元素是单行文本, 则可设置 line-height 等于父元素高度

元素高度不定 

2) 可用 **vertical-align** 属性, 而vertical-align只有在父层为 td 或者 th 时, 才会生效, 对于其他块级元素, 例如 div、p 等, 默认情况是不支持的. 为了使用vertical-align, 我们需要设置父元素display:table, 子元素 display:table-cell;vertical-align:middle; 

**优点**

元素高度可以动态改变, 不需再CSS中定义, 如果父元素没有足够空间时, 该元素内容也不会被截断.

**缺点**

IE6~7, 甚至IE8 beta中无效.

3) 父元素做如下设置即可保证子元素垂直居中:

```
.parent { 
  display: flex; 
  align-items: center; 
} 
```

**优点**

- 内容块的宽高任意, 优雅的溢出. 

- 可用于更复杂高级的布局技术中. 

**缺点**

- IE8/IE9不支持 

- 需要浏览器厂商前缀 

- 渲染上可能会有一些问题 

4) 可用 **transform** , 设置父元素相对定位(position:relative), 子元素如下css样式: 

```
.son{ 
    position:absolute; 
    top:50%; 
    -webkit-transform: translate(-50%,-50%);  
    -ms-transform: translate(-50%,-50%); 
    transform: translate(-50%,-50%); 
} 
```

7) 设置父元素相对定位(position:relative), 子元素如下css样式: 

```
.son{ 
    position:absolute; 
    top:50%; 
    height:固定; 
    margin-top:-0.5高度; 
} 
```

**优点**

适用于所有浏览器.

**缺点**

父元素空间不够时, 子元素可能不可见(当浏览器窗口缩小时,滚动条不出现时).如果子元素设置了overflow:auto, 则高度不够时, 会出现滚动条.

**总结** 

水平居中较为简单, 一般情况下 text-align:center,marin:0 auto; 足矣

- text-align:center; 

- margin:0 auto; 

- flex  

- transform 

垂直居中

- 单行文本, line-height  

- vertical-align 

- flex  

- transform  





## **CSS的权重，结合实例具体分析**

!important > 行内样式>ID选择器 > 类选择器/属性/伪类 > 标签 > 通配符 > 继承 > 浏览器默认属性

!important :无穷

Style:1000

id:0100

Class/伪类:0010

标签:0001

通配符：0000

## **如果设置background-color: red，那么盒子模型各个部分的背景是什么情况？**

background-color 属性为元素设置一种纯色。这种颜色会填充元素的内容、内边距和边框区域，扩展到元素边框的外边界（但不包括外边距）。如果边框有透明部分（如虚线边框），会透过这些透明部分显示出背景色。



## **px、em、rem、vw的区别**

vw：视窗宽度的百分比（1vw 代表视窗的宽度为 1%）

vh：视窗高度的百分比

vmin：当前 vw 和 vh 中较小的一个值

vmax：当前 vw 和 vh 中较大的一个值

Rem: 大小继承根元素的大小； 

Em：继承父元素对的大小； 



## **在什么场景下会出现外边距合并？如何合并？如何不让相邻元素外边距合并？给个父子外边距合并的范例？**

兄弟元素合并：当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并。

父子间合并：当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。

空元素：假设有一个空元素，它有外边距，但是没有边框或填充。在这种情况下，上外边距与下外边距就碰到了一起，它们会发生合并。



- 如何合并：
- 两个相邻的外边距都是正数时，合并结果是它们两者之间较大的值。
- 两个相邻的外边距都是负数时，合并结果是两者绝对值的较大值。
- 两个外边距一正一负时，合并结果是两者的相加的和。



- 解决办法：
- 对于兄弟元素：设置浮动，display：inline-block或使其父元素均形成BFC可防止边距合并（如overflow:hidden）
- 对于父子间的元素：可以给父元素设置border或padding属性来防止合并。
- 对于空元素：设置border或padding属性或overflow：hidden等（形成BFC）来防止合并。





## **单行文本溢出加 ... 如何实现？**

​    text-overflow: ellipsis;



## **z-index 有什么作用？如何使用？**

![image-20190904002507927](/Users/caiyifan/Library/Application Support/typora-user-images/image-20190904002507927.png)

下面这两个是层叠领域的黄金准则。当元素发生层叠的时候，其覆盖关系遵循下面2个准则：

1. **谁大谁上：**当具有明显的层叠水平标示的时候，如识别的z-indx值，在同一个层叠上下文领域，层叠水平值大的那一个覆盖小的那一个。通俗讲就是官大的压死官小的。

2. **后来居上：**当元素的层叠水平一致、层叠顺序相同的时候，在DOM流中处于后面的元素会覆盖前面的元素。



## **BFC 是什么？如何生成 BFC？BFC 有什么作用？举例说明。**

BFC即块状格式化上下文，BFC 是一个独立的布局环境,可以理解为一个容器,在这个容器中按照一定规则进行物品摆放,并且不会影响其它环境中的物品。如果一个元素符合触发 BFC 的条件，则 BFC 中的元素布局不受外部影响。

创建BFC的方法：浮动元素 、绝对定位元素、display值为 inline-block | flex | inline-flex | table-cell 或 table-caption、overflow值为hidden，auto，scroll

\- 浮动元素 (元素的 `float` 不是 `none`) 

\- 绝对定位元素 (元素具有 `position` 为 `absolute` 或 `fixed`) 

\- 内联块 (元素具有 `display: inline-block`) 

\- 表格单元格 (元素具有 `display: table-cell`，HTML表格单元格默认属性) 

\- 表格标题 (元素具有 `display: table-caption`, HTML表格标题默认属性) 

\- 具有`overflow` 且值不是 `visible` 的块元素 

\- 弹性盒（`flex`或`inline-flex`） 

比如浮动元素会形成BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。这里有点类似一个BFC就是一个独立的行政单位的意思。可以说BFC就是一个作用范围，把它理解成是一个独立的容器，并且这个容器里box的布局与这个容器外的box毫不相干。

\- `display: flow-root` 

\- `column-span: all` 

\#### BFC的约束规则 

\- 内部的盒会在垂直方向一个接一个排列（可以看作BFC中有一个的常规流） 

\- 处于同一个BFC中的元素相互影响，可能会发生外边距重叠 

\- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然 

\- 计算BFC的高度时，考虑BFC所包含的所有元素，连浮动元素也参与计算 

\- 浮动盒区域不叠加到BFC上 

\#### BFC可以解决的问题 

\- 垂直外边距重叠问题 

\- 去除浮动 

\- 自适用两列布局（`float` + `overflow`） 



## **伪元素清除浮动**

**方法一：使用带clear属性的空元素**

.clear{clear:both;}



**优点：简单，代码少，浏览器兼容性好。**

**缺点：需要添加大量无语义的html元素，代码不够优雅，后期不容易维护。**



**方法二：使用CSS的overflow属性**

overflow:hidden;

overflow:auto;



**方法三：使用CSS的:after伪元素**

**结合 :after 伪元素（注意这不是伪类，而是伪元素，代表一个元素之后最近的元素）和 IEhack ，可以完美兼容当前主流的各大浏览器，这里的 IEhack 指的是触发 hasLayout。**

**给浮动元素的容器添加一个clearfix的class，然后给这个class添加一个:after伪元素实现元素末尾添加一个看不见的块元素（Block element）清理浮动。**

```
.clearfix :before, clear :after{
	content: " ";
	display:table;
}
.clearfix:after{
	clear:both;

//触发BFC，BFC可以清除浮动
}
.clearfix {
	zoom:1;
//触发haslayout，这个属性只有2个值，true代表有自己的布局，false代表继承至父元素
}
```



## **LESS和SASS相对CSS有什么优势**

**LESS**

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。 

**变量（Variables）**

```
These are pretty self-explanatory:
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;
#header {
  color: @light-blue;
}
```

**混合（Mixins）**

```
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu a {
  color: #111;
  .bordered;
}
.post a {
  color: red;
  .bordered;
}
```

**嵌套（Nesting）**

```
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
==》
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

**运算（Operations）**

```
// example with variables
@base: 5%;
@filler: @base * 2; // result is 10%
@other: @base + @filler; // result is 15%
```

为了与 CSS 保持兼容，calc() 并不对数学表达式进行计算

**函数（Functions）**

```
@base: #f04615;
@width: 0.5;
.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```

Less 内置大量函数（image-size("file.png")，image-width("file.png")等等，用的时候查AI）。

if 函数

```
@some: foo;
div {
    margin: if((2 > 1), 0, 3px);
    color:  if((iscolor(@some)), darken(@some, 10%), black);
}
```

**作用域（Scope）**

```
@var: red;
#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```

**导入（Importing）**

“导入”的工作方式和你预期的一样。你可以导入一个 .less 文件，此文件中的所有变量就可以全部使用了。如果导入的文件是 .less 扩展名，则可以将扩展名省略掉：

@import "library"; // library.less

@import "typo.css”;

## **如何实现左侧宽度固定，右侧宽度自适应的布局**

小提示：这个问题面试官会要求说出几种解决方法。

**利用float + margin实现** 

```
.box { 
height: 200px; 
} 
.box > div { 
  height: 100%; 
} 
.box-left { 
  width: 200px; 
  float: left; 
  background-color: blue; 
} 
.box-right { 
  margin-left: 200px; 
  background-color: red; 
} 
```

**利用calc计算宽度** 

```
.box { 
height: 200px; 
} 
.box > div { 
  height: 100%; 
} 
.box-left { 
  width: 200px; 
  float: left; 
  background-color: blue; 
} 
.box-right { 
  width: calc(100% - 200px); 
  float: right; 
  background-color: red; 
} 
```



利用float + overflow实现 

```
.box { 
height: 200px; 
} 

.box > div { 
  height: 100%; 
} 
.box-left { 
  width: 200px; 
  float: left; 
  background-color: blue; 
} 
.box-right { 
  overflow: hidden; 
  background-color: red; 
} 
```

**利用flex实现** 

这里不是最佳答案，应该是使用flex-basis实现更合理

```
.box { 
  height: 200px; 
  display: flex; 
} 
.box > div { 
  height: 100%; 
} 
.box-left { 
  width: 200px; 
  background-color: blue; 
} 
.box-right { 
  flex: 1; // 设置flex-grow属性为1，默认为0 
  overflow: hidden; 
  background-color: red; 
} 


```



## 分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景

1. display: none (不占空间，不能点击)（场景，显示出原来这里不存在的结构）
2. visibility: hidden（占据空间，不能点击）（场景：显示不会导致页面结构发生变动，不会撑开）
3. opacity: 0（占据空间，可以点击）（场景：可以跟transition搭配）

## fixed定位是什么意思，父级标签设置什么可以让子标签fixed定位失效？

答案：transform

 CSS3的动画最小间隔多少（多数显示器的默认频率是60Hz，每秒刷新60次，最小间隔是16.7ms）



## **伪类和伪元素的区别：**

1、伪类的操作对象是文档树中已有的元素，而伪元素则创建了一个文档数外的元素；

2、CSS3规范中要求使用双冒号(::)表示伪元素，以此来区分伪元素和伪类。

## tramsform 和translate 以及animation

translate:移动，transform的一个方法

transform:变形。改变

transition: 允许CSS属性值在一定的时间区间内平滑的过渡，**（过渡动画）**

与Transition不同的是：

1. Animation可以通过keyframe显示控制当前帧的属性值，而   Transition只能隐式来进行（不能指定每帧的属性值），所以相对 而言Animation的功能更加灵活。
2. Animation通过模拟属性值改变来实现动画，动画结束之后元素的属性没有变化；而Transiton确实改变了元素的属性值，动画结束之后元素的属性发生了变化；这一点，这在实际应用中会产生很大的区别。

## translate和top/left的比较

translate是transform的一个方法，top、left是基于父元素的：  translate的参数：left（x 坐标） 和 top（y 坐标） 位置参数 ，
如果是百分比，会以本身的长宽做参考top/left是布局类的样式，

top/left是布局类样式     这个样式的变化会导致重排（reflow/relayout），所谓重排即指对这些节点以及受这些节点影响的其它节点，进行CSS计算->布局->重绘过程，这个过程的前2步是消耗大量资源的

translate是一个绘制样式（这名词我瞎编的），这个样式的变化会导致重绘（repaint），即在屏幕上重新画一下，不会进行CSS计算和布局这2个性能大户，所以我们认为translate性能上要明显好于top/left

translate3D传说因为走的是3D，所以能得到更完整的GPU加速的支持，在GPU中还有贴图缓存等手段帮你优化性能，所以更快

## viewport

- **width**: 设置viewport宽度，为一个正整数，或字符串 device-width
- **device-width**: 设备宽度
- **height**: 设置viewport高度，一般设置了宽度，会自动解析出高度，可以不用设置
- **initial-scale**: 默认缩放比例（初始缩放比例），为一个数字，可以带小数
- **minimum-scale**: 允许用户最小缩放比例，为一个数字，可以带小数
- **maximum-scale**: 允许用户最大缩放比例，为一个数字，可以带小数
- **user-scalable**: 是否允许手动缩放

**延伸提问：怎样处理 移动端 1px 被渲染成 2px 问题？**

1、局部处理

meta 标签中的 viewport 属性 ，initial-scale 设置为 1

rem 按照设计稿标准走，外加利用 transfrome 的 scale(0.5) 缩小一倍即可；

2、全局处理

meta 标签中的 viewport 属性 ，initial-scale 设置为 0.5

rem 按照设计稿标准走即可

## 谈谈CSS中link和@import的区别是

1）link属于HTML标签，而@import是CSS提供的；
2）页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载；
3）import只在IE5以上才能识别，而link是HTML标签，无兼容问题；

4）link方式的样式权重高于@import的权重。

## CSS三角形绘制

```
div {
    width: 0;
    height: 0;
    border: 40px solid;
    border-color: transparent transparent red;
}
```

## scoped 的缺陷。

如果你子组件的根元素上有一个类已经在这个父组件中定义过了，那么这个父组件的样式就会**泄露到子组件中**
我们在父元素中定义wrapper类边框为蓝色，在子组件中定义同一个名字的类wrapper类边框为橙色，结果会导致子组件中既有父元素的蓝色边框又有自己的橙色边框。

因此，我们要避免在父组件中书写和子组件同名的css类。

## CSS文件没下载完会影响DOM树吗？

1、css是由单独的下载线程异步下载的。

2、css加载不会阻塞DOM树解析（异步加载时DOM照常构建）

3、但会阻塞render树渲染（渲染时需等css加载完毕，因为render树需要css信息）

## IFC

IFC(Inline Formatting Contexts)直译为"内联格式化上下文"，在常规流中横着排列。

**行级盒子高度是由font-size决定的**
宽度等于其子行级盒子的外宽度，但是当行盒的宽度大于父宽度时会被拆分为多个行盒。

**如何计算行级盒子的高度**：

1、位于该行上的所有in-flow的inline-level box均参与该行line box高度的计算;

2、各inline-level box根据vertical-align属性值相对各自的父容器作垂直方向对齐;

3、最上方的box的上边界到最下方的下边界则是line box的高度。

**IFC的规则**

1、盒子是水平一个接一个的排列，水平的margin，内边距，边框是可以有的。

2、垂直方向的对齐，可能是底部对齐，顶部对齐，也可能是基线对齐（这个是默认的）；

3、行框中的内联盒子的高度小于行框的高度时，内联盒子的垂直方向的对齐方式取决于vertical-align属性

4、当一个行框水平不能容纳内联盒子时，他们将会在垂直方向上产生多个行框，他们上下一个挨着一个，但是不会重叠

5、一般来说，行框的左边界紧挨着包含容器的左边界，行框的右边界紧挨着包含容器的右边界。

6、多个内联盒子的宽度小于包含他们的行框时，他们在水平方向的分布取决于text-align属性（默认是left）

## 介绍flex布局，说说align-items和align-content的区别

align-items属性适用于所有的flex容器，它是用来设置每个flex元素在侧轴上的默认对齐方式。
align-items和align-content有相同的功能，不过不同点是它是用来让每一个单行的容器居中而不是让整个容器居中。
align-content属性只适用于多行的flex容器，并且当侧轴上有多余空间使flex容器内的flex线对齐。

## position, display, float一起设置，会怎么样

当display: none，position和float无作用;

当position: absolute或 fixed,float为none。

## 图片预加载和懒加载

### 懒加载

##### 场景：

对于图片过多的页面，为了加快页面加载速度，需要将页面内未出现的可视区域内的图片先不做加载，等到滚动可视区域后再去加载。

##### 原理：

img标签的src属性用来表示图片的URL，当这个属性值不为空时，浏览器就会根据这个值发送请求，如果没有src属性就不会发送请求。所以，在页面加入时将img标签的src指向为空或者指向一个小图片（loading或者缺省图），将真实地址存在一个自定义属性data-src中，当页面滚动时，将可视区域的图片的src值赋为真实的值。

### 预加载

##### 场景：

图鼠标移入一张图片时，换成另一张图片，移出时换回原来的图片，正常做法是，鼠标移入的时候，改变图片的src，但这时就要去加载图片了，会等待一段时间，这样体验不好。预加载的做法是，在页面加载完，鼠标移入之前就通过Image对象把图片加载进缓存了，这样鼠标移入的时候直接从缓存里读取了，速度很快，解决此问题的方案就是实现图片预加载。

##### 原理：

事先把网页的图片记载到本地，之后就直接到缓存中拿图片
实现方法一般有三种：

##### 1）、使用CSS进行图片预加载

原理：将需要加载的图片作为标签的背景图预先加载出来，但是不显示在可视区域内
缺点：加载的图片会同页面的其他内容一起加载，增加了页面的整体加载时间

##### 2）、使用CSS+JS进行图片预加载

为了解决上述问题，可以增加一些JS代码来推迟加载的时间，直到页面加载完毕

##### 3）、使用Ajax实现预加载

使用Ajax方法实现预加载，不仅仅是针对图片的预加载，还会预加载CSS、JS等相关的东西

## 雪碧图怎么操作的

background-position的值（默认为（0，0），也就是图片的左上角）

## 标签语义化的优点

1、HTML结构清晰

2、代码可读性较好

3、无障碍阅读

4、搜索引擎可以根据标签的语言确定上下文和权重问题

5、移动设备能够更完美的展现网页（对css支持较弱的设备）

6、便于团队维护和开发

## 可继承的样式

可继承的样式： font-size, font-family, color, text-indent;

不可继承的样式：border, padding, margin, width, height ;

## **说一下圣杯和双飞翼布局？**

转自文档：https://juejin.im/post/5caf4043f265da039f0eff94

![image-20190918143334510](/Users/caiyifan/.Trash/image-20190918143334510.png)

##### 圣杯布局

1. 设置left、middle、right三个盒子
2. 设置float: left, 脱离文档流；
3. 给container设置overflow: hidden; 可以形成BFC撑开文档
4. left、right设置上各自的宽度
5. middle设置width: 100%;
6. 给left、middle、right设置position: relative;
7. left设置 left: -leftWidth, right设置 right: -rightWidth;
8. container设置padding: 0, rightWidth, 0, leftWidth；

##### 双飞翼

1. 首先把left、middle、right都放出来, middle中增加inner
2. 给它们三个设置上float: left, 脱离文档流；
3. 一定记得给container设置上overflow: hidden; 可以形成BFC撑开文档
4. left、right设置上各自的宽度
5. middle设置width: 100%;
6. left设置 margin-left: -100%, right设置 right: -rightWidth;
7. container设置padding: 0, rightWidth, 0, leftWidth;

### 可能用到的meta标签

```
<!-- 设置缩放 -->
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, minimal-ui" />
<!-- 可隐藏地址栏，仅针对IOS的Safari（注：IOS7.0版本以后，safari上已看不到效果） -->
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- 仅针对IOS的Safari顶端状态条的样式（可选default/black/black-translucent ） -->
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- IOS中禁用将数字识别为电话号码/忽略Android平台中对邮箱地址的识别 -->
<meta name="format-detection"content="telephone=no, email=no" />
```

### display有哪些值？说明它们的作用?

block 块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
        none 缺省值。像行内元素类型一样显示。(隐藏)
        inline 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
        inline-block 默认宽度为内容宽度，可以设置宽高，同行显示。
        list-item 像块类型元素一样显示，并添加样式列表标记。
        table 此元素会作为块级表格来显示。

​        inherit 规定应该从父元素继承display属性的值。

### img标签的title和alt有什么区别

alt用于图片无法加载时显示，title为该属性提供信息，通常当鼠标滑动到元素上的时候显示.

### ransition 闪屏

**启动硬件加速**

最常用的方式：translate3d、translateZ、transform

opacity 属性/过渡动画（需要动画执行的过程中才会创建合成层，动画没有开始或结束后元素还会回到之前的状态）

will-chang 属性（这个比较偏僻），一般配合opacity与translate使用（而且经测试，除了上述可以引发硬件加速的属性外，其它属性并不会变成复合层）。

弊端：硬件加速会导致 CPU 性能占用量过大，电池电量消耗加大 ；因此尽量避免泛滥使用硬件加速。

### css实现单行文本溢出显示 ...

```
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```

### 实现多行文本溢出显示...

```
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
overflow: hidden;
text-overflow: ellipsis;
```

### 改变 placeholder 的字体颜色大小

pc可以 移动端有问题

```
input::-webkit-input-placeholder {
    /* WebKit browsers */
    font-size:14px;
    color: #333;
}
input::-moz-placeholder {
    /* Mozilla Firefox 19+ */
    font-size:14px;
    color: #333;
}
input:-ms-input-placeholder {
    /* Internet Explorer 10+ */
    font-size:14px;
    color: #333;
}
```

### 字体font-family

```
@ 宋体      SimSun
@ 黑体      SimHei
@ 微软雅黑   Microsoft Yahei
@ 微软正黑体 Microsoft JhengHei
@ 新宋体    NSimSun
@ 新细明体  MingLiU
@ 细明体    MingLiU
@ 标楷体    DFKai-SB
@ 仿宋     FangSong
@ 楷体     KaiTi
@ 仿宋_GB2312  FangSong_GB2312
@ 楷体_GB2312  KaiTi_GB2312
@
@ 说明：中文字体多数使用宋体、雅黑，英文用Helvetica

body { font-family: Microsoft Yahei,SimSun,Helvetica; }
```

### 