### CSS的盒子模型？

（1）两种， IE 盒子模型、标准 W3C 盒子模型；IE 的content部分包含了 border 和 pading;
（2）盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)。

### **如何利用CSS实现三角形？**

将一个div的宽度和高度，都设置为0，设置div四个边框的宽度，并利用transparent属性隐藏三个边框，只留下一个边框，就可得到一个三角形？如果是直角三角形，则隐藏2个相邻的边框，剩下2个相邻的边框就拼成了一个直角三角形。

### **如何利用CSS2实现元素水平垂直居中？**

利用绝对定位 + margin: auto实现，设置top left bottom right为0
利用绝对定位，设置top left为50%，并设置transform： translate（-50%，-50%）实现
行内元素，使用text-align: center，并将line-height值设置为父元素高度
利用flex进行布局，设置align-items: center; justify-content: center;实现居中

将元素设置为table-cell，使用vertical-align: middle 以及text-align: center实现居中

### **rem和em的区别？**

### tramsform 和translate 以及translate

translate:移动，transform的一个方法

transform:变形。改变

transition: 允许CSS属性值在一定的时间区间内平滑的过渡，**（过渡动画）**

与Transition不同的是：

1. Animation可以通过keyframe显示控制当前帧的属性值，而   Transition只能隐式来进行（不能指定每帧的属性值），所以相对 而言Animation的功能更加灵活。

2. Animation通过模拟属性值改变来实现动画，动画结束之后元素的属性没有变化；而Transiton确实改变了元素的属性值，动画结束之后元素的属性发生了变化；这一点，这在实际应用中会产生很大的区别。

### **position属性有哪些？**

static: 默认值，位于文档流之中，正常布局
relative：位于文档流之中，可以使用top和left等属性，使其相对于原位置进行偏移
absolute：绝对定位，元素脱离文档流，相对于其包含块定位（第一个非static值的父元素）
fixed：与absolute类似，不过其包含块为页面。
inherit：从父元素那继承position属性
initial：默认样式

unset：未设置，若该样式可继承，则相当于inherit，若不可继承，则相当于initial

### **如何解决float属性引起的父元素塌陷问题？**

当两个盒子在垂直方向上设置margin值时，会出现塌陷现象

给父元素设置`overflow: hidden`属性
给父元素添加一个高度
通过伪类，给这个伪类添加`clear: both` 和 `display: block`

### **CSS各选择器的优先级？**

!important > 行内样式>ID选择器 > 类选择器/属性/伪类 > 标签 > 通配符 > 继承 > 浏览器默认属性

### **vh和vw单位是什么？**

vw：视窗宽度的百分比（1vw 代表视窗的宽度为 1%）
vh：视窗高度的百分比
vmin：当前 vw 和 vh 中较小的一个值
vmax：当前 vw 和 vh 中较大的一个值

### **说一下圣杯和双飞翼布局？**

### **CSS可继承的属性**

### **CSS选择器解析的方向是什么？为什么？**

### **实现一个两栏布局，左边固定宽度，右边自适应？**

### **伪类和伪元素的区别？**

### **说一下CSS中的BFC是什么？有什么用？**

BFC即块状格式化上下文，BFC 是一个独立的布局环境,可以理解为一个容器,在这个容器中按照一定规则进行物品摆放,并且不会影响其它环境中的物品。如果一个元素符合触发 BFC 的条件，则 BFC 中的元素布局不受外部影响。
创建BFC的方法：浮动元素 、绝对定位元素、display值为 inline-block | flex | inline-flex | table-cell 或 table-caption、overflow值为hidden，auto，scroll

用处：避免外边距折叠，包含浮动元素（防止高度塌陷），避免浮动元素覆盖（防止文字环绕）

### translate和top/left的比较

translate是transform的一个方法，top、left是基于父元素的：  translate的参数：left（x 坐标） 和 top（y 坐标） 位置参数 ，
如果是百分比，会以本身的长宽做参考top/left是布局类的样式，

top/left是布局类样式     这个样式的变化会导致重排（reflow/relayout），所谓重排即指对这些节点以及受这些节点影响的其它节点，进行CSS计算->布局->重绘过程，这个过程的前2步是消耗大量资源的

translate是一个绘制样式（这名词我瞎编的），这个样式的变化会导致重绘（repaint），即在屏幕上重新画一下，不会进行CSS计算和布局这2个性能大户，所以我们认为translate性能上要明显好于top/left

translate3D传说因为走的是3D，所以能得到更完整的GPU加速的支持，在GPU中还有贴图缓存等手段帮你优化性能，所以更快

### viewport

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

### px和em的区别?

 答:相同点：px和em都是长度单位；
       异同点：px的值是固定的，指定是多少就是多少，计算比较容易。em得值不是固定的，并且em会继承父级元素的字体大小。

​       浏览器的默认字体高都是16px。所以未经调整的浏览器都符合: 1em=16px。那么12px=0.75em, 10px=0.625em。

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

### 谈谈CSS中link和@import的区别是

1）link属于HTML标签，而@import是CSS提供的；
2）页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载；
3）import只在IE5以上才能识别，而link是HTML标签，无兼容问题；

4）link方式的样式权重高于@import的权重。

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

