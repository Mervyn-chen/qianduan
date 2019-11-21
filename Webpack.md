## webpack 热更新原理

热更新就是 **在应用程序的开发环境，方便开发人员在不刷新页面的情况下，就能修改代码，并且直观地在页面上看到变化的机制**。

HotModuleReplacementPlugin();

```
module.exports = {
  // ...
  devServer: {
    hot: true,
    // ...
  }
}
```

![image-20190916162238784](/Users/caiyifan/Library/Application Support/typora-user-images/image-20190916162238784.png)

如上图所示，右侧`Server`端使用`webpack-dev-server`去启动本地服务，内部实现主要使用了`webpack`、`express`、`websocket`。

- 使用`express`启动本地服务，当浏览器访问资源时对此做响应。
- 服务端和客户端使用`websocket`实现长连接
- `webpack`监听源文件的变化，即当开发者保存文件时触发`webpack`的重新编译。
  - 每次编译都会生成`hash值`、`已改动模块的json文件`、`已改动模块代码的js文件`
  - 编译完成后通过`socket`向客户端推送当前编译的`hash戳`
- 客户端的`websocket`监听到有文件改动推送过来的`hash戳`，会和上一次对比
  - 一致则走缓存
  - 不一致则通过`ajax`和`jsonp`向服务端获取最新资源
- 使用`内存文件系统`去替换有修改的内容实现局部刷新

## 断点续传

断点续传：中途暂停之后，可以从上次上传完成的Part的位置继续上传。

## nginx负载均衡的方式

1.轮询（默认）——每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。

2.weight ——指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。

3. ip_hash ——每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器。

4.backup——其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

5.down——表示单前的server暂时不参与负载

6.fair（第三方）按后端服务器的响应时间来分配请求，响应时间短的优先分配。与weight分配策略类似。

## webpack插件的生命周期

初始化-->编译--->分析入口文件，创建模块对象--->构建模块--->对象构建完毕，完成编译---->编译器输出assets--->结束

## webpack的优势

#### 1、代码拆分（支持异步模块加载）

> Webpack 有两种组织模块依赖的方式，同步(默认)和异步（高级）。异步依赖作为分割点，形成一个新的块。在优化了依赖树后，每一个异步区块都作为一个文件被打包。

#### 2、Loader（支持任意模块加载，比如图片、less、css等等）

> Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。

#### 3、智能解析

> Webpack 有一个智能解析器，几乎可以处理任何第三方库，无论它们的模块形式是 CommonJS、 AMD 还是普通的 JS 文件。甚至在加载依赖的时候，允许使用动态表达式 require("./templates/" + name + ".jade")。

#### 4、插件系统

> Webpack 还有一个功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的 Webpack 插件，来满足各式各样的需求。

#### 5、快速运行

> Webpack 使用异步 I/O （NodeJs）和多级“缓存”提高运行效率，这使得 Webpack 能够以令人难以置信的速度快速增量编译。

## git fetch和pull区别， git reset --soft 和 --hard区别

fetch 只能更新远程仓库的代码为最新的，本地仓库的代码还未被更新，我们需要通过 git merge origin/master 来合并这两个版本，你可以把它理解为合并分支一样的。

pull 操作是将本地仓库和远程仓库（本地的）更新到远程的最新版本。

如果想要更加可控一点的话推荐使用fetch + merge。



git reset –-soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可；
git reset -–hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容，撤销的commit中所包含的更改被冲掉；

## webpack遵循哪些规范   AMD和commonjs  这两个有什么区别

Webpack中有说到，它都支持CommonJS规范和AMD规范。

commonJS用同步的方式加载模块。在服务端，模块文件都存在本地磁盘，读取非常快，所以这样做不会有问题。但是在浏览器端，限于网络原因，更合理的方案是使用异步加载。

AMD规范采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。这里介绍用require.js实现AMD规范的模块化：用`require.config()`指定引用路径等，用`define()`定义模块，用`require()`加载模块。

## ESLint原理

https://www.jianshu.com/p/526db7eeeecc

抽象语法树

## webpack 之 loader 和 plugin 

### webpack常用的loader

- 样式：style-loader、css-loader、less-loader、sass-loader等
- 文件：raw-loader、file-loader 、url-loader等
- 编译：babel-loader、coffee-loader 、ts-loader等
- 校验测试：mocha-loader、jshint-loader 、eslint-loader等

### webpack常用的plugin

- 首先webpack内置`UglifyJsPlugin`，压缩和混淆代码。
- webpack内置`CommonsChunkPlugin`，提高打包效率，将第三方库和业务代码分开打包。
- `html-webpack-plugin`可以根据模板自动生成html代码，并自动引用css和js文件
- `extract-text-webpack-plugin`将js文件中引用的样式单独抽离成css文件
- `DefinePlugin`编译时配置全局变量，这对开发模式和发布模式的构建允许不同的行为非常有用。
- `HotModuleReplacementPlugin` 热更新
- `optimize-css-assets-webpack-plugin` 不同组件中重复的css可以快速去重
- `compression-webpack-plugin` 生产环境可采用gzip压缩JS和CSS