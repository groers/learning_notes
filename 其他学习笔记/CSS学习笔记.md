# CSS学习笔记

[TOC]

# CSS语法

CSS 规则由两个主要的部分构成：**选择器**，以及一条或多条**声明**。CSS声明总是以分号(;)结束，声明总以大括号({})括起来。为了让CSS可读性更强，你可以每行只描述一个属性:

![image-20200916105221476](C:\Users\etan\AppData\Roaming\Typora\typora-user-images\image-20200916105221476.png)

CSS注释以 **/\*** 开始, 以 ***/** 结束

# CSS id和class选择器

## id选择器

id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。HTML元素以id属性来设置id选择器,CSS 中 i**d 选择器以 "#" 来定义**。

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style>
#para1
{
	text-align:center;
	color:red;
} 
</style>
</head>

<body>
<p id="para1">Hello World!</p>
<p>这个段落不受该样式的影响。</p>
</body>
</html>
```

## class选择器

class 选择器用于描述一组元素的样式，class 选择器有别于id选择器，class可以在多个元素中使用。class 选择器在HTML中以class属性表示, 在 CSS 中，**类选择器以一个点"."号显示**：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style>
.center
{
	text-align:center;
}
</style>
</head>

<body>
<h1 class="center">标题居中</h1>
<p class="center">段落居中。</p> 
</body>
</html>
```

你也可以指定特定的HTML元素使用class。

在以下实例中, 所有的 p 元素使用 class="center" 让该元素的文本居中:

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style>
p.center
{
	text-align:center;
}
</style>
</head>

<body>
<h1 class="center">这个标题不受影响</h1>
<p class="center">这个段落居中对齐。</p> 
</body>
</html>
```

# CSS插入样式表

插入样式表的方法有三种:

- 外部样式表(External style sheet)
- 内部样式表(Internal style sheet)
- 内联样式(Inline style)

## 外部样式表

当样式需要应用于很多页面时，外部样式表将是理想的选择。在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观。每个页面使用 <link> 标签链接到样式表。 <link> 标签在（文档的）头部：

```html
<head> <link rel="stylesheet" type="text/css" href="mystyle.css"> </head>
```

浏览器会从文件 mystyle.css 中读到样式声明，并根据它来格式文档。

外部样式表可以在任何文本编辑器中进行编辑。文件不能包含任何的 html 标签。样式表应该以 .css 扩展名进行保存。下面是一个样式表文件的例子：

```
hr {color:sienna;} 
p {margin-left:20px;} 
body {background-image:url("/images/back40.gif");}
```

> 不要在属性值与单位之间留有空格（如："margin-left: 20 px" ），正确的写法是 "margin-left: 20px" 。

## 内部样式表

当单个文档需要特殊的样式时，就应该使用内部样式表。你可以使用 <style> 标签在文档头部定义内部样式表，就像这样:

```
<head> <style> hr {color:sienna;} p {margin-left:20px;} body {background-image:url("images/back40.gif");} </style> </head>
```

## 内联样式

由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。

要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性。本例展示如何改变段落的颜色和左外边距：

```
<p style="color:sienna;margin-left:20px">这是一个段落。</p>
```

## 多重样式优先级

一般情况下，优先级如下：

**内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**

**内联样式 > id 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 标签选择器 = 伪元素选择器**

# CSS背景图像

默认情况下，背景图像进行平铺**重复**显示，以覆盖整个元素实体.

```html
body {background-image:url('paper.gif');}
```

如果图像只在水平方向平铺 (repeat-x), 页面背景会更好些:

```html
body
{
background-image:url('gradient2.png');
background-repeat:repeat-x;
}
```

如果你不想让图像平铺，你可以使用 background-repeat 属性:

```html
body
{
background-image:url('img_tree.png');
background-repeat:no-repeat;
}
```

利用 background-position 属性改变图像在背景中的位置:

```html
body
{
background-image:url('img_tree.png');
background-repeat:no-repeat;
background-position:right top;
}
```

设置`background-attachment:fixed`属性，使背景图片固定

```html
body 
{
background-image:url('smiley.gif');
background-repeat:no-repeat;
background-attachment:fixed;
}
```

背景属性简写，为了简化这些属性的代码，我们可以将这些属性合并在同一个属性中，背景的简写属性为 "background":

```html
body {background:#ffffff url('img_tree.png') no-repeat right top;}
```

当使用简写属性时，属性值的顺序为：:

- background-color
- background-image
- background-repeat
- background-attachment
- background-position

以上属性无需全部使用，你可以按照页面的实际需要使用.

# CSS文本

## 文本对齐

文本排列属性是用来设置文本的水平对齐方式。文本可居中或对齐到左或右,两端对齐.

当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）。

```html
h1 {text-align:center;}
p.date {text-align:right;}
p.main {text-align:justify;}
```

## 文本修饰

text-decoration 属性用来设置或删除文本的装饰。从设计的角度看 text-decoration属性主要是用来删除链接的下划线：

```
a {text-decoration:none;}
```

也可以这样装饰文字：

```
h1 {text-decoration:overline;/*上划线*/}
h2 {text-decoration:line-through;/*删除线*/}
h3 {text-decoration:underline;/*下划线*/}
```

## 文本转换

文本转换属性是用来指定在一个文本中的大写和小写字母。可用于所有字句变成大写或小写字母，或每个单词的首字母大写。

```
p.uppercase {text-transform:uppercase;}
p.lowercase {text-transform:lowercase;}
p.capitalize {text-transform:capitalize;/*首字母大写*/}
```

## 文本缩进

文本缩进属性是用来指定文本的第一行的缩进。

```
p {text-indent:50px;}
```

# CSS字体

font-family 属性设置文本的字体系列。

font-family 属性应该设置几个字体名称作为一种"后备"机制，如果浏览器不支持第一种字体，他将尝试下一种字体。

**注意**: 如果字体系列的名称超过一个字，它必须用引号，如Font Family："宋体"。

多个字体系列是用一个逗号分隔指明：

```
p{font-family:"Times New Roman", Times, serif;}
```

## 字体大小

font-size 属性设置文本的大小。

字体大小的值可以是绝对或相对的大小。

绝对大小：

- 设置一个指定大小的文本
- 不允许用户在所有浏览器中改变文本大小
- 确定了输出的物理尺寸时绝对大小很有用

相对大小：

- 相对于周围的元素来设置大小
- 允许用户在浏览器中改变文字大小

如果你不指定一个字体的大小，默认大小和普通文本段落一样，是16像素（16px=1em）。

### 设置字体大小像素

设置文字的大小与像素，让您完全控制文字大小：

```
h1 {font-size:40px;}
h2 {font-size:30px;}
p {font-size:14px;}
```

上面的例子可以在 Internet Explorer 9, Firefox, Chrome, Opera, 和 Safari 中通过缩放浏览器调整文本大小。

虽然可以通过浏览器的缩放工具调整文本大小，但是，这种调整是整个页面，而不仅仅是文本

### 用em来设置字体大小

为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。

em的尺寸单位由W3C建议。

1em和当前字体大小相等。在浏览器中默认的文字大小是16px。

因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：**px/16=em**

```
h1 {font-size:2.5em;} /* 40px/16=2.5em */
h2 {font-size:1.875em;} /* 30px/16=1.875em */
p {font-size:0.875em;} /* 14px/16=0.875em */
```

在上面的例子，em的文字大小是与前面的例子中像素一样。不过，如果使用 em 单位，则可以在所有浏览器中调整文本大小。

不幸的是，仍然是IE浏览器的问题。调整文本的大小时，会比正常的尺寸更大或更小。

### 使用百分比和EM组合

在所有浏览器的解决方案中，设置 <body>元素的默认字体大小的是百分比：

```
body {font-size:100%;}
h1 {font-size:2.5em;}
h2 {font-size:1.875em;}
p {font-size:0.875em;}
```

我们的代码非常有效。在所有浏览器中，可以显示相同的文本大小，并允许所有浏览器缩放文本的大小。

# CSS链接

链接的样式，可以用任何CSS属性（如颜色，字体，背景等）。特别的链接，可以有不同的样式，这取决于他们是什么状态。这四个链接状态是：

```
a:link {color:#000000;}      /* 未访问链接*/
a:visited {color:#00FF00;}  /* 已访问链接 */
a:hover {color:#FF00FF;}  /* 鼠标移动到链接上 */
a:active {color:#0000FF;}  /* 鼠标点击时 */
```

当设置为若干链路状态的样式，也有一些顺序规则：

- a:hover 必须跟在 a:link 和 a:visited后面
- a:active 必须跟在 a:hover后面

常见链接样式：

```
a:link {text-decoration:none;}
a:visited {text-decoration:none;}
a:hover {text-decoration:underline;}
a:active {text-decoration:underline;}
```

