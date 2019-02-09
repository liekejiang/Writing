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


