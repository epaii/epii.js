# Epii.js 简约而不简单的Js模板引擎

<p align=center>
<img src="https://raw.githubusercontent.com/epaii/epii.js/master/docs/Epii.js.jpg" alt="epii.js" width="520">
 
</p>
<p align=center >
  <b>Epii.js 简约而不简单的JavaScript模板引擎</b>
</p>

---


# 特性
------
1. 一个轻量级模板引擎，可快速实现数据与ui绑定（数据变动，UI自动变动），快速实现事件绑定和处理，**不依赖任何第三方库,仅仅8k**。
2. 可快速应用于**web开发**,**native+webapp**开发,**h5**微网页开发,不与其它框架冲突。
3. **让开发者更多关注与应用本身，而不用花费大量时间实现数据与ui的，和事件处理。效率大幅度提升。**

# 名字由来

```
自然数e，圆周率π，虚数单位i，三者合在一起组成 epii。
```
# 文档目录
1. [如何使用，并写出第一个程序](#第一个程序)
2. 数据与模板的绑定
	* [变量的解析(基础)](#变量的解析基础)
	* [变量的解析(高级)](#变量的解析高级)
	* [节点的隐藏和显示](#节点的隐藏和显示)
3. [事件](#事件)
4. 列表
	* [基础列表](#基础)
	* [列表(多模板)](#多模板)
	* [列表(追加数据)](#追加数据)
	* [列表(空数据)](#空数据)
4. [数据的获取](#数据获取获取已设置的数据)

# 第一个程序
---

1.下载 [**epii.min.js**][epiijslink],并在网页中引用

```html
<script src="path/to/epii.min.js"></script>
```
2.编写一个最简单模板

```html
<body>
	<div id="content">
		<span r-data='{hi}' style='font-size:{font}'></span>
	</div>
</body>
```

3.使用**epii(dom)**方法初始化epii对象

```javascript
var myepii = epii(document.getElementById("content"))//初始化epii对象，需要指定dom节点 可以是 document.body
```

4.数据与模板绑定

```javascript
    var myepii = epii(document.getElementById("content"));//初始化epii对象，需要指定dom节点 可以是 document.body
    myepii.setData({
        hi: "hello epii.js",
        font: "50px"
    });

    setTimeout(function () {
        myepii.setData({
            font: "100px"
        });
    },3000);//3秒后数据变动，ui自动变动

```
> 点击查看效果[demo1.html][demo1.html]

# 变量的解析（基础）
---
## 特性(重点)
* 变量在模板中一般用 *{}* 表示。**如{a},{b}**
* 变量只能在dom标签属性中使用。如 *style="width:{width}"*
* *r-data* 标签是epii.js自定义最重要的一个标签。一般用来赋值。
	* *`<input>`* 标签将用于赋予 value 属性值。
	* *`<img>`* 标签将用于赋予 src 属性值。
	* *`<div> <span> <p> 等其它标签`*  将用于赋予 innerHTML 属性值。
* *r-data-default* 标签，是当*r-data*标签值得变量在没有数据时候显示默认值。
* `r-data="title"`  和 `r-data="{title}"` 的区别是，在`title`值不存在时，第一种情况 将显示 title 字符串，第二种情况 显示空，如果第二种情况设置了r-data-default 则显示其设置的默认值。

## 示例
```javascript
<div id="content">
    <span r-data='您好，我是{name}' style='font-size:{font}'></span>
    <div style="background-color: {bgcolor}">
        我的Logo是:<br><img r-data="logo_img">
    </div>

    <span r-data="{subject}" r-data-default="默认的简介：我叫：{name}"></span>
    <p >
        成立于：<input r-data="{time}">
    </p>
</div>
<script>
    var myepii = epii(document.getElementById("content")); 

    myepii.setData({
        name: "epii.js",
        font: "50px",
        logo_img: "https://raw.githubusercontent.com/epaii/epii.js/master/docs/Epii.js.jpg",
        bgcolor: "red",
        time: "2017-06-22"
    });


    setTimeout(function () {
        myepii.setData({
            subject: "我的简介是：Epii.js 简约而不简单的JavaScript模板引擎",
            bgcolor:"#999999"
        });
    },3000);

</script>
```
> 点击查看效果[demo2.html][demo2.html]

# 变量的解析（高级）
---
* 支持链条式变量，如*{info.name}*,*{info.user.sex}*
* *r-data* 可定义变量空间。可大幅度简化变量写法。

> 未使用变量空间的写法

```html
 <!-- 不设置空间的写法-->
<div>
        <p>title:<span r-data="{info.title}" style="color:{info.title_color}"></span></p>
        <p>subject:<span r-data="{info.subject}"></span></p>
        <div>
            作者信息: name:<span r-data="{info.author.name}"></span>,sex:<span r-data="{info.author.sex}"></span>
        </div>
    </div>
<div>

<div>
```
> 设置空间的写法

```html
<!--r-data 设置变量空间 设置空间为 info,在空间内部 info.title 直接写 title就可以 的写法-->
<div r-data="{info}" style="background: cadetblue">
        <p>title:<span r-data="{title}" style="color:{title_color}"></span></p>
        <p>subject:<span r-data="{subject}"></span></p>
        <div r-data="author">
            作者信息: name:<span r-data="name"></span>,sex:<span r-data="{sex}"></span>
        </div>
</div>
```

> 全部代码

```javascript
<div id="content">
    <!-- 不设置空间的写法-->
    <div>
        <p>title:<span r-data="{info.title}" style="color:{info.title_color}"></span></p>
        <p>subject:<span r-data="{info.subject}"></span></p>
        <div>
            作者信息: name:<span r-data="{info.author.name}"></span>,sex:<span r-data="{info.author.sex}"></span>
        </div>
    </div>

    <!--r-data 设置变量空间 设置空间为 info,在空间内部 info.title 直接写 title就可以 的写法-->
    <div r-data="{info}" style="background: cadetblue">
        <p>title:<span r-data="{title}" style="color:{title_color}"></span></p>
        <p>subject:<span r-data="{subject}"></span></p>
        <div r-data="author">
            作者信息: name:<span r-data="name"></span>,sex:<span r-data="{sex}"></span>
        </div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content")); 

    myepii.setData({
        info: {
            title: "epii.js 简介",
            title_color:"red",
            subject: "epii.js 简约而不简单的JavaScript模板引擎",
            author: {
                name: "MrRen",
                sex: "男"

            }

        }
    });

    setTimeout(function () {
        myepii.setData({
            info: {
                title: "epii.js 新的简介"

            }
        });
    }, 3000);

</script>
```
> 点击查看效果[demo3.html][demo3.html]

# 节点的隐藏和显示
---
 *epii.js* 提共两种方式设置dom节点隐藏和显示。
 
 * 方法1 `style="display: {h1_display}"`  通过style属性来控制。
 * 方法2 通过 `r-display` 标签来设定。 `r-display="{img_show}-1==0"`，必须为bool 等式字符串 ，推荐使用这种方式
 * 两种方法都支持变量空间


```javascript
<div id="content">
    <h1 r-data="title" style="display: {h1_display}"> <!--第一种方法，直接在style中 用变量，不推荐-->
    </h1>
    <br>
    <img r-data="img_url" r-display="{img_show}-1==0"><!--第二种方法，使用 r-display 标签，推荐-->

</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化引擎，需要制定dom节点 可以是 body

    myepii.setData({
        title: "我是标题",
        h1_display:"block",

        img_url:"https://raw.githubusercontent.com/epaii/epii.js/master/docs/Epii.js.jpg",
        img_show:1
    });

    setTimeout(function () {//两种方法隐藏
        myepii.setData({
            h1_display:"none",
            img_show:0
        });
    }, 3000);
</script>
```
> 点击查看效果[demo4.html][demo4.html]

# 事件
---
* 1、dom 事件，仍可通过常规设置来实现，如 `onclick="fun('{name}','{age}')"` 
	`onblur="myblur('{name}','{age}')"`  
* 2、*epii.js* 自定义 `r-click-change` 和 `r-click-function` 两个标签来处理 点击跳转 和点击执行函数事件（这两种事件占比最高）。
* 3、`r-click-change` 标签设置点击跳转链接。 如 `r-click-change='http://www.baidu.com?name={name}'`
* 4、`r-click-function` 标签设置点击执行函数。 如 `r-click-function="on_subject_click#{info.subject}#{title}"`,这种写法和 `onclick="on_subject_click('{info.subject}','{title}')" ` 实现效果一样，推荐使用前者。
* 5、* onclick，r-click-change，r-click-function * 同一节点不可重复使用

```html
<div id="content">
    <h1 r-data="title" r-click-change="{baidu_link}">
    </h1>
    <br>
    <img r-data="img_url" r-click-function="{imgclick}#{title}#{img_url}">

</div>
<script>
    var myepii = epii(document.getElementById("content"));

    myepii.setData({
        title: "点我跳转到百度",
        baidu_link: "http://www.baidu.com",

        img_url: "https://raw.githubusercontent.com/epaii/epii.js/master/docs/Epii.js.jpg",

        imgclick: "myfunction"


    });
    function myfunction(title, img_url) {
        console.log(this.src);//this is dom itself
        console.log(title);
        console.log(img_url);
    }

</script>
```
> 点击查看效果[demo5.html][demo5.html]

### 自定义跳转事件
> 通过 `epii.setClickToChangeFunction(f);` 来自定义 `r-click-change` 事件， 在`native+webapp`开发中 一般需要不会直接通过location 页面跳转，而是需要处理自定义协议。

```html
<div id="content">
    <h1 r-data="title" r-click-change="baidu://?a=1&b=2"></h1>

</div>
<script>
    //自定义r-click-change 处理事件， 在native+webapp开发中 一般需要自定义协议
    epii.setClickToChangeFunction(function (url) {
        console.log(url);
    });

    var myepii = epii(document.getElementById("content"));

    myepii.setData({
        title: "我是 Epii.js"
    });


</script>
```
> 点击查看效果[demo6.html][demo6.html]




# 列表
---
### 基础
>  *epii.js* 通过自定义标签 *`r-list`* 来设置此dom节点将显示列表， 在列表节点内的 变量 将自切换为 列表某一项数据。
>  支持多级列表展示
 
```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div>名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content")); 
    myepii.setData({
        title: "列表展示",
        users:[
            {name:"张三",age:"12岁"},
            {name:"李四",age:"14岁"}
        ]
    });

</script>
```
> 点击查看效果[demo7.html][demo7.html]

### 多模板
> 如果列表中有多个模板，则根据 `r-display` 来自动选择对应的模板,

```javascript
<div id="content">
    <h1 r-data="title" > </h1>
    <div r-list="users">
        <div r-display="{item_type}-1==0" style="background-color: blueviolet">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-display="{item_type}-2==0" style="background-color: red">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content"));
    myepii.setData({
        title: "列表展示",
        users:[
            {name:"张三",age:"12岁",item_type:1},
            {name:"李四",age:"14岁",item_type:2},
            {name:"张三1",age:"121岁",item_type:1},
            {name:"李四1",age:"141岁",item_type:2}
        ]
    });
</script>
```
> 点击查看效果[demo8.html][demo8.html]

### 追加数据

* epii 可两种方式对列表追加数据
* 方法1 ，重新 *setData*, 将重新显示列表所有数据，如果旧数据有改变，则用这种方法 。
* 方法2 ， *addData* ，已有数据不变，追加数据，如果旧数据没有任何改变，推荐使用这种方式 。
 


```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div r-display="{item_type}-1==0" style="background-color: blueviolet">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-display="{item_type}-2==0" style="background-color: red">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content")); 
    myepii.setData({
        title: "列表展示",
        users:[
            {name:"张三",age:"12岁",item_type:1},
            {name:"李四",age:"14岁",item_type:2},
            {name:"张三1",age:"121岁",item_type:1},
            {name:"李四1",age:"141岁",item_type:2}
        ]
    });
    setTimeout(function () {//3秒后追加列表
        myepii.addData({ //追加已有数据，列表将被追加，其它类型直接覆盖
            title: "追加列表展示",
            users:[
                {name:"张三5",age:"12岁",item_type:1},
                {name:"李四6",age:"14岁",item_type:2},
                {name:"张三7",age:"121岁",item_type:1},
                {name:"李四8",age:"141岁",item_type:2}
            ]
        });

    },3000);

</script>
```
> 点击查看效果[demo9.html][demo9.html]

### 空数据
> 通过 `r-empty="1"` 设置当数据为空，或者未设置时候列表的样式 。

```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div r-display="{item_type}-1==0" style="background-color: blueviolet">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-display="{item_type}-2==0" style="background-color: red">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-empty="1" style="background-color: cadetblue">没有数据的时候显示</div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content"));
        myepii.setData({
        title: "列表展示",
        users:[]
    });
    setTimeout(function () {
        myepii.addData({ //追加已有数据，列表将别被加，其它类型直接覆盖
            title: "追加列表展示",
            users:[
                {name:"张三5",age:"12岁",item_type:1},
                {name:"李四6",age:"14岁",item_type:2},
                {name:"张三7",age:"121岁",item_type:1},
                {name:"李四8",age:"141岁",item_type:2}
            ]
        });

    },3000);

</script>
```
> 点击查看效果[demo10.html][demo10.html]

# 数据获取，获取已设置的数据
---
``` getData,getDataValue两个方法```

### 特性

* 通过 *`epii.js`* 的 `getData` 方法 可以获取所有设置的数据
* 通过  epii的 `getDataValue` 方法 可以快速获取已设置的数据，`getDataValue` 支持多参数，链条`key`
* 如 `myepii.getDataValue("title");`  `myepii.getDataValue("info","subject");  ` `myepii.getDataValue("users",1,"age")`

```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div r-display="{item_type}-1==0" style="background-color: blueviolet">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-display="{item_type}-2==0" style="background-color: red">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content"));
    myepii.setData({
        title: "获取数据",
        info:{subject:"标题"},
        users:[
            {name:"张三",age:"12岁",item_type:1},
            {name:"李四",age:"14岁",item_type:2},
            {name:"张三1",age:"121岁",item_type:1},
            {name:"李四1",age:"141岁",item_type:2}
        ]
    });
    console.log(myepii.getData());
    alert(myepii.getDataValue("title"));
    alert(myepii.getDataValue("info","subject"));
    alert(myepii.getDataValue("users",1,"age"));
</script>

```
> 点击查看效果[demo11.html][demo11.html]


# 一个复杂的demo，几乎涉及所有语法
---

```html

<div>
    <div r-data="我的名字是{name}，性别:{sex}" r-click-function="index#{name}#{sex}">

    </div>
    <div r-click-change="http://www.baidu.cc/?a={name}">click_to_change</div>
    <div r-data="show_name" r-display="{isshow}-1==0" style="background-color: green">

    </div>
    <div r-data="{hebei}" r-data-default="默认值{name}"  style="width:{width}px;height:{height}px;background-color:{bgcolor};display: {display}" >

    </div>
    <div r-data="{map.age}"  r-display="{map.show}-1==0" >

    </div>
    <img r-data="{img_url}"    >
    <img src="{img_url}"    >
    <input type="text" r-data="{img_url}"    >
    <input type="text" value="{img_url}"    >
    <div r-list="list" style="background-color: #007bc7">

        <span r-data="name" r-display="{moban}-1==0"></span>
        <span r-data="name" style="color: red" r-display="{moban}-2==0" r-click-change="http://www.ddle.cc/?a={age}">

        </span>
        <div r-display="{moban}-3==0" r-click-function="index#2#{age}">
            <div> 二级列表：</div>
            <div r-list="wanju">
                <span r-data="name" r-display="{moban}-1==0"></span>
                <span r-data="name" style="color: blue" r-display="{moban}-2==0"
                      r-click-change="http://www.ddle.cc/?a={a}">
                    </span>
            </div>


        </div>
        <span   r-empty="1">
            真的没有数据
        </span>

    </div>
</div>
 
<script>
    epii.setClickToChangeFunction(function (url) {
        alert(url);
    });


    function index(c, b) {//this  bind to uiview
        console.log(this.innerHTML);
        console.log(c);
        console.log(b);
    }
    var data = {
        "img_url":"https://raw.githubusercontent.com/epaii/epii.js/master/docs/Epii.js.jpg",
        "display":"block",
        "width":100,
        "height":200,
        "bgcolor":"red",
        "name": "张三",
        "sex": "男",
        "isshow": 1,
        "show_name": "show/hide",
        "map":{"show":"1","age":"map_age"},
         "list": [{"name": "list_item_1", "moban": 1}, {"name": "list_item_2", "moban": 2, "age": 2}]
    };
    var myepii = epii(document.body);

    myepii.setData(data);



    //模拟数据变化
    setTimeout(function () {
        myepii.setData({//改变已有数据
            "hebei":"河北邯郸",
            "name": "李四",
            "sex": "女",
            "map":{"show":"0","age":"map_age1"},
            "bgcolor":"blue",
            "width":500,
            "height":50,
            isshow: 0
        });
        setTimeout(function () {
            myepii.addData({//追加已有数据，列表将被追加，其它类型直接覆盖
                "hebei":"河北石家庄",
                 "display":"none",
                "list": [
                    {"name": "list_item_3", "moban": 1},
                    {"name": "list_item_4", "moban": 2, "age": 4},
                    {
                        "moban": 3,
                        "age": 10,
                        "wanju": [{"name": "list_item_list1", "moban": 1}, {"name": "list_item_list2", "moban": 2, a: 5}]
                    }]
            });
            console.log(myepii.getDataValue("name"));
            console.log(myepii.getDataValue("list",1,"age"));
            console.log(myepii.getDataValue("list",4,"wanju",1,"name"));
        },3000);




    }, 3000);



</script>
```
> 点击查看效果[demo12.html][demo12.html]

[epiijslink]:https://raw.githubusercontent.com/epaii/epii.js/master/epii.min.js
[demo1.html]:https://epaii.github.io/epii.js/demo/docs_html/demo1.html "epii.js JavaScript 模板引擎"
[demo2.html]:https://epaii.github.io/epii.js/demo/docs_html/demo2.html "epii.js JavaScript 模板引擎"
[demo3.html]:https://epaii.github.io/epii.js/demo/docs_html/demo3.html "epii.js JavaScript 模板引擎"
[demo4.html]:https://epaii.github.io/epii.js/demo/docs_html/demo4.html "epii.js JavaScript 模板引擎"
[demo5.html]:https://epaii.github.io/epii.js/demo/docs_html/demo5.html "epii.js JavaScript 模板引擎"
[demo6.html]:https://epaii.github.io/epii.js/demo/docs_html/demo6.html "epii.js JavaScript 模板引擎"
[demo7.html]:https://epaii.github.io/epii.js/demo/docs_html/demo7.html "epii.js JavaScript 模板引擎"
[demo8.html]:https://epaii.github.io/epii.js/demo/docs_html/demo8.html "epii.js JavaScript 模板引擎"
[demo9.html]:https://epaii.github.io/epii.js/demo/docs_html/demo9.html "epii.js JavaScript 模板引擎"
[demo10.html]:https://epaii.github.io/epii.js/demo/docs_html/demo10.html "epii.js JavaScript 模板引擎"
[demo11.html]:https://epaii.github.io/epii.js/demo/docs_html/demo11.html "epii.js JavaScript 模板引擎"
[demo12.html]:https://epaii.github.io/epii.js/demo/docs_html/demo12.html "epii.js JavaScript 模板引擎"