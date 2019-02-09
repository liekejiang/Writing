# CSS
CSS（Cascading Style Sheets）全称是层叠样式表，用于渲染HTML元素。CSS代码通常存储再样式表（.css）中。
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
<style>
body {background-color:white;}
h1   {font-size:36pt;}
h2   {color:blue;}
p    {margin-left:50px;}
</style>
</head>

<body>

<h1>这个标题设置的大小为 36 pt</h1>
<h2>这个标题设置的颜色为蓝色：blue</h2>

<p>这个段落的左外边距为 50 像素：50px</p> 

</html>
```
这是一个最简单的实例，显示内容可以在[此](http://www.runoob.com/try/try.php?filename=ex1)看到。这里使用了内联样式，将CSS写在HTML表头的&lt;style&gt;里面。这里的CSS代码主要定义了显示的背景颜色，&lt;h1&gt;&lt;h2&gt;标题的字体大小和&lt;p&gt;段落的左外边距。

TIPS: CSS注释以 /* 开始 */ 结束

### 基本语法
CSS规则主要由两个主要的部分构成：选择器以及一条或多条声明
```html
h1 {color:blue ; font-size:12px;}
```
在上面这个例子里面，**h1**就是选择器，表示选择了&lt;h1&gt;这条标注代码进行渲染。而两个大括号中间的内容代表声明，表示具体的渲染内容。 color和font-size代表属性，blue和12px代表属性对应的值。声明总是以分号";"结束。

### ID和class选择器
在阅读SAAS这本书的内容时总会在html相关部分看到选择器这个词，现在就来介绍一下：
如果要在HTML元素中设置CSS格式，需要在元素中设置**id**和**class**选择器。

#### ID选择器
id选择器可以为标有特定id的HTML元素指定特定的样式。HTML元素以**ID**属性来设置**ID**选择器，**ID**选择器以#井号定义。一般**ID**不能以数字开头。
```css
#para1
{
    text-align:center;
    color:red;
}
```
在上面这个实例里面，定义的样式规则适用于id = “para1”的元素。

#### class选择器
class选择器用于描述一组元素的样式，class选择器有别于id选择器，id是唯一的，而class可以在多个元素中使用。在CSS中，类选择器以一个点号“.”显示:
```css
.center{text-align:center;}
```
上面实例代表所有拥有center类的HTML元素均居中。
我们也可以指定特定的HTML元素使用class, 如下所示，所有p元素都居中。
```css
p.center {text-algin:center;}
```

### HTML与CSS
在HTML的内容已经简要介绍过CSS的使用，这里稍微详细一点。
CSS(Cascading Style Sheets)用于渲染HTML元素标签的样式，可以通过以下方法添加到HTML中：
* 内联样式-在HTML元素中使用"stlye"属性。
* 内部样式表-在HTML文档头部&lt;head&gt;区域使用&lt;style&gt;元素来包含CSS。
* 外部引用-使用外部CSS文件。
最推荐的使用方式是外部引用CSS文件。

#### 内联样式
当特殊的样式需要应用到个别元素的时候，就可以使用内联样式。使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何CSS属性。
以下实例显示出如何改变段落的颜色和做外边距。
```html
<p style="color:blue;margin-left:20px;">This is a paragraph.</p>
```
或者改变段落的颜色和左外边距：
```html
<body style="background-color:yellow;">
<h2 style="background-color:red;">这是一个标题</h2>
<p style="background-color:green;">这是一个段落。</p>
</body>
```
还可以改变字体的格式、颜色和大小：
```html
<h1 style="font-family:verdana;">一个标题</h1>
<p style="font-family:arial;color:red;font-size:20px;">一个段落。</p>
```

#### 内部样式表
当单个文件需要特别样式时，就可以使用内部样式表。你可以在&lt;head&gt;部分通过&lt;style&gt;标签定义内部样式：
```html
<head>
<style type="text/css">
body {background-color:yellow;}
p {color:blue;}
</style>
</head>
```
#### 外部样式表
当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。使用外部样式表，你就可以通过改变一个文件来改变整个站点的外观。
```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

#### 番外：多重样式
有些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中继承过来。例如下例：
```css
/*外部样式表拥有针对h3选择器的三个属性*/
h3
{
    color:red;
    text-align:left;
    font-size:8pt;
}
/*内部样式表拥有针对h3选择器的两个属性*/
h3{
    text-align:right;
    font-size:20pt;
}
```
内外样式表同时使用得到的格式是：
```css
{
color:red;
text-aligh:right;
font-size:20pt;
}
```
即内部样式表优先，只有内部样式表没有定义的格式会采用外部样式表的值。一般情况下：
**（内联样式）Inline style**> **（内部样式）Internal style sheet** >**（外部样式）External style sheet** > **浏览器默认样式**
