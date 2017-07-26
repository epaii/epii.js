![](http://cdn.layui.com/upload/2016_10/168_1476644144774_50450.png)
<center> Epii.js 简约而不简单的JavaScript模板引擎。</center >

#特性
------
1. 一个轻量级模板引擎，可快速实现数据与ui绑定（数据变动，UI自动变动），快速实现事件绑定和处理，**不依赖任何第三方库,仅仅8k**。
2. 可快速应用于**web开发**,**native+webapp**开发,**h5**微网页开发,不与其它框架冲突。
3. **让开发者更多关注与应用本身，而不用花费大量时间实现数据与ui的，和事件处理。效率大幅度提升。**

#名字由来

```
自然数e，圆周率π，虚数单位i，三者合在一起组成 epii。
```
#文档目录
1. [如何使用，并写出第一个程序][install]
2. [数据与模板的绑定][bangding]
3. [事件][shijian]
4. [数据的获取][huoqu]

#第一个程序


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






[install]:http://blog.csdn.net/guodongxiaren "我的博客"
[bangding]:http://blog.csdn.net/guodongxiaren "我的博客"
[shijian]:http://blog.csdn.net/guodongxiaren "我的博客"
[huoqu]:http://blog.csdn.net/guodongxiaren "我的博客"
[epiijslink]:http://blog.csdn.net/guodongxiaren "我的博客"
[demo1.html]:http://blog.csdn.net/guodongxiaren "我的博客"