# Egg框架概述

## Egg.js是什么

**Egg.js 为企业级框架和应用而生**，我们希望由 Egg.js 孕育出更多上层框架，帮助开发团队和开发人员降低开发和维护成本。

> 注：Egg.js 缩写为 Egg

**基于==Node 和 Koa==开发，性能优异**



## 关于学习Egg的总结：

1. Egg可以开发企业级的应用，但是市场占有率不高（肯定java高哇）
2. 通过Egg，更平滑地学习后台的相关知识
3. ==在工作中可以使用Egg模拟后台接口==
4. 通过Egg，了解后台的工作流程（定义后台数据接口，mvc框架，orm框架，操作数据库）
5. 利用Egg，独立完成一个系统



## Egg项目初始化

npm init egg --type=simple

1.项目目录需要手动创建，与vue不同。

2.创建项目时间较长

没有node_modules执行 **cnpm install** 安装依赖





# 路由与控制器

## MVC概述

模型M-视图V-控制器C

MVC是一种软件设计规范，主要作用就是**逻辑拆分**：

1.视图为用户展示数据

2.控制器用来处理用户输入

3.模型用于处理数据

![img](https://bkimg.cdn.bcebos.com/pic/b03533fa828ba61edbddc04d4034970a304e59a4?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U4MA==,g_7,xp_5,yp_5/format,f_auto)

## 控制器

### Egg中 的控制器（controller）

1.直接响应数据或渲染模板

2.接收用户的输入

3.与路由建立对应关系

==this.ctx==可以==获取当前请求的上下文对象==，通过此对象可以便捷获取到请求与响应的属性与方法。

## 路由

### egg中的路由：app/router.js文件

```javascript
module.exports = app => { //暴露 路由函数  形参app，是整个egg应用的实例
    const { router, controller } = app;//通过解构赋值获得实例的router和controller
    router.get('/', controller.home.index);//执行home.js中的index方法
};
```

### 路由传递参数(==get==请求获取参数的两种方式)

1.获取query参数

```javascript
let query = this.ctx.request.query;
```

2.获取params参数

```javascript
let id = this.ctx.params.id;
```



## 提交表单

### 在controller中处理表单提交的数据（获取post请求的参数）

获取post请求的参数：**this.ctx.request.body**

==CSRF跨站请求伪造==，==Egg中对post请求做了一些安全验证==，可以在==config.default.js==文件中，通过下面的设置验证：

```javascript
config.security = {
	csrf:{
        enable:false,
    },
};
```



## RESTful接口的URL风格

### restful风格的url可以简化路由文件 

router.resources('posts', '/posts', controller.posts);    一个方法同时定义增删改查 

👆第一个参数：接口名；第二个参数：接口的URL；第三个参数：控制器；

<img src="C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210816161953231.png" alt="image-20210816161953231" style="zoom:67%;" />

**复习一下ctx的重定向**：==this.ctx.redirect(' /路径 ');==

例：

```javascript
//router.js
module.exports = app => {
    router.resources('fruits','/fruits',controller.fruits)//在路由文件中定义resources
}
```

```javascript
//fruits.js
class FruitsController extends Controller{
    async index(){ //index对应的路径是 /fruiis
        this.ctx.body = fruitList;
    }
    
    async new(){//new对应的是 /fruits/new
        //新增页面信息
    }
    
    async create(){//create对应的也是 /fruits 不过是以post请求得到的 与index得到的不同
        //post请求                                           所以一般使用重定向到首页
        this.ctx.redirect('/fruits');//重定向
    }
}
```



# 插件

## Egg通过插件来实现功能的扩展

1.egg-view-nunjucks:nunjucks模板插件

2.egg-cors:跨域请求配置插件



## egg-view-nunjucks

1.安装：cnpm install --save egg-view-nunjucks

2.在plugin.js文件中引入插件，在config.default.js中配置插件（都在config目录下）

3.在app文件夹下创建view目录，在view中创建模板文件，并在控制器中使用render方法渲染模板

```JavaScript
//plugin.js 在module.exports中引入插件
nunjucks:{
    enable:true,
    package:'egg-view-nunjucks',
}
    
//config.default.js 配置插件 
config.view = {//配置模板引擎
    defaultViewEngine:'nunjucks'//设置默认的模板引擎是nunjucks
}
```

在view目录中创建模板index.html 到控制器中用render 例如：

```javascript
// controller/home.js
async index(){
    const { ctx } = this;
    await ctx.render("index")//渲染模板 可以逗号隔开加参数 传数据到index中
}
```



## Egg-cors

1.安装：npm install --save egg-cors

2.在plugin.js文件中引入插件

3.在config.default.js文件中配置egg-cors插件

```javascript
//plugin.js
cors:{  //与nunjucks同级
    enable:true,
        package:'egg-cors',
}
        
//config.default.js
config.cors = {
    origin:"*",//*代表允许所以地址跨域请求
    allowMethods:'GET,HEAD,PUT,POST,DELETE,PATCH'//允许请求的方法
}
```



这里写一个简单的前后端交互：

```javascript
//前端
<template>
  <div>
    {{message}}
  </div>
</template>
<script>
    import axios from 'axios'; //引入axios
    export default {
        data() {
            return {
                message: "hello"
            }
        },
        created() {
            axios.get("http://127.0.0.1:7001/data").then(res => {
                this.message = res.data
            })
        }
    }
</script>
```

```javascript
//后端 要配置好cors噢
//先在 router.js 中把路径写好 给前端传数据用的http://127.0.0.1:7001/data
router.get('/data',controller.home.getData);

//然后再在 home.js 中把getData方法写好 写入要传递的数据
async getData(){
    this.ctx.body = "后台传过来的数据"
}
```



## 安全设置

在config.default.js中设置下属代码，允许post请求：

```javascript
config.security = {
        csrf: {
            enable: false,
        },
    };

```



## 实现水果列表 增删查 的功能（重点很多）

前端代码：

```javascript
<template>
  <div>
    <form @submit.prevent="postData">//表单提交触发事件，记得prevent去掉默认事件
      <input type="text" v-model="fruit">//绑定fruit
      <input type="submit" value="添加">
    </form>
    <ul>
      <li v-for="(item,index) of fruitList" :key="index">//for循环展示数据
        {{item}}--
        <button @click="del(index)">删除</button>//删除按钮，记得传入索引方便删除
      </li>
    </ul>
  </div>
</template>

<script>
    import axios from 'axios';//引入axios 
    export default {
        data() {
            return {
                fruitList: [],
                fruit: "" //存放键盘输入的数据
            }
        },
        created() {//钩子函数中调用get方法，开局自动获取数据
            this.getData();
        },
        methods: {
            getData() {//把get方法写出来方便多次调用
                axios.get("http://127.0.0.1:7001/fruits").then(res => {
                    this.fruitList = res.data
                })
            },
            postData() {
                axios.post("http://127.0.0.1:7001/fruits", {
                        fruit: this.fruit//前面的fruit是传给后端的
                    })                   //this.fruit才是这里键盘输入的
                    .then(() => {
                        this.getData();//添加完之后重新获取数据
                    })
            },
            del(id) {//将index传进来，用于找到要删除的数据
                axios.delete(`http://127.0.0.1:7001/fruits/${id}`)
                    .then(() => {//👆这里使用模板字符串 因为需要引入${id}
                        this.getData();
                    })
            }
        }
    }
</script>
```

后端代码：

```javascript
//router.js
module.exports = app => {
    router.resources('fruits','/fruits',controller.fruits);
}

//fruits.js
'use strict';

const Controller = require('egg').Controller;
let fruitList = ["香蕉", "苹果", "鸭梨"]
class FruitsController extends Controller {
    //GET请求
    async index() {
        this.ctx.body = fruitList;
    }

    //POST请求
    async create() {
        const fruit = this.ctx.request.body.fruit; //接收前端传过来的fruit
        fruitList.push(fruit);
        this.ctx.body = "添加成功" //给一个响应，每次请求controller都要给响应
    }

    //DELETE请求  它的url是这样的 /fruits/:id
    async destroy() {
        let id = this.ctx.params.id; //先获取id，通过id来删除水果列表，这里我们                                          自己前后端规定它为索引
        fruitList.splice(id, 1);
        this.ctx.body = "删除成功"
    }
}

module.exports = FruitsController;
```



# 用户登录状态



## http协议

### 1.请求，响应

请求：get、post、put、delete

响应：状态码(200,404,500)，数据( json格式:JavaScript对象表示法)



### 2.http协议是一个==无状态==的协议

问题：服务器如何识别用户

1.使用cookie与session识别用户

2.时使用JWT（Json Web Token）识别用户 



#### 1.使用session

在egg中设置session非常简单：this.ctx.session.user = username;



#### 2.JWT

1.json：javascript对象表示法， 用js的==对象==来表示数据格式，例[{name:" "},{name:" "}]

2.token：由服务器生成的加密的字符串(标识)，客户端带着token向服务器发送请求，以证明自己的身份。



## 生成 token

1.安装 ==egg-jwt== 插件：cnpm install --save egg-jwt

2.在plugin.js中引入插件

```javascript
jwt:{
    enable:true,
    package:"egg-jwt"
}
```

3.配置config.default.js文件，设置==secret (密钥)==；注意，secret不能泄露！我们就是通过密钥进行加密的

config.jwt = {secret:"GHOME"}

egg-jwt 已经封装好了一个 ==加密算法==，直接调用就可以实现token的加密功能。例：

```javascript
// controller/jwt.js
const Controller = require("egg").Controller;
class JwtController extends Controller{//定义类噢
    async index(){
        let user = { username:"GHOME" }//一个js对象，来生成token 
    }
    this.app.jwt.sign(user,this.app.config.jwt.secret)
    //加密算法,第一个参数是要加密的对象 第二个参数是之前定义的secret,这样就生成了一个token
}
module.exports = JwtController
```



## 验证流程

1. 根据用户信息，（服务器）生成token，并响应给客户端
2. 客户端将token存储在localstorage（本地存储）中
3. 客户端每次请求数据，请求头都会携带token
4. 服务器接受请求时，验证请求头的token，验证成功则响应数据

### 生成 / 校验token的方法

**生成token**：this.app.jwt.sign(对象名，this.app.config.jwt.secret);

**校验token**：this.app.jwt.verify(token，this.app.config.jwt.secre);



### ==try 语句==

try语句可以让我们更优雅地处理后台的异常，例：

```javascript
//在校验token的时候，传入的token 是假的（随便输的token），那么会报错 跳转到egg报错页。
//我们这里用try语句 来优雅地处理后台的异常：
try{
    let decode = this.app.jwt.verify("666",this.app.config.jwt.secret);
    this.ctx.body = decode;//如果通过校验会执行这一句
}catch(e){// e 表示错误信息，这一句表示 校验有异常的时候会执行catch
    this.ctx.body = "token未能通过验证"
}
```

输出结果：token未能通过验证



## ==补充一个知识（关于axios）==

我们获取数据使用 get方法，提交数据使用post方法：

在axios.get("",{})中 第一个参数为url ==第二个参数可以设置请求头==

在axios.post("",{},{})中 第一个参数为url 第二个参数为数据 ==第三个参数可以设置请求头==

例：

```javascript
getMessage(){
    let token = localStorage.getItem("token");//获得本地存储中的token
    axios.get("http://127.0.0.1:7001/jwtmessage",{headers:{token:token}})
}                                         //在请求头中创建一个token对象 值为上面定义的token
```



## 实现登录验证 和 交互

### 登录验证：账号密码正确的话，会给一个 token

前端：

```javascript
//Login.vue
<template>
    <div>
        <form @submit.prevent="doLogin">//提交表单，触发doLogin方法
            <h1>登录</h1>
            <input type="text" v-model="user.username">  |
            <input type="password" v-model="user.password">  |
            <button>提交</button>
        </form>
    </div>
</template>
<script>
    import axios from "axios";
    export default {
        data() {
            return {
                user: {//user对象，包含username password
                    username: "",
                    password: ""
                }
            }
        },
        methods: {
            doLogin() {
                axios.post("http://127.0.0.1:7001/jwtlogin", {//将数据提交到"/jwtlogin"
                        user: this.user //传递user对象到后台
                    })
                    .then((res) => { //对返回值进行判断
                        if (res.data.code === 20000) { //在后台自己设置的code
                            localStorage.setItem("token", res.data.token);//本地存储
                            console.log(res.data.token);
                            this.$router.push("/");//跳转到首页
                        }
                    })

            }
        }
    }
</script>
```

后端：

```javascript
//jwt.js
const Controller = require("egg").Controller;

class JwtController extends Controller{
    async doLogin(){
        let user = this.ctx.request.body.user;//接收前端传过来的对象user
        if(user.username==="xiaoming" && password==="123"){
            let user_jwt = {username:user.username};//定义json对象用于签名
            this.app.jwt.sign(user_jwt,this.app.config.jwt.secret);//签名
            this.ctx.body = {
                code:20000,//自己随便定义的code 用来代表成功了
                token:token//将上面获得的token赋值
            }
        }else{
            this.ctx.body = {
                code:40000,
                msg:"❌用户名或密码错误！"
            }
        }
    }
}
```



### 登录的用户与后台交互数据

前端：

```javascript
//Home.vue
<template>
    <button @click="getMessage">获取数据</button>
</template>
<script>
    import axios from 'axios';
    export default{
		methods:{
            getMessage(){
                let token = localStorage.getItem("token");
                axios.get("http://127.0.0.1/jwtmessage",{headers:token:token})
                          //在后端router中定义的路径        ↑设置请求头
            }
        }
}
</script>
```

后端：

```javascript
//jwt.js
const Controller = require("egg").Controller;

class JwtController extends Controller{
    async getMessage(){
        let token = this.ctx.request.header.token;
        try{//更优雅地处理后台异常
            let decode = this.app.jwt.verify(token,this.app.config.jwt.secret);
            console.log(decode);//正常的话，就执行这一句
        }catch(e){//出现异常，会有 错误警告e ，就会执行这一句
            console.log("token未通过验证！")
        }
    }
}
```



# 中间件

## 中间件的基本概念

egg是一个基于koa的框架，中间件是一个**函数**，在**请求与响应之间**执行。

把验证token的过程放在中间件里，达到简化系统的效果。



## 定义一个中间件

1.在app目录下创建middleware目录

2.在middleware中创建 **js文件**→这就是一个中间件

```javascript
//app > middleware > checktoken.js
function checktoken(){
    return async function(ctx,next){
        try{
        let token = ctx.request.header.token;
        //校验token  之前都是用this.app 这里使用ctx.app
        let decode = ctx.app.jwt.verify(token, ctx.app.config.jwt.secret);
        if (decode.username) {//decode是之前定义用来转化为token的对象，他有一个username
            await next();     //用户校验 （真正项目中 与数据库中的数据进行比对）
        } else {
            ctx.body = {
                code: 40000,
                msg: "用户校验失败"
            }
          }
        }catch(e){
            ctx.body={
                code: 40000,
                msg: "token未通过验证"
            }
        }
    }
}
module.exports = checktoken;
```

### 中间件的使用： 将它放到router.js 的需要拦截的位置

```javascript
//router.js
module.exports = app => {
    router.get("/jwtmessage", app.middleware.checktoken() ,controller.jwt.getMessage);
}                           //放在 请求 和 响应 之间
           //原本是router.get("/jwtmessage",controller.jwt.getMessage);
```



## 有了中间件，如果有多个需要登录后验证才能获得的数据，都可以用同一个中间件，非常方便：

```javascript
router.get("/1",app.middleware.checktoken(),controller.jwt.getMessage);
router.get("/2",app.middleware.checktoken(),controller.jwt.getMessage);
router.get("/3",app.middleware.checktoken(),controller.jwt.getMessage);
router.get("/4",app.middleware.checktoken(),controller.jwt.getMessage);
router.get("/5",app.middleware.checktoken(),controller.jwt.getMessage);
```



# 数据持久化

## 概述

1.应用程序的数据通常存储在数据库中。

2.我们使用MySQL数据库实现数据的持久化。

3.为了更方便的操作mysql，我们使用sequelize（ORM框架）管理数据层的代码。



## ORM框架（对象关系映射）

### 对象关系映射（Object Relational Mapping，简称ORM）

1.将数据从对象的形式，转换成二维表格的形式。

2.sequelize是一个基于node的orm框架

3.通过egg-sequelize，可以直接使用sequelize提供的方法操作数据库，而不需要动手写SQL语句。



## 安装sequelize

1.下载egg-sequelize: cnpm install --save egg-sequelize mysql2 

​                            (这里需要下两个模块egg-sequelize 和 mysql2)

2.在plugin.js文件中引入插件

3.在config.default.js文件中配置数据库连接

4.在app/model文件中创建数据模型

5.添加app.js文件，初始化数据库

参考sequelize文档：https://github.com/demopark/sequelize-docs-Zh-CN/tree/v5



### 在plugin.js文件中引入插件

```javascript
//plugin.js
module.exports={
    sequelize:{
        enable:true,
        package:'egg-sequelize'
    }
}
```

### 在config.default.js文件中配置egg-sequelize

```javascript
config.sequelize={
    dialect:'mysql',   //确定 数据库的类型
    database:'test,     //连接的 数据库名称
    host:'localhost',  //链接地址🔗
    port:3306,         //端口
    username:'root',   //用户名
    password:'wf1120314609..',
    timezone:'+08:00'  //时区，这是北京时间
}
```



## ==数据类型==

MySQL数据类型 与 sequelize数据类型 对应如下：

1.STRING    =>  varchar(255)

2.INTEGER =>  int

3.DOUBLE  =>  double

4.DATE       =>  datetime

5.TEXT       =>  text



## ==model==

在app目录下建一个==model== 来==定义一个数据模型==，==与==MySQL数据库中的==表做对应==。

在app/model中创建clazz.js文件(因为class与 类class重复 换成clazz)，对应数据库中的clazz表(班级表)

```javascript
//clazz.js
module.exports = app =>{//app可以拿到整个egg应用的应用实例
    const {STRING} = app.Sequelize;//获取sequelize的应用实例
    //这里用 解构赋值 拿到sequelize的STRING数据类型
    //默认情况下，sequelize将自动将所有传递的模型名称(define的第一个参数)转换为复数
    const Clazz = app.model.define('clazz',{//通过define方法创建一个数据表clazz
        //Sequelize会自动生成 id
        name:STRING,
    })
    return Clazz;
}
```



## app.js

用来==执行创建表的操作==

在项目==根目录==中创建app.js文件，初始化数据库。

```javascript
module.exports = app =>{
	app.beforeStart(async function(){
        //await app.model.sync({force:true}); ←--开发环境使用，会删除数据表
        //当model有更新的时候可以用
        //真正做项目的时候千万不能加 force:true 否则数据全没了
        //sync方法会根据模型去创建表
        await app.model.sync({})
    })
}
```

==系统启动的时候==会执行 beforeStart，有点类似于生命周期函数：里面通过app.model拿到数据模型，model有一个sync的方法，会根据模型（model）创建表。



## Controller

现在有了表之后，我们需要写增删改查的部分了。操作的话我们需要到controller里来做。

在controller中创建一个 叫clazz.js的controller。前端去请求数据 到controller 去操作数据库。

```javascript
// controller/clazz.js
const Controller = request("egg").Controller;
class ClazzController extends Controller {
     //restful：index/create/destroy/update
    async index(){
        
    }
    
    async create(){
        
    }
    
    async destroy(){
        
    }
    
    async update(){
        
    }
}
module.exports = ClazzController;
```

写路由：

```javascript
router.resoces("clazz","/clazz",controller.clazz);
```



### 数据操作

在controller中实现数据的增删改查

说明：在真实项目中，controller 和 操作数据的逻辑 要分离，以便于项目的扩展与维护。

```javascript
this.app.model.Clazz.findAll();                           //查询数据
this.app.model.Clazz.findAll({where:{id:1}});             //通过where进行条件查询
this.app.model.Clazz.create({name:"xx"});                 //添加数据
this.app.model.Clazz.update({name:"xx"},{where:{id:1}});  //通过条件修改数据
this.app.model.Clazz.destroy({where:{id:1}});             //通过条件删除数据
```

 用到代码中：

```javascript
async index() {
        // let clazzList = await this.app.model.Clazz.findAll()
        let id = this.ctx.request.query.id //query get请求的参数
        let clazzList = await this.app.model.Clazz.findAll({ //将数据存到数组中
            where: {//类似mysql语句
                id: id //让id从前端传进来 条件查询
            }
        })
        this.ctx.body = clazzList
    }

    async create() {
        let name = this.ctx.request.body.name; //接收前端发送过来的一个name的值
        await this.app.model.Clazz.create({
            name: name
        })
        this.ctx.body = "添加成功"
    }

    async destroy() {
        let id = this.ctx.params.id
        await this.app.model.Clazz.destroy({
            where: {
                id: id
            }
        })
        this.ctx.body = "删除成功"
    }

    async update() {
        let id = this.ctx.params.id
        let name = this.ctx.request.body.name;
        await this.app.model.Clazz.update({ name: name }, {
            where: {
                id: id
            }
        })
        this.ctx.body = "修改成功"
    }
```



## 主外键（表之间关系）

### 添加外键

创建一个Student模型：model / student.js

在Student通过associate属性指定外键：

```javascript
module.exports = app => {
    const { STRING,DOUBLE } = app.Sequelize;
    const Student = app.model.define('student', {
        name: STRING,
        achievement:DOUBLE
    })
    Student.associate = function(){ //associate属性，是一个函数：所属表和指向对应表的主键
        app.model.Student.belongsTo(app.model.Clazz,{//属于(第一个参数)表
            foreignKey:'clazz_id',//Student的外键是Clazz的主键
            as:'clazz'//起个别名，联查的时候需要用到
        })
    }

    return Student
}
```

app.js

```javascript
module.exports = app => {
    app.beforeStart(async function() {
        await app.model.sync({ force: true }); //开发环境使用，会清空数据表，
        //当model有更新的时候可以用
        //真正做项目的时候千万不能加 force:true 否则数据全没了
        // await app.model.sync({})
        //sync方法会根据模型去创建表
    })
}
```

controller/student.js

```javascript
const Controller = require("egg").Controller

class StudentController extends Controller {
    async create() {
        let name = this.ctx.request.body.name
        let achievement = this.ctx.request.body.achievement
        let clazz_id = this.ctx.request.body.clazz_id//外键
        await this.app.model.Student.create({
            name: name,
            achievement: achievement,
            clazz_id: clazz_id
        })
        this.ctx.body = "添加成功"
    }

    async index() {
        let StudentList = await this.app.model.Student.findAll()
        this.ctx.body = StudentList
    }

}
module.exports = StudentController
```



# Service（服务层）

举个栗子：

![image-20210823164507321](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210823164507321.png)

在controller和database中间 加了一个service

**之前写的功能代码大部分都没有进行异常处理，再加上异常处理等等操作的话，代码量会非常的大、代码也会非常的乱。因此需要将功能封装起来。**

简单来说，Service就是在复杂业务场景下==用于做业务逻辑封装的一个抽象层==，提供这个抽象有以下几个好处：

1. 保持Controller中的逻辑更加简洁
2. 保持业务逻辑的独立性，抽象出来的Service可以被多个Controller重复调用



## 定义service

在app目录下创建service目录，在app/service下再创建 js文件

```JavaScript
// service/student.js  这里用“查”来打比方
const Service = require("egg").Service
class StudentService extends Service{
    async getStudentList(){ //定义获取学生数据的方法
        try{
            let StudentList = this.app.model.Student.findAll()
            return StudentList
        }catch(e){
            return null
        }
    }
}
module.exports = StudentService
```

```javascript
//  controller/student.js
const Controller = require("egg").Controller
class StudentController extends Controller{
    async index(){
        let list = await this.ctx.service.student.getStudentList()//获得返回值
        if(list){ //对返回值进行判断
            this.ctx.body={
                code:20000,
                data:list
            }
        }else{
            this.ctx.body={
                code:50000,
                msg:'服务器异常，请与管理员联系'
            }
        }
    }
}
module.exports = StudentController
```

 

添加有些不同：

```javascript
// service/student.js
async addStudent(name,achievement,clazz_id){//添加方法是需要传参数进来，因为它直接接收不到
    try{
        await this.app.model.student.create({
            name:name,
            achievement:achievement,
            clazz_id:clazz_id
        })
        return true
    }else{
        return false
    }
}
```

```javascript
//  controller/student.js
async create(){
    let name = this.ctx.request.body.name
    let achievement = this.ctx.request.body.achievement
    let clazz_id = this.ctx.request.body.clazz_id
    let result = await this.ctx.service.clazz.addStudent(name,achievement,clazz_id)
    if(result){
        this.ctx.body={
            code:20000,
            msg:'添加成功'
        }
    }else{
        this.ctx.body={
            code:50000,
            msg:'数据添加失败，请与管理员联系'
        }
    }
}
```



##  在controller中调用service方法

在上下文对象中找到方法：await this.ctx.service.方法()



