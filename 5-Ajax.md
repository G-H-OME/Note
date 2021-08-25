# Http协议概述

http协议常用的四种方法：

get：获取数据

post：提交数据

put：修改数据

delete：删除数据

 

## 常用状态码

200：OK，请求被正常处理

404：Not Found，服务器找不到客户端请求的资源。

500：Internal Server Error：服务器内部错误





# Postman

模拟发送请求；

Params异步；

## 路由传参的方法：

get，put，post，delete都可以，这里用put做例子：

路由传参   固定格式：   /:id   传入id 通过ctx.params.id接收   

```java
router.put("/login/:id",ctx () => {
    ctx.body = ctx.params.id;
})
```



## GET：提交get页面请求。



## POST：提交post数据请求。

post的数据在Body中，这里登录传递数据在表单中：

![image-20210807002148679](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210807002148679.png)





## PUT：修改数据。

这里演示修改数组中的值，通过/:id接收参数代表index，再用ctx.request.body.fruit接收要替换成的值

使用==数组的splice()方法==：

```java
router.put("/fruits/:id",ctx => {
    let id = ctx.params.id;
    let fruit = ctx.request.body.fruit;
    dataList.splice(id,1,fruit);  //splice(要删除元素的索引，删除几个元素，替换成xx)
    ctx.body = dataList;
})
```

![image-20210807013741821](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210807013741821.png)





## DELETE：删除数据

跟PUT很像，更加简洁，因为不用替换 只用删除：

```javascript
router.delete("/fruits/:id",ctx => {
    let id = ctx.params.id;
    dataList.splice(id,1); //splice(要删除元素的索引，删除几个元素)
    ctx.body = dataList;
})
```

![image-20210807014015950](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210807014015950.png)





# Ajax入门

在某些项目中，我们只希望获取页面的局部数据，而不必整页刷新，这个时候就需要使用Ajax来实现功能了。

全称是（异步的JavaScript 和 XML）。这个概念出现得比较早，那个时候前端和后台的数据交互主要以 XML 格式为主，例如：

```javascript
 //XML格式
<fruit>
   <name>apple</name>
   <weight>0.5kg</weight>
   <color>red</color>
</fruit>
```

现在仍然存在很多用xml交互数据的情况，但是目前主流的数据格式使用的是json(JavaScript对象表示法)，例：

```javascript
 //json格式
{
    "fruit":{
        "name":"apple",
        "weight":"0.5kg",
        "color":"red"
    }
}
```

以后我们使用Ajax传递数据都是使用 json 格式。



## ajax的优缺点：

优点：按需获取数据，提升系统性能。

缺点：异步获取数据，不利于搜索引擎优化。



## Ajax原理：

Ajax的原理 是通过==XMLhttpRequest对象==向服务器发送请求。

用按钮触发事件做例子：

```javascript
document.querySelector("button").onclick = function(){
    let xhr = new XMLHttpRequest();
    xhr.open("get","/data");//定义请求的类型（get,post等），获取数据的路径url
    xhr.send();//发送请求
    xhr.onreadystatechange =funcion(){  //获取响应
        //这个方法可以监听readyState事件，它有5个值
        //0:请求未初始化
        //1:服务器连接已建立
        //2:请求已接收
        //3:请求处理中
        //4:请求已完成，且响应已就绪
        if(xhr.readyState === 4 && xhr.status === 200){
            alert(xhr.responseText)//alert出响应的文本
        }
    };
}
```

理解上方代码即可，因为实际工作中都是用封装好的ajax代码，这样写太繁琐。



## 巩固（自己封装一个ajax代码）

因为这是一个异步的操作，而回调函数容易出现回调地狱，现在的话都建议不使用回调函数来解决异步问题。可以使用Promise对象：

```javascript
function myajax(method,url){
    return new Promise(function(resolve){
        let xhr = new XMLHttpRequest();
    xhr.open(method,url);
    xhr.send();//发送请求
    xhr.onreadystatechange =funcion(){  //获取响应
        if(xhr.readyState === 4 && xhr.status === 200){
            resolve(xhr.responseText);  //通过resolve把值传递出来
        }
    };
  })
}
```

使用上面封装的方法来操作：

```javascript
//因为是封装成的方法，返回值是一个Promise对象，因此可以用then来获取数据
myajax("get","/data").then( res => {
    alert(res);//把resolve获得的值 alert出来。
})
```



# Ajax第三方模块（Axios）

下载Axios：（由于是前端模块，就不--save了）

cnpm install axios

由于这是前端模块要在前端用html文件引入这个js文件，不像后端用require引入。

所以我们要将Axios引入到前端的页面，所以需要将axios.js文件拷贝到静态文件目录。

> 在node_modules目录中，找到axios>dist>axios.min.js文件，拷贝到public目录中即可。然后在模板中用script标签来引入此js文件。

在模板中引入：

```javascript
<script src="/js/axios.min.js"></script>
```



用axios接收数据，首先 axios.get等方法返回值是一个Promise对象，所以可以用.then获取resolve的值res.data。

```javascript
axios.get("/fruits").then(res => {
    console.log(res.data);
})
```

用post实现添加与上面大致相同，只有post("/fruits",{ fruit:"草莓" }) 这里面多一个参数，第二个参数传的是对象，将要传的 {名：值} 传到后台中进行操作。



用put实现修改，与之前的修改又大同小异，基本上就是结合起来：

```javascript
axios.put("/fruits/1",{ fruit:"西瓜" })
```

这样就把 index=1的值 改成了西瓜。

用delete实现删除，比put要少掉第二个参数（对象），因为不需要改数据。





# 跨域请求



## 同源策略

相同ip（域名），同端口，则为同源，否则为不同源。

**同源策略**：在跨域（不同源）的情况下，不能通过ajax直接获取其他服务器的数据。



## jsonp原理

==script可以跨域获取其他服务器的js文件==，那么只需要把数据动态地放到js文件里，就可以被跨域拿到了。例：

```javascript
<script src="http://127.0.0.1:3000/xxx.js"></script>
```

这样在别的端口服务器中输入上面的代码，就可以获取到js文件的内容了。

这里注意的是，==jsonp本质上并不是Ajax==，但是功能很像，所以经常会把jsonp方法和Ajax放在一起讨论。



安装jsonp。是jquery封装好的功能，需要在public中放入jquery.min.js：

cnpm install --save koa-jsonp



## ==设置响应头==

通过设置http协议的响应头部属性`Access-Control-Allow-Origin`可以允许其他服务器对本服务进行跨域请求。

例如在8080端口中，有个3000端口的数据要传过来，要接收它：

```javascript
router.get("/getdata", async(ctx) => {
    //设置服务器的响应头信息，就可以允许跨域请求，第二个参数可以是 *，表示所有。
    ctx.set('Access-Control-Allow-Origin','http://127.0.0.1:8080');
    ctx.body = "data"
})
```

不过这有一点安全隐患。

开发过程中，一般项目是有 **开发过程**  和  **部署过程**。

**开发过程**一般是 前后端分离的，交互数据可能用大量的跨域请求。

**部署过程** 真正放到服务器是不跨域的。

因此：

==在开发阶段可以设置响应头 允许跨域请求（因为都是内部的工作流程）。直到系统上线的话就去掉响应头，再将前端程序部署到后台服务器里。==（这样即解决了安全问题，又实现了快速开发）











































