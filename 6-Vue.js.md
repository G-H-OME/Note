# Vue基础入门

不罗嗦直接开始。

在script标签的src引入来使用vue：

```javascript
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>//最新版本
```



## 入门知识

1.绑定文本：{{}}

2.绑定属性：v-bind（指令）

3.绑定事件：v-on（事件修饰符）



v-bind:  可以简写为   :   例：

```javascript
<img v-bind:src="url">
---------相当于-----------
<img :src="url">
```

  

v-on:  可以简写为  @  例：

```javascript
<button v-on:click="sayHi">按钮</button>
-------------------相当于---------------------
<button @click="sayHi">按钮</button>
```





# 创建Vue项目

基于node环境创建vue项目（正式开发）



## 使用vue/cli创建Vue项目

```javascript
cnpm install -g @vue/cli  //全局安装vue/cli
vue create hello          //用vue命令，创建项目
npm run server            //启动项目
```

==入口文件main.js==中，有一句代码：

```javascript
Vue.config.productionTip = false;//配置开发选项，友好的错误提示
```

当它为false时，报错时会以一个更友好的方式展现出来，以便于找到错误。

当它为true时，例如是产品发布后，正式运行了，就看不到错误信息。（不会被用户看到）

==ES6的模块化语法： import--引入第三方模块   export--暴露接口，可以被其他模块调用==



## 组件化开发概述

以vue为后缀的文件是vue的单文件组件

这样用组件拆分的方式开发项目，思路清晰，简洁高效，而且还可以复用相同的组件。





一个vue文件中分成三部分：

```javascript
<template> 网页模板，编写html文件 </template>
<script> js代码 </script>
<style> css代码 </style>
```



### template标签中

❗ ❗ ❗在template里面，==只能暴露一个标签==，因此有多个标签的时候，要==放在一个div里==。

#### HTML模板

> {{ }}为HTML模板，用于输出对象属性和函数返回值，
>                 里面不仅可以写js变量，
>                 还可以写一些简单的表达式，比如三目运算符，累加等等，
>                 当里面写方法的时候，必须要在vue中申明



### script标签中

首先第一步，一定要先写export，为什么呢❓

因为在main.js中，我们利用import将.vue文件引入，要使用import引入，首先我们得先将.vue文件暴露出来，==使用export 暴露文件 才能被引入==。

又由于是vue文件我们肯定要定义一个new Vue()的一个对象，==这里使用default（默认）==，因为默认就是一个Vue对象：

```javascript
<script>
export default{
	data(){//这里，因为是基于node开发，data应该是个函数了。
        return{//数据是data函数的返回值对象
            message："hello vue"
        }
    },
    methods:{//只有data有些不同，其他的没什么改变。
        showHi(){
            alert("hi");
        }
    }    
}
</script>
```

注意代码中讲到的关于data的问题 ❗ ❗ ❗



### 练手，写一个计数器功能

```javascript
<template>
  <div>
    <p>{{message}}</p>//使用HTML模板
    <button @click="add()">点击</button>
  </div>
</template>

<script>
    export default {
        data() {
            return {//记得这里要return
                message: 0
            }
        },
        methods: {//方法是对象噢
            add() {
                this.message++
            }
        }
    }
</script>
```





# 模板语法

## 指令

指令是带有 ‘v-’ 前缀的特殊属性，例如v-bind v-on等



## 条件判断

**控制元素的可见性**

v-if：不渲染dom（还有==v-else==） 实际上和 if else 使用相同，甚至还有v-else-if

v-show：渲染dom，然后将元素设置为 display:xxx

例：

```javascript
<template>
  <div>
    <p v-if="isLogin">欢迎用户！</p>
	<p v-if="!isLogin"><a href=">请登录</a></p> //这里也可以写成<p v-else>即可
  </div>
</template>

<script>
    export default {
        data() {
            return {//记得这里要return
                isLogin:true/false //当为true时，已登录则显示欢迎，否则请登录
            }
        }
    }
</script>
```



## 显示列表

v-for：说到for想到什么，没错就是for循环。这里v-for可以把集合中的数据，遍历输出出来。

在要重复的标签中（例如列表中的li）写入v-for

例：

```javascript
<li v-for="(item,index) of items" :key="index">
    {{index}} - {{item}} //这里自己想怎么输出都可以，但获取数据就是用{{ }}
</li>
```

首先第一点：v-for的值  "(item,index) of items" 意思是 获取 items(集合)中的 每一个项目和对应的索引。   

第二点：  :key  这里我们之前讲过，==v-bind:== 可以缩写成 ==:==  这里就是。所以这里是绑定key的意思，key就像id一样  确保列表的唯一性，它后面应该加一个唯一的值。



小练习：

```JavaScript
<template>
  <div>
    <table> //定义一个表格
      <thead>//表头
        <th>序号</th>
        <th>名字</th>
        <th>口头禅</th>
      </thead>
      <tbody>//表体   重复的是行，所以在tr加v-for
        <tr v-for="(value,index) of friends" :key="index">
          <td>{{index}}</td>
          <td>{{value.name}}</td>
          <td>{{value.say}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                friends: [//定义一个对象数组
                    {name: "张嘉文",say: "我可没时间狐闹"},
                    {name: "提莫king",say: "之如磐石，正在待命"}, 
                    {name: "老国灿",say: "得一"}, 
                    {name: "小宝鱼4",say: "环来环来"}
                ]
            }
        }
    }
</script>
```

结果：

![image-20210808160232674](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210808160232674.png)



## 组件嵌套

components：组件

一般将自定义组件都放到components目录下。

==组件的命名规范==：大写字母开头，驼峰命名（大写字母开头是为了防止与已有标签重名）

例如  在components目录中写了一个 Hello.vue组件，现在要引入：

```JavaScript
<template>
    <Hello></Hello>  //这样使用，就可以把Hello.vue中的页面传到这里来
</template>
<script>
import Hello from "./components/Hello.vue"  //引入组件
export default{
    //注册组件
    components:{
        Hello:Hello//这里 属性名和属性值一样的情况，可以简写为一个Hello即可。
    }
}	
</script>
```

使用组件必须 ==引入组件==后==注册组件==，才能使用。



这里还有一个特点：

引入并注册完组件后，是在标签中直接使用它。这里有两个要点

==1.在标签中使用时，不区分大小写==(例如：Hello是可以用hello代替，但记住不能与已于标签重复

==2.在标签中使用时，如果使用了驼峰命名，那两个单词中间可以用 杠 分开==

(例:MyName一般都写My-name)



# 组件传值

组件之间的关系



## 1.父级向子级传递数据

使用==属性传值==

 例：

首先父级引入了子级，在父级中有一个属性： message

```javascript
//Father.vue
<template>  //用v-bind指令绑定msg,绑定message属性传递给Child内部
    <Child :msg="message"></Child>
</template>
<script>
    import Child from "./components/Child"
    export default{
	//注册
        components:{ Child },
        data(){
            return {
                message:"Father data"
            }
        }
}
</script>
```



之后，在子级中获取message：

```javascript
//Child.vue
<template>
    <h1>{{msg}}</h1>
</template>
<script>
export default{
	props:["msg"]//接收父级传过来的属性
}
</script>
```

这样就成功地把父级的属性 传递给子级。

## 2.子级向父级传递数据

使用==自定义事件==

```javascript
//Father.vue
<template>  
    <h1>{{chlidData}}</h1>  //将传过来的数据展示
    <Child @myevent="changeData"></Child>//定义一个自定义事件，叫myevent， 
</template>                              //触发它的时候，就会触发changeData
<script>
    import Child from "./components/Child"
    export default{
	//注册
        components:{ Child },
        data(){
            return {
                childData:""  //接收子级传过来的数据
            }
        },
        methods:{
            changeData(childData){
                this.childData = childData; 
            }
        }
}
</script>
```

```javascript
//Child.vue
<template>
    <button @click="sendData">传递数据</button>
</template>
<script>
export default{
	data(){
        return{
            childData:"I'm childData"
        }
    }
	methods:{
        sendData(){//发送数据的方法
            this.$emit("myevent",this.childData)//("自定义事件"，要传的参数)
        }//this.多了艾米特 可以触发父级的自定义事件myevent
    }
}
</script>
```



## 3.非父子级传递数据

定义一个共享数据的状态（state）

与main.js平级建一个js文件**(公共数据)**：

```JavaScript
//store.js
export default{
    //状态
    state:{
        message:"hello"
    },
    setStateMessage(str){ //设置state的message，通过这个方法来操作state而不是直接赋值
        this.state.message = str;
    }
}
```

在components下建两个 平级的.vue文件:

```javascript
//brother.vue
<template>
    <div>
    	<p>{{state:message}}</p>
	</div>
</template>
<script>
import store from "../store"  //由于这个Store.js与components文件夹同级，所以要返回两次
export default{  //不用注册
	data(){
        return{
            state:store.state
        }
    }
}
</script>
```

```javascript
//sister.vue
<template>
    <div>
    	<p>{{state:message}}</p>
	</div>
</template>
<script>
import store from "../store"  //由于这个Store.js与components文件夹同级，所以要返回两次
export default{  //不用注册
	data(){
        return{
            state:store.state
        }
    }
}
</script>
```

这样就实现了数据共享。



接下来实现数据传递：

例如要实现在btother.vue中 按按钮，之后 这个公共值发送改变：

```JavaScript
//brother.vue
<template>
    <div>
    	<button @click="changeData">改变数据</button>
    	<p>{{state:message}}</p>
	</div>
</template>
<script>
import store from "../store"  //由于这个Store.js与components文件夹同级，所以要返回两次
export default{  //不用注册
	data(){
        return{
            state:store.state
        }
    },
    methods:{
        changeData(){
            store.setStateMessage("我变啦！");
        }
    }
}
</script>
```

实质是使用store内的setStateMessage(str)方法，将要传递的数据输入，执行这个方法来传值。

​                                                      **🤔 （引用调用内味）**





# 计算属性与侦听器

## 概述

computed：计算属性

watch：侦听器（监听器）

从底层 性能上来说，计算属性的性能要比侦听器要高。



==一个值的改变，会影响多个值(或处理多件事)==，使用侦听器（为了**观察**一个值）

==多个值的改变，为了得到一个结果==，使用计算属性（为了**得到**一个值）



实际开发中，**大部分问题都可以用computed解决**。

后面用到路由的时候，**监测路由的变化 就只能用监听器来实现**。

## 计算属性

data属性和computed属性定义的值都可以直接绑定在表达式中。如果某些值需要通过计算才能得到，那使用计算属性再适合不过了。



例如 名字是由 name = firstName + lastName  

```JavaScript
<template>
    <p>名字：{{name}}</p>
</template>
<script>
export default{
	data(){
        return {
            firstName:"The",
            lastName:"Shy"
        }
    },
    computed:{
        name(){
            return this.firstName + this.lastName;
        }
    }
}
</script>
```



## 侦听器

侦听==一个属性==的变化

```JavaScript
<template>
    ....
    /* 假设这里有加减按钮用来对number进行加减 */
    ....
    <p>{{sum}}</p>
</template>
<script>
export default{
	data(){
        return {
            price:5,
            number:0,
            sum:0
        }
    },
    watch:{
        number(num){//这里监听的number就是data里定义的number，参数num是修改后的number
            this.sum = this.price * this.num;
        }
    }
}
</script>
```





# 生命周期钩子



## 生命周期钩子的作用

页面加载的时候，主动执行某些程序。



### Vue实例

new Vue()创建一个Vue实例

所有的组件其实都是Vue实例 （例如App.vue）



### Vue实例的生命周期钩子（函数）

每个Vue实例在被创建时（new Vue()）都要经过一系列的初始化过程



created( )组件初始化完成的时候触发

mounted( )模板创建完毕触发

（红色的都是生命周期钩子）👇 

<img src="Vue的生命周期图.png" alt="Vue的生命周期图" style="zoom: 50%;" />

生命周期常用的作用：在页面加载的时候，通过ajax来获取数据：

```javascript
<template>
  <div>
      <p v-if="loading">loading...</p> //组件数据未加载完毕时，显示loading...
      <ul v-if="!loading">             //数据加载完毕后显示数据
        <li v-for="(item,index) of fruitList" :key="index">
          {{item}}
        </li>
      </ul>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                fruitList: [],
                loading: true
            }
        },
        created() {
            this.getData();
        },
        methods: {
            getData() {
                setTimeout(() => { //这里用计时器模拟ajax进行异步获取数据
                    this.fruitList = ["香蕉", "苹果", "鸭梨"];
                    this.loading = false;
                }, 2000)
            }
        }
    }
</script>
```



# 插槽、DOM操作、过滤器

## 三个知识点：

1.插槽

2.DOM操作

3.过滤器



## 插槽（vue中定义的slot标签）

让我们更灵活地使用组件，增强组件的扩展性

通常的传值：

```javascript
//Father.vue
<template>
    <my-button text="天神下凡"></my-button>
</template>
<script>
    import MyButton from "./components/MyButton"\
	export default{
        components:{MyButton}
    }
</script>
```

```javascript
//MyButton.vue
<template>
    <button>{{text}}</button>
</template>
<script>
	export default{
        props:["text"]//接收传过来的text
    }
</script>
```



使用插槽：

```javascript
//Father.vue
<template>
    <my-button>天神下凡</my-button> //直接写在标签中间即可
</template>
<script>
    import MyButton from "./components/MyButton"
	export default{
        components:{MyButton}
    }
</script>
```

```javascript
//MyButton.vue
<template>
    <button>
    	<slot></slot> //用一个插槽代替，vue中定义的slot标签
    </button>
</template>
<script>
	export default{
        props:["text"]//接收传过来的text
    }
</script>
```



### 具名插槽

给插槽取名后，在父组件中可以使用==<template v-slot:插槽名>==，来为特定的插槽插入特定的内容

```javascript
//Layout.vue 模板
<template>
    <div>
    	<div class="header">
    		<slot name="header"></slot>//分别给slot取名
    	</div>
		<div class="content">
    		<slot name="content"></slot>
    	</div>
		<div class="footer">
    		<slot name="footer"></slot>
    	</div>
    </div>
</template>
```

```javascript
//App.vue
<template>
    <Layout>
    	<template v-slot:header>//通过template的v-slot指令来对应插槽的名字
        	<h1>header</h1>
        </template>
		<template v-slot:content>
            <h1>content</h1>
        </template>
		<template v-slot:footer>
        	<h1>footer</h1>
        </template>
    </Layout>
</template>
<script>
    import MyButton from "./components/Layout"
	export default{
        components:{Layout}
    }
</script>
```



### 插槽在项目中的应用

1.创建更加灵活、易扩展组件：自定义button，自定义table等。

2.开发或使用UI库，了解组件制作原理。

==后续学习的ElementUI组件，基本都是基于插槽实现的。==

​                         👆PC端的一个组件库





## DOM操作

DOM：文档对象模型。

DOM节点：元素，属性，文本节点。

通过操作DOM实现页面效果：JQuery  👉 $("h1").text("hello world")



### 虚拟DOM

Vue并不是直接操作真实的DOM。它是==用js模拟做了一个虚拟DOM==，当我们的==数据更新的时候==，利用==虚拟的DOM与真实的DOM进行比较==，==得到其中的差异==。更新DOM的时候，==只更新有差异的部分==，这样就能达到一个==提升性能==的效果。



原生js的方式，想要获得元素样式，需先获得dom节点，再window.getComputedStyle(dom)来获取：

```javascript
let box = document.querySelector("#box");//获得dom节点
let style = window.getComputedStyle(box);//获得其样式
console.log(style.height)
```



 更简便的获取dom的方式：（使用ref）

```javascript
//template
<div ref="box"></div> //给要获取dom节点的元素 设置一个ref

//script
let box = this.$refs.box //vue提供的$refs 获取真实dom
```



### 获取真实DOM总结

1.这里主要学习如何获取真实DOM，进一步操作请参照DOM文档。

2.Vue应用开发的过程中，大部分情况是不需要获取真实DOM的。不要把jQuery的思想带到Vue中来。





## 过滤器

通过固定算法重新组织数据 ，过滤器是可以复用的。

例：（将一个字符串"hello"，变成"h,e,l,l,o"）

```javascript
<template>
    <h1>{{message | mySplit}}</h1> //前面是要传入的值，后面是使用的过滤器
</template>
<script>
export default {
	data(){
        return{
            message:"hello"
        }
    },
    filters:{
        mySplit(value){
            return value.split("").join();
        }
    }
}
</script>
```

 



# 表单

一、通过表单可以向服务器发送数据

二、常用表单控件包括：

1.文本输入框

2.密码输入框

3.下拉菜单

4.单选框

5.复选框

6.提交按钮



## 数据双向绑定

通过**v-model**指令在文本输入框中创建双向数据绑定。

```javascript
<h1>{{message}}</h1>
<input type="text" v-model="message">  
```



之前学的表单提交 ：

```JavaScript
<form action="/login" method="post"> //提交后，会实现路径变化和页面跳转
    <input type="text" v-model="message">
    <button>提交表单</button>
</form>
```



用vue来做项目是**使用ajax实现表单的提交**（异步数据交互）:

```javascript
//提交事件不应该实现页面跳转，而是触发事件
//template
<form @submit.prevent="postData"> //提交后，会实现路径变化和页面跳转
            //👆 这里加 prevent，取消表单的默认行为 
    <input type="text" v-model="message">
    <button>提交表单</button>
</form>

//script
data(){
    return{
        message:""
    }
},
methods:{
    postData(){
        console.log(this.message);
        //ajax
    }
}
```

具体ajax操作在下一个知识点 数据交互 中写



表单控件很多，一般情况下提交多项数据的话，我们会将数据都放到**一个对象**中：

```JavaScript
//template
<form @submit.prevent="postData"> //提交后，会实现路径变化和页面跳转
            //👆 这里加 prevent，取消表单的默认行为 
    <input type="text" v-model="username">
    <input type="password" v-model="password">
    <button>提交表单</button>
</form>

//script
data(){
    return{
        formData:{//多项数据，放到一个对象中
            username:"",
            password:"" 
        }
    }
},
methods:{
    postData(){
        console.log(this.formData);
    }
}
```



对**表单其他控件**的数据绑定：

```javascript
//template

//用户名username
<div>
    <label>用户名：</label>
    <input type="text" v-model="formData.username">
</div>

//密码password
<div>
    <label>密码：</label>
    <input type="password" v-model="formData.password">
</div>

//选择框select - option
<div>
    <label>爱好：</label>
	<select v-model="formData.hobby">
    	<option value="basketball">篮球</option>
		<option value="football">足球</option>//注意，并不会获取到"篮球"或"足球"，所以
    </select>                                //必须传递value，来代表它们*****
</div>

//单选框input.radio
<div>
    <label>性别：</label>
	<label>   男</label>
	<input type="radio" v-model="formData.sex">
    <label>   女</label>
	<input type="radio" v-model="formData.sex">//这两个v-model要一样，才能实现单选
</div>

//复选框input.checkbox
<div>
    <label>技能：</label>
	<label>前端</label>
	<input type="checkbox" value="前端" v-model="formData.skills">
	<label>java</label>
	<input type="checkbox" value="java" v-model="formData.skills">
</div>

//script
export default{
    data(){
        return{
            formData:{
                username:"",
                password:"",
                hobby:"",
                sex:"",
                skills:[]//复选框，所以这里使用数组
            }
        }
    }
}
```



# 数据交互

## 知识点回顾

http协议：前端（浏览器）发送请求，服务器给予响应。

请求方法：get（查询）、post（添加）、put（修改）、delete（删除）

ajax：**不刷新页面** 与后台交互数据

koa：基于node的web框架

 

## 首先 下载引入axios

下载axios ：cnpm install --save axios   

引入axios ：import axios from "axios";

 

## 后台使用 koa

这里需要下载 koa2-cors 来允许跨域

```javascript
//serve.js
const cors = require('koa2-cors');//允许跨域
app.use(cors());
//有一个fruits数组存放数据
//get post delete put方法
//这里端口设置为3000
```



## 使用axios获取数据、提交数据、删除数据

```javascript
//App.vue
methods:{
    //获取数据
    getFruitList(){
        axios.get("http://127.0.0.1:3000/fruits").then(res => {//跨域，所以路径要写完整
             this.fruitList = res.data;
        })
    },
    //添加数据
    postData(){
        axios.post("http://127.0.0.1:3000/fruits",{
            fruit:this.fruit //App.vue中定义了一个fruit 用来接收键盘传入的数据，将它传给后台
        }).then(res => {//响应 自己再次获取后台的数据，然后更新水果列表
            this.getFruitList();
        })
    },
    del(index){//索引由template里调用del方法的按钮传过来
        axios.delete(`http://127.0.0.1:3000/fruits/${index}`)//用的模板字符串，因为要用到
        	.then(res => {								  //传入${index}
            	this.getFruitList();
        })
    },
    created(){
        this.getFruitList();//初始化数据
    }
}
```

## 项目实践（先导）

1.在真实的项目中，我们会对**axios进行进一步的封装，以便于更轻松的发送请求。**

2.通过**配置文件**，设置项目**开发**与项目**部署**的**无缝切换**。

 

# ==路由==

创建项目的时候选择router；

```javascript
<div id="app">
<div id="nav">
    <router-link to="/"> HOME </router-link> |
	<router-link to="/about"> About </router-link>
</div>
<router-view/>
</div>
```

**router-link**：相当于a标签，可以进行**页面跳转**，但是a标签跳转的属性是href属性，而router-link的跳转属性是**to**

**router-view**：link为导航菜单，而view显而易见是**页面的内容**，实际页面的内容在这里面实现。



## 实例

1.制作一个简单的博客系统

2.增加登录功能



### ==本地存储==

```javascript
localStorage.setItem("usr",this.username);  //把输入的username存储到usr了。
localStorage.getItem("usr"); //在别的页面可以使用此方法得到username了

localStorage.clear();//清空localStorage
```



### ==编程式导航==

```JavaScript
this.$router.push("/路径");  这样就可以随意跳转
```



### 监听器（监听路由跳转）

vue程序是一个单页面应用，通过路由切换页面并没有刷新导航栏的内容，因此 想要实现登录过后，导航栏显示“欢迎：xxx”的效果必须手动刷新一下，这样==用户体验就不好了==。

这里可以使用监听器，监听路由路径的变化，使之自动刷新

```javascript
watch:{
	'$route.path':function(){//监测路由的路径，当路由切换的时候触发事件
		this.username = localStorage.getItem("usr")
	}
}
```



### 导航守卫（未登录只能查看登录页）

```javascript
//index.js
router.beforeEach((to,from,next) => {//回调函数中有三个参数:访问哪里，从哪访问，继续访问
    if(to.path !=="/login"){//如果访问路径不是登陆页，则进行判断
       if(localStorage.getItem("usr")){ //如果能检测到usr有值
           next(); //则继续访问
       }else{      //否则
           next("/login") //重定向到登录页
       }
    }else{
           next();//不会被拦截
    }
})
```



# ElementUI

## UI框架

### 一、基于Vue的 UI 框架

1.ElementUI

2.vux

3.vant

...

### 二、基于jQuery的 UI 框架

1.bootstrap



## 引入ElementUI

1.cnpm install --save element-ui (简写：cnpm i element-ui -S)

```javascript
import ElementUI from 'element-ui' //引入elementui
import 'element-ui/lib/theme-chalk/index' //引入elementui的样式
Vue.use(ElementUI); //用use把第三方插件加载到vue应用中
```

==在Vue中引入第三方插件 都必须要使用Vue的use方法来将其加载到vue应用中==



## 组件示例

1.按钮

2.表单

3.表格

对于初学者来说，阅读文档是必备技能 😟



# 项目部署

## 如何发布一个基于Vue的web项目

1.npm run build

 将整个vue项目打包成一个静态文件(包括html、css、js)

2.再放到服务器上发布



## 配置文件

.env.development   开发阶段的配置文件

.env.production   （生产环境）发布vue之后的配置文件 

注意：命名变量必须以VUE_APP_XXXX命名

创建配置文件：

![image-20210815234340304](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210815234340304.png)

例如之前说到的，在开发阶段，我们获取后端的数据要从"http://127.0.0.1:3000/student"，而在部署阶段前面那一长串太过复杂，进行简化：

```javascript
//.env.development
VUE_APP_BASE_URL = "http://127.0.0.1:3000"
```

```javascript
//.env.production 
VUE_APP_BASE_URL = "" //不涉及跨域了，不需要完整的路径
```

在使用中：

```javascript
//App.vue
methods:{
    onSubmit(){
        axios.post(`${process.env.VUE_APP_BASE_URL}/student`,{
            student:this.form
        })
    }
}
```



# Vue-cli

vue提供了一个官方的CLI，为==单页面应用(SPA)==快速搭建繁杂的脚手架。

安装：npm install vue-cli -g (通过vue list查看)

创建一个demo：vue init webpack projectname (基于webpack，要用它打包)

创建完毕以后，安装依赖：npm install



# Webpack

webpack是一个现代JavaScript应用程序静态模块打包器。

安装：npm install webpack webpack-cli -g















