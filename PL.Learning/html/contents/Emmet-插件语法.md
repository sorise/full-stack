## [Emmet 插件语法](#)
> **介绍**: 在前端开发的过程中，最费时间的工作就是写 HTML、CSS 代码。一堆的标签、属性、括号等，头疼。
这里推荐一个Emmet语法规则，让你写的时候爽到飞起，能大大提高代码书写，只需要敲一行代码就能生成你想要的完整HTML结构。


### [1. id（#）,class（.）语法](#)

```html
<!-- # . -->

<!-- div#show-table 按个tab键 -->
<div id="show-table">
    <!-- table.student -->
    <table class="student"></table>
</div>
```
### [2. 子节点（>），兄弟节点（+），上级节点（^）](#)

#### 2.1 > 子节点

```html
<!-- nav>ol.menu>li{菜单$}*10 -->
<nav>
    <ol class="menu">
        <li>菜单1</li>
        <li>菜单2</li>
        <li>菜单3</li>
        <li>菜单4</li>
        <li>菜单5</li>
        <li>菜单6</li>
        <li>菜单7</li>
        <li>菜单8</li>
        <li>菜单9</li>
        <li>菜单10</li>
    </ol>
</nav>
```

#### 2.2 + 兄弟节点

```html
<!-- div.student+div.teacher+div -->
<div class="student"></div>
<div class="teacher"></div>
<div></div>
```

#### 2.3 ^ 上级节点
`div>ul>li^div (这里的^是接在li后面所以在li的上一级，与ul成了兄弟关系,当然两个^^就是上上级）`

```html
<div>
   <ul>
     <li></li>
   </ul>
   <div></div>
/div>
```

### [3. 重复（*）](#)
`（*号后面添加数字表示重复的元素个数）`
```html
<!-- div*5 -->
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
```

##### [4. 分组 ()](#)
分组指令：()

```html
<!-- div>(ul>li*5>a[href='#']{菜单$})+div>p.passage -->
<div>
    <ul>
        <li><a href="#">菜单1</a></li>
        <li><a href="#">菜单2</a></li>
        <li><a href="#">菜单3</a></li>
        <li><a href="#">菜单4</a></li>
        <li><a href="#">菜单5</a></li>
    </ul>
    <div>
        <p class="passage"></p>
    </div>
</div>
```

### [5. 属性（[attr]）](#)
属性指令：`[]` :`a[href='#' id='val']`（中括号内填写属性键值对的形式，并且空格隔开）

```html
<!-- a[href='http://www.baidu.com' id='val']{百度一下} -->
<a href="http://www.baidu.com" id="val">百度一下</a>
```

##### [6. $ 编号](#)
$代表一位数，后面更上*数字就代表从1递增到填写的数字

```html
<!-- ul>li.test$*3 -->

<ul>
    <li class="test1"></li>
    <li class="test2"></li>
    <li class="test3"></li>
</ul>
```
如果想自定义从几开始递增的话就利用：$@+数字*数字

```html
<!-- ul>li.test$@3*3 -->
<ul>
    <li class="test3"></li>
    <li class="test4"></li>
    <li class="test5"></li>
</ul>
```

### [7. 文本 {} ](#)
{里面填写内容，可以和$一起组合使用哦}

```html
<!-- ul>li.test$*3{测试$}  -->
ul>
  <li class="test1">测试1</li>
  <li class="test2">测试2</li>
  <li class="test3">测试3</li>
</ul>
```

### [8. 灵活运用，语法简单，并不困难](#)
多运用,很多事,唯手熟尔