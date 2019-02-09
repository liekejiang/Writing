本章介绍CSS里面的一些基本元素:

### 背景 background
CSS背景用于定义HTML元素的背景，有以下几种选项：
* background-color
* background-image
* background-repeat
* background-attachment
* background-position

#### 从颜色开始
**background-color**属性定义了元素的背景颜色，要改变页面的背景颜色应该在**&lt;body&gt;**中使用,如下例：
```css
body {background-color:#b0c4de;}
```
颜色值通常有3种定义方式：
* 十六进制如上面的实例
* RGB： rgb(255,0,0)
* 颜色名称："red"

#### 其他属性留坑

### 文本 Text
#### 颜色
颜色属性通常用来设置文字的颜色。格式还是上面介绍的三种。
```css
h1 {color:rgb(255,0,0);}
```
#### 文本对齐
当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）。
```css
h1 {text-align:center;}
p.date {text-align:right;}
p.main {text-align:justify;}
```

#### 文本修饰
text-decoration 属性用来设置或删除文本的装饰。
从设计的角度看 text-decoration属性主要是用来删除链接的下划线：
```css
/*a {text-decoration:none;}*/
```
若不使用该修饰符，超链接会有明显的下划线。

#### 文本转换与缩进
文本转换属性是用来指定在一个文本中的大写和小写字母。可用于所有字句变成大写或小写字母，或每个单词的首字母大写。
```css
p.uppercase {text-transform:uppercase;}
p.lowercase {text-transform:lowercase;}
p.capitalize {text-transform:capitalize;}
```
文本缩进属性是用来指定文本的第一行的缩进。
```html
p {text-indent:50px;}
```

### 字体 font-size
字体属性包括字体格式(sans-serif)，大小，加粗，文字样式。
#### 字体样式
CSS中有两种类型的字体系列名词:
* 通用字体系列 - 拥有相似外观的字体系统组合(如 “Serif”,"Monospace")
* 特定字体系列 - 一个特定的字体系列(如 "Times" "Courier")

font-family 属性设置文本的字体系列。在工程中一般会设置多个字体名称作为一种“后备机制”， 如果浏览器不支持第一种字体，将会自动跳转下一种字体。若字体系列名称超过一个字，必须用引号，如Font Family：“宋体”。多个字体使用逗号分隔。
```css
p{
    font-family: "Times New Roman", Times, serif;
}
```
#### 字体格式
可以设置三种不同的格式：
* 正常
* 斜体
* 倾斜的文字（与斜体有区别, 用的很少)

```css
p.normal {font-style:normal;}
p.italic {font-style:italic;}
p.oblique {font-style:oblique;}
```

#### 字体大小
font-size属性设置文本的大小。字体大小可以是相对的或者绝对的，如果不指定大小，默认值即普通文本段落大小为16像素(16px=1em)。

**绝对大小：**
* 设置一个指定大小的文本
* 不允许用户在所有浏览器中改变文本大小
* 确定了输出的物理尺寸时绝对大小很有用

**相对大小**：
* 相对周围的元素来设置大小
* 允许用户在浏览器中改变文字大小

```css
h1 {font-size:40px;}
h2 {font-size:30px;}
p {font-size:14px;}

body {font-size:100%;}
h1 {font-size:2.5em;}
h2 {font-size:1.875em;}
p {font-size:0.875em;}
```
除了像素px还有另一种单位**em**，(16px=1em)可以有小数。**em*还可以和百分比相结合。
```css
h1 {font-size:2.5em;} /* 40px/16=2.5em */
h2 {font-size:1.875em;} /* 30px/16=1.875em */
p {font-size:0.875em;} /* 14px/16=0.875em */
```

#### 其他属性
* font-weight : 字体粗细
* font-variant: 字体小写大写字体

### 链接