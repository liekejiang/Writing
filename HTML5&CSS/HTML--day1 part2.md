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
&lt;b&gt;这个文本加粗&lt;/b&gt;
&lt;strong&gt;这个文本也加粗&lt;/strong&gt;

&lt;big&gt;这个文本放大&lt;big&gt;
&lt;small&gt;这个文本缩小 &lt;/small&gt;

&lt;i&gt;这个文本斜体&lt;/i&gt;
&lt;em&gt;这个文本也斜体&lt;/em&gt;

&lt;p&gt;这个文本有&lt;sup&gt;上标&lt;/sup&gt;以及&lt;sub&gt;下标&lt;/sub&gt;&lt;/p&gt; 

&lt;pre&gt;
空 格
和空行会完整显现
&lt;/pre&gt;
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
&lt;p&gt;The &lt;abbr title="World Health Organization"&gt;WHO&lt;/abbr&gt; was founded in 1948.&lt;/p&gt;
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
&lt;address&gt;
Written by &lt;a href="mailto:webmaster@example.com"&gt;Jon Doe&lt;/a&gt;.&lt;br&gt; 
Visit us at:&lt;br&gt;
Example.com&lt;br&gt;
Box 564, Disneyland&lt;br&gt;
USA
&lt;/address&gt;
```
上面这个链接的用法，点击显示的"Jon Doe"就会打开链接指向的页面。

##### 3.链接
超链接可以是一个词，一幅图像或者是一个网页。
```html
&lt;a href="url"&gt;链接文本&lt;/a&gt;
```
通过**target**属性，可以自由定义被链接的文档再何处显示。
```html
&lt;a href="http://www.runoob.com/" target="_blank"&gt;访问菜鸟教程!&lt;/a&gt; 
&lt;!--会在新页面中打开 --&gt;
```
还可以通过**id**链接创建一个HTML文档书签标记，它是隐藏的。
```html
&lt;a id="tips"&gt;这里是书签标记地点&lt;/a&gt;  &lt;!--创建一个链接到“有用的提示部分” --&gt;
&lt;a href="#tips"&gt;访问有用的提示部分&lt;/a&gt;  &lt;!--点击跳转回去 --&gt;

&lt;a href="http://www.runoob.com/html/html-links.html#tips"&gt;访问有用的提示部分&lt;/a&gt;
&lt;!--也可以从别的页面跳回来 --&gt;
```
