# ECMAScript（ES）

ECMAScript是JavaScript语言的标准（可以理解为是js的版本）

比较重要的版本：ES5和ES6（ES2015）

后续的版本统称为：ES2015+

ES2015是完全向下兼容的。



# 变量与数据类型

## 声明变量

var：声明变量（ES2015中使用let声明变量，后续会记载let以及它们的区别）

 =  ：为变量赋值（不是等于的意思，而是赋值的意思，将右侧的值放入左侧的容器） 

var  声明变量 = 赋值

可以用逗号分开，定义多个变量

var s1 = "hello",s2 = "world";



## 变量命名规范

1.变量名要见名知意

2.变量名可以是字母、下划线、$、还有数字；但不能数字开头

3.小写字母开头，多个单词-第二个单词首字母大写（驼峰命名）



## 数据类型

数值（Number）：100，1，2...

字符串（String）：“哈哈哈”

布尔（Boolean）：true，false

未定义（Undefined）：undefined

空（Null）：null

对象（Object）：{ }





# 表达式与运算符

## 字面量

例如："hello world"、100

​      console.log ( 字面量 )  



## 表达式

通过运算符将变量、字面量组合起来，就是表达式：

==每一个表达式都有一个固定返回值==（表达式的结果）



## 数学运算符的运算规则

### 数学运算符

+加、-减、*乘、/除、%取模（求余数）、++递增、--递减、+正号 -负号



### 加性操作符

#### 加法

- 当左右两侧的操作数，任何一侧为字符串，另一侧非字符串的类型则转为字符串，进行字符串的拼接 

- 当左右两侧都为Number类型的情况下：

  1.左右两侧均为数值，执行常规的加法计算；

  2.如果有一个操作数是NaN(NaN属于Number)，则会返回NaN；

- 如果操作数是Boolean/undefined/null，则会（根据规则）转为Number，然后再进行计算，如果其中转化结果为NaN，则结果就是NaN

  例：

  ```javascript
  <script>
  	alert('1'+ 2 + 3 );//从左至右计算，先将2转化为字符串，拼接后'12'+3，将3转化为字符串...
  </script>
  ```

  输出：123

#### 减法

- 当左右两侧都为Number类型的情况下：

  1.左右两侧均为数值，执行常规的减法计算；

  2.如果有一个操作数是NaN(NaN属于Number)，则会返回NaN；

- 如果操作数是Boolean/undefined/null，则会（根据规则）转为Number，然后再进行计算，如果其中转化结果为NaN，则结果就是NaN

#### 减法为什么是加性操作符呢？？

计算机只能进行加法，减法实际上是 a + (-b) 



#### 隐式类型转换规则

我们之前接触到的通常是 **强制类型转换**，由我们主动控制。

隐式类型转换：因为我们的某些操作引起JS自动帮我们进行转换，这样的转换是在JS内部自己实现的。

 

## 比较运算符（返回值只有true/false）

大于小于简单的就不用说了，

== 等于                ：  **不比较数据类型**，如果是不一样的数据类型，会进行转换再比较

===恒等于           ：  既比较 数据类型 又比较 值

！==非恒等于

但由于==需要进行类型转换，会有所消耗资源，因此之后的代码中，我们均使用恒等于

同理得 非等于中 我们也要多用  ！==  节约资源

总结：  等于时我们使用 ===

​            不等于时我们使用!==



## 逻辑运算符

&&  逻辑与

||   逻辑或

！   逻辑非



## 赋值运算符

![image-20210728025754949](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210728025754949.png)





# 条件语句



## 控制流程

1.按顺序执行（之前学的内容都为顺序执行）

2.按条件执行

3.循环执行



## 条件执行



### 语句块

多条js语句组成的语句，用大括号括起来，就是一个语句块。



### if语句

根据if后面的条件，决定是否执行后面的语句块。

if（条件）{ 语句块 }

<img src="C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210728131205961.png" alt="image-20210728131205961" style="zoom:80%;" />



### switch语句

可以实现多重选择，某些情况相对于if语句更简洁。

switch（表达式）{

​	case 值1：语句1；break；

​	case 值2：语句2；break；

​	.......

​	default：语句4；

}

<img src="C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210728131402938.png" alt="image-20210728131402938" style="zoom:80%;" />





### ==条件运算符==

boolean ？value1 ：value2

（简化条件语句）

含义：如果前面的表达式结果为true？ 那么返回值为value1，为false 返回值为value2。



# 循环语句

## while

根据条件循环

与if很像，但是if是判断是否执行，而while是判断是否==循环执行==。

<img src="C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210728153145677.png" alt="image-20210728153145677" style="zoom: 67%;" />



## do...while

先执行一个语句块，然后再根据条件循环

与while的区别是：while可能一次都不执行，但do...while至少会执行一次。

```
do{
	语句；
}while(条件)
```



## for

根据条件循环

大部分情况下，while和for是可以互换的。

例：

```javascript
for(var i = 1;i<=100;i++){
	if(i % 2 === 1){
		console.log(i)
	}
}
```



## 中断循环

break      ：结束本次循环，并且**结束所有循环**

continue ：结束本次循环，但会**继续后续的循环**



# 函数基础

## 函数概述

函数是一个**可执行**的**语句块**（通过function关键字声明）

声明的时候不执行语句块

调用函数时执行。

```
//声明函数
function fun(){
	//语句...
}
//调用函数
fun();
```



## 传递函数

通过参数可以让函数的功能更具扩展性。

形参：声明函数的时候可以定义；  相当于数学中f(x) 里面的  x。

实参：调用函数的时候传递实参；  相当于数学中f(5) 里面的 5。

```
function fun(x){
	var result = 3 * x + 4;
	console.log(result)
}
fun(5);//内部执行3*5+4=19

//得到结果为：19
```

一次可以传递多个参数例如：

```
function fun(x,y){
	var result = 3 * x + 4 * y;
	console.log(result)
}
fun(2,3);//内部执行3*2+4*3=18

//得到结果：18
```



## 返回值

返回值表示函数运行之后的结果。

将返回值赋值给变量。



## 理解函数

可以把函数理解为一个生产车间：

<img src="C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210728161115476.png" alt="image-20210728161115476" style="zoom: 50%;" />





# 对象

世间万物皆对象



## js中的对象

六种数据类型中的一种。

==属性的无序集合==：

```
var cat = {
	name:"mm",
	age: 2
}
```

获取cat中name的方法（两种）：

```
cat.name
cat["name"]
```



## 方法

在对象中，有许多属性，当这个属性的值是一个函数，则称为方法。

```
var cat = {
	name:"mm",
	age: 2，
	sayName:function(){     //sayName属性的值是一个function函数
		console.log("我叫mm");
	}
}

cat.sayName()//调用cat中的sayName方法
```



## this

方法中的this可以指向对象本身。

```
var cat = {
	name:"mm",
	age: 2，
	sayName:function(){     
		console.log("我叫" + this.name); //这里的this，指的就是 对象cat
	}
}
```



## 对象的分类

自定义对象

内置对象（例如Date，获取当前时间）

宿主对象（document）

第三方库的对象（jQuery，Vue等）





# 数组

## 数组概述

数组是一个特殊的对象，可以按顺序存储数据。

以下两种方法可以创建数值类型的数组：

```
var list = [1,2,3,4,5];
// new 构造函数 →    创建新的对象
var list = new Array(1,2,3);
```



获取数组中元素的方法

```
var item = list[0];//获取到了数组中第1个元素
```

上方代码中，list[0]，中的0是下标。统称为==index ：索引==

[index]，索引 是从0开始的。



数组有一个常用的属性，==length==：可以获取数组的长度（有多少个元素）



## 遍历数组

1. while循环遍历
2. for循环遍历
3. for in遍历（i 为索引）
4. for of遍历（i 为值）
5. map方法遍历（数组自带的方法）



### for in遍历数组（i为索引）

==for==(var **i** ==in== list){

​	console.log(list[i]);

}

i in list 会让 i 成为list的索引



### for of遍历数组（i为元素）

==for==(var **i** ==of== list){

​	console.log( i );

}

i of list 会让 i 成为 list内的值





### map方法遍历数组（数组自带的方法）

list.map(  function(value,index){

​		console.log(value);

}   );

list.map(); 里面需要参数，

这个参数 是一个 函数（叫做==回调函数==）：

```
//回调函数
function(value,index){//形参为 值，索引
	console.log("第"+(index+1)"+个元素是："+value);
}                      ↑这里因为index是从0开始的，所以+1
```







## 数组的常用方法

1. map：可以**遍历**数组
2. push：可以在数组的**结尾** 追加元素     ===> pop：可以在数组的**头部** 插入元素
3. sort：可以将数组进行**排序**
4. filter（过滤器）：可以对数组进行**筛选**
5. join ： **连接数组**，连接完之后数组会变成一个**字符串**

​    ==string的split方法 和 数组的join方法  正好是逆向的==：**分裂**，将字符串分裂为一个**数组**

   6.（补充）splice：向 / 从数组中添加 / 删除项目，然后返回被删除的项目。



### filter（过滤器）

过滤器和map一样 需要使用到  **回调函数**

演示：将list中 大于等于3 的元素 传入新数组newList中：

```
var list = [1,5,4,8,3]
var newList = list.filter(function(item){
	if(item >= 3){
		return true;
	}
})
```





### join

**连接数组**，连接完之后数组会变成一个**字符串**

```
var list = ["a","b","c"];
var str = list.join();
console.log(str);
```

输出结果为：  a，b，c

可以通过给 join（）里加参数，**改变**元素之间的 **连接符号**

```
var list = ["a","b","c"];
var str = list.join("");//空字符串，则结果为： abc；若为"+",则结果为：a+b+c
console.log(str);
```





### ==拓展（字符串的split方法）==

==string的split方法 和 数组的join方法  正好是逆向的==：**分裂**，将字符串分裂为一个**数组**

```
var str = "banana";
var list = str.split();
console.log(list);
```

输出结果为： ["banana"]

可以通过给split（）里加参数，设置拆分的连接符，例如：

```
var str = "banana";
var list = str.split("");
//空字符串，会拆分所有单词：["b","a","n","a","n","a"]
//若输入"n",则会以n为连接符 进行拆分：["ba","a","a"]
console.log(list);
```



### ==splice==

有三个参数

splice（要删除元素的索引，删除几个元素，将删除的元素替换成xx）；



## 结合数组与对象

```
var list = [
		{name:"大聪明",age:2,sex:"男"},
		{name:"中聪明",age:2,sex:"女"},
		{name:"小聪明",age:2,sex:"男"}
]
```

获取数据：   list[0].name





# 内置对象

## 内置对象概述

JavaSript语言中提供了一些对象，让开发人员可以使用一些现成的功能。





## 常用的内置对象

Array-数组

Math-数学

Date-日期

RegExp-正则表达式

Promise：专门用来解决异步问题的对象，在异步编程中会讲到。

### Math对象

Math.floor()：向下取整

Math.random()：0-1的随机数

Math.abs()：绝对值

Math.sqrt()：开方

Math.pow(2，4)：乘方-----这里表示 2 的 4 次方



#### 拓展 获取指定范围的随机数

获取1-10的==整数==随机数：

```
var number = Math.floor(Math.random()*10 + 1)

//首先Math.random() 获得 0-1之间的随机数 即 0<x<1;
//后使之乘以10 则 0<x<10 再加1 则 1<x<11
//最后用Math.floor向下取整，则 1<=x<=10,且x为整数。
```





### Date对象

可通过var  d = new Date(); 来获取一个 **当前时间 Date** 对象

可以在（）中输入时间  就会获取 输入的时间

通过d来获取信息：

```
d.getFullYear();  //获取年份
d.getMonth();     //获取月份 （月份是从0开始的，所以会比真实月份少1）
d.getDate();
d.getHours();
d.getMinutes();
d.getSeconds();
d.getTime();//时间戳：格林威治时间1970年1月1日0时0分0秒起至现在的总毫秒数
```





#### 时钟

在控制台显示当前的时间

使用下面的方法：



#### 计时器方法setInterval（window对象的一个方法）

```
setInterval(function(){},ms)
```

第一个参数是个 ==函数== ，第二个参数是个 ==毫秒数==，意思是：

每隔 **毫秒数** 的时间，就会执行一次这个 **函数**。（偷偷告诉自己1s=1000ms）



#### 在控制台输出当前时间，每秒一次

```
setInterval(function() {
            var d = new Date();
            var hour = d.getHours();
            var min = d.getMinutes();
            var s = d.getSeconds();
            console.log(hour + ":" + min + ":" + s)
        }, 1000)
```



### 正则表达式

正则表达式可以用来==匹配字符串==

通过正则表达式，可以实现字符串的截取或按规则替换和验证字符串内容。



正则表达式独立于语言，很多语言都支持正则表达式。（**并非JavaScript特性**）



#### 在Js中创建正则表达式对象

```
var reg = new RegExp("123");

var reg = /123/;    //简写
```



#### 正则表达式的语法

^：开头

$：结尾

[]：范围  [a-z]表示输入a-z（小写）；[a-zA-Z0-9_]中间不需要分隔，代表**数字字母下划线**，可简写为   **\w**

{}：位数

()：分组

+：匹配1位或多位，同{1,}

？：0位或1位，同{0,1}

.   ：匹配所有

\   ：转义

\d ：数字，同[0-9]

\w ：数字、字母、下划线

\s：空格或换行



#### 匹配邮箱格式

通过 RegExp的test方法，验证字符串的内容是否符合要求。

```javascript
var reg = /^\w{6,12}@\w{1,5}\.com$/
        var str = "1120314609@qq.com"
        if (reg.test(str)) {
            console.log("验证成功^v^")
        } else {
            console.log("验证失败QAQ")
        }
```

输出结果：验证成功^v^



#### 字符串替换

通过replace（x，y）方法，替换字符串中想被替换的字符。

```
var str ="123abc456def"
var result = str.replace("a","9")
console.log(result);
```

输出结果：1239bc456def



可以再通过把replace第一个参数 换成  正则表达式，来匹配字符

```
var str ="123abc456def"
var reg = /[a-zA-Z]/g //这里的g 表示全局的意思
var result = str.replace(reg,"")
console.log(result);
```

输出结果：123456



#### 截取字符串

通过exec方法来截取信息

```
var str = "2019-12-01"
var reg = /^\d{4}-\d{2}-\d{2}$/;
console.log(reg.exec(str));
```

获得的是一个数组：

![image-20210729171039362](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210729171039362.png)

这里可以再加上 上面说到的 （）分组

```
var str = "2019-12-01"
var reg = /^(\d{4})-(\d{2})-(\d{2})$/;
console.log(reg.exec(str));
```

获得的结果：

![image-20210729172654268](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210729172654268.png)

接着可以以数组形式取出想要的数据，例：

```
var str = "2019-12-01"
var reg = /^(\d{4})-(\d{2})-(\d{2})$/;
var result = reg.exec(str);
console.log(result[1]);
```

输出结果：2019



#### 实际开发中...

大部分情况下，是不需要我们自己写正则表达式的。

邮箱或电话的验证方式：有很多现成的解决方案（比我们自己想的要全面）。

截取和替换数据，掌握一些常用的语法。





# ES2015基础语法

## 概述

1.变量

2.常量

3.模板字符串

4.解构赋值



## 变量

使用 ==let== 替代 var

**让变量更加规范**

### let和var的区别

#### 1.块级作用域

let在语句块中定义的变量，在语句块外部是无法使用的，而var可以。

```
{
	var str1 = "niu"
	let str2 = "shuai"
}
console.log(str1);    //这样可以输出结果niu
console.log(str2);	 //无法输出shuai
```



#### 2.不存在变量提升

在变量声明语句的上方无法使用变量。

```
console.log(str1);
console.log(str2);

var str1 = "niu"  //输出：undefined  （能使用str1，但它还没有值）
let str2 = "shuai"//输出：报错，str还未声明。
```



#### 3.不允许重复声明

用var的时候，可以重复声明一个变量，不过后声明的会覆盖前声明的。而let会直接报错。

```javascript
var str = "niu"				|      let str = "niu"
var str = "shuai"            |      let str = "shuai"
console.log(str)
//输出：shuai                       //输出：报错，不能重复声明一个变量
```







## 常量

1.const定义常量

2.定义（被赋值）之后不可以修改

3.不变的值用常量声明（例如：PI=3.14/函数表达式）

4.函数表达式的例子：

```
//ES5版本中
var fun = function(){....}
fun();
//ES6版本中
const fun =function(){....}  //当然，这里const换成let也是可以的
fun();
```



5.对象声明可以使用常量

```
function getObject(){
	return {name:"xiaoming"}//返回一个对象
}
const xm = getObject();
xm.name = "xiaohong";//这里的修改，只是相当于改变对象的一个属性，所以xm这个常量没有                        影响，并不是修改了常量。
console.log(xm.name);//这里可以输出：xiaohong
```



6.引入外部模块可以使用常量（在node中会使用）



## 模板字符串

#### 1.支持换行

模板字符串使用的不再是 "" 和 ''  而是反引号(1左边的按键) ``

```
let str = `hello
        world`  //前面的空格也会算在里面
console.log(str)
```

那么输出为：hello

​                                 world 



#### 2.支持嵌入变量

通过嵌入参数的方式，实现字符串的连接，更直观简便。

```
let year = "2020";
let month = "10";
let date = "10";
//let result = year + "年" + month + "月" + date + "日";
let result = `${year}年${month}月${date}日`;
console.log(result)//上面两条语句输出结果相同。
```





## 解构赋值

### 1.数组的解构赋值

通过数组解构赋值给多个变量赋值，方便值的调换：

```
let n = 10;          |let [n,m] = [10,20];
let m = 20;          |    [n,m] = [m,n]
let temp;            |    
temp = n;            |
n = m;               |
m = temp;            |
这样就实现了值的调换	     这样更方便实现		
```



### 2.对象的解构赋值

{ }代表一个对象，可以直接用 { 属性 } 来直接取得这个对象的属性值

```
let {name,sex} = {name:"xiaoming",age:10}
console.log(name)//就会得到：xiaoming

    //这里，就算sex和name反过来，还是一样，会获得自己的值
let {sex,name} = {name:"xiaoming",age:10}
console.log(name)//依旧会得到：xiaoming
```



### 3.通过解构赋值传参

可以说是 对象的解构赋值 的进一步的应用

```javascript
let xm ={name:"xiaoming",age:10};|let xm ={name:"xiaoming",age:10};
function getName(obj){           |function getName( {name} ){//只拿到name
	return obj.name;             |	  return name;	
}                                |}
let result = getName(xm);        |let result = getName(xm);
console.log(result);             |console.log(result);
```





# 函数进阶

## 总结一下函数知识点：

### 一、声明函数：

1.一次声明，多次使用的语句块。

2.参数：形参、实参。

3.返回值：函数运行的结果。



### 二、函数声明提升

### 三、匿名函数

==回调函数==：将**匿名函数**作为参数传递给另一个函数、方法。

```
setInterval(function(){...},1000);
//function(){...} 这就是一个匿名函数，没有函数名。
```



### 四、函数表达式

==函数表达式 不具有 函数提升==

```
const fun = function(){...}
```



### 五、方法：当函数属于某一个对象时 被称为 方法。

ES2015对方法进行了一个简化：

```
const cat ={
		name:"猫猫",
		sayName:function(){//这是之前的写法
			console.log(this.name);
		}
}
cat.sayName();
-----------------------------------------------------
const cat ={
		name:"猫猫",
		sayName(){         //现在可以省略掉:function
			console.log(this.name);
		}
}
cat.sayName();
```



## 进阶内容



### 设置函数默认值

ES2015的语法可以为函数的参数设置默认值：

```
function fun(x=10,y=20){
	return x+y;
}
```





### 立即执行函数

方法被声明之后直接调用；

不可以多次调用；

可以用来**封装代码**；

某些第三方库实现封装；

（ES2015**有模块化的语法**，所以立即执行函数现在也不常用了）

```
(function(){...})();
//感觉形似 cat.sayName(); 前面是方法，后面是一个传参括号
```





### 作用域链

每一个函数都会创建一个新的作用域。

函数外部无法访问函数内部的值。

函数内部的值可以访问函数外部的值，依次往外找，如果找不到就会报错。

函数执行的特性：函数执行完后，函数内的变量就会被销毁，在内存中不存在。



### 闭包

闭包函数：声明 在一个函数中 的函数，叫做闭包函数。

闭包：内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回了之后。



利用闭包实现了代码的封装；

闭包的特性：内部函数未执行完(被返回)，外部函数即使执行完成，外部函数中的变量也不会被销毁。

```javascript
function fun1(){
	let n = 10;
	let m = 20;
	function fun2(){
		console.log(n+m);
	}
	return fun2();//返回了fun2(),代表内部函数未执行完。
}

const f = fun1();//把fun2()传给了f
f;//执行f 即 执行fun2()
```

输出结果：30



#### 代码封装

利用 **闭包** 和 **立即执行函数** 进行代码封装：

```javascript
const module = (function(){
	let a =10;
	let b =20;
	function add(){
		console.log(a+b)
	}
	return add();
})();
```

这样就做到了 把add()赋值给module，外界可以通过module来调用add()，但是又无法获取a和b

不过这是**ES5**的一个**模块化语法**







### ==箭头函数==

#### 1.简化函数的写法

```
function(x,y){
	return x*y;
}
----------------可以简化为：
(x,y) => {//去掉function 并在参数与语句块之间加一个箭头
	return x*y;
}
----------------甚至，如果只有一个参数，并且只有返回值，可以更简化：
x => x*x;
```



#### 2.箭头函数中的this

使用function定义的函数，this取决于调用的函数。

使用 箭头函数 ，this取决于 函数定义的位置。

例：

```
const cat = {
	name:"喵喵",
	sayName(){
		setInterval(function(){//setInterval是window对象的方法，调用它的是                 console.log(this.name)                              window
		},1000)
	}
}
cat.sayName();
```

输出结果：什么都没有。。

因为这里setInterval是window对象的方法，调用它的是window。

普通函数的 this 取决于调用的函数。因此这里的 this 指的是 window。它没有name属性。



而使用箭头函数就可以成功取得值：

```
const cat = {
	name:"喵喵",
	sayName(){
		setInterval(() => {//去掉function，在 参数 和 语句块 之间加箭头
        	console.log(this.name)                              
		},1000)
	}
}
cat.sayName();
```

输出结果：喵喵（每秒输出一次）

使用 箭头函数 ，this取决于 函数定义的位置。

这里实在sayName中定义方法，因此 this 指向的是sayName的对象，cat。



==因此以后  **将函数作为参数传给另一个函数**  的时候 通常使用 箭头函数==





# 面向对象

## 基本概念

类：类型、模板、统称。

对象：是类的一个实例，会具体到某一个事物上。

继承：狗类继承至哺乳动物类，继承后，子类可以使用父类的属性和方法。



## 新语法与旧语法

ES5面向对象语法（prototype）

ES6面向对象语法



### ES5面向对象的知识

构造函数：用于创建对象的函数。

原型对象：prototype

原型链：实现继承（了解即可，现在有更好的方法）



#### 用构造函数实现一个类

构造函数的函数名，**首字母大写**

构造函数是用来 **创建对象** 的

```
function Dog(name,age){
	this.name = name;
	this.age = age;
}
var dog = new Dog("旺财",2);//通过new 创建一个对象，狗类的实例。
```





#### prototype

**prototype**是**构造函数的**一个**属性**

==通过 原型对象，为构造函数生成的对象赋予新的方法。==（可以用在js内置对象上）

可以在prototype属性上 添加一些方法，可以被所有这个(构造函数创建的)类的所有实例使用。

```JavaScript
function Dog(name,age){
	this.name = name;
	this.age = age;
}
Dog.prototype.sayName = function(){
	console.log(`我的名字是${this.name}`)
}
var dog = new Dog("旺财",2);
dog.sayName();
```

输出结果：我的名字是旺财





#### 原型链

万物皆对象，任何对象都是属于object的实例

==函数必然有prototype和\_proto\_两个属性，所有的函数(包括自定义函数)都是Function实例的对象。==

==而Function其实是Object的实例对象。==

实例的对象通过\_proto\_属性连接到构造函数的prototype属性上，就这样逐层向下继承。

```javascript
function Animal(name){
	this.name = name;
}
Animal.prototype.sayName(){
	console.log(`我是${this.name}`);
}

function Dog(name){
	this.name = name;
}
Dog.prototype = new Animal();//Dog的原型对象 赋值为 Animal的一个实例
// Dog 的上一层不再是Function 而是Animal
let dog = new Dog("旺财");
dog.sayName();//因此可以调用Animal的方法
```





### ES6(2015)面向对象语法

Class关键字

继承



#### Class关键字

ES2015支持类的概念了，可以用class关键字

```
class Dog{
	constructor(name,age){  //构造函数
		this.name=name;
		this.age = age;
	},
	sayName(){ //直接在类中添加方法，不需要用prototype
		console.log(`我是${}`);
	}
}

let dog = new Dog("旺财",2);
dog.sayName();
```





#### 继承

同样使用class 创建类。

```javascript
class Animal{
	constructor(name){
		this.name = name;
	}
	sayName(){
		console.log(`我是${this.name}`);
	}
}
class Dog extends Animal{ } //extends 继承

let dog = new Dog("旺财");
dog.sayName();
```

输出结果：我是旺财



如果想在Dog类中 定义自己的构造函数，必须接收父类传来的参数 用super

```javascript
class Animal{
	constructor(name){
		this.name = name;
	}
	sayName(){
		console.log(`我是${this.name}`);
	}
}
class Dog extends Animal{
	constructor(name,age){//这里父级的name要写进来
		super(name);
		this.age=age;
	}
} 

let dog = new Animal("旺财",2);
dog.sayName();
console.log(dog.age);
```

输出结果：我是旺财   和    2

​                    

# DOM基础



## DOM：文档对象模型

是一套 标准编程接口。

我们通过DOM这套接口来操作html元素。



## 树状结构中的==节点类型==

元素节点

属性节点

文本节点



网页效果：操作元素节点、属性节点、文本节点，以及修改元素的样式



## document对象

DOM通过document对象，为开发者提供了大量的接口(api)来操作DOM树。



### 获取节点

document.getElementById( ); 

document.getElementByClassName( );

-------------------------------------------------------------------

document.querySelector( );

document.querySelectorAll( );



element.innerHTML：获取和修改元素内的所有内容。

​      ↑

指的是获取到的元素



#### getElementById( ); 返回值是一个dom节点



#### getElementByClassName( );返回值是一个dom节点的集合



#### element.innerHTML：获取和修改元素内的所有内容：

通过dom节点的innerHTML属性，设置元素内的一些文本节点。

1.使用getElementById( )返回一个dom节点 来使用

```JavaScript
<body>
	<h1 id="title">hello</h1>
</body>

<script>
	let h1 = document.getElementById("#title");//获取dom节点
	h1.innerHTML = "芜湖"
</script>
```

输出结果： 芜湖



2.使用getElementByClassName( )返回一个dom节点的集合，通过遍历集合来获取节点 来使用。

```javascript
<body>
	<button class="btn">1</button>
	<button class="btn">2</button>
	<button class="btn">3</button>
</body>

<script>
	let btns = document.getElementByClassName(".btn");//获取dom节点的集合
	for(let i in btns){//使用for in 遍历
        btns[i].innerHTML = "按钮";
    }
</script>
```





#### document.querySelector( );

与getElementById一样的效果。

```
let h1 = document.querySelector("#title");
```



#### document.querySelectorAll( );

与getElementByClassName一样的效果。

```
let btns = document.querySelectorAll(".btn");
```



## ==强调==

==获得dom节点的集合，之后使用的时候切记！==

==切记！切记！切记！==

==因为是集合，所以使用的时候需要这样：btns[ i ]==



## 事件类型

### click  点击事件  在事件前面+on ：绑定事件

```javascript
let btn = document.querySelector("button");
//事件监听函数
btn.onclick = function(){  //当btn被触发时，事件会被执行
	console.log("hello btn!")
}
```

### mouseenter  鼠标移入元素  在事件前面+on ：绑定事件

```
box.onmouseenter = function(){...}
```

### mouseleave  鼠标移出元素  在事件前面+on ：绑定事件

```
box.onmouseleave = function(){...}
```





## 设置样式

element.style.color

element.style.backgroundColor

通过click，mouseenter，mouseleave,mousemove事件控制样式





## 设置属性

element.src

element.id

点击数字列表切换图片：

```javascript
<body>

    <div class="i">
        <img src="C:\Users\Administrator\Desktop\VSCode's file\pic\KDA1.jpg">
    </div>
    <button>1</button>
    <button>2</button>
    <button>3</button>

    <script>
        let imgArray = [`C:\Users/Administrator/Desktop
         /VSCode's file/pic/KDA1.jpg`, //这里换行了 所以用 ` `
        "C:\Users/Administrator/Desktop/VSCode's file/pic/KDA2.jpg", 			"C:\Users/Administrator/Desktop/VSCode's file/pic/timg.jpg"]
        let btns = document.querySelectorAll("button");
        let img = document.querySelector("img")
        for (let i in btns) {
            btns[i].onclick = function() {
                img.src = imgArray[i];
            }
        }
    </script>
```

**这里注意细节**，img中的src路径 可以都是  "\\"  但是，在**数组imgArray中** 路径只有C:\用了反斜杠

，其他全部都得使用/，或者可以用两个反斜杠，否则==一个反斜杠会被判定为转义==。

（亲身经历 🤮吐了）





## 通过class属性设置样式

element.className  -- 获取class属性

可以改变元素的 class 的值。

```javascript
        <div class="btns">
            <button></button>
            <button></button>
            <button></button>
        </div>

    <script>
        let btns = document.querySelectorAll(".btns button");
        for (let i in btns) {
            btns[i].onmouseenter = function() {
                this.className = "change";//当鼠标移入时，class="change"
            }
            btns[i].onmouseleave = function() {
                this.className = "";      //当鼠标移出时，class=""
            }
        }
    </script>
```

可以做轮播图中的 指哪显示哪里的按钮。





# DOM操作

## DOM节点分类

1.元素节点（获取元素节点：querySelector；querySelectorAll）

2.文本节点（innerHTML）

3.属性节点（element.src;element.id）



## innerHTML

在ul中添加li标签：

```
ul.innerHTML=`
	<li>1</li>
	<li>2</li>
	<li>3</li>
`
```



## ==节点操作==

创建元素节点：createElement

创建文本节点：createTextNode

添加节点：appendChild

删除节点：removeChild



### 水果列表的添加与删除

```javascript
<input type="text">
    <button>添加</button>
    <ul class="lll">
        <li>香蕉</li>
        <li>苹果</li>
        <li>凤梨</li>
    </ul>

    <script>
        let btn = document.querySelector('button');
        let inp = document.querySelector('input');
        let ul = document.querySelector('ul');
        let list = document.querySelectorAll('.lll li')
        btn.onclick = function() {
            let value = inp.value;
            let li = document.createElement('li');
            let txt = document.createTextNode(value)
            ul.appendChild(li);
            li.appendChild(txt);
        }
        for (let i in list) {
            list[i].onclick = function() {
                ul.removeChild(this);
            }
        }
    </script>
```

但这里  **只能删除原本有的 li标签  而不能删除新加的 li标签。**





## 事件对象

事件监听函数的形参可以获取事件对象。btn.onclick=function(e){ console.log(e) }

通过事件对象可以获取鼠标坐标。



1、获取x坐标：e.clientX

2、获取y坐标：e.clientY

# 事件

## 内容概述

1.绑定事件

2.事件流

3.事件对象扩展

4.事件委托

5.事件类型



## 事件绑定

1.addEventListener("eventType",fun)//第一个参数是 事件类型（click、等等）

2.element.onEventType = fun

区别：

1.addEventListener在同一元素上的同一事件类型添加多个事件，不会被覆盖，而onEventType会覆盖。

2.addEventListener可以设置元素的捕获阶段触发事件，而onEventType不能





## 事件流

有两个阶段：1.事件捕获  （由外层到内层）   2.事件冒泡（由内层到外层）



### 事件捕获与事件冒泡

默认情况下，**事件**会在**冒泡阶段执行**。



addEventListener(eventType,fun,boolean);//默认false：冒泡阶段触发；true：捕获阶段触发



## 阻止事件冒泡

e.stopPropagation();

```
close.onclick = function(e){   //e别忘了
	box.style.display = "block";
	e.stopPropagation();       //只有自己执行
}
```



## 事件默认行为

阻止事件的默认行为：

e.preventDefault();	

```
<a href="http://baidu.com">baidu</a>
<script>
	let a =document.querySelector("a");
	a.onclick = function(e){
		console.log("hello world")      <----------------------------↓
		e.preventDefault(); //阻止事件默认行为，这里阻止跳转到百度，从而可以执行
	}
</script>
```

点击后，不会跳转到百度，而是在控制台输出了 hello world





## 事件委托

通过 **e.target** 将子元素的事件委托给父级处理。

e.target可以获得最底层的元素

```
ul.onclick = function(e) {//事件委托
            ul.removeChild(e.target);
        }
```



## 事件类型

1.鼠标事件（click，mouseenter等等）

2.键盘事件

3.触屏事件



### 键盘事件

每个键盘上的键，将都有属于自己的keyCode。

document.onkeydown = function(e){	}

通过上下左右键 移动 红色方格：(左上右下的keyCode分别是37，38，39，40)

```javascript
<style>
        * {
            margin: 0;
            padding: 0;
        }
        
        .square {
            width: 100px;
            height: 100px;
            top: 100px;
            left: 100px;
            position: absolute;
            background-color: red;
        }
    </style>
</head>

<body>
    <div class="square"></div>
    <script>
        let box = document.querySelector(".square");
        document.onkeydown = function(e) {
            let code = e.keyCode;
            switch (code) {
                case 37:
                    box.style.left = box.offsetLeft - 5 + "px";
                    break;
                case 38:
                    box.style.top = box.offsetTop - 5 + "px";
                    break;
                case 39:
                    box.style.left = box.offsetLeft + 5 + "px";
                    break;
                case 40:
                    box.style.top = box.offsetTop + 5 + "px";
                    break;
            }
        }
    </script>
</body>
```

**补充：**==offsetLeft,offsetTop 是 偏移量==，现在这种设置 初始偏移量都是100px



### 触屏事件

element.ontouchstart	:触碰屏幕

element.ontouchmove  :滑动​

element.ontouchend	 :离开屏幕





# 计时器方法

1.setInterval与clearInterval

2.setTimeout与clearTimeout

​		**设置**       与        **清空**



## setInterval与clearInterval

1.实现当前时间

```javascript
<h1></h1>//用于展示时间

    <script>
        let h1 = document.querySelector("h1");//获取h1的dom节点
        setInterval(() => {
            let now = new Date();//获取Date对象
            let h = now.getHours();
            let min = now.getMinutes();
            let s = now.getSeconds();
            h1.innerHTML = `${h}:${min}:${s}`;
        }, 500)
    </script>
```



2.实现一个秒表⏱

```javascript
<div class="box">
        <button class="start">开始</button>
        <button class="pause">暂停</button>
        <button class="end">结束</button>
    </div>
    <h1></h1>  //用于存放秒表计时器

    <script>
        let start = document.querySelector(".start");
        let pause = document.querySelector(".pause");
        let end = document.querySelector(".end");
        let h1 = document.querySelector("h1");
        let s = 0;
        let ms = 0;
        h1.innerHTML = `${s}:${ms}`; //展示初始秒表0:0
        let timer = null;   //timer用于存放setInterval方法
        start.onclick = function() {// ，多次点击触发事件    		
            clearInterval(timer);//会及时删除之前创造的方法，防止方法叠加读秒过快
            timer = setInterval(() => {
                if (ms === 9) {
                    s++;
                    ms = 0;
                } else {
                    ms++;
                }

                h1.innerHTML = `${s}:${ms}`;
            }, 100)
        }
        pause.onclick = function() {
            clearInterval(timer);//清除掉方法，停止读秒
        }
        end.onclick = function() {
            s = 0;
            ms = 0;    //设为初始值
            h1.innerHTML = `${s}:${ms}`;
        }
    </script>
```





## setTimeout和clearTimeout

补充知识点：location.href="" 实现网页跳转，与<a>标签不同，这是写在<script>标签中的。

用法后面会学到，这里先用着。 里面是http://baidu.com（没有www.）



timeout 相当于 是一次性的 interval，时间到了就执行，例如5秒后跳转到网址。

这里自己做了个结合：

```
        <h1><span></span>秒后跳转到百度</h1>
        <button>我不去</button>

    <script>
        let btn = document.querySelector("button");
        let span = document.querySelector("span");
        let secounds = 5;//初始化5秒倒计时
        let i = 0;
        span.innerHTML = `${secounds - i}`;
        let timer = setInterval(() => {
            i++;
            span.innerHTML = `${secounds - i}`;//每秒-1，作为倒计时
        }, 1000);
        let timerr = setTimeout(() => {
            location.href = "http://baidu.com"//5秒到了后跳转到百度
        }, 5000)
        btn.onclick = function() {//点击按钮，清空两个事件。
            clearInterval(timer);
            clearTimeout(timerr);
        }
    </script>
```





## ==防抖与节流==

==解决性能问题，开发中常会遇到==

**防抖**：对于短时间内多次触发事件的情况，可以使用防抖停止事件连续触发。

**节流**：防止短时间内多次触发事件的情况，但是间隔事件内，还是需要不断触发。



window.onscroll 滚动事件



### 防抖（debounce）

例：滚动的事件不会被连续触发

```javascript
let timer = null;
window.onscroll = function(){
	if(timer!==null){
		clearTimeout(timer);
	}
	timer = setTimeout(() => {
		console.log("hello")
		timer = null;
	},2000)//操作结束2秒后，才会实现，所以在2秒内一直触发的话，就会一直被重置
}
```



### 节流（throttle）

例：滚动的事件按时间间隔触发

```javascript
let mark = true;//标记
window.onscroll = function(){
	if(mark){
		setTimeout(() => {
			console.log("hello");
			mark = true;
		},2000)
	}
	mark = false;//在2秒内，mark一直处于false状态，因此2秒内方法不会被再次执行
}
```





## 返回顶部效果

1.window.onscroll事件：滚动条滚动事件。

2.document.documentElement.scrollTop：页面滚动位置距离顶部距离。（检测是否在顶部）

3.window.scrollTo(0,0)：让页面滚动条返回顶部。



==利用闭包，封装防抖算法==

```javascript
function debounce(fn){//fn 传入方法
	let timer = null;
	function eventFun(){
		if(timer !== null){
			clearTimeout(timer);
		}
		timer = setTimeout(() => {
			fn();
			timer = null;
		},500)
	}
	return eventFun
}
```

之后使用，就例如滚动事件：

```
window.onscroll = debounce(这里写具体的方法即可)
```



==用闭包，封装节流算法==

```
function throttle(fn){
	let mark = true;
	return function(){//这里原本是先定义一个function再return，这样简写 很常用！
		if(mark){
			setTimeout(() => {
			fn();
			mark = true;
			},500)
		}
		mark = false;
	}
}
```

之后使用，就例如滚动事件：

```
window.onscroll = throttle(这里写具体的方法即可)
```







# BOM

JavaScript=ECMAScript+DOM+BOM

DOM：文档对象模型（document对象）

BOM：浏览器对象模型



## BOM对象

window对象（全局对象）

screen对象包含有关用户屏幕的信息

location对象用于获得当前页面的地址（URL），并把浏览器重定向到新的页面

history对象包含浏览器的历史

navigator对象包含有关访问者浏览器的信息

https://www.w3cschool.cn/javascript/js-window-screen.html



## window 弹出框

1.alert：提示

2.prompt（""）：带输入框的、确定、取消的提示，可以接收输入的信息

3.confirm（）：有确定、取消的提示，返回值是boolean值 



补充知识-==递归函数：在函数体内调用自己==



使用prompt做猜数字的游戏：

```javascript
let number1 = Math.floor(Math.random() * 100 + 1);

        function guessNumber() {
            let number2 = prompt("请输入您猜的数字：");//用number2接收输入的数字
            if (number1 == number2) {
                alert("回答正确！");
            } else if (number1 > number2) {
                alert("小于目标值");
                guessNumber();
            } else if (number1 < number2) {
                alert("大于目标值")
                guessNumber();
            }

        }
        guessNumber();
```



confirm的用法：

```
let mark = confirm("是否xxxxx");//用mark接收用户点的是确认还是取消。
if(mark){//点确认的话，返回值是true
	....
}else{....}//取消的话，返回值是false
```





## location对象

1    .href -属性返回当前页面的URL- "https://www.baidu.com/"

2    .hostname -主机的域名- "www.baidu.com"

3    .pathname -当前页面的路径和文件名- "/s"

4    .port -端口- "8080"

5    .protocol -协议- "https:"



页面跳转location.href = "http://baidu.com"





## ==navigator对象==

navigator.userAgent   可以判断使用的设备（手机型号，电脑，平板等）

能判断设备类型，浏览器类型等，这种代码去网上找。

判断浏览器类型：

```javascript
function userBrowser () {
                var browserName = navigator.userAgent.toLowerCase();
                if(/mise/i.test(browserName) && !/opera/.test(browserName)){
                    alert("IE");
                    return;
                }else if(/firefox/i.test(browserName)){
                    alert("Firefox");
                    return;
                }else if(/chrome/i.test(browserName) && /webkit/i.test(browserName) && /mozilla/i.test(browserName)){
                    alert("Chrome");
                    return;
                }else if(/opera/i.test(browserName)){
                    alert("Opera");
                    return;
                }else if(/webkit/i.test(browserName) && !(/chrome/i.test(browserName) && /webkit/i.test(browserName) && /mozilla/i.test(browserName))){
                    alert("Safari");
                    return;
                }else{
                    alert("unKnow");
                }
            }
```



# 数据类型分类

六种数据类型：

​                      原始类型: Number / String / Boolean / Null / Undefined

数据类型----{

​                       引用类型:Object {  Array / Date / Math / RegExp..





## 原始类型和引用类型的区别

1.赋值和传参的区别：原始类型-==传值调用==  ，  引用类型-==引用调用==

2.比较的区别：原始：比较的是值  引用：比较的是引用是否指向同一个对象



## 原始类型和引用类型的类型检测

1.原始数据类型检测：typeof(值)

typeof：检测**null** 和 **所有引用类型** 都会返回**object** 分辨不出的

2.引用数据类型检测：**值 instanceof 类型**  返回true或false 

reg instanceof Array  返回false



## 数组存储学生列表

1.数组StudentList

2.点击按钮，将表单中的【姓名】和【年龄】赋值给Student对象，并push到StudentList中。

```javascript
<input class="name" type="text" placeholder="姓名">
    <input class="age" type="text" placeholder="年龄">
    <button>添加</button>
	<ul></ul>


    <script>
        let name = document.querySelector(".name");
        let age = document.querySelector(".age");
        let btn = document.querySelector("button");
        let StudentList = [];
        class Student {  //定义一个Student类，因为每次输入值都要new一个新的对象
            constructor(name, age) {//如果不这样做，只用一个对象进行赋值的话，
                this.name = name;   //由于是引用类型，传的是地址，值只能有一个
                this.age = age;
            }
        }
        btn.onclick = function() {
            let std = new Student(name.value, age.value);
            StudentList.push(std);
            let value = `姓名：${name.value}---年龄：${age.value}`;
            let li = document.createElement("li");
            let txt = document.createTextNode(value);
            ul.appendChild(li);
            li.appendChild(txt);
        }
        }
    </script>
```





# 异步编程

## 同步和异步

例子：打电话与发微信（短信）

异步可以多条任务线去执行程序，一条任务卡顿不影响其他任务。



## 解决异步引出的问题

==return只能返回同步的数据==

例：

```javascript
let target = "hello"
function getData(){
	setTimeout(() => {
		return target  //因为在程序执行的瞬间，此方法还没返回target，要500毫秒
	},500)             //但是在500毫秒之前，程序就会执行完，
}
let result = getData();//所以这里获取不到数据 为undefined。
console.log(result);
```



解决方法1：用**回调函数**获取数据

回调函数：一个函数作为另一个函数的参数，这个函数叫做回调函数。

```javascript
let target = "hello";
function getData( fn ){ 这里的fn()是getData的回调函数
	setTimeout(() => {
        fn(target);
    },500)
}
getData((d) => {//这里的d 就被赋值为fn(target)中的target了。
    console.log(d);
})
```





### 用Promise解决异步问题

Promise的语法原理：（里面也用到回调函数）

```javascript
let target = "hello promise"
let p = new Promise( (resolve) => {
	setTimeout(() => {
        resolve(target);
    },500)
} )

p.then((d) => {
    console.log(d);
})
```



那么现在用Promise解决上面的第一个问题：

```javascript
let target = "hello"
function getData(){  //Promise的封装
    return new Promise((resolve) => {
        setTimeout( () => {
            resolve(target);
        },500)
    })
}

let p = getData();
p.then((data) => {//Promise的使用
    console.log(data);
})
```





## Promise结合async函数解决异步问题

async定义函数：

```
async function fun(){...}
```

在async函数中，有一个特有的 await 

await 后面跟Promise对象。直接通过赋值的方式就能获取到Promise的值，不需要用then。

```javascript
async function fun(){
	let data = await getData();
	console.log(data);
}
```

await 等待。像Promise中写的setInterval或者setTimeout，这种计时器方法，需要等待。而await可以等你执行完了之后，再进行赋值，就不会有undefined的情况。





# JavaScript运行机制

## 一、什么是同步与异步

异步：

1.计时器（setInterval,setTimeout）

2.ajax

3.读取文件



同步程序执行完成后，执行异步程序。

## 二、单线程

JS是单线程的，一个任务完成之后 才能执行另一个任务。



## 三、process.nextTick与setImmediate方法

运行顺序：

1.同步

2.process.nextTick

3.异步

4.setImmediate（当前事件循环结束 执行）//在node中才能执行

## 四、事件循环

<img src="C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210803135647793.png" alt="image-20210803135647793" style="zoom:67%;" />

==同步的任务，会直接在运行栈中执行==

其他的会放入任务队列中。

“事件循环”：不断循环、不断检测任务队列中，有无任务可以执行，按顺序将其执行。

”计时器方法“：计时时间到了之后，会把任务查到任务队列中，等待执行（必须等运行栈中的任务先执行完）。（所以，有时候，计时器”并不准“）。



例：

```javascript
setImmediate(() => {//当前事件循环结束之后
            console.log(1)
        })
        process.nextTick(() => {//nextTick，同步之后 异步之前
            console.log(2)
        })
        console.log(3);//同步事件先执行
        setTimeout(() => {console.log(4)},0);//nextTick之后
        setTimeout(() => {console.log(5)},1000);//等1秒后，才被放到任务队列
        setTimeout(() => {console.log(6)},0);//nextTick之后
        console.log(7);//同步事件
```

结果：3 7 2 4 6 1 5

## 五、微任务与宏任务

宏任务：计时器、ajax、读取文件

微任务：promise.then

执行顺序：

1.同步

2.process.nextTick

3.微任务

4.宏任务

5.setImmediate（当前事件循环结束 执行）//在node中才能执行

## 六、async函数

ES6新增的一个对象 Promise

```javascript
let p = new Promise(() => {
	console.log(1)//就是一个同步任务
})
p.then(() => {
    console.log(2)//不会执行
})
```

==Promise有一个方法.then，只有在执行resolve的时候 才会执行then==

```javascript
let p = new Promise((resolve) => {
	resolve("hello world");
})
p.then((data) => {
    console.log(data)//输出的是 hello world
})
```



Promise在ajax中的应用常是：

```javascript
axios.get("").then((res) => {//get方法的返回值，是一个Promise对象
	console.log(res);
})
```

将获取到的远程数据，通过resolve传出来，用then拿到数据。 



==async函数的返回值是Promise对象==

```javascript
async function fun(){	
	return 1;
}
let a = fun();
console.log(a);
```

输出结果：Promise{ 1 }      //是一个Promise对象。

```javascript
async function fun(){//这已经是封装好的代码，不需要自己写resolve：
	return 1;
}
----------------相当于---------------
function fun(){
    return new Promise((resolve) => {
        resolve(1);
    })
}
```



想获得 1 就可以直接使用then

```javascript
async function fun(){
	return 1;
}
fun().then((data) => {
    console.log(data);
})
```



==async 可以通过 await 加Promise对象，直接获得resolve的值==

```javascript
let p1 = new Promise((resolve) => {
	resolve(1);
})
 
async function fun(){
    let a = await p1;
    console.log(a);//x
}
fun();
```

输出结果： 1 

**async结合await** 也是常用的 **解决异步问题的方法**





# 对象拷贝

## js内存结构

原始类型：数值，字符串，布尔，undefined，null

引用类型：对象

栈内存       堆内存

==对象存储再堆内存中。==

























