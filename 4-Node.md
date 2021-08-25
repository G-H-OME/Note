# 命令行工具

常用命令：

1. cd 跳转目录   cd ..返回上一级
2. dir 查看目录中所有文件
3. 用node运行js文件： node xxx.js





# npm常用命令

本地安装 npm install 第三方模块 （安装到当前目录）

全局安装 npm install 第三方模块 -g（-g 全局，会被安装到系统目录中 C盘）

==使用cnpm也可以安装，并且更快==



## 静态服务器

cnpm install http-server -g 

![image-20210803162646199](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210803162646199.png)





## npm项目初始化

npm init 可以在当前目录编程一个node项目（实质上就是添加了一个package.json文件）

每个node项目都会有一个package.json文件，用于**存放项目的一些信息和依赖模块**。

npm install 第三方 --save 

 ==（--save 会把模块的依赖关系记录到package.json的"dependencies"中）==



在实际开发中，一般来说node_modules太大了，只传递package.json文件，那么可以使用下面的代码，使项目下载第三方依赖： ==cnpm install==

它会找到package.json，的dependencies，下载依赖。



# ==Node基础==

使用模块化语法，让js文件之间互相调用！

==node的模块化开发==

**require()**: 引入一个外部模块

**module.exports**: 暴露模块接口，使之可以被引用

```javascript
//文件add.js
function add(a,b){
	return a+b;
}
module.exports = add;//使模块暴露
```

```javascript
//文件main.js
const add = require("./add.js")//引入add，这里一定要写路径
let result = add(1,2);
console.log(result);//得到3
```

与ES6的模块化语法略有差异，ES6的：

引入外部模块是：import

暴露模块接口是：export



## 外部模块

1.核心模块：node自带的模块，可以在require引入后直接使用。

2.自定义模板：是我们自己编写的模块，例如上面的add.js，引入自定义模块需要些完整的路径。

3.第三方模块：使用npm下载的模块是第三方模块，下载后可以直接使用require引入。



## 核心模块（node自带模块）

fs

fs是node的文件系统模块，可以通过readFile方法读取文件。

```javascript
const fs = require("fs");
                      //这里是一个回调函数
fs.readFile("文件路径",(err,data)=>{//第一个参数是错误信息，第二个是读取文件的内容
    if(err){//如果没找到，报错了则err会显示错误信息。如果没有错误则err为null
        console.log(err)
    }
    console.log(data.toString());//data是buffer类型（二进制类型）,用toString()。
})
```



path

path模块提供了一些用于处理文件路径的小工具，例如我们可以用path的join方法连接两个路径

```
const path = require("path");
let domain = "http://www.xiaozhoubg.com"
let url = "docs";
let id = "22";

let result = path.join(domain,url,id);
console.log(result);// 会用“\”连接
```



# Web服务器概述

客户端  发送请求 给  服务器

服务器  发送响应 给  客户端



## 基本概念

请求：浏览器向服务器要数据

响应：服务器给浏览器发送数据

地址：我们可以通过域名或ip访问到一个网站，这些就是这个网站的地址。

端口：一个ip或者一个域名可以找到一台服务器，但是这台服务器可以对外服务多个网站，他们的端口都是不同的，因此访问一个站点 除了ip或域名，还要输入端口。（**平时我们很少输入端口，是因为几乎所有的网站都会使用默认的80端口，因此不必输入。**）



## ==创建服务器==

通过http模块的createServer方法在本地创建一个服务器：

```javascript
const http = require("http");	//用回调函数设置响应的内容
const server = http.createServer((req,res)=>{//req请求，res响应
    res.end("hello!");//定义响应的文本
})
//设置端口
server.listen(3000,()=>{
    console.log("server is running");//服务器启动的提示 
})
```

createServer方法用来创建服务器对象，然后服务器对象通过listen方法定义服务器的端口。

在http://127.0.0.1:3000/就可以看到这个服务器了。



### nodemon

每次我们修改过服务器之后，都需要重启才能生效。全局安装nodemon，然后使用nodemon启动服务器，在修改文件之后，服务器可以自动重启。

cnpm install -g nodemon 全局安装

==nodemon server.js== 启动服务器





# Koa框架基础

Koa是一个基于Node的web服务器开发框架。



## 安装Koa

在安装Koa之前，先初始化一个项目。

在文件夹下 npm init

安装koa      cnpm install --save koa

```javascript
const Koa = require("koa"); //引入Koa构造函数
const app = new Koa(); //创建koa应用（对象）

app.listen(3000, () => {
        console.log("server is running")
    }) //设置监听端口
```

使用nodemon启动服务器    nodemon server.js



在接收到请求之后，给客户端一些响应实际的内容：

使用**Koa的对象app**的  **use方法**：==可以用来引入一个中间件（一个函数）==

==每个第三方模块，都需要通过use方法，引入到koa应用里面，才能被使用==

**中间件**：（实际是一个函数）在**请求和响应之间运行**。（服务器在响应之前会执行）

而且里面的函数是一个  async函数

```javascript
const Koa = require("koa"); //引入Koa构造函数
const app = new Koa(); //创建应用

//引入一个中间件，中间件即一个函数
app.use(async(ctx) => {//ctx上下文：存储了请求的相关信息，也可以设置响应的相关信息
    ctx.body = "hello koa!";//ctx就包含了(req,res)
})

app.listen(3000, () => {
        console.log("server is running")
    }) //设置监听端口
```



## ctx

ctx是koa自己封装的一个上下文对象，这个对象可以看做是原生http中req和res的集合。



## 路由

可以通过指定路径，响应对应的内容

浏览器可以使用不同的方法发送请求，常用的方法如下：

1、get请求：用来**获取**页面或数据

2、post请求：用来**提交**数据，一般登录的时候，向后台发送用户名和密码可以使用。 



这里主要使用get请求来获取页面：

先需要下载koa-router：cnpm install --save koa-router

需要在文件中引入koa-router:（引入的是一个函数，需要调用 ( )）

```javascript
const router = require("koa-router")()//引入并执行koa-router
```

这样就获得了一个router变量，可以用router指定请求的路径

进一步设置router：

```javascript
router.get("/",async(ctx) => {//第一个参数是路径
    ctx.body = "home page";
})

router.get("/video",async(ctx) => {
    ctx.body = "video page";
})
```

这里router和app是两个独立的块，

接下来需要将router加入到应用app中：

```javascript
app.use(router.routes());//在koa项目中引入router
```

​                                             ↑==这里别写错了，是routes 不是 routers==

route ：路线

router：路由



## 静态文件

在网页中插入图片，需要在img标签中填写图片的路径。web应用的服务器，只有静态文件目录的文件才可以被html页面直接访问。

也就是说，我们需要先创建一个静态文件目录，啊然后再里面放图片（或js，或css），才能被html页面访问。

先下载koa-static，可以设置静态文件目录。

cnpm install --save koa-static

引入koa-static:

```
const static = require("koa-static");
```

==__dirname 可以直接获取当前项目的绝对路径==（最前面有**两个下划线**），是node的一个全局变量

手动在文件夹中创建一个目录用于当作静态目录，📂public

指定静态文件目录：

```javascript
app.use(static(__dirname + "/public"))//括号里指定一个静态文件目录
                             //当前项目的绝对路径
```



基本实现：

```javascript
const Koa = require("koa"); //引入Koa构造函数
const router = require("koa-router")() //引入koa-router，这是一个函数 需要调用。
const static = require("koa-static");
const app = new Koa(); //创建应用

app.use(static(__dirname + "/public")) //括号里指定一个静态文件目录

//引入一个中间件，中间件即一个函数
// app.use(async(ctx) => {
//     ctx.body = "hello koa!";
//     console.log(ctx);
// })

router.get("/", async(ctx) => { //第一个参数是路径
    ctx.body = "home page";
})

router.get("/video", async(ctx) => {
    ctx.body = `<style>
                h1{color:blue;}
                </style>
                <h1>芜湖</h1>
                <p>下面是一个图片</p>
                <img src="/images/main.jpg"> 
    `; //使用反斜杠
})

app.use(router.routes()); //在koa项目中引入router

app.listen(3000, () => {
        console.log("server is running")
    }) //设置监听端口
```



总结一下引入的语句吧：

```javascript
app.use(router.routes());
app.use(static(__dirname + "/"))
```



# ==养成习惯！每次引入新模块，都要看有没有用app.use引入==



# Nunjucks模板入门

Nunjucks模板引擎，通过模板引擎，可以直接设置响应的html页面，并且可以把后台数据**绑定到模板**中，然后发送给客户端。



### 安装Nunjucks

**在koa框架下**安装Nunjucks需要两个第三方模块

1.koa-views：负责配置koa的模板引擎（==指定这个koa项目 使用哪个模板引擎==）

2.nunjucks：下载模板引擎

安装两个模块：

cnpm install --save koa-views

cnpm install --save nunjucks

在文件夹中新建文件夹views，并写一个html文件，来引入：

```javascript
const Koa = require("koa"); //引入Koa构造函数
const views = require("koa-views");
const nunjucks = require("nunjucks");
const app = new Koa(); //创建应用

//告诉app 模板文件放在这个目录下
app.use(views(__dirname + "/views", {//views里面有两个参数，第一个路径，第二个模板
    map: { html: "nunjucks" }//使用的是这个模板引擎，检测的是html文件
}))

//用ctx渲染模板，让app使用这个模板
app.use(async(ctx) => {
    await ctx.render("index") //render是一个异步的操作，要await
})

app.listen(3000, () => {
        console.log("server is running")
    }) //设置监听端口
```

与router配合使用：

```javascript
const Koa = require("koa"); //引入Koa构造函数
const views = require("koa-views");
const nunjucks = require("nunjucks");
const router = require("koa-router")();
const app = new Koa(); //创建应用

//模板文件放在这个目录下
app.use(views(__dirname + "/views", {
    map: { html: "nunjucks" } //使用的是这个模板引擎
}))

//用ctx渲染模板
// app.use(async(ctx) => {
//     await ctx.render("index") //render是一个异步的操作
// })

router.get("/", async ctx => {
    await ctx.render("index", { title: "首页" });
})

router.get("/video", async ctx => {
    await ctx.render("index", { title: "视频" }); //通过渲染的方式将title传递给模板
})


app.use(router.routes());

app.listen(3000, () => {
        console.log("server is running")
    }) //设置监听端口
```





## 处理表单数据

表单的属性：

action：指定表单提交数据的路径

method：指定表单提交数据的请求方法，（get,post）

form标签设置完成后，要对表单空间进行设置

input.name属性：指定数据传输的字段

input.type="submit"：指定提交按钮，点击后提交表单数据



html文件中，在表单中发送登录请求：

```javascript
<form action="/login" method="POST"></form> //默认是get方法，将数据传到后台需要用                                                post方法
```



js文件中，接收get请求：(加一个html页面模板 叫做home.html)

```javascript
router.get("/login",async ctx => {//接收一个get请求
	let username = ctx.query.username;//query可以获取get请求的参数
    let password = ctx.query.password;
    await ctx.render("home",{
        username:username,
        password:password
    })
})
```

想接收post请求，因为query只能接收get请求的参数，所以要下载koa-parser来解析post接收的请求

cnpm install --save koa-parser

引入parser：

```javascript
const parser = require("koa-parser");
app.use(parser());//引入parser
```



js文件中，接收post请求：

```javascript
router.post("/login",async ctx => {//接收一个post请求
	let username = ctx.request.body.username;//就不能用query了
    let password = ctx.request.body.password;
    await ctx.render("home",{
        username:username,
        password:password
    })
})
```



# Nunjucks模板语法

## 循环语句

首先数据从后台，由数组的形式传递到html页面中

```javascript
//在server.js中
router.post("/",async ctx => {
	let studentList = ["小明","小红","小刚"];
    await ctx.render("index",{
        studentList
    })
})
```

### 使用 =={%%}==可以定义一些模板指令 比如for，if等等，记得有始有终（endfor等）

在index.html中接收这个数据：

```javascript
//在index.html中
<ul> //给<li>嵌套一个for语句   ---  模板for语句
    {% for a in studentList %}
	<li>{{a}}</li>
	{% endfor %}
</ul>
```



## 分支语句

使用场景：判断网页该显示什么内容 不该显示什么内容。（登陆状态等）

判断登录状态：

```javascript
//在html模板中
{% if isLogin %}    //如果已经登录..这里if后跟着的是传过来的一个boolean值true/false
<p>欢迎{{username}}</p>
{% else %}          //否则
<p>请登录</p>
{% endif %}         //结束if
```

下面是server.js中的代码：

```javascript
router.post("/",async ctx => {
    await ctx.render("index",{
        isLogin:true,//true或false 传递给if判断
        username:xiaoming //后期就通过用户输入传过来username
    })
})
```





## 模板继承

### 导航栏(公共菜单)

实际上是 顶部的导航栏在一个html文件中，在其下方有一个 块 用于存放其他html中的代码：

```javascript
//layout.html
<body>
    <div>//导航栏内容
    	<a href="/">首页</a>
		<a href="/video">视频</a>
	</div>
	{% block content %} {% endblock %} //给block起名叫content
</body>
```

让其他的html文件 继承layout.html 这样内容就会被放到 block 中。



其他html文件，不需要头部的那些html/head/body标签了，只需要内容即可：

```javascript
{% extends "./view/layout.html" %} //extends 加上路径

{% block content %}
   <h1>内容</h1>
 	......
{% endblock %}
```





## include

某些页面包含相同的组件，并不是所有页面都有 就不能使用继承来实现了，可以通过include引入到网页中，降低网页的耦合。



将其单独定义一个html文件

```javascript
//footer.html  页脚文件
<div>
	powerby GHOME
</div>
```

然后在 想要加入的html文件中 加入：

```javascript
{% extends "./view/layout.html" %} //extends 加上路径

{% block content %}
   <h1>内容</h1>
 	......

{% include "./views/footer.html" %}//include 加上路径    
    
{% endblock %}
```



# Cookie与Session

Cookie是服务器**写在客户端的数据**

Session是**记录在服务器**，用于**识别Cookie的数据** 



## Cookie

ctx有**cookies属性**，有一个**set方法用来设置cookie**

cookie是以**明值对**的方式记录在客户端（浏览器）的

明值对：想js对象一样，一个名 一个值，一对一对的。

在主页设置cookie的 名和值

```javascript
//server.js
router.get("/",async ctx => {//从登录页面，设置user
    ctx.cookie.set("user","admin");
})
```

在响应页面中通过cookie的名获取cookie的值：

```javascript
router.get("/test",async ctx => {
    let user = ctx.cookie.get("user");
})
```

可以在设置cookie时给它设置一个过期事件：

```javascript
ctx.cookie.set("user","admin",{
    maxAge:2000;//设置cookie过期时间为2秒，这里单位是ms
})
```





## Session

为了记录用户的登录状态，需要使用cookie和session结合的方式。

先要安装session：

```
cnpm install --save koa-session
```

同样需要引入：

```javascript
const session = require("koa-session");
app.keys = ["123456"]//设置密钥
app.use(session({},app))//第一个参数是对象，可以定义maxAge等属性
```

这里切记：==use引入，还要记得设置key==，否则无法使用！



例如给session设置一个count属性，可以直接通过赋值的方式：

```
ctx.session.user = "admin";
```

获取也可以使用相同的方式：

```
let user = ctx.session.user;
```





## ==登录验证==

1.首页：任何人都可以访问

2.登录页：提供表单，用户可以通过表单输入登录信息

3.视频页：登录成功后可查看，并提供注销功能

这里主要写处理post请求的路由：

```javascript
router.post("/login", async ctx => {
    let username = ctx.request.body.username;
    let password = ctx.request.body.password;
    if (username === "admin" && password === "123456") {
        //设置session
        ctx.session.user = "admin";
        //重定向
        ctx.redirect("/list");
    } else {
        ctx.redirect("/");
    }
})
```

重定向是ctx的方法：redirect，实现网页跳转。

只有登录成功才能访问的list页面：

```java
router.get("/list",async ctx => {
    if(ctx.session.user){//如果有session并有user属性（即登录成功）
        await ctx.render("list.html");
    }else{
        ctx.redirect("/");//没有session则重定向去首页
    }
})
```



注销：

```java
//注销
router.get("/logout", async ctx => {
    ctx.session.user = "";
    ctx.redirect("/");
})
```





















