 

# epii.js是什么
epii.js是一个 模板数据绑定和事件绑定的快速实现工具，不依赖任何第三方库,仅仅8k。

> * [1 基础数据绑定](https://github.com/epaii/epii.js#1基础数据绑定)
> * [2 数据绑定其它语法](https://github.com/epaii/epii.js#2-数据绑定其它语法)
> * [3 节点的隐藏/显示](https://github.com/epaii/epii.js#3-节点的隐藏显示)
> * [4 列表（基础）](https://github.com/epaii/epii.js#4-列表基础)
> * [5 列表（多模板）](https://github.com/epaii/epii.js#5-列表多模板)
> * [6 列表（追加数据）](https://github.com/epaii/epii.js#6-列表追加数据)
> * [7 列表（空数据）](https://github.com/epaii/epii.js#7-列表空数据)
> * [8 数据获取，获取已设置的数据，getData,getDataValue连个方法](https://github.com/epaii/epii.js#8-数据获取获取已设置的数据getdatagetdatavalue连个方法)
> * [9 完整的demo，几乎涉及所有语法](https://github.com/epaii/epii.js#9-完整的demo几乎涉及所有语法)

# 1,基础数据绑定
* epii 自定义dom节点属性 r-data 可以对任何类型节点赋值，其中 input 节点最终 赋值其value 属性，img节点赋值其 src 属性，其它类型节点均赋值innerHtml 属性。
* 如果设置r-data-default 则在没有数据时候显示默认值。
* r-data="title"  和 r-data="{title}" 的区别是，在title值不存在时，第一种情况 将显示 title 字符串，第二种情况 显示空，如果第二种情况设置了r-data-default 则显示其设置的默认值
* 以下代码效果可在此处预览 https://epaii.github.io/epii.js/demo/demo1.html
```javascript
<div id="content">
    <h1 r-data="title">
    </h1>
    <div r-data="文章内容：{content}"></div>
    <br>
     <div r-data="{subject}" r-data-default="没有被赋值，只能用：{title}"></div><!-- 默认值-->
    <br>
    <input r-data="inputvalue"><!-- input 负值方法1-->
    <input value="{inputvalue}"><!-- input 负值方法2-->
    <br>
    <img r-data="img_url" style="width: {img_width}px"><!-- img 负值方法1-->
    <img src="{img_url}" style="width: {img_width}px"><!-- img 负值方法2  ,但这种存在缺点，因为在解析前，已经加载一次不存在的图片，多一次请求，不推荐-->
</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body

    myepii.setData({
        title: "我是标题",
        content: "我是内容主题",
        inputvalue: "input内容",
        img_url:"https://www.baidu.com/img/bd_logo1.png",
        img_width:100
    });

    setTimeout(function () {
        myepii.setData({
            title: "我是新的标题",
            content: "我是新的内容主题"
        });
    }, 3000);
</script>
```
# 2 数据绑定其它语法
* epii 可以实现dom节点 属性的变量绑定，可以在任意属性中使用变量标签，比如 style ，width，等任意属性,以下代码效果可在此处预览 
* 支持 链条式变量，比如 {info.subject}  
https://epaii.github.io/epii.js/demo/demo2.html

```javascript
<div id="content">
    <h1 r-data="title" style="width: {h1_width}px;height: {h1_height}px;background-color: {h1_color}">
    </h1>
    <div r-data="{info.subject}"></div>
    <br>
    <img r-data="{img.img_url}" style="width: {img.img_width}px">

</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body

    myepii.setData({
        h1_width:100,
        h1_height:100,
        h1_color:"red",
        title: "我是标题",
        info:{subject:"文章简介"},
        img:{
            img_url: "https://www.baidu.com/img/bd_logo1.png",
            img_width: 100
        }
    });

    setTimeout(function () {
        myepii.setData({
            title: "我是新的标题",
            h1_width:300,
            h1_height:300,
            h1_color:"blue",
            img:{  img_width:300}
        });
    }, 3000);
</script>
```
# 3 节点的隐藏/显示
* epii 提共两种方式设置dom节点隐藏和显示
* 方法1 ，style="display: {h1_display}"  通过style 属性绑定
* 方法2 ， 通过 r-display 标签 r-display="{img_show}-1==0"，必须为bool 等式字符串 ，推荐使用这种方式
* 以下代码效果可在此处预览 https://epaii.github.io/epii.js/demo/demo3.html
```javascript
<div id="content">
    <h1 r-data="title" style="display: {h1_display}"> <!--第一种方法，直接在style中 用变量，不推荐-->
    </h1>
    <br>
    <img r-data="img_url" r-display="{img_show}-1==0"><!--第二种方法，使用 r-display 标签，推荐-->

</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body

    myepii.setData({
        title: "我是标题",
        h1_display:"block",

        img_url:"https://www.baidu.com/img/bd_logo1.png",
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
# 4 列表（基础）
* epii 通过 r-list 标签指定此dom节点将显示列表， 在列表节点内的 变量 将自切换为 列表某一项数据，在列表内之前所有标签,以下代码效果可在此处预览 https://epaii.github.io/epii.js/demo/demo4.html
```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div>名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body

    myepii.setData({
        title: "列表展示",
        users:[
            {name:"张三",age:"12岁"},
            {name:"李四",age:"14岁"}
        ]
    });

</script>
```
# 5 列表（多模板）
* 如果列表中有多个模板，则根据r-display 来自动选择对应的模板,以下代码效果可在此处预览 https://epaii.github.io/epii.js/demo/demo5.html
```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div r-display="{item_type}-1==0" style="background-color: blueviolet">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-display="{item_type}-2==0" style="background-color: red">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body

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
# 6 列表（追加数据）
* epii 可两种方式对列表追加数据
* 方法1 ，重新 setData, 将重新显示列表所有数据，如果旧数据有改变，则用这种方法
* 方法2 ， addData ，已有数据不变，追加数据，如果旧数据没有任何改变，推荐使用这种方式
* 以下代码效果可在此处预览 https://epaii.github.io/epii.js/demo/demo6.html
```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div r-display="{item_type}-1==0" style="background-color: blueviolet">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-display="{item_type}-2==0" style="background-color: red">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body
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
        myepii.addData({ //追加已有数据，列表将别追加，其它类型直接覆盖
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
# 7 列表（空数据）
通过 r-empty="1" 设置当数据为空，或者未设置时候列表的样式，以下代码效果可在此处预览 https://epaii.github.io/epii.js/demo/demo7.html
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
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body
    myepii.setData({
        title: "列表展示",
        users:[]
    });
    setTimeout(function () {//3秒后追加列表
        myepii.addData({ //追加已有数据，列表将别追加，其它类型直接覆盖
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

# 8 数据获取，获取已设置的数据，getData,getDataValue连个方法
* 通过 epii 的 getData 方法 可以获取所有设置的数据
* 通过  epii的 getDataValue 方法 可以快速获取已设置的数据，getDataValue 支持多参数，链条key
* 如 myepii.getDataValue("title");  myepii.getDataValue("info","subject");   myepii.getDataValue("users",1,"age")
* 以下代码效果可在此处预览 https://epaii.github.io/epii.js/demo/demo8.html
```javascript
<div id="content">
    <h1 r-data="title" >  </h1>
    <div r-list="users">
        <div r-display="{item_type}-1==0" style="background-color: blueviolet">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
        <div r-display="{item_type}-2==0" style="background-color: red">名称<span r-data="name"></span>,年龄<span r-data="age"></span></div>
    </div>
</div>
<script>
    var myepii = epii(document.getElementById("content"));//初始化殷勤，需要制定dom节点 可以是 body

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
# 9 完整的demo，几乎涉及所有语法

#demo案例源码:(https://github.com/epaii/epii.js/blob/master/index.html)

#demo案例效果:(https://epaii.github.io/epii.js/index.html)

```html

<div  >
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
```
```javascript
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
        "img_url":"https://www.baidu.com/img/bd_logo1.png",
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
            myepii.addData({//追加已有数据，列表将别追加，其它类型直接覆盖
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
