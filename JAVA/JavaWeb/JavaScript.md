# JavaScript

## 简介

JavaScript语言诞生主要是完成页面的数据验证。它可以在客户端运行，需要运行浏览器来解析JavaScript代码。

JS是弱类型，Java是强类型。

例如：

Java:

int i=12；

i="ads";//非法操作，i是int类型已经定了。

JS：

var i=12;

i="ads"；合法，i是弱类型，可以随便变。

#### JS的特点：

1、交互性（可以完成信息的交互）

2、安全性（不允许直接访问本地硬盘）

3、跨平台性（只要可以解释JS的浏览器都可以执行，和平台无关）

## JavaScript和html代码的结合方式

#### 第一种

在head标签，或者在body标签中，使用script标签来书写JavaScript代码。

```html
<head>
    <meta charset="UTF-8">
    <title>JS</title>
    <script type="text/javascript">
        alert("哈哈哈哈");
    </script>
</head>
```

#### 第二种

新建一个JS为文件，文件里写JS语句，再HTML里用script导JS文件。

src属性专门用来引入js文件路径（绝对，相对路径都可以）

例如：

html：

```html
<head>
    <meta charset="UTF-8">
    <title>JS</title>
    <script type="text/javascript" src="../JavaScript/与HTML交互.js"></script>
</head>
```

JS：

```javascript
alert("Hello World!!!")
```

![image-20220206161820528](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220206161820528.png)

script标签可以定义js代码，也可以引人js文件，但是不能又引入又定义。

比如：

```html
<script type="text/javascript" src="../JavaScript/与HTML交互.js">
    alert("哈哈哈哈");
</script>
```

比如这样子就可以：就会弹出两次警告。

```html
<script type="text/javascript" src="../JavaScript/与HTML交互.js"></script>
<script type="text/javascript">
    alert("哈哈哈哈");
</script>
```

## JS变量

JS变量类型：

数值类型：			number

字符串类型：		string

对象类型：			object

布尔类型：			boolean

函数类型：			function



JS里面的特殊值：

undefined				未定义，当j定义一个变量未给初始值，默认值都是undefined。

null							空值

NAN							全称：Not a Number，非数值，非数字

当定义var a=1，var b="abc";

alert(a*b);

它就会弹出NAN，代表它非数值和数字。

## JS运算符

和java不一样的运算符：

== 		等于（字面上的等于）

===		全等于（不仅字面上等于，而且数据类型也要等于）

```html
<script type="text/javascript">
    var a=123;
    var b="123"
    alert(a==b);//true
    alert(a===b);//false
</script>
```

#### js逻辑运算

js的所有变量都可以当做boolean类型的变量去用。

在js中0、null、undefined、""(空字符串)都可以代表false，"132",123等可以当作true使用。

和java不同的&&运算和||运算。

&&：

当表达式全为真的时候。返回最后一个表达式的值。

当表达式中，有为假的值时，返回第一个为假的表达式的值。

||：

当表达式全为假时，返回最后一个表达式的值。

当表达式，有值为真，就把第一个真值返回回来。

&&和||短路：

当&&和||的运算已经有结果，就不再执行后面的表达式。

```html
<script type="text/javascript">
    var a=123;
    var b=true;
    var c="";
    var d=false;
    var e=null;

    // alert(b&&a);//123
    // alert(a&&b&&e&&d);//null

    // alert(c||a);//123
    alert(d||c||e);//null

</script>
```

# JS数组（重点）***

js的数组可以存不同类型的值。

js的数组可以自由扩容，而且如果根据数组下标赋值（arr[3]=12）扩容，那么最大下标，将作为扩容的操作大小的标准。

```html
   <script type="text/javascript">
var arr=[];//定义一个空数组
arr[0]=1;
arr[2]=3;
arr[9]="123";//最大下标9，作为扩容的标准，扩容10
alert(arr[1]);//undefined    扩容会导致中间的值没有赋值。
alert(arr.length)//10
for(var i=0;i<arr.length;i++){
    alert(arr[i]);//1,undefined,3,undefined,undefined,undefined,undefined,undefined,undefined,123
}
   </script>
```

# JS函数（重点）***

第一种定义方式：

function 函数名（形参）{

函数体；

}

```html
<script type="text/javascript">
    function f1() {
        alert("调用无参函数");
    }
    function f2(a,b) {//参数不用加类型
        alert("调用有参函数"+a+b);
    }
    function f3(c,d) {//带返回值的函数，直接加return
        return c+d;
    }
    f1();//调用无参函数
    f2();//也能调用有参函数，但是输出：调用有参函数undefinedundefined
    f2(1,"abc")//能正常输出：调用有参函数1abc
    alert(f3(100,20));//120
</script>
```

第二种定义方式：

var 函数名=function（形参）{

函数体；

}

```html
<script type="text/javascript">
    var f1=function(){
        alert("调用无参函数");
    }
    var f2=function(a,b){
        alert("调用有参函数"+a+b);
    }
    var f3=function(c,d){
        return c+d;
    }
    f1();//调用无参函数
    f2(1,"abc");//调用有参函数1abc
    alert(f3(100,20));//120

</script>
```

### JS函数不允许重载

如果定义两个同名函数，后面一个函数将覆盖掉上一次的函数的定义。

例如：

 function f1(a,b) {//参数不用加类型
        alert("调用有参函数"+a+b);
    }

 function f1() {
        alert("调用无参函数");
    }

f1(1,2);

结果：调用无参函数。

#### 函数中的arguments隐形参数（只在function函数内）

当定义函数没有参数，但是调用函数又有参数时，该参数就由arguments参数保存。

隐形参数arguments就像java中的可变长参数一样。

public void fun（Object....args）；

其实他们是保存参数的数组。

```html
<script>
    function f(a) {
        alert(arguments.length);//4
        alert(arguments[2]);//3
        alert(a);//1,a代表第一个参数，有a没a不影响arguments
        alert("无参函数");
    }
    f(1,2,3,4);//四个参数被arguments数组保存。
</script>
```

设计一个返回参数值之和的函数。

```html
<script>
    function sum(num1,num2) {
        var sum=0;
        for (var i=0;i<arguments.length;i++){
            if(typeof (arguments[i])=="number"){//参数中有字符串，相加会输出字符串，所以加判断。
                sum+=arguments[i];
            }
        }
        return sum;
    }
    alert(sum(1,2,3,4,5,"abc",6,7,8,9));//输出：45
</script>
```

# JS中自定义对象

对象的定义：

var 对象名=new Object();  实例化对象

对象名.属性名=值； 定义一个属性

对象名.函数名=function(){函数体}；定义一个函数

```html
<script type="text/javascript">
    var obj=new Object();
    obj.name="张三";
    obj.age=20;
    obj.fun=function () {
        <script type="text/javascript">
  var obj={
      name:"李四",
      age:21,
      fun:function (){
          alert("姓名:"+this.name+"年龄:"+this.age);
      }
  };
  obj.fun();
</script>
    }
    alert(obj.name);//张三
    alert(obj.age);//20
    obj.fun();//姓名:张三年龄:20
</script>
```

花括号形式定义对象：

var 对象名={      空对象

属性名：值，   //定义一个属性

函数名：function(){}   //定义一个函数

}

# JS事件

啥是事件？事件就是电脑输入设备与页面进行交互的响应。

常用的事件：

onload 加载完成事件					页面加载完成后，常用与做页面js代码初始化操作

onclick 单击事件							常用于按钮的点击响应操作

onblur 失去焦点事件					常用于输入框失焦后验证其输入内容是否合法

onchange 内容发生改变事件		常用于下拉列表和输入框内容发生改变后操作

onsubmit	表单提交事件			常用于表单提交前，验证所有表单项是否合法。







### 事件注册（绑定）

事件注册就是告诉浏览器当事件响应后要执行哪些操作代码。

#### 静态注册事件

通过html标签的事件属性直接赋予事件响应后的代码。

#### 动态注册事件

指先通过JS代码得到标签的dom对象，再通过dom对象.事件名=function（）{}这种形式赋予事件响应后的代码。

基本步骤：

1、获取标签对象

2、标签对象.事件名=function(){}

onload事件是浏览器解析完页面后就会自动触发的事件

#### onload事件

静态注册事件：

```html
<head>
    <meta charset="UTF-8">
    <title>onload</title>
    <script type="text/javascript">
        function f(){
            alert('静态注册onload事件')
        }
    </script>
</head>
<body onload="f()">

</body>
```

动态注册页面：

```html
<script type="text/javascript">
    window.onload=function (){
        alert('动态注册onload事件');
    }
</script>
```

#### onclick事件

```html
<head>
    <meta charset="UTF-8">
    <title>单击事件</title>
    <script type="text/javascript">
        function onclickfun(){
            alert('静态单击事件');
        }
       window.onload=function () {
           var obj=document.getElementById("dong");//document是JS语言提供的一个对象（文档），getElementById 通过id属性获取标签对象
           obj.onclick=function () {//obj就是标签对象
               alert("动态单击事件");
           }
       }
    </script>
</head>
<body>
<button onclick="onclickfun()">静态</button><br>
<button id="dong">动态</button>
</body>
```

#### onblur事件（失焦事件）

当鼠标从焦点位置移开，单击其他地方，就会失焦。

```html
<head>
    <meta charset="UTF-8">
    <title>onblur</title>
    <script type="text/javascript">
        function onblurfun(){
            console.log("静态onblur事件");
        }
        window.onload=function () {
            var obj=document.getElementById("pw");
            obj.onblur=function () {
                console.log("动态onblur事件");
            }
        }
    </script>
</head>
<body>
用户名：<input type="text" onblur="onblurfun()">//当鼠标从用户名框焦点，点其他地方，就会触发动态事件
密码：<input type="password" id="pw">
</body>
```

![image-20220207131229294](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220207131229294.png)

#### onchange事件

当内容发生改变，会触发onchange事件

```html
<head>
    <meta charset="UTF-8">
    <title>onchange</title>
    <script type="text/javascript">
        function onchangefun(){
            alert("静态onchange事件");
        }
        window.onload=function () {
            var obj=document.getElementById("dong");
            obj.onchange=function () {
                alert("动态onchange事件");
            }
        }
    </script>
</head>
<body>
<select onchange="onchangefun()">
    <option>中国</option>
    <option>美国</option>
    <option>日本</option>
</select>
<select id="dong">
    <option>汉族</option>
    <option>壮族</option>
    <option>苗族</option>
</select>
</body>
```

#### onsubmit事件

验证提交的表单是否合法

```html
<head>
    <meta charset="UTF-8">
    <title>onsubmit</title>
    <script type="text/javascript">
        function onsubmitfun(){
            alert("静态提交事件，有问题");
            return false;//不合法，返回false；
        }
        window.onload=function () {
            var obj=document.getElementById("dong");
            obj.onsubmit=function () {
                alert("动态态提交事件，有问题");
                return false;
            }
        }
    </script>
</head>
<body>
<form action="https://www.baidu.com" method="get" onsubmit=" return onsubmitfun()">//return不能少，return false可以阻止表单提交
    <input type="submit" value="静态提交">
</form>
<form action="https://www.baidu.com" method="get" id="dong">
    <input type="submit" value="动态提交">
</form>
</body>
```

# Dom模型

Dom全称是Document Object Model文档对象

就是把当前文档中的标签，属性，文本，转换成为对象来管理。

### Document对象（***重点）

![image-20220207142550328](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220207142550328.png)

Document对象的理解

第一：Document管理了所有HTML文档内容。

第二：document它是一种树结构的文档。

第三：它将所有的标签都对象化。

第四：可以通过document访问所有的标签对象。

#### HTML怎么对象化？

相当于：

class Dom{

private String id;      		//id属性

private String tagName;	//表示标签名 

private Dom parentNode; 				//	父节点

private  List<Dom>  children;			//孩子节点 

private String innerHTML;			//起始标签和结束标签中间的内容 

}

## Document对象的一些方法(重点***)

![image-20220207191844980](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220207191844980.png)

## getElementById方法

返回拥有指定id的第一个对象引用。

下面是验证用户名是否在5到12个字符之间，如果是，则合法，如不是，则不合法。

```html
<head>
    <meta charset="UTF-8">
    <title>验证</title>
    <script type="text/javascript">
    function onclickfun(){
        var obj=document.getElementById("text");
        var username=obj.value;//取出用户名
        var pat=/^\w{5,12}$/;//正则表达式，验证字符是否是5到12个
        if(pat.test(username)){//test方法，如果满足要求返回true，否则返回false。
            alert("该用户名合法");
        }else {
            alert("该用户名不合法");
        }
    }
    </script>
</head>
<body>
用户名：<input type="text"  value="hello" id="text">
<button onclick="onclickfun()">验证</button>
</body>
```

## 正则表达式

创建方法：

方法一：

var 对象名=new RegExp();

例如：var patt=new RegExp("e")；//判断字符串是否有e.

## 方法二:（重要）

/规则/

例如：/e/		//判断字符串是否有e。

下面举例常用的正则表达式：、

```html
<head>
    <meta charset="UTF-8">
    <title>正则</title>
    <script type="text/javascript">
        // var pat=/e/;是否有e
        // var str="acasd";  false
        // var pat1=/[a-z]/;是否有小写字母
        // var str1="SaD";  true
        // var pat2=/[A-z]/;是否有英文字母
        // var str2="AaD";  true
        // var pat3=/[0-9]/;是否有数字
        // var str3="dasd13";  true
        // alert(pat3.test(str3));
        //var  pat=/a{3,5}/; 是否包含（注意这个包含满足有3个a就返回true,就不看后面了）3个连续a最多5个a
        // var str="aaa";  true
        // var str="aa";	false
        //var str="aaaaaaaaa"; 	true
        var  pat=/^a{3,5}$/;	//从头到尾检查是否满足3到5个连续的a，少于或多于都返回false。
        var str="aaa";	//true
        var str="aaaaaa";	//false
        alert(pat.test(str));
    </script>
</head>
```

#### 常见的验证提示效果:innerHTML方法

innerHTML方法：取标签里的所有内容。

innerText方法：取标签里的所有文本内容。（只取文字）

下图：innerText方法里取图片内容：

![image-20220208120421939](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220208120421939.png)



效果一：

![image-20220207164757321](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220207164757321.png)

```html
<head>
    <meta charset="UTF-8">
    <title>验证</title>
    <script type="text/javascript">
    function onclickfun(){
        var obj=document.getElementById("text");
        var username=obj.value;
        var pat=/^\w{5,12}$/;
        var spanobj=document.getElementById("namespan");
        //innerHTML表示起始标签和结束标签中的内容。（就是span里的文本内容），它可读可写
        spanobj.innerHTML='hh';//修改span里的文本内容
        if(pat.test(username)){
            spanobj.innerText="用户名合法";
        }else {
            spanobj.innerText="用户名不合法";
        }
    }
    </script>
</head>
<body>
用户名：<input type="text"  value="hello" id="text">
<span style="color: red" id="namespan">123</span>
<button onclick="onclickfun()">验证</button>
</body>
```

效果二：

```html
<head>
    <meta charset="UTF-8">
    <title>验证</title>
    <script type="text/javascript">
    function onclickfun(){
        var obj=document.getElementById("text");
        var username=obj.value;
        var pat=/^\w{5,12}$/;
        var spanobj=document.getElementById("namespan");
        spanobj.innerHTML='hh';
        if(pat.test(username)){
            spanobj.innerHTML= "<img src=\"../imgs/打勾.png\" height=\"18\",weight=\"18\">";//将span里的内容改为图片。
        }else {
            spanobj.innerHTML="<img src=\"../imgs/打叉.png\" height=\"18\",weight=\"18\">";
        }
    }
    </script>
</head>
<body>
用户名：<input type="text"  value="hello" id="text">
<span style="color: red" id="namespan"></span>
<button onclick="onclickfun()">验证</button>
</body>
```



## ![image-20220208120134784](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220208120134784.png)



## getElementsByName方法

getElementsByName方法：返回带有指定的name的对象集合。

```html
<head>
    <meta charset="UTF-8">
    <title>全选</title>
    <script type="text/javascript">
        function allselect() {
            var hobbies=document.getElementsByName("hobby");//hobby是个对象集合，[object NodeList]类型，可以把它当成数组用
            alert(hobbies[0].value);//java
            for(var i=0;i<hobbies.length;i++){
                hobbies[i].checked=true;
            }
        }
        function noneselect() {
            var hobbies=document.getElementsByName("hobby");
            for(var i=0;i<hobbies.length;i++){
                hobbies[i].checked=false;
            }
        }
        function reverseselect(){
            var hobbies=document.getElementsByName("hobby");
            for(var i=0;i<hobbies.length;i++){
                hobbies[i].checked=!hobbies[i].checked;
            }
        }
    </script>
</head>
<body>
<input type="checkbox" name="hobby" value="java"> Java
<input type="checkbox" name="hobby" value="c" > C
<input type="checkbox" name="hobby" value="cpp"> C++

<br>
<button onclick="allselect()">全选</button>
<button onclick="noneselect()">全不选</button>
<button onclick="reverseselect()">反选</button>
</body>
```

## getElementsByTagName方法

和getElementsByName方法差不多，不同的是，当标签没有name的时候getElementsByTagName方法可以获取标签的名字，并返回对象集合。

多选，全选测试代码：

```html
<head>
    <meta charset="UTF-8">
    <title>全选</title>
    <script type="text/javascript">
        function allselect() {
            var inputs=document.getElementsByTagName("input");
            for(var i=0;i<inputs.length;i++){
                inputs[i].checked=true;
            }
        }
        function noneselect() {
            var inputs=document.getElementsByTagName("input");
            for(var i=0;i<inputs.length;i++){
                inputs[i].checked=false;
            }
        }
        function reverseselect(){
            var inputs=document.getElementsByTagName("input");
            for(var i=0;i<inputs.length;i++){
                inputs[i].checked=!inputs[i].checked;
            }
        }
    </script>
</head>
<body>
<input type="checkbox"  value="java"> Java
<input type="checkbox"  value="c" > C
<input type="checkbox"  value="cpp"> C++

<br>
<button onclick="allselect()">全选</button>
<button onclick="noneselect()">全不选</button>
<button onclick="reverseselect()">反选</button>
</body>
```

## 三个方法的注意事项

1、  document对象的三个查询方法优先顺序，有id就用getElementById方法，没id有name就用getElementsByName方法，都没就就用getElementsByTagName方法。范围越来越大，查询越来越不精细。

2、查询要在页面加载之后执行，比如alert（document.getElementsByName("hobby")）要在方法里面，不然会显示null

## 补充说明：createElement方法

该方法就是创建一个对象元素，然后可以将该对象元素作为节点执行添加操作。

例如：

```html
<head>
    <meta charset="UTF-8">
    <title>createElement</title>
  <script type="text/javascript">
    window.onload=function () {
        // var divobj=document.createElement("div");
        // divobj.innerHTML="啊哈哈哈，鸡汤来喽";
        // document.body.appendChild(divobj);//创建一个body的子节点，该子节点就是div对象，相当于直接把div直接写进body里。
        var textobj=document.createTextNode("啊哈哈哈，鸡汤来喽");//创建一个文本对象
        document.body.appendChild(textobj);//将文本对象作为body的子节点
    }
  </script>
</head>
<body>

</body>
```

# JQuery

介绍：

JQuery是JavaScript和查询（Query），是辅助JS开发的JS库类。

好处：

JQuery的语法设计可以使开发更加便捷，例如操作文档对象、选择DOM元素、这座动画效果、处理事件、使用Ajax以及其他功能。

### JQuery测试：

```html
<head>
    <meta charset="UTF-8">
    <title >测试</title>
    <script type="text/javascript"  src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script type="text/javascript">
        $(function () {
            var $btnobj=$("#oo");//按id查询到标签对象
            $btnobj.click(function () {//调用点击响应函数
                alert("jQuery测试");
            });
        });
    </script>
</head>
<body>
<button id="oo">测试</button>
</body>
```

## $是啥？

jQuery源代码有与一句：window.jQuery=window.$=jQuery

所以$就是一个函数，就是jQuery函数。

## jQuery核心函数（$）

$是jQuery的核心函数，能完成jQuery的很多功能。$（）就是调用$这个函数。

### $()

#### 当传入的参数为[函数]时。

表示页面加载完成后，相当于window.onload=function(){}

```html
<head>
    <meta charset="UTF-8">
    <title>函数</title>
    <script src=https://code.jquery.com/jquery-3.3.1.min.js></script>
    <script type="text/javascript">
      $(function () {
          var bodyobj=document.getElementById("hh");
          alert(bodyobj);
      })

    </script>
</head>
<body id="hh">

</body>
```

#### 当传入参数为字符串时

它会根据这个字符串创建元素节点对象.

例如：

```html
<head>
    <meta charset="UTF-8">
    <title>函数</title>
    <script src=https://code.jquery.com/jquery-3.3.1.min.js></script>
    <script type="text/javascript">
        $(function () {
            $("<div>\n" +
                "    <span>哦哦哦哦哦</span>\n" +
                "    <span>哈哈哈哈哈</span>\n" +
                "</div>").appendTo("body");//创建div节点对象，成为body的子节点。
        });

    </script>
</head>
<body>

</body>
```

#### 当传入的参数为[选择器字符串]时

$("#id属性值")；id选择器，根据id查询标签对象

$("标签名")；标签名选择器，根据id查询标签对象，返回对象集合，可以做数组用。

$(".class属性值"); 类型选择器，可以根据class属性查询标签对象。

#### 当传入[DOM对象]时:

将DOM对象包装为jQuery对象返回。[object Object]

#### dom对象和jQuery对象的区别

DOM对象Alert出来的效果是：[object HTML 标签名 Element]

jQuery对象Alert效果：

1、包装Dom对象。

```html
var bodyobj=document.getElementById("bo");
alert($(bodyobj));
```

2、通过jQuery提供的API创建的对象

```html
alert($("<p>啊哈哈哈哈</p>"));
```

3、通过通过jQuery提供的API查询到的对象。

```html
alert($("#bo"));
```

上面这三种方式返货的都是jQuery对象，Alert效果都是[object Object]。

## jQuery的本质

jQuery对象是存dom对象的数组+jQuery提供的一系列的功能函数。

## dom对象和jQuery的使用区别

dom的单击事件：

```html
  <script type="text/javascript">
        $(function () {
            var divobj=document.getElementById("di");
            divobj.onclick=function () {
                alert("dom单击事件");
            }

        });

    </script>
</head>
<body>
<div id="di">哈哈哈</div>
</body>
```

jQuery单击事件：

```html
    <script type="text/javascript">
        $(function () {
            $("#di").click(function (){
               alert("JQ点击事件");
            });
        });

    </script>
</head>
<body>
<div id="di">哈哈哈</div>
</body>
```

jQuery对象不能使用DOM对象的属性和方法，反之也不行。就比如dom对象不能用click方法。

## Dom对象和jQuery对象互转（重点***）

#### 1.dom对象转化为jQuery对象

1、先创建一个dom对象。

2、$(Dom对象)，就可以转化为jQuery对象。



#### 2、jQuery对象转化为dom对象

1、先创建一个jQuery对象。

2、jQuery对象[下标取出相应的dom对象]  因为jQuery对象是一个存dom对象的数组。

## 遍历被jQuery对象封装的dom对象

### 使用方法：each()

each(function(){}),each遍历的function函数中，有个this指针指向当前遍历到的dom对象。

## 层级选择器

$("form input")

查找form内的所有元素input

$("form>input")

查找form下的直接子元素input(form子元素下的input不算)。

$("label+input")

查找所有跟在lable后面的inp元素。

$("form~input")

查找所有与form同级且在form后面的input元素

+和~都是找同级元素。

## 基本过滤选择器

$("li：first")

获取匹配的第一个元素

$("li:odd")

查找索引值为基数的元素

$("div:not(.one)")

查找class不是one的所有div元素。

$("div:gt(3)")

查找索引值大于3的div元素。

## 内容过滤器

$("rd:empty")

查找内容为空的表格

$("div:contains("Tom")")

查找文本内容包含"Tom"的div元素

$("div:has(p)").addClass("test")

返回含有p元素的div,并在p的内容加上test

## 属性过滤

$("div[id]")

查找所有含有id属性的div元素

$("div[name="hh"]")

查找所有含有是hh的name属性的div

$("input[id][name$="man[])

查找所有含有id属性，并且它的name属性是以man结尾

## 表单过滤选择器

$(":text")

查找所有文本框

$(":password")

查找所有密码框

$("intput:checked")

查找所有选中的复选框元素

jQuery对象去表单的value值用对象.val()

jQuery对象变量封装的dom对象用each(function(){})方法,还提供了this指针，指向当前遍历到的元素

## jQuery元素的筛选

其实是写在外面的方法

#### .equal()

$("li").equal(1) 				//和:equal()一样

查找索引为1的li元素

#### .first()

$("li").first()					//和:first()一样

查找li中第一个元素

#### .not(exp)

$("p").not($("#selected")[0])   //[0]是转化为dom对象删除

删除id属性为selected的元素后返回

![image-20220209122854493](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220209122854493.png)

结果：

![image-20220209122958314](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220209122958314.png)

#### .filter(exp)//exp是匹配的属性

$("p").filter(".selected")

筛选出带有select类的p元素

![image-20220209115103966](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220209115103966.png)

结果：

![image-20220209115135989](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220209115135989.png)

#### .is(exp)

$("input[type='checkbox']").parent().is("form")  //is()满足一个条件就返回true。

判断input元素的父元素是不是form

![image-20220209115711444](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220209115711444.png)

结果：true.

#### .has(exp)

和：has(exp)一样

如果子类(不是直接子类也算)包含该属性，has(属性)，就返回它的父类。

```html
<div><p id="oo"><span id="hh"></span></p></div>
<div>yy</div>
<div>xx</div>
```

```html
$("div").has("#hh")
```

结果：

返回第一个div对象。

#### .children(exp) 

返回匹配给定选择器的子元素。和parent>child一样

#### .find（exp）

和(parent child)一样

#### .next（）

和(prev + next)一样，找到紧跟后面的同辈元素。

#### .nextAll()

和(prev~next)一样

#### .html()

和 dom对象的innerHTML一样，html()有参数时是修改内容，没参数是读取。

### .text()

和 dom对象的innerText一样,text()有参数时是修改内容，没参数是读取。

#### .val()

和 dom对象的value一样,val()有参数时是修改内容，没参数是读取。val()只在表单中能使用。

### 批量操作表单选项

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="../script/jquery-1.7.2.js">
    </script> <script type="text/javascript">
        $(function () {
            //批量操作单选框
            // $(":radio").val(["radio2"]);
            //批量操作复选框
            // $(":checkbox").val(["checkbox1","checkbox3"]);
            //批量操作下拉多选框
            // $("#multiple").val(["mul1","mul3","mul4"]);
            $(":radio,:checkbox,#multiple,#single").val(["radio2","checkbox1","checkbox3","mul1","mul3","mul4","sin2"]);
         });
    </script>
</head>
<body>
    单选： <input name="radio" type="radio" value="radio1" />radio1 <input name="radio" type="radio" value="radio2" />radio2 <br/>
    多选： <input name="checkbox" type="checkbox" value="checkbox1" />checkbox1 <input name="checkbox" type="checkbox" value="checkbox2" />checkbox2 <input name="checkbox" type="checkbox" value="checkbox3" />checkbox3 <br/>
    下拉多选 ： <select id="multiple" multiple="multiple" size="4"> <option value="mul1">mul1</option> <option value="mul2">mul2</option> <option value="mul3">mul3</option> <option value="mul4">mul4</option> </select> <br/>
    下拉单选 ： <select id="single"> <option value="sin1">sin1</option> <option value="sin2">sin2</option> <option value="sin3">sin3</option> </select>
</body>
```

##  attr()、prop()方法

attr()			可以设置和获取属性的值（一个参数获取，两个参数设置），不推荐操作：cheked/readOnly/selected/disabled等等。

prop() 	可以设置和获取属性的值（一个参数获取，两个参数设置），只推荐操作：cheked/readOnly/selected/disabled等等。

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
    <script type="text/javascript">
        $(function (){
            // $(":checkbox:first").attr("name","boxx");
            // alert($(":checkbox:first").attr("name"));
            // alert($(":checkbox").attr("checked"));//undefined(模糊，不知道是没定义还是没选)
            // alert($(":checkbox").prop("checked"));//false;
            alert($(":checkbox").prop("checked","true"));//将多选框全选
        });
    </script>
</head>
<body>
<input type="checkbox" name="box" value="box1" >
<input type="checkbox" name="box" value="box2" >

</body>
```

## 全选、全不选、反选、提交功能测试

![image-20220209171149293](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220209171149293.png)

```html
    <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
    <script type="text/javascript">
        $(function (){
            //全选，点击全选按钮将选项全选
            $("#checkedAllBtn").click(function () {
                $(":checkbox").prop("checked",true);
            });
            //全不选，点击全不选按钮将选项全不选
            $("#checkedNoBtn").click(function () {
                $(":checkbox").prop("checked",false);
            });
            //反选
            $("#checkedRevBtn").click(function () {
               $(":checkbox[name='items']").each(function () {//遍历jQuery对象数组
                   this.checked=!this.checked;//this在function中表示dom对象
               })
                //反选查看选项个数是否等于选中个数。等于就把全选/全不选选项勾上。
                var alllength= $(":checkbox[name='items']").length;
               var selectlength= $(":checkbox[name='items']:checked").length;
               $("#checkedAllBox").prop("checked",alllength==selectlength);

            });
            //提交
            $("#sendBtn").click(function () {
                $(":checkbox[name='items']:checked").each(function (){
                    alert(this.value);
                })
            })
            //全选/全不选
            $("#checkedAllBox").click(function () {
                //全选/全不选选项要和四个球类选项一致，四个球类的checked由this.checked来决定。
                $(":checkbox[name='items']").prop("checked",this.checked);
            })
            $(":checkbox[name='items']").click(function () {
                //将四个球类选项绑定单击事件，每单机一个选项，都要检查选项个数和已选个数是否相等，如果相等，就把全选/全不选给勾上。
                var alllength=$(":checkbox[name='items']").length;
                var selectlength=$(":checkbox[name='items']:checked").length;
                $("#checkedAllBox").prop("checked",alllength==selectlength);
            })
        });
    </script>
</head>
<body>
    <form method="post" action="">
        你爱好的运动是？<input type="checkbox" id="checkedAllBox" />全选/全不选 <br />
        <input type="checkbox" name="items" value="足球" />足球
        <input type="checkbox" name="items" value="篮球" />篮球
        <input type="checkbox" name="items" value="羽毛球" />羽毛球
        <input type="checkbox" name="items" value="乒乓球" />乒乓球
        <br />
        <input type="button" id="checkedAllBtn" value="全 选" />
        <input type="button" id="checkedNoBtn" value="全不选" />
        <input type="button" id="checkedRevBtn" value="反 选" />
        <input type="button" id="sendBtn" value="提 交" />
    </form>
</body>
```

## 插入 删除 替换操作

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="../script/jquery-1.7.2.js">
    </script> <script type="text/javascript">
    $(function () {
       // $("<h1>123</h1>").appendTo("div");//插在div子元素的最后
       //  $("<h1>123</h1>").prependTo("div");//插在div子元素的最前面
       //  $("div").remove();//删除div,连标签也删除
       //  $("div").empty();//删除div中的内容，标签还在
       //  $("div").replaceWith(("<h1>123</h1>"));//将div全部替换成h1
        $("<h1>123</h1>").replaceAll("div");//将h1替换所有的div


    });
</script>
</head>
<body>
<div>哈哈哈</div>
<div></div>
</body>
```

相关练习:实现左右元素框互相转移

![image-20220209184447755](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220209184447755.png)

```html
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
   <style type="text/css">
      select {
         width: 100px;
         height: 140px;
      }
      
      div {
         width: 130px;
         float: left;
         text-align: center;
      }
   </style>
   <script type="text/javascript" src="script/jquery-1.7.2.js"></script>
   <script type="text/javascript">
      // 页面加载完成
      $(function () {
         // 第一个按钮 【选中添加到右边】
         $("button:eq(0)").click(function (){//索引为0的按钮
            $("select:eq(0) option:selected").appendTo($("select:eq(1)"));
         })
         // 第二个按钮 【全部添加到右边】
         $("button:eq(1)").click(function (){
            $("select:eq(0) option").appendTo($("select:eq(1)"));
         })
         // 第三个按钮 【选中删除到左边】
         $("button:eq(2)").click(function (){
            $("select:eq(1) option:selected").appendTo($("select:eq(0)"));
         })
         // 第四个按钮 【全部删除到左边】
         $("button:eq(3)").click(function (){
            $("select:eq(1) option").appendTo($("select:eq(0)"));
         })
      });
   </script>
</head>
<body>

   <div id="left">
      <select multiple="multiple" name="sel01">
         <option value="opt01">选项1</option>
         <option value="opt02">选项2</option>
         <option value="opt03">选项3</option>
         <option value="opt04">选项4</option>
         <option value="opt05">选项5</option>
         <option value="opt06">选项6</option>
         <option value="opt07">选项7</option>
         <option value="opt08">选项8</option>
      </select>
      
      <button>选中添加到右边</button>
      <button>全部添加到右边</button>
   </div>
   <div id="rigth">
      <select multiple="multiple" name="sel02">
      </select>
      <button>选中删除到左边</button>
      <button>全部删除到左边</button>
   </div>

</body>
```

## 动态添加和删除表格练习：

```html
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Untitled Document</title>
<link rel="stylesheet" type="text/css" href="styleB/css.css" />
<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
   $(function () {
      //删除操作函数
      var delet=function () {
         var $trobj=$(this).parent().parent();
         var name=$trobj.children("td").first().text();//行元素的直接子元素td的第一个孩子节点的文本内容
         // confirm("你真的要删除吗？");//js提供的确认提示框函数，点确定返回true,点取消返回false。
         if(confirm("你真的要删除"+name+"吗？")){
            $trobj.remove();
         }

         return false;//不让它跳转，阻止默认行为
      }
      //添加员工
      $("#addEmpButton").click(function () {
         var name=$("#empName").val();
         var email=$("#email").val();
         var salary=$("#salary").val();
         var $trobj=$("<tr>" +
               "<td>"+ name +"</td>" +
               "<td>"+ email +"</td>" +
               "<td>"+ salary +"</td>" +
               "<td><a href=\"deleteEmp?id=002\">Delete</a></td>" +
               "</tr>");
         $trobj.appendTo("#employeeTable");
         //删除补绑的新员工
         $trobj.find("a").click(delet);
      });
      //删除员工
      $("a").click(delet);
   });


   
</script>
</head>
<body>

   <table id="employeeTable">
      <tr>
         <th>Name</th>
         <th>Email</th>
         <th>Salary</th>
         <th>&nbsp;</th>
      </tr>
      <tr>
         <td>Tom</td>
         <td>tom@tom.com</td>
         <td>5000</td>
         <td><a href="deleteEmp?id=001">Delete</a></td>
      </tr>
      <tr>
         <td>Jerry</td>
         <td>jerry@sohu.com</td>
         <td>8000</td>
         <td><a href="deleteEmp?id=002">Delete</a></td>
      </tr>
      <tr>
         <td>Bob</td>
         <td>bob@tom.com</td>
         <td>10000</td>
         <td><a href="deleteEmp?id=003">Delete</a></td>
      </tr>
   </table>

   <div id="formDiv">
   
      <h4>添加新员工</h4>

      <table>
         <tr>
            <td class="word">name: </td>
            <td class="inp">
               <input type="text" name="empName" id="empName" />
            </td>
         </tr>
         <tr>
            <td class="word">email: </td>
            <td class="inp">
               <input type="text" name="email" id="email" />
            </td>
         </tr>
         <tr>
            <td class="word">salary: </td>
            <td class="inp">
               <input type="text" name="salary" id="salary" />
            </td>
         </tr>
         <tr>
            <td colspan="2" align="center">
               <button id="addEmpButton" value="abc">
                  Submit
               </button>
            </td>
         </tr>
      </table>

   </div>

</body>
```

## css样式操作

addClass()			添加样式

removeClass		删除样式

toggleClass()			有就删除样式，没有就添加样式

offset()					获取和设置元素坐标。

## 动画操作

show()			将隐藏的元素显示

hide()				将可见的元素隐藏

toggle()			可见就隐藏，隐藏就显示。

以上方法都可以加参数

第一个参数是执行时长

第二个参数是动画的回调函数(递归)。

## 原生js和jQuery页面加载完成之后的区别

### window.onload=function(){}和$(function(){})的执行顺序

$(function(){})比window.onload=function(){}先执行。

jQuery的页面加载在浏览器的内核解析完页面的标签创建好Dom对象之后马上执行

原生js的页面，要在浏览器的内核解析完页面的标签创建好Dom对象，还有等标签内容显示完成(比如图片的加载)才能执行.

### 执行次数：

```html
<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
    window.onload=function () {
        alert("原生js的触发1");
    }
    window.onload=function () {
        alert("原生js的触发2");
    }
    window.onload=function () {
        alert("原生js的触发3");
    }
    $(function (){
       alert("jQuery触发1");
    });
    $(function (){
        alert("jQuery触发2");
    });
    $(function (){
        alert("jQuery触发3");
    });
</script>
```

js页面加载只执行一次，因为js不允许函数重载，只能取最后一次的函数定义。

和jQuery页面会执行三次，而且是按照定义的顺序来执行。

## jQuery中的一些事件处理方法

1、click(function(){})		绑定单击事件或触发单击事件，有函数绑定，没函数触发

2、mouseover(function(){})		鼠标移入事件

3、mouseout(function(){})			鼠标移出事件

4、bind(事件1	事件2	事件n,function(){})											可以给元素一次绑定多个事件

5、one(事件1	事件2	事件n,function(){})											和bind一样，但是绑定的事件只相应一次

6、unbind(事件1	事件2	事件n)							解绑事件，括号没有事件就全部解绑

7、live(事件1，function(){})						绑定事件，而且绑定的事件对后面创建出来的元素也有效。

## 事件冒泡

当父子元素同时监听一个事件，当触发子元素时，同个事件也被传递到了父元素的事件里去相应。

```html
<script type="text/javascript" src="jquery-1.7.2.js"></script>
<script type="text/javascript">
   $(function(){
      $("#content").click(function () {
         alert('我是div');
      });

      $("span").click(function () {
         alert('我是span');

         return false;//阻止事件冒泡
      });


   })
</script>
```

## 事件对象

事件对象就是封装出发事件信息的javascript对象。

如何获取事件对象？

事件的function(event)，event就是传递参数事件处理函数的事件对象。

event一般和bind方法合用。

```html
<script type="text/javascript" src="jquery-1.7.2.js"></script>
<script type="text/javascript">

   //1.原生javascript获取 事件对象
   // window.onload = function () {
   //     document.getElementById("areaDiv").onclick = function (event) {
   //        console.log(event);
   //     }
   // }
   //2.JQuery代码获取 事件对象
   $(function () {
      // $("#areaDiv").click(function (event) {
      //     console.log(event);
      // });
      //3.使用bind同时对多个事件绑定同一个函数。怎么获取当前操作是什么事件。

      $("#areaDiv").bind("mouseover mouseout",function (event) {
         if(event.type=="mouseover"){
            console.log("鼠标移入");
         }else if(event.type=="mouseout"){
            console.log("鼠标移出");
         }
      });


   });



</script>
```

## event事件对象配合bind实验

鼠标放在小图片上移动，当图片也跟着移动，鼠标移出小图片，大图片消失。

![image-20220210170909039](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220210170909039.png)

```html
<style type="text/css">
   body {
      text-align: center;
   }
   #small {
      margin-top: 150px;
   }
   #showBig {
      position: absolute;
      display: none;
   }
</style>
<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
   $(function(){
      $("#small").bind("mouseover mouseout mousemove",function (event) {
         if(event.type=="mouseover"){//鼠标移人小图片
            $("#showBig").show();
         }else if(event.type=="mouseout"){//鼠标移出小图片
            $("#showBig").hide();
         }else if(event.type=="mousemove"){//鼠标移动
            console.log(event);//记录鼠标移动的信息
            $("#showBig").offset({
               left:event.pageX+10,//将鼠标的横坐标+10赋给大图片的左距离
               top: event.pageY+10//将鼠标的纵坐标+10赋给大图片的上距离
            });
         }
      })
   });
</script>
</head>
<body>

   <img id="small" src="img/small.jpg" />
   
   <div id="showBig">
      <img src="img/big.jpg">
   </div>

</body>
```
