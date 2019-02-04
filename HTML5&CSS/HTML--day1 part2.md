### 1.HTML标题
在day1 part1中已经介绍过了在HTML&lt;body&gt;&lt;/body&gt;中标题的使用方法&lt;h1&gt;-&lt;h6&gt;，一共六个等级。

#### 规范
使用要规范，仅在标题使用&lt;h1&gt;-&lt;h6&gt;,而不要把其当作文本加粗的方法使用，文本加粗有其他命令。

#### 水平线
&lt;hr&gt;标签在HTML页面中创建水平线, 同一行的多个&lt;hr&gt;会创建多个水平线。一般使用水平线分割内容。与&lt;br&gt;一样，&lt;hr&gt;也是一个空元素，使用&lt;hr&gt;会更加安全。

### 2.HTML段落
同样在part1 已经介绍过使用&lt;p&gt;&lt;/p&gt;构建段落。以及使用&lt;br&gt;对段落换行, 每组表示文本的&lt;p&gt;&lt;/p&gt;以及下面将要介绍的格式化标记符不会自动换行。

#### 格式化
我们无法确定HTML在不同浏览器上显示的效果，因为这收到屏幕大小以及不同浏览器版本的制约。我们无法通过在HTML代码中添加额外的空格或者换行来改变输出的效果。当显示页面时，浏览器会移除源代码中多余的空格和空行，所有连续的空格空行只会算作一个。

### 3.文本格式化操作
使用标签&lt;b&gt;&lt;/b&gt;或&lt;i&gt;&lt;/i&gt;对输出的文本进行格式操作，如**加粗**或者*斜体*。这种标签被称为格式化标签,格式化标签在使用时与&lt;p&gt;&lt;/p&gt;一致，替换&lt;p&gt;&lt;/p&gt;使用：
```html
<b>这个文本加粗</b>
<strong>这个文本也加粗</strong>

<big>这个文本放大<big>
<small>这个文本缩小 </small>

<i>这个文本斜体</i>
<em>这个文本也斜体</em>

<p>这个文本有<sup>上标</sup>以及<sub>下标</sub></p> 

<pre>
空 格
和空行会完整显现
</pre>
```
需要注意，使用上下标的行前后会自动换行，而且现在倾向于使用&lt;strong&gt;&lt;/strong&gt;代替&lt;b&gt;&lt;/b&gt;进行加粗，使用&lt;em&gt;&lt;/em&gt;代替&lt;i&gt;&lt;/i&gt;进行斜体操作。
&lt;pre&gt; &lt;/pre&gt;标签中的内容的空格和空行会完整保留, 但是字体会缩小。可以导致段落断开的标签（例如标题、&lt;p&gt; 和 &lt;address&gt; 标签）绝不能包含在&lt;pre&gt; 所定义的块里。尽管有些浏览器会把段落结束标签解释为简单地换行，但是这种行为在所有浏览器上并不都是一样的。&lt;pre&gt;元素中允许的文本可以包括物理样式和基于内容的样式变化，还有链接、图像和水平分隔线。当把其他标签（比如 &lt;a&gt; 标签）放到 &lt;pre&gt; 块中时，就像放在 HTML/XHTML 文档的其他部分中一样即可。
多个格式化操作符可以共存，按照stack的方式排布，先布置后结束。此外还有其他格式化控制符：
* &lt;del&gt;&lt;/del&gt; 表示删除线
* &lt;ins&gt;&lt;/ins&gt; 表示下划线

### 4.计算机输出标签
* &lt;code&gt;&lt;/code&gt; 定义计算机源程序代码,
* &lt;kbd&gt;&lt;/kbd&gt; 定义从键盘输入的代码.
* &lt;samp&gt;&lt;/samp&gt; 定义计算机样本代码
* &lt;var&gt;&lt;/var&gt; 定义变量，显示为斜体
* &lt;tt&gt;&lt;/tt&gt; 定义打字机文本

&lt;code&gt;&lt;/code&gt;表示计算机源程序代码，文本等宽显示，但是如果只是想显示等宽的字体，使用&lt;tt&gt;&lt;/tt&gt;。&lt;code&gt;&lt;/code&gt;是一种特殊标记，只能在合适的地方使用，且经常与&lt;pre&gt;一起使用。
&lt;samp&gt;&lt;/samp&gt;也是一种特殊标记，用于上下文提取，使用不多。
&lt;var&gt;&lt;var&gt;经常与&lt;code&gt;&lt;pre&gt;一起使用，用于标记文档中的某些参数。

### 5.引文，引用及其他标签
##### 1.&lt;abbr&gt;
&lt;abbr&gt;用来表示一个缩写词或者首字母错略词，通过对缩写词语进行标记，您就能够为浏览器、拼写检查程序、翻译系统以及搜索引擎分度器提供有用的信息。某些浏览器中，当您把鼠标移至带有 &lt;abbr&gt; 标签的缩写词/首字母缩略词上时，&lt;abbr&gt; 标签的 title 属性可被用来展示缩写词/首字母缩略词的完整版本。

```html
<p>The <abbr title="World Health Organization">WHO</abbr> was founded in 1948.</p>
##The WHO was founded in 1948
```
在开始/结束符中间的为缩写。

##### 2.&lt;address&gt;
在有需要地址格式的地方使用，&lt;address&gt; 标签定义文档作者/所有者的联系信息。
如果 &lt;address&gt; 元素位于 &lt;body&gt; 元素内部，则它表示该文档作者/所有者的联系信息。
如果 &lt;address&gt; 元素位于 &lt;article&gt; 元素内部，则它表示该文章作者/所有者的联系信息。
&lt;address&gt; 元素的文本通常呈现为斜体。大多数浏览器会在该元素的前后添加换行。
但是&lt;addresss&gt;一般不用来表示邮政地址，除非是联系信息的组成部分。
```html
<address>
Written by <a href="mailto:webmaster@example.com">Jon Doe</a>.<br> 
Visit us at:<br>
Example.com<br>
Box 564, Disneyland<br>
USA
</address>
```
上面这个链接的用法，点击显示的"Jon Doe"就会打开链接指向的页面。

##### 3.文本链接
超链接可以是一个词，一幅图像或者是一个网页。
```html
<a href="url">链接文本</a>
```
通过**target**属性，可以自由定义被链接的文档再何处显示：
* target = "_blank" 在一个新的页面打开
* target = "_top"  在当前的页面打开，显示新页面的顶端
* target = "_self" 不会在新页面显示，在当前窗口？
* target = "_partent" 不知道

```html
<a href="http://www.runoob.com/" target="_blank">访问菜鸟教程!</a>; 
<!--会在新页面中打开 -->;
```
还可以通过**id**链接创建一个HTML文档书签标记，它是隐藏的。
```html
<a id="tips">这里是书签标记地点</a>  <!--创建一个链接到“有用的提示部分” -->
<a href="#tips">访问有用的提示部分</a>  <!--点击跳转回去 -->

<a href="http://www.runoob.com/html/html-links.html#tips">访问有用的提示部分</a>
<!--也可以从别的页面跳回来 -->
```
##### 4.图片链接
可以创建一些表现为图片的超链接，图片链接&lt;img&gt;有两个必须的属性，**src**和**alt**。
**src**属性代表了图象文件的URL, 也就是引用该文件的绝对路径或者相对路径。因此开发者通常会把图像文件存放在一个专门的文件夹里来简化路径。
而**alt**代表了图片的描述性文本，如果无法显示图象，将用该文本代替图象。**alt**的值使用双引号包括，运行标点和空格
```html
<p>创建图片链接:
<a href="http://www.runoob.com/html/html-tutorial.html">
<img  border="10" src="smiley.gif" alt="HTML 教程" width="32" height="32"></a></p>

<p>无边框的图片链接:
<a href="http://www.runoob.com/html/html-tutorial.html">
<img border="0" src="smiley.gif" alt="HTML 教程" width="32" height="32"></a></p>
```

### 6. HEAD元素
&lt;head&gt;元素包含了所有的头部标签元素。在&lt;head&gt;元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。可以添加在头部区域的元素标签为: &lt;title&gt;, &lt;style&gt;, &lt;meta&gt;, &lt;link&gt;, &lt;script&gt;, &lt;noscript&gt;, and &lt;base&gt;。

#### HEAD里的&lt;title&gt; 元素
&lt;title&gt;&lt;/title&gt;定义了不同文档的标题,包括:浏览器工具栏的标题、网页在收藏夹里显示的标题和显示在搜索引擎结构页面的标题
#### HEAD里的&lt;base&gt; 元素
&lt;base&gt;标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有链接标签的默认链接。
```html
<head>
<base href="http://www.runoob.com/images/" target="_blank">
</head>
```
#### HEAD里的&lt;link&gt; 元素（不懂）
&lt;link&gt; 标签定义了文档与外部资源之间的关系。&lt;link&gt; 标签通常用于链接到样式表:
```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```
#### HEAD里的&lt;style&gt; 元素（不懂）
&lt;style&gt; 标签定义了HTML文档的样式文件引用地址。在&lt;style&gt;元素中你也可以直接添加样式来渲染 HTML 文档:
```html
<head>
<style type="text/css">
body {background-color:yellow}
p {color:blue}
</style>
</head>
```
#### HEAD里的&lt;script&gt; 元素（不懂）
&lt;script&gt;标签用于加载脚本文件，如： JavaScript。以后会经常使用。

#### HEAD里的&lt;meta&gt; 元素（不懂）
meta标签描述了一些基本的元数据。&lt;meta&gt;标签提供了元数据,元数据也不显示在页面上，但会被浏览器解析。META 元素通常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他Web服务。&lt;meta&gt; 一般放置于 &lt;head&gt; 区域。
```html
<!--为搜索引擎定义关键词:-->
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">

<!--为网页定义描述内容:-->
<meta name="description" content="免费 Web & 编程 教程">

<!--定义网页作者:-->
<meta name="author" content="Runoob">

<!--每30秒钟刷新当前页面:-->
<meta http-equiv="refresh" content="30">
```

### HTML与CSS
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


