# epii.js是什么
epii.js是一个 模板数据绑定和事件绑定的快速实现工具，不依赖任何第三方库,仅仅8k。


# 1基础数据绑定
epii 自定义dom节点属性 r-data 可以对任何类型节点赋值，其中 input 节点最终 赋值其value 属性，img节点赋值其 src 属性，其它类型节点均赋值innerHtml 属性，以下代码效果可在此处预览 https://github.com/epaii/epii.js/blob/master/demo/demo1.html
```javascript
<div id="content">
    <h1 r-data="title">
    </h1>
    <div r-data="文章内容：{content}"></div>
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
