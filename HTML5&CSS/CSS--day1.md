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




