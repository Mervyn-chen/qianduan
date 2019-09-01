# 请尽可能详尽的解释ajax的工作原理？

Ajax的工作原理相当于在用户和服务器之间加了—个中间层，使用户操作与服务器响应异步化。这样把以前的一些服务器负担的工作转嫁到客户端，利于客户端闲置的处理能力来处理，减轻服务器和带宽的负担，从而达到节约ISP的空间及带宽租用成本的目的。

简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。

```
<script>
   window.onload = function (ev) {
       var oBtn = document.querySelector("button");
       oBtn.onclick = function (ev1) {
           // 1.创建一个异步对象
           var xmlhttp=new XMLHttpRequest();
           // 2.设置请求方式和请求地址
           /*
           method：请求的类型；GET 或 POST
           url：文件在服务器上的位置
           async：true（异步）或 false（同步）
           */
           xmlhttp.open("GET", "04-ajax-get.php", true);
           // 3.发送请求
           xmlhttp.send();
           // 4.监听状态的变化
           xmlhttp.onreadystatechange = function (ev2) {
               /*
               0: 请求未初始化
               1: 服务器连接已建立
               2: 请求已接收
               3: 请求处理中
               4: 请求已完成，且响应已就绪
               */
               if(xmlhttp.readyState === 4){
                   // 判断是否请求成功
                   if(xmlhttp.status >= 200 && xmlhttp.status < 300 ||
                       xmlhttp.status === 304){
                       // 5.处理返回的结果
                       console.log("接收到服务器返回的数据");
                   }else{
                       console.log("没有接收到服务器返回的数据");
                   }

               }
           }
       }
   }
</script>

```

# 13.什么是AJAX?为什么要使用Ajax?（请谈一下你对Ajax的认识

答:什么是ajax：
AJAX是“Asynchronous JavaScript and XML”的缩写。他是指一种创建交互式网页应用的网页开发技术。
Ajax包含下列技术：
基于web标准（standards-basedpresentation）XHTML+CSS的表示；
使用 DOM（Document ObjectModel）进行动态显示及交互；
使用 XML 和 XSLT 进行数据交换及相关操作；
使用 XMLHttpRequest 进行异步数据查询、检索；
使用 JavaScript 将所有的东西绑定在一起。
2、为什么要用ajax：
Ajax应用程序的优势在于：

1. 通过异步模式，提升了用户体验
2. 优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用
3. Ajax引擎在客户端运行，承担了一部分本来由服务器承担的工作，从而减少了大用户量下的服务器负载

# 14.AJAX最大的特点是什么?

答:Ajax可以实现动态不刷新（局部刷新）
    就是能在不更新整个页面的前提下维护数据。
    这使得Web应用程序更为迅捷地回应用户动作，并避免了在网络上发送那些没有改变过的信息

15.简述 AJAX的交互模型，以及同步和异步的区别?
    答:AJAX主要用于实现从服务器获取数据并局部刷新页面。其交互模型为，
     AJAX在浏览器端引入一个执行引擎，它一边接收     user的请求，一边传送数据给服务器，并把服务器端返回的结果展现给 u se r。
    同步：脚本会停留并等待服务器发送回复然后继续。
    异步：脚本不停留并处理可能的回复

# 什么是跨域？跨域请求资源的方法有哪些？

答:1、什么是跨域？
由于浏览器同源策略，凡是发送请求url的协议、域名、端口三者之间任意一与当前页面地址不同即为跨域。存在跨域的情况：
网络协议不同，如http协议访问https协议。
端口不同，如80端口访问8080端口。
域名不同，如qianduanblog.com访问baidu.com。
子域名不同，如abc.qianduanblog.com访问def.qianduanblog.com。
域名和域名对应ip,如www.it.com访问20.205.28.90.

   2、跨域请求资源的方法：
  **(**1)、porxy代理**
    **定义和用法：proxy代理用于将请求发送给后台服务器，通过服务器来发送请求，然后将请求的结果传递给前端。**
    **实现方法：通过nginx代理；**
    **注意点：1、如果你代理的是https协议的请求，那么你的proxy首先需要信任该证书（尤其是自定义证书）**
    或者忽略证书检查，否则你的请求无法成功。**
    (2)、CORS 【Cross-Origin Resource Sharing】
    定义和用法：是现代浏览器支持跨域资源请求的一种最常用的方式。
    使用方法：一般需要后端人员在处理请求数据的时候，添加允许跨域的相关操作。如下：
    res.writeHead(200, {
        "Content-Type": "text/html; charset=UTF-8",
        "Access-Control-Allow-Origin":'http://localhost',
        'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
        'Access-Control-Allow-Headers': 'X-Requested-With, Content-Type'
    });

浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。

只要同时满足以下两大条件，就属于简单请求。
（1) 请求方法是以下三种方法之一：

- HEAD
- GET
- POST

（2）HTTP的头信息不超出以下几种字段：

- Accept
- Accept-Language
- Content-Language
- Last-Event-ID
- Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain

##### 非简单请求

非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。

- Access-Control-Request-Method：该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法，上例是PUT。
- Access-Control-Request-Headers：该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段，上例是X-Custom-Header。

如果浏览器否定了"预检"请求，会返回一个正常的HTTP回应，但是没有任何CORS相关的头信息字段。这时，浏览器就会认定，服务器不同意预检请求，因此触发一个错误，被XMLHttpRequest对象的onerror回调函数捕获。



​    (3)、jsonp
​    定义和用法：通过动态插入一个script标签。浏览器对script的资源引用没有同源限制
​    同时资源加载到页面后会立即执行（没有阻塞的情况下）。
​    特点：通过情况下，通过动态创建script来读取他域的动态资源，获取的数据一般为json格式。
​    实例如下：

```
  <script>
        function testjsonp(data) {
           console.log(data.name); // 获取返回的结果
        }
    </script>
    <script>
        var _script = document.createElement('script');
        _script.type = "text/javascript";
        _script.src = "http://localhost:8888/jsonp?callback=testjsonp";
        document.head.appendChild(_script);
    </script>
```

   后端写个小接口

```
// 处理成功失败返回格式的工具
const {successBody} = require('../utli')
class CrossDomain {
  static async jsonp (ctx) {
    // 前端传过来的参数
    const query = ctx.request.query
    // 设置一个cookies
    ctx.cookies.set('tokenId', '1')
    // query.cb是前后端约定的方法名字，其实就是后端返回一个直接执行的方法给前端，由于前端是用script标签发起的请求，所以返回了这个方法后相当于立马执行，并且把要返回的数据放在方法的参数里。
    ctx.body = `${query.cb}(${JSON.stringify(successBody({msg: query.msg}, 'success'))})`
  }
}
module.exports = CrossDomain
```

简单版前端



```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
  </head>
  <body>
    <script type='text/javascript'>
      // 后端返回直接执行的方法，相当于执行这个方法，由于后端把返回的数据放在方法的参数里，所以这里能拿到res。
      window.jsonpCb = function (res) {
        console.log(res)
      }
    </script>
    <script src='http://localhost:9871/api/jsonp?msg=helloJsonp&cb=jsonpCb' type='text/javascript'></script>
  </body>
</html>
```

 缺点：
​    　　1、这种方式无法发送post请求（这里）
​    　　2、另外要确定jsonp的请求是否失败并不容易，大多数框架的实现都是结合超时时间来判定。

# Ajax的优缺点及工作原理？

答:定义和用法:
    AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）
    Ajax 是一种用于创建快速动态网页的技术。Ajax 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。
    传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。

优点：
1.减轻服务器的负担,按需取数据,最大程度的减少冗余请求

2.局部刷新页面,减少用户心理和实际的等待时间,带来更好的用户体验

3.基于xml标准化,并被广泛支持,不需安装插件等,进一步促进页面和数据的分离

缺点：
1.AJAX大量的使用了javascript和ajax引擎,这些取决于浏览器的支持.在编写的时候考虑对浏览器的兼容性.

2.AJAX只是局部刷新,所以页面的后退按钮是没有用的.

3.对流媒体还有移动设备的支持不是太好等

AJAX的工作原理：
1.创建ajax对象（XMLHttpRequest/ActiveXObject(Microsoft.XMLHttp)）

2.判断数据传输方式(GET/POST)

3.打开链接 open()

4.发送 send()

5.当ajax对象完成第四步（onreadystatechange）数据接收完成，判断http响应状态（status）200-300之间或者304（缓存）执行回调函数

# 简述ajax 的过程?

1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象
2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息
3. 设置响应HTTP请求状态变化的函数
4. 发送HTTP请求
5. 获取异步调用返回的数据
6. 使用JavaScript和DOM实现局部刷新

