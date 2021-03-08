# HTML学习笔记

[TOC]

# 重要标签

## 常见类标签

1. `<!DOCTYPE html>` 声明为 HTML5 文档

2. `<html>` 元素是 HTML 页面的根元素

3. `<head>` 元素包含了文档的元（meta）数据，如 `<meta charset="utf-8">` 定义网页编码格式为 `utf-8`。

4. `<title>` 元素描述了文档的标题

5. `<body>` 元素包含了可见的页面内容，定义了HTML文档的主体

6. `<h1>` 元素定义一个大标题（html标题有1到6共六个等级）

7. `<p>` 元素定义一个段落

8. `<!DOCTYPE>`<!DOCTYPE>声明有助于浏览器中正确显示网页。网络上有很多不同的文件，如果能够正确声明HTML的版本，浏览器就能正确显示网页内容。doctype 声明是不区分大小写的。

9. 目前在大部分浏览器中，直接输出中文会出现中文乱码的情况，这时候我们就需要在头部将字符声明为 UTF-8 或 GBK。

   ```html
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="UTF-8"> <!--<meta charset="GBK">-->
   <title>页面标题</title>
   </head>
   <body>
    
   <h1>我的第一个标题</h1>
    
   <p>我的第一个段落。</p>
    
   </body>
   ```

10. `<a href="https://www.runoob.com">这是一个链接</a>`定义一个链接（href代表hypertext reference）

11. `<br/>`换行标签

12. `<hr>`分割线，是horizontal rule的缩写

13. `<!-- 这是一个注释 -->`注释



# HTML简介

- HTML 指的是超文本标记语言: **H**yper**T**ext **M**arkup **L**anguage

- HTML 不是一种编程语言，而是一种**标记**语言

- 标记语言是一套**标记标签** (markup tag)

- HTML 使用标记标签来**描述**网页

- HTML 文档包含了HTML **标签**及**文本**内容

- HTML文档也叫做 **web 页面**

- HTML 元素："HTML 标签" 和 "HTML 元素" 通常都是描述同样的意思。但是严格来讲, 一个 HTML 元素包含了**开始标签**与**结束标签**。

- html网页结构

  ![image-20200911163132665](C:\Users\etan\AppData\Roaming\Typora\typora-user-images\image-20200911163132665.png)

  > 只有 <body> 区域 (白色部分) 才会在浏览器中显示。

# HTML元素

- HTML 元素以**开始标签**起始
- HTML 元素以**结束标签**终止
- **元素的内容**是开始标签与结束标签之间的内容
- 某些 HTML 元素具有**空内容（empty content）**
- 空元素**在开始标签中进行关闭**（以开始标签的结束而结束）
- 大多数 HTML 元素可拥有**属性**
- 没有内容的HTML元素被称为空元素，空元素是在开始标签中关闭的。如`<br>`就是没有关闭标签的空元素。但是建议使用`<br/>`而不是`<br>`。

# HTML 属性

- HTML 元素可以设置**属性**

- 属性可以在元素中添加**附加信息**

- 属性一般描述于**开始标签**

- 属性总是以**名称/值对**的形式出现，**比如：name="value"**。

- 属性值应该始终被包括在引号内。双引号是最常用的，不过使用单引号也没有问题。在某些个别的情况下，比如属性值本身就含有双引号，那么您必须使用单引号。

- 常用属性

  | 属性  | 描述                                                         |
  | :---- | :----------------------------------------------------------- |
  | class | 为html元素定义一个或多个类名（classname）(类名从样式文件引入) |
  | id    | 定义元素的唯一id                                             |
  | style | 规定元素的行内样式（inline style）                           |
  | title | 描述了元素的额外信息 (作为工具条使用)                        |

# HTML文本格式化（格式化标签）

## 文本格式化类

- `<b></b>`和`<strong></strong>`代表加粗

- `<i></i>`和`<em></em>`代表斜体(em是emphasize的缩写)

- `<big>`代表放大

- `<small>`代表缩小

- `<sub>`代表下标

- `<sup>`代表上标

- `<del>`代表删除文本

- `<ins>`代表插入的文本，表示形式是加上下划线

  ```html
  <p>My favorite color is <del>blue</del> <ins>red</ins>!</p>
  
  > My favorite color is blue red!
  ```

## 计算机输出类（网页上显示格式不会变基本）

- `<code>`定义计算机代码

- `<kbd>`定义键盘码

- `<samp>`定义计算机样本

- `<var>`定义变量

- `<dfn>`定义项目

- `<pre>`定义预格式文本，类似于编程语言中反撇号包括的文字的输出

  ```html
  <pre>
  此例演示如何使用 pre 标签
  对空行和    空格
  进行控制
  </pre>
  
  > 此例演示如何使用 pre 标签
  > 对空行和    空格
  > 进行控制
  ```

## 引文，引用，标签定义类

- `<abbr>`定义缩写abbr代表abbreviation 

  ```html
  The<abbr title="World Health Organization">WHO</abbr> was founded in 1948.
  ```

- `<address>`代表地址元素内容在网页上表现形式同`<pre>`

- `<bdo>`可以切换文字的书写顺序

  ```html
  <p><bdo dir="rtl">该段落文字从右到左显示。</bdo></p>  
  ```

- `<blockquote>`代表长引用语，可以设置site属性表示来源，通常浏览器会把blockquote元素呈现为一段左右两侧缩进(40px)的文本。
- `<q>`代表短引用语，表现形式为以双引号括起来
- `<cite>`代表引用，引证，表现形式为斜体

# HTML链接

- 使用`<a href="https://www.runoob.com/">访问菜鸟教程!</a>`创建一个链接

- target属性可以设置打开方式，默认在当前页面打开链接，使用`target = "_blank"`可以在新页面打开链接

  ```html
  <a href="https://www.runoob.com/" target = "_blank">访问菜鸟教程!</a>
  ```

- **id属性**可用于创建在一个HTML**文档书签标记**。

  **提示:** 书签是不以任何特殊的方式显示，在HTML文档中是不显示的，所以对于读者来说是隐藏的。

  在HTML文档中插入ID:

  ```html
  <a id="tips">有用的提示部分</a>
  ```

  在HTML文档中创建一个链接到"有用的提示部分(id="tips"）"：

  ```html
  <a href="#tips">访问有用的提示部分</a>
  ```

  或者，从另一个页面创建一个链接到"有用的提示部分(id="tips"）"：

  ```html
  <a href="https://www.runoob.com/html/html-links.html#tips">访问有用的提示部分</a>
  ```

- 请始终在路径末尾加上正斜杠，否就会向服务器产生两次 HTTP 请求。这是因为服务器会添加正斜杠到这个地址，然后创建一个新的请求。

- 利用`style`属性创建一个没有下划线的链接

  ```html
  <a href="//www.runoob.com/" style="text-decoration:none;">访问 runoob.com!</a>
  ```

# HTML头部

## HTML `<head>` 元素

- `<head>` 元素包含了所有的头部标签元素。在 `<head>`元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。
- 可以添加在头部区域的元素标签为: `<title>, <style>, <meta>, <link>, <script>, <noscript> 和 <base>。`

## HTML`<base>`元素 

`<base>`标签定义了页面所有链接的基地址，往后的链接使用相对地址即可，此外，它还可以定义整个页面连接的打开方式，如设置为在新窗口打开。

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<base href="//www.runoob.com/images/" target="_blank">
</head>

<body>
<img src="logo.png"> - 注意这里我们设置了图片的相对地址。能正常显示是因为我们在 head 部分设置了 base 标签，该标签指定了页面上所有链接的默认 URL，所以该图片的访问地址为 "http://www.runoob.com/images/logo.png"
<br><br>
<a href="//www.runoob.com">菜鸟教程</a> - 注意这个链接会在新窗口打开，即便它没有 target="_blank" 属性。因为在 base 标签里我们已经设置了 target 属性的值为 "_blank"。

</body>
</html>
```

## HTML`<meta>`元素

`<meta>` 元素来描述HTML文档的描述，关键词，作者，字符集等。

```html
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<meta name="description" content="免费在线教程">
<meta name="keywords" content="HTML,CSS,XML,JavaScript">
<meta name="author" content="runoob">
<meta http-equiv="refresh" content="30"> <!--每30s刷新页面-->
</head>
```

## HTML `<link>` 元素

`<link>` 标签定义了文档与外部资源之间的关系。

`<link> `标签通常用于链接到样式表:

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

## HTML `<style> `元素

`<style>` 标签定义了HTML文档的样式文件引用地址.

在`<style> `元素中你也可以直接添加样式来渲染 HTML 文档:

```html
<head>
<style type="text/css">
body {background-color:yellow}
p {color:blue}
</style>
</head>
```

## HTML `<script>` 元素

`<script>`标签用于加载脚本文件，如： JavaScript。

# HTML样式-CSS

CSS 是在 HTML 4 开始使用的,是为了更好的渲染HTML元素而引入的.

CSS 可以通过以下方式添加到HTML中:

- 内联样式- 在HTML元素中使用"style" **属性**
- 内部样式表 -在HTML文档头部 <head> 区域使用<style> **元素** 来包含CSS
- 外部引用 - 使用外部 CSS **文件**

最好的方式是通过外部引用CSS文件.

## 内联样式

- 当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何 CSS 属性。以下实例显示出如何改变段落的颜色和左外边距。

    ```html
    <p style="color:blue;margin-left:20px;">这是一个段落。</p>
    ```

- 使用`style="background-color:green;"`属性可以改变背景颜色

- 可以使用font-family（字体），color（颜色），和font-size（字体大小）属性来定义字体的样式:

  ```html
  <h1 style="font-family:verdana;">一个标题</h1> 
  <p style="font-family:arial;color:red;font-size:20px;">一个段落</p>
  ```

- 使用 `text-align`（文字对齐）属性指定文本的水平与垂直对齐方式：

## 内部样式表

当单个文件需要特别样式时，就可以使用内部样式表。你可以在<head> 部分通过 <style>标签定义内部样式表:

```html
<head>
<style type="text/css">
body {background-color:yellow;}
p {color:blue;}
</style>
</head>
```

## 外部样式表

当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
<!--rel="stylesheet"指的是被连接文档是此文档的外部样式表-->
```

# HTML图像

可以使用以下语句添加图像，其中alt代表alternative，为预备的可替换的文本，无法载入图像时将显示这个信息。border代表镶边宽度。

```html
<img border="0" src="/images/pulpit.jpg" alt="Pulpit rock" width="304" height="228">
```

**提示:** 指定图像的高度和宽度是一个很好的习惯。如果图像指定了高度宽度，页面加载时就会保留指定的尺寸。如果没有指定图片的大小，加载页面时有可能会破坏HTML页面的整体布局。

**注意:** 假如某个 HTML 文件包含十个图像，那么为了正确显示这个页面，需要加载 11 个文件。加载图片是需要时间的，所以我们的建议是：慎用图片。

**注意:** 加载页面时，要注意插入页面图像的路径，如果不能正确设置图像的位置，浏览器无法加载图片，图像标签就会显示一个破碎的图片。

## 创建图片映射，每一个区域都是一个超链接

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <base href="//www.runoob.com/try/demo_source/" target="_blank">
    <title>菜鸟教程(runoob.com)</title>
</head>
<body>

<p>点击太阳或其他行星，注意变化：</p>

<img src="planets.gif" width="145" height="126" alt="Planets" usemap="#planetmap">

<map name="planetmap">
    <area shape="rect" coords="0,0,82,126" alt="Sun" href="sun.htm">
    <area shape="circle" coords="90,58,3" alt="Mercury" href="mercur.htm">
    <area shape="circle" coords="124,58,8" alt="Venus" href="venus.htm">
</map>

</body>
</html>
```

# HTML表格

表格实例，`<tr>`代表table row，`<td>`代表table data是单元格内容。`<th>`代表table header表头。`<caption>`代表标题

```html
<table border="1">
    <caption>Monthly savings</caption>
    <tr>
        <th>Header 1</th>
        <th>Header 2</th>
    </tr>
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```

表格跨列，通过设置`colspan`属性：

```html
<table border="1">
<tr>
  <th>Name</th>
  <th colspan="2">Telephone</th>
</tr>
<tr>
  <td>Bill Gates</td>
  <td colspan = '2'>555 77 854 555 77 855</td>
</tr>
</table>
```

表格跨行，设置`rowspan属性`：

```html
<table border="1">
<tr>
  <th>First Name:</th>
  <td>Bill Gates</td>
</tr>
<tr>
  <th rowspan="2">Telephone:</th>
  <td>555 77 854</td>
</tr>
<tr>
  <td>555 77 855</td>
</tr>
</table>
```

通过在`table`元素中添加`cellpadding`属性来设置单元格边距：

```html
<table border="1" cellpadding="10">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>
```

通过在`<table>`元素中添加`cellspacing`属性来设置单元格间距：

```html
<table border="1" cellspacing="10">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>
```

通过在`<table>`元素中的`<colgroup>`元素来为列分组，可以为每个组设置不同的属性

```html

<table border="1">
  <colgroup>
    <col span="2" style="background-color:red">
    <col style="background-color:yellow">
  </colgroup>
  <tr>
    <th>ISBN</th>
    <th>Title</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>3476896</td>
    <td>My first HTML</td>
    <td>$53</td>
  </tr>
  <tr>
    <td>5869207</td>
    <td>My first CSS</td>
    <td>$49</td>
  </tr>
</table>
```

通过在`<head>`和`<table>`元素中的`<thead>`元素设置表格页眉样式，通过`<tbody>`设置主题样式，通过`<tfoot>`设置页脚样式

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style type="text/css">
thead {color:green;}
tbody {color:blue;}
tfoot {color:red;}
</style>
</head>
<body>

<table border="1">
  <thead>
    <tr>
      <th>Month</th>
      <th>Savings</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Sum</td>
      <td>$180</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>January</td>
      <td>$100</td>
    </tr>
    <tr>
      <td>February</td>
      <td>$80</td>
    </tr>
  </tbody>
</table>

<p><b>提示:</b>  thead, tbody, 和 tfoot 元素默认不会影响表格的布局。不过，您可以使用 CSS 来为这些元素定义样式，从而改变表格的外观。</p>

</body>
</html>
```

![image-20200914153112072](C:\Users\etan\AppData\Roaming\Typora\typora-user-images\image-20200914153112072.png)

# HTML列表

## 无序列表

使用`<ul>`标签，项目用小圆点标记

```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

在`<ul>`标签中设置`<style>`属性可以设置标记样式，style="list-style-type:disc"代表圆点，为默认设置，style="list-style-type:circle"代表圆圈，style="list-style-type:square"代表正方形。

## 有序列表

使用`<ol>`标签，项目用编号的数字标记

```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```

通过设置`<ol>`标签的`type`属性可以设置编号样式，不设置或者设置“1”代表数字，“A”代表大写字母，“a”代表小写字母，设置"I"代表大写罗马数字，设置”i“代表小写罗马数字。

# HTML区块

## HTML 区块元素

大多数 HTML 元素被定义为**块级元素**或**内联元素**。

块级元素在浏览器显示时，通常会以新行来开始（和结束）。

实例: <h1>, <p>, <ul>, <table>

## HTML 内联元素

内联元素在显示时通常不会以新行开始。

实例: <b>, <td>, <a>, <img>

## HTML `<div>`元素

HTML <div> 元素是块级元素，它可用于组合其他 HTML 元素的容器。<div> 元素没有特定的含义。如以下代码，可以将文档中的一个区域显示为蓝色。div代表divsion

```html
<div style="color:#0000FF">
  <h3>这是一个在 div 元素中的标题。</h3>
  <p>这是一个在 div 元素中的文本。</p>
</div>
```

## HTML `<span> `元素

HTML <span> 元素是内联元素，可用作文本的容器<span> 元素也没有特定的含义。当与 CSS 一同使用时，<span> 元素可用于为部分文本设置样式属性。

# HTML 表单

表单是一个包含表单元素的区域。

表单元素是允许用户在表单中输入内容,比如：文本域(textarea)、下拉列表、单选框(radio-buttons)、复选框(checkboxes)等等。

表单使用表单标签 <form> 来设置:

```html
<form>
.
input 元素
.
</form>
```

多数情况下被用到的表单标签是输入标签（<input>）。

输入类型是由类型属性（type）定义的。大多数经常被用到的输入类型如下：

## 文本域（Text Fields）

文本域通过<input type="text"> 标签来设定，当用户要在表单中键入字母、数字等内容时，就会用到文本域。

```html
<form>
First name: <input type="text" name="firstname"><br>
Last name: <input type="text" name="lastname">
</form>
```

浏览器显示如下：

<form>
First name: <input type="text" name="firstname"><br>
Last name: <input type="text" name="lastname">
</form>

## 密码字段

密码字段通过标签<input type="password"> 来定义:

```html
<form>
Password: <input type="password" name="pwd">
</form>
```

浏览器显示效果如下

<form>
Password: <input type="password" name="pwd">
</form>

## 单选按钮（Radio Buttons）

<input type="radio"> 标签定义了表单单选框选项

```html
<form>
<input type="radio" name="sex" value="male">Male<br>
<input type="radio" name="sex" value="female">Female
</form>
```

显示效果如下

<form>
<input type="radio" name="sex" value="male">Male<br>
<input type="radio" name="sex" value="female">Female
</form>

## 复选框（Checkboxes）

<input type="checkbox"> 定义了复选框. 用户需要从若干给定的选择中选取一个或若干选项。

```html
<form>
<input type="checkbox" name="vehicle" value="Bike">I have a bike<br>
<input type="checkbox" name="vehicle" value="Car">I have a car
</form>
```

显示效果如下

<form>
<input type="checkbox" name="vehicle" value="Bike">I have a bike<br>
<input type="checkbox" name="vehicle" value="Car">I have a car
</form>

## 提交按钮(Submit Button)

<input type="submit"> 定义了提交按钮.

当用户单击确认按钮时，表单的内容会被传送到另一个文件。表单的动作属性定义了目的文件的文件名。由动作属性定义的这个文件通常会对接收到的输入数据进行相关的处理。

```html
<form name="input" action="html_form_action.php" method="get">
Username: <input type="text" name="user">
<input type="submit" value="Submit">
</form>
```

<form name="input" action="html_form_action.php" method="get">
Username: <input type="text" name="user">
<input type="submit" value="Submit">
</form>

假如您在上面的文本框内键入几个字母，然后点击确认按钮，那么输入数据会传送到 "html_form_action.php" 的页面。该页面将显示出输入的结果。

## 设置简单选下拉列表

```html
<form action="">
<select name="cars">
<option value="volvo">Volvo</option>
<option value="saab">Saab</option>
<option value="fiat">Fiat</option>
<option value="audi">Audi</option>
</select>
</form>
```

## 设置文本框

```html
<textarea rows="10" cols="30">
我是一个文本框。
</textarea>
```

## 重置表单

```html
<input type="reset" value="重置">
```

# HTML框架

通过使用框架，你可以在同一个浏览器窗口中显示不止一个页面。height 和 width 属性用来定义iframe标签的高度与宽度。可以通过设置`frameborder="0"`去除边框

```html
<iframe src="demo_iframe.htm" width="200" height="200" frameborder="0"></iframe>
```

## 在iframe框架中显示连接内容

```html
<iframe src="demo_iframe.htm" name="iframe_a"></iframe>
<p><a href="//www.runoob.com" target="iframe_a">RUNOOB.COM</a></p>
```

# HTML颜色

在style属性中，可以通过6位十六进制数（每2位代表一个通道)设置颜色：

```html
style="background-color:#900000
```

也可以通过文字设置颜色：

```
style="background-color:green
```

也可以通过这种括号方式设置，透明度范围 0~1，0 表示全透明。

```
style="background-color:rgba(255,0,0,.5) <!--在最后的透明通道中，省略了0-->
```

# HTML字符实体

## HTML 实体

在 HTML 中，某些字符是预留的。

在 HTML 中不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。

如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。 字符实体类似这样：

```
&entity_name;
或
&#entity_number;
```

如需显示小于号，我们必须这样写：`&lt; 或 &#60; 或 &#060;`

浏览器总是会截短 HTML 页面中的空格。如果您在文本中写 10 个空格，在显示该页面之前，浏览器会删除它们中的 9 个。如需在页面中增加空格的数量，你需要使用`&nbsp`添加不间断空格。

提示： 使用实体名而不是数字的好处是，名称易于记忆。不过坏处是，浏览器也许并不支持所有实体名称（对实体数字的支持却很好）。

| 显示结果 | 描述        | 实体名称          | 实体编号 |
| :------- | :---------- | :---------------- | :------- |
|          | 空格        | `&nbsp;`            | `&#160;`   |
| <        | 小于号      | `&lt;`              | `&#60;`    |
| >        | 大于号      | `&gt;`              | `&#62;`    |
| &        | 和号        | `&amp;`             | `&#38;`    |
| "        | 引号        | `&quot;`            | `&#34;`    |
| '        | 撇号        | `&apos; (IE不支持)` | `&#39;`    |
| ￠       | 分          | `&cent;`            | `&#162;`   |
| £        | 镑          | `&pound;`           | `&#163;`   |
| ¥        | 人民币/日元 | `&yen;`             | `&#165;`   |
| €        | 欧元        | `&euro;`            | `&#8364;`  |
| §        | 小节        | `&sect;`            | `&#167;`   |
| ©        | 版权        | `&copy;`            | `&#169;`   |
| ®        | 注册商标    | `&reg;`             | `&#174;`   |
| ™        | 商标        | `&trade;`           | `&#8482;`  |
| ×        | 乘号        | `&times;`           | `&#215;`   |
| ÷        | 除号        | `&divide;`          | `&#247;`   |

# HTML元素全称

> 参考：https://www.runoob.com/html/html-tag-name.html