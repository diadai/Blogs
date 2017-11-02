### 样式的优先级
 当一个元素被**多重样式**作用时，如被外部样式、内部样式和内联样式同时作用于同一个元素，一般情况下，

 **优先级是这样的：外部样式 < 内部样式 < 内联样式**

当然也有一个例外就是当**外部样式**放在**内部样式**的**后面**的时候，则外部样式将**覆盖**内部样式。

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Bootstrap 实例 - 标签式的导航菜单</title>
     <style type="text/css">
         /*  内部样式*/
         h1{color:blue;}
     </style>

    <!-- 外部样式-->
    <link rel="stylesheet" href="css/1.css">
    <!-- 设置 h1{color:red}-->
</head>
<body>
<h1>显示的颜色将是外部样式设置的红色！</h1>
</body>
</html>
```

### 选择器的优先权
1、内部样式表的权值最高 1000；
2、ID 选择器的权值为 100；
3、Class 类选择器的权值为 10；
4 HTML 标签选择器的权值为 1；

也就是你在用选择器选中设置元素样式的时候，实现机制如下；

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>tab</title>
    <style>
        #tab{/* 权值为：100*/
            width:99%;
            margin: auto;
        }
        ul{width:100%;margin:20px auto;padding: 0;text-align: center;}/* 权值为：1*/
        li{/* 权值为：1*/
            width: 19%;
            list-style: none;
            line-height: 2.5em;
            display: inline-table;
            background-color: #e4b9c0;
            cursor: default;
        }
        li:hover{background-color: #c7254e;}
        .content{/* 权值为：10 */
            width:96%;
            margin: auto;
            background-color: aquamarine;
            text-align: center;
            height: 200px;
        }
        .content div{/* 权值为：100+1=101   */
            display: none;
        }
        .content .show{/* 权值为：100+100=200  */
            width: 100%;
            display: block;
            background-color: #adadad;
        }
    </style>
</head>
<body>
<div id="tab">
    <ul>
        <li>团队介绍</li>
        <li>团队成就</li>
        <li>团队生活</li>
        <li>团队成员</li>
        <li>团队引力</li>
    </ul>

    <div class="content">
        <div class="each show">简介</div>
        <div class="each">时间线 荣誉展示</div>
        <div class="each">平时生活照片</div>
        <div class="each">地图</div>
        <div class="each">优点  条件</div>
    </div>
</div>
<script>
    var oul=document.getElementById("tab");
    var oli=oul.getElementsByTagName("li");
    var odiv=oul.getElementsByTagName("div")[0].getElementsByTagName("div");
    var len=oli.length;
    function show(index) {
        for(var i=0; i<len;i++){
            if(i===index){
                odiv[i].className="show";
            }
            else{odiv[i].className=""}
        }
    }
    for(var i=0;i<len;i++){
        oli[i].index=i;
        oli[i].onclick=function () {
            show(this.index);
        };
    }
</script>
</body>
</html>
```

在遇见一个以上的选择器是要把每种类型选择器的权值相加，最后的结果就是最后的权值，也就是说当你选择器选的越多。你的权值就会越大，但是我们不建议选择器大于三个（因为浏览器的解读选择器时是从右向左，选择器多会加大浏览器渲染时间）。

上面的代码是我在写tab时遇到一个小问题，就是当我把上面的代码改一下，功能就不能实现（上面的代码是能实现功能的）

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>tab</title>
    <style>
        #tab{/* 权值为：100*/
            width:99%;
            margin: auto;
        }
        ul{width:100%;margin:20px auto;padding: 0;text-align: center;}/* 权值为：1*/
        li{/* 权值为：1*/
            width: 19%;
            list-style: none;
            line-height: 2.5em;
            display: inline-table;
            background-color: #e4b9c0;
            cursor: default;
        }
        li:hover{background-color: #c7254e;}
        .content{/* 权值为：10 */
            width:96%;
            margin: auto;
            background-color: aquamarine;
            text-align: center;
            height: 200px;
        }
          .content div  {/*权值为：100+1=101   */
            display: none;
        }
        /*.content*/ .show{/* 权值为：100 */
            width: 100%;
            display: block;
            background-color: #adadad;
        }
    </style>
</head>
<body>
<div id="tab">
    <ul>
        <li>团队介绍</li>
        <li>团队成就</li>
        <li>团队生活</li>
        <li>团队成员</li>
        <li>团队引力</li>
    </ul>

    <div class="content">
        <div class="each show">简介</div>
        <div class="each">时间线 荣誉展示</div>
        <div class="each">平时生活照片</div>
        <div class="each">地图</div>
        <div class="each">优点  条件</div>
    </div>
</div>
<script>
    var oul=document.getElementById("tab");
    var oli=oul.getElementsByTagName("li");
    var odiv=oul.getElementsByTagName("div")[0].getElementsByTagName("div");
    var len=oli.length;
    function show(index) {
        for(var i=0; i<len;i++){
            if(i===index){
                odiv[i].className="show";
            }
            else{odiv[i].className=""}  //不加 "each"
        }
    }
    for(var i=0;i<len;i++){
        oli[i].index=i;
        oli[i].onclick=function () {
            show(this.index);
        };
    }
</script>
</body>
</html>
```
我发现每个每个该显示的div没有显示，就是在我点击动作点击了后，相对应的没有显示，但是这并不是我想要的结果，经过查证，错误就是这里我们讲的优先级，大家注意没有 我把样式表里的
`    .content .show ` 改成了`  .show` 这样权值也就变了  由之前的 200 变成了这里的 100

```
       .content div  {/*权值为：100+1=101   */
            display: none;
        }
        /*.content*/ .show{/* 权值为：100 */
            width: 100%;
            display: block;
            background-color: #adadad;
        }
```
权值是 .content div 大于 .show，所以浏览器渲染代码最后显示的是优先级高的样式。则始终是 display: none;即使点击把有的元素样式选择器设置成了.show。按着权值还是 display: none; 不管你在什么地方设置，原理是不变的。所以我上面那个代码就是对的，.content .show的权值是200比 .content div的101大，所以当在点击事件发生设置成为 .show时。整个功能是对的。

### CSS 优先级法则：
A  选择器都有一个权值，权值越大越优先；

B  当权值相等时，后出现的样式表设置要优于先出现的样式表设置；

C  创作者的规则高于浏览者：即网页编写者设置的CSS 样式的优先权高于浏览器所设置的样式；

D  继承的CSS 样式不如后来指定的CSS 样式；

E  在同一组属性设置中标有“!important”规则的优先级最大；


**IE又是很特殊的一个详细内容参考**：
[CSS 的优先级机制[总结]](http://www.cnblogs.com/xugang/archive/2010/09/24/1833760.html")
[IE中奇怪的应用CSS的BUG](http://www.cnblogs.com/lhb25/archive/2010/09/19/1831209.html")