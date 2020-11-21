# css 伪类（pseudo-classes）

**伪类**选择元素基于的是当前元素处于的状态，或者说元素当前所具有的特性，而不是元素的id、class、属性等静态的标志。由于状态是动态变化的，所以一个元素达到一个特定状态时，它可能得到一个伪类的样式；当状态改变时，它又会失去这个样式。由此可以看出，它的功能和class有些类似，但它是基于文档之外的抽象，所以叫伪类。

与伪类针对特殊状态的元素不同的是，**伪元素**是对元素中的特定内容进行操作，它所操作的层次比伪类更深了一层，也因此它的动态性比伪类要低得多。实际上，设计伪元素的目的就是去选取诸如元素内容第一个字（母）、第一行，选取某些内容前面或后面这种普通的选择器无法完成的工作。它控制的内容实际上和元素是相同的，但是它本身只是基于元素的抽象，并不存在于文档中，所以叫伪元素。



CSS伪类是用来添加一些选择器的特殊效果。

伪类的语法：

```css
selector:pseudo-class {property:value;}
```

CSS类也可以使用伪类：

```css
selector.class:pseudo-class {property:value;}
```





## *Ⅰ.锚类伪类（anchor）*

在支持 CSS 的浏览器中，链接的不同状态都可以以不同的方式显示

```css
a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 鼠标点击时的链接 */
```

**注意：** 在CSS定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

**注意：** 在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

**注意：**伪类的名称不区分大小写。

> 顺序： :link|:visited、:hover、:active





### :target

＃ 锚的名称是在一个文件中链接到某个元素的URL。元素被链接到目标元素。

:target选择器可用于当前活动的target元素的样式。

```html
<style>
:target
{
	border: 2px solid #D4D4D4;
	background-color: #e5eecc;
}
</style>
<body>

<h1>This is a heading</h1>

<p><a href="#news1">Jump to New content 1</a></p>
<p><a href="#news2">Jump to New content 2</a></p>

<p>Click on the links above and the :target selector highlight the current active HTML anchor.</p>

<p id="news1"><b>New content 1...</b></p>
<p id="news2"><b>New content 2...</b></p>

<p><b>注意:</b> IE 8 以及更早版本的浏览器不支持 :target 选择器.</p>

</body>
```

<a href="selector_html\target.html">点击查看具体效果</a>







## *Ⅱ.选择器伪类*



### :first-child

:first-child 选择器匹配其父元素中的第一个子元素。

匹配` <div>` 的父元素的第一个子元素是`<p>`

**提示:** p:first-child等同于p:nth-child(1)

**注意:** :first-child在IE8和更早版本IE版本中必须声明!doctype html

```html
<style>
p:first-child
{ 
    background-color:yellow;
}
</style>
<div>
	<p>
   	 	welcome
	</p>
    <p>
    	renlink
    </p>
</div>
```

父元素div选中第一个p子元素

<div style="border:1px dotted">
	<p style="background:yellow">
   	 	welcome
	</p>
    <p>
    	renlink
    </p>
</div>



如果父元素的第一个子元素不是p，样式就不会生效

```html
<style>
p:first-child
{ 
    background-color:yellow;
}
</style>
<div>
    <span>
    span
    </span>
    <!--虽然当前这个p是div的第一个p，但却不是div的第一个子元素，所以样式不生效,当前p对应到p:nth-child(2)-->
	<p>
   	 	welcome
	</p>
    <p>
    	renlink
    </p>
</div>
```

<div style="border:1px dotted">
    <span>span</span>
	<p>
   	 	welcome
	</p>
    <p>
    	renlink
    </p>
</div>





选择父元素`<body>`的第一个 `<p>`中的每个 `<i>` 元素并设置其样式，其中的` <p>` 元素是其父元素的第一个子元素：

```html
<style>
p:first-child i
{
	background:yellow;
}
</style>
</head>

<body>
<p>I am a <i>strong</i> man. I am a <i>strong</i> man.</p>
<p>I am a <i>strong</i> man. I am a <i>strong</i> man.</p>
</body>
```

<div style="border:1px dotted">
<p>I am a <i style="background:yellow">strong</i> man. I am a <i style="background:yellow">strong</i> man.</p>
<p>I am a <i>strong</i> man. I am a <i>strong</i> man.</p>
</div>



每一个`<ul>`元素的第一个子元素选择的样式：

```html
<style>
ul>:first-child
{
	background:yellow;
}
</style>
<ul>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Coca Cola</li>
</ul>

<ul>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Coca Cola</li>
</ul>
```

<ul>
  <li style="background:yellow">Coffee</li>
  <li>Tea</li>
  <li>Coca Cola</li>
</ul>
<ul>
  <li style="background:yellow">Coffee</li>
  <li>Tea</li>
  <li>Coca Cola</li>
</ul>





### :last-child

last-child选择器用来匹配父元素中最后一个子元素。

**提示:** p:last-child等同于p:nth-last-child(1)。

```html
<style> 
p:last-child
{
	background:#ff0000;
}
</style>

<div>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>
</div>
```

<div>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p style="background:red">The fourth paragraph.</p>
</div>





### :only-child

:only-child 选择器匹配属于父元素中唯一子元素的元素,有且只有一个子元素才生效

```html
<style>
li:only-child {
  color: red;
  list-style-type: disc;
}
</style> 
<ul>
      <li>This list has just one element.</li>
</ul>

<ul>
    <!--父元素有三个li子元素，样式不生效-->
    <li>This list has three elements.</li>
    <li>This list has three elements.</li>
    <li>This list has three elements.</li>
</ul>
```

<ul>
      <li style="color: red;
  list-style-type: disc;">This list has just one element.</li>
</ul>
<ul>
    <!--父元素有三个li子元素，样式不生效-->
    <li>This list has three elements.</li>
    <li>This list has three elements.</li>
    <li>This list has three elements.</li>
</ul>





### :nth-child (n)

nth-child(n) 选择器匹配父元素中的第 n 个子元素，元素类型没有限制。

*n* 可以是一个数字，一个关键字，或者一个公式。



指定每个 p 元素匹配的父元素中第 2 个子元素的背景色：

```html
<style> 
p:nth-child(2)
{
	background:#ff0000;
}
</style>

<body>
<p>这是第一个段落。</p>
<p>这是第二个段落。</p>
<p>这是第三个段落。</p>
<p>这是第四个段落。</p>
</body>
```

<div>
<p>这是第一个段落。</p>
<p style="background:red">这是第二个段落。</p>
<p>这是第三个段落。</p>
<p>这是第四个段落。</p>
</div>



#### 奇数odd 和 偶数even关键字

奇数和偶数是可以作为关键字使用用于相匹配的子元素，其索引是奇数或偶数（该索引的第一个子节点是1）。 在这里，为奇数和偶数p元素指定两个不同的背景颜色：

```html
<style> 
p:nth-child(odd)
{
	background:red;
}
p:nth-child(even)
{
	background:blue;
}
</style>


<div>
	<p>The first paragraph.</p>
	<p>The second paragraph.</p>
	<p>The third paragraph.</p>
</div>
```

<div>
	<p style="background:red">The first paragraph.</p>
	<p style="background:blue">The second paragraph.</p>
	<p style="background:red">The third paragraph.</p>
</div>





#### 循环公式（an+b）

使用公式（an+ b）.描述：a代表一个循环的大小，N是一个计数器（从0开始），以及b是偏移量。 在这里，我们对所有索引是3的倍数的p元素指定了背景颜色：

```html
p:nth-child(3n+0)
{
	background:#ff0000;
}
</style>

<div>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>
<p>The fifth paragraph.</p>
<p>The sixth paragraph.</p>
<p>The seventh paragraph.</p>
<p>The eight paragraph.</p>
<p>The ninth paragraph.</p>
</div>
```

<div>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p style="background:red">The third paragraph.</p>
<p>The fourth paragraph.</p>
<p>The fifth paragraph.</p>
<p style="background:red">The sixth paragraph.</p>
<p>The seventh paragraph.</p>
<p>The eight paragraph.</p>
<p style="background:red">The ninth paragraph.</p>
</div>







### :nth-last-child(n)

nth-last-child(n) 选择器匹配属于其元素的第 N 个子元素的每个元素，不论元素的类型，从最后一个子元素开始计数。

*n*可以是一个数字，一个关键字，或者一个公式。



指定每个 p 元素匹配同类型中的倒数第 2 个同级兄弟元素背景色：

```html
<style> 
p:nth-last-child(2)
{
	background:#ff0000;
}
</style>

<div>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>
</div>
```

<div>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p style="background:red">The third paragraph.</p>
<p>The fourth paragraph.</p>
</div>





### :empty

:empty选择器选择每个没有任何子级的元素（包括文本节点）。

```html
<style> 
p:empty
{
	height:20px;
	background:#ff0000;
}
</style>
<body>
<p></p>
<p>A paragraph.</p>
<p>Another paragraph.</p>
</body>
```

<p style="background:red;height:20px;"></p>
<p>A paragraph.</p>
<p>Another paragraph.</p>





### :first-of-type

:first-of-type 选择器匹配元素其父级是特定类型的第一个子元素。

```html
<style> 
p:first-of-type
{
	background:#ff0000;
}
</style>
</head>
<body>
<h1>这是一个标题</h1>
<p>这是第一个段落。</p>
<p>这是第二个段落。</p>
<p>这是第三个段落。</p>
<p>这是第四个段落。</p>
</body>
```

<h1>这是一个标题</h1>
<p style="background:red">这是第一个段落。</p>
<p>这是第二个段落。</p>
<p>这是第三个段落。</p>
<p>这是第四个段落。</p>



#### :first-of-type 和  :first-child 的区别

:first-child选择器是css2中定义的选择器，从字面意思上来看也很好理解，就是第一个子元素。比如有段代码：

```html
<div>
    <p>第一个子元素</p>
    <h1>第二个子元素</h1>
    <span>第三个子元素</span>
    <span>第四个子元素</span>
</div>
```

p:first-child匹配到的是p元素,因为p元素是div的第一个子元素；

h1:first-child匹配不到任何元素，因为在这里h1是div的第二个子元素，而不是第一个；

span:first-child匹配不到任何元素，因为在这里两个span元素都不是div的第一个子元素；

然后，在css3中又定义了:first-of-type这个选择器，这个跟:first-child有什么区别呢？还是看上段代码：

```html
<div>
    <p>第一个子元素</p>
    <h1>第二个子元素</h1>
    <span>第三个子元素</span>
    <span>第四个子元素</span>
</div>
```

p:first-of-type匹配到的是p元素,因为p是div的所有类型为p的子元素中的第一个；

h1:first-of-type匹配到的是h1元素，因为h1是div的所有类型为h1的子元素中的第一个；

span:first-of-type匹配到的是第三个子元素span。这里div有两个为span的子元素，匹配到的是它们中的第一个

所以，通过以上两个例子可以得出结论：

**:first-child** 匹配的是某父元素的第一个子元素，可以说是**结构上**的第一个子元素。

**:first-of-type** 匹配的是某父元素下**相同类型**子元素中的第一个，比如 p:first-of-type，就是指所有类型为p的子元素中的第一个。这里不再限制是第一个子元素了，只要是该类型元素的第一个就行了

同样类型的选择器 :last-child 和 :last-of-type、:nth-child(n) 和 :nth-of-type(n) 也可以这样去理解







### :last-of-type

:last-of-type选择器匹配元素其父级是特定类型的最后一个子元素。

**提示:** 和:nth-last-of-type(1)是一个意思。

指定其父级的最后一个p元素的背景色：

```html
<style> 
p:last-of-type
{
	background:#ff0000;
}
</style>

<body>
<h1>This is a heading</h1>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>
</body>
```

<h1>This is a heading</h1>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p style="background:red">The fourth paragraph.</p>







### :nth-of-type(n)

:nth-of-type(n)选择器匹配同类型中的第n个同级兄弟元素。

n可以是一个数字，一个关键字，或者一个公式。



指定每个p元素匹配同类型中的第2个同级兄弟元素的背景色：

```html
<style> 
p:nth-of-type(2)
{
	background:#ff0000;
}
</style>

<body>
<h1>This is a heading</h1>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>
</body>
```

<h1>This is a heading</h1>
<p>The first paragraph.</p>
<p  style="background:red">The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>



**奇数和偶数关键字**

奇数和偶数是可以作为关键字使用用于相匹配的子元素，其索引是奇数或偶数（该索引的第一个子节点是1）。

在这里，为奇数和偶数p元素指定两个不同的背景颜色：

```css
p:nth-of-type(odd)
{
background:#ff0000;
}
p:nth-of-type(even)
{
background:#0000ff;
}
```



**循环公式**

使用公式（an+ b）.描述：a代表一个循环的大小，N是一个计数器（从0开始），以及b是偏移量。

在这里，对所有索引是3的倍数的p元素指定了背景颜色：

```css
p:nth-of-type(3n+0)
{
background:#ff0000;
}
```





### :nth-last-of-type(n) 

:nth-last-of-type(*n*)选择器匹配同类型中的倒数第n个同级兄弟元素。

*n* 可以是一个数字，一个关键字，或者一个公式。



指定每个p元素匹配同类型中的倒数第2个同级兄弟元素背景色：

```html
<style> 
p:nth-last-of-type(2)
{
	background:#ff0000;
}
</style>

<body>
<h1>This is a heading</h1>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>
</body>
```

<h1>This is a heading</h1>
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p style="background:red">The third paragraph.</p>
<p>The fourth paragraph.</p>







### :only-of-type

:only-of-type 代表了任意一个元素，这个元素没有其他相同类型的兄弟元素。

指定属于父元素的特定类型的唯一子元素的每个 p 元素：

```html
<style> 
p:only-of-type
{
	background:#ff0000;
}
</style>
<body>

<div><p>This is a paragraph.</p></div>
    
<div><p>This is a paragraph.</p><p>This is a paragraph.</p></div>

</body>
```

<div><p style="background:red">This is a paragraph.</p></div>
<div><p>This is a paragraph.</p><p>This is a paragraph.</p></div>





### :not()

:not(selector) 选择器匹配所有selector以外的元素



为每个并非`<p>`元素的元素设置背景颜色：

```html
<style>
:not(p) {
    color: #ff0000;
}
</style>

<h1>这是一个标题</h1>
<p>这是一个段落.</p>
<p>这是另一个段落.</p>
<div>这是div元素的一些文本。</div>

```

<h1 style="color:red">这是一个标题</h1>
<p>这是一个段落.</p>
<p>这是另一个段落.</p>
<div style="color:red">这是div元素的一些文本。</div>







### :root

**`:root`** 这个 CSS [伪类](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)匹配文档树的根元素。对于 HTML 来说，**`:root`** 表示 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/html) 元素，除了[优先级](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)更高之外，与 `html` 选择器相同。

在声明全局 [CSS 变量](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)时 **`:root`** 会很有用：

```css
:root {
  --main-color: hotpink;
  --pane-padding: 5px 42px;
}
```











## *Ⅲ.内容伪类*



### :after

:after 选择器向选定的元素之后插入内容。

使用[content](https://www.runoob.com/cssref/pr-gen-content.html) 属性来指定要插入的内容。



每个`<p>`元素之后插入内容：

```html
<style>
p:after
{ 
content:"- 注意我";
}
</style>

<body>
    
<p>我的名字是 Donald</p>
<p>我住在 Ducksburg</p>

</body>
```

<p>我的名字是 Donald-注意我</p>
<p>我住在 Ducksburg-注意我</p>





### :before

:before 选择器向选定的元素前插入内容。

使用[content](https://www.runoob.com/cssref/pr-gen-content.html) 属性来指定要插入的内容。



每个 `<p>`元素之前插入内容：

```html
<style>
p:before
{
	content:"Read this -";
}
</style>

<body>
<p>My name is Donald</p>
<p>I live in Ducksburg</p>

</body>
```

<p>Read this -My name is Donald</p>
<p>Read this -I live in Ducksburg</p>





### :first-line

:first-line选择器用来指定选择器第一行的样式。

first-line选择器用来指定选择器第一行的样式。

**注意:**:first-line选择器可以使用以下属性： 

- font properties
- color properties 
- background properties
- word-spacing
- letter-spacing
- text-decoration
- vertical-align
- text-transform
- line-height
- clear

**注意:** "first-line" 选择器适用于**块级元素**中。



```html
<style>
p:first-line
{
	background-color:yellow;
}
</style>

<body>
<h1>WWF's Mission Statement</h1>
<p>To stop the degradation of the planet's natural environment<br>
    and to build a future in which humans live in harmony with nature, 
    by; conserving the world's biological diversity, ensuring that the use of renewable natural resources is 		sustainable, and promoting the reduction of pollution and wasteful consumption.
</p>
</body>
```

<h1>WWF's Mission Statement</h1>
<p><mark>To stop the degradation of the planet's natural environment</mark><br>
    and to build a future in which humans live in harmony with nature, 
    by; conserving the world's biological diversity, ensuring that the use of renewable natural resources is 		sustainable, and promoting the reduction of pollution and wasteful consumption.
</p>







### :first-letter

:first-letter选择器用来指定元素第一个字母的样式。

**提示:** :first-letter选择器可以使用以下属性： 

- font properties
- color properties 
- background properties
- margin properties
- padding properties
- border properties
- text-decoration
- vertical-align (only if float is 'none')
- text-transform
- line-height
- float
- clear

**注意:** "first-letter" 选择器仅适用于在**块级元素**中.

```html
<style>
p:first-letter
{
	font-size:200%;
	color:#8A2BE2;
}
</style>

<body>

<h1>Welcome to My Homepage</h1>
<p>My name is Donald.</p>
<p>I live in Duckburg.</p>
<p>My best friend is Mickey.</p>

</body>
```

<h1>Welcome to My Homepage</h1>
<p><span style="color:#8A2BE2;">M</span>y name is Donald.</p>
<p><span style="color:#8A2BE2;">I</span> live in Duckburg.</p>
<p><i style="color:#8A2BE2;">M</i>y best friend is Mickey.</p>







### ::selection

::selection选择器匹配元素中被用户选中或处于高亮状态的部分。

::selection只可以应用于少数的CSS属性：color, background, cursor,和outline。

```html
<style type="text/css">
::selection
{
color:#ff0000;
}
::-moz-selection
{
color:#ff0000;
}
</style>
<body>

<h1>尝试选择本页的一些文本</h1>
<p>这是一些文本.</p>
<div>这是div元素中的一些文本.</div>
<a href="//www.w3cschool.cc/" target="_blank">链接W3Cschool!</a>

</body>
```

<iframe src="selector_html\selection.html" style="background:white;height:200px;"></iframe>









## *Ⅳ.表单伪类*



### :checked

:checked 选择器匹配每个选中的输入元素（仅适用于单选按钮或复选框）。

为所有选中的输入元素设置背景颜色：

```html
<style> 
input:checked {
    height: 50px;
    width: 50px;
}
</style>
<body>

<form action="">
<input type="radio" checked="checked" value="male" name="gender" /> Male<br>
<input type="radio" value="female" name="gender" /> Female<br>
<input type="checkbox" checked="checked" value="Bike" /> I have a bike<br>
<input type="checkbox" value="Car" /> I have a car 
</form>

</body>
```

<form action="">
<input type="radio" checked="checked" value="male" name="gender" style="width:50px;height:50px;"/> Male<br>
<input type="radio" value="female" name="gender" /> Female<br>
<input type="checkbox" checked="checked" value="Bike" style="width:50px;height:50px;"/> I have a bike<br>
<input type="checkbox" value="Car" /> I have a car 
</form>







### :disabled

:disabled 选择器匹配每个禁用的的元素（主要用于表单元素）。

设置所有type="text"的禁用的输入元素的背景颜色：

```html
<style> 
input[type="text"]:disabled
{
	background:#dddddd;
}
</style>
<body>

<form action="">
First name: <input type="text" value="Mickey" /><br>
Last name: <input type="text" value="Mouse" /><br>
Country: <input type="text" disabled="disabled" value="Disneyland" /><br>
</form>
```

<form action="">
First name: <input type="text" value="Mickey" /><br>
Last name: <input type="text" value="Mouse" /><br>
Country: <input type="text" disabled="disabled" value="Disneyland" style="background:#ddd;" /><br>
</form>





### :enabled

:enabled 选择器匹配每个启用的的元素（主要用于表单元素）。

设置所有type="text"的启用的输入元素的背景颜色：

```html
<style> 
input[type="text"]:enabled
{
	background:#ffff00;
}
</style>
<body>

<form action="">
First name: <input type="text" value="Mickey" /><br>
Last name: <input type="text" value="Mouse" /><br>
Country: <input type="text" disabled="disabled" value="Disneyland" /><br>
</form>

</body>
```

<form action="">
First name: <input type="text" value="Mickey" style="background:#ffff00;" /><br>
Last name: <input type="text" value="Mouse" style="background:#ffff00;" /><br>
Country: <input type="text" disabled="disabled" value="Disneyland" /><br>
</form>







### :in-range

:in-range 选择器用于标签的值在指定区间值时显示的样式。

**注意：** :in-range 选择器只作用于能指定区间值的元素，例如 input 元素中的 min 和 max 属性

输入的值在指定区间内时，设置指定样式:

```html
<style>
input:in-range
{
	border:2px solid yellow;
}
</style>

<body>

<h3>:in-range 选择器实例演示。</h3>
<input type="number" min="5" max="10" value="7" />
<p>在input中输入一个值 (小于 5 或者 大于 10), 查看样式的变化。</p>

</body>
```

<iframe src="selector_html\in-range.html" style="overflow:visible;height:auto;background:white"><iframe>





### :invalid

:invalid 选择器用于在表单元素中的值是非法时设置指定样式。

**注意：** :invalid 选择器只作用于能指定区间值的元素，例如 input 元素中的 min 和 max 属性，及 email 字段, 合法的数字字段等。

如果 input 元素中的值是非法\无法通过验证的，设置样式为红色：

```html
<style>
input:invalid
{
	border:2px solid red;
}
</style>

<body>

<h3> :invalid 选择器实例演示。</h3>

<input type="email" value="supportEmail" />

<p>请输入合法 e-mail 地址，查看样式变化。</p>

</body>
```

<iframe src="selector_html\invalid.html" style="overflow:visible;height:auto;background:white"><iframe>





### :required

:required 选择器在表单元素是必填项时设置指定样式。

表单元素可以使用 required 属性来设置必填项。

如果 input 元素设置了 "required" 属性，设置其为黄色:

````html
<style>
input:required
{
	background-color: yellow;
}
</style>
<body>

<h3>A demonstration of the :required selector.</h3>
<p>An optional input element:<br><input></p>
<p>A required input element:<br><input required></p>
<p> :required选择器选择表单元素有“required”属性.</p>

</body>
````

<div style="background:white;color:black">
<h3 style="color:black">A demonstration of the :required selector.</h3>
<p>An optional input element:<br><input style="border:2px dotted silver"></p>
<p>A required input element:<br><input required  style="background:yellow"></p>
<p> :required选择器选择表单元素有“required”属性.</p>
</div>





### :optional

:optional 选择器在表单元素是可选项时设置指定样式。

表单元素中如果没有特别设置 required 属性即为 optional 属性。

**注意:** :optional 选择器只适用于表单元素: input, select 和 textarea。

如果 input 元素没有设置 "required" 属性，设置其为黄色:

```html
<style>
input:optional
{
background-color: yellow;
}
</style>

<body>

<h3>:optional 选择器演示实例。</h3>
<p>可选的 input 元素:<br><input></p>
<p>必填的 input 元素:<br><input required></p>
<p> :optional 选择器用于表单中未设置"required" 属性的元素。</p>

</body>
```

<div style="background:white;color:black">
<h3 style="color:black">A demonstration of the :required selector.</h3>
<p>An optional input element:<br><input style="background:yellow"></p>
<p>A required input element:<br><input required  style="border:2px dotted silver"></p>
<p> :required选择器选择表单元素有“required”属性.</p>
</div>





### :out-of-range

:out-of-range 选择器用于标签的值在指定区间之外时显示的样式。

**注意：** :out-of-range 选择器只作用于能指定区间之外值的元素，例如 input 元素中的 min 和 max 属性。

输入的值在指定区间外时，设置指定样式:

```html
<style>
input:out-of-range
{
	border:2px solid red;
}
</style>
<body>

<h3> :out-of-range 选择器实例演示。</h3>
<input type="number" min="5" max="10" value="17" />
<p>在input中输入一个值 (小于 5 或者 大于 10), 查看样式的变化。</p>

</body>
```

<iframe src="selector_html\out-of-range.html" style="overflow:visible;height:auto;background:white"><iframe>





### :focus

:focus选择器用于选择具有焦点的元素。

**提示:** :focus选择器接受键盘事件或其他用户输入的元素。

一个输入字段获得焦点时选择的样式：

```html
<style>
input:focus
{
	background-color:yellow;
}
</style>
<body>

<p>点击文本输入框表单可以看到黄色背景:</p>

<form>
First name: <input type="text" name="firstname" /><br>
Last name: <input type="text" name="lastname" />
</form>

<p><b>注意:</b>  :focus 作用于 IE8,DOCTYPE 必须已声明</p>

</body>
```

<iframe src="selector_html\focus.html" style="overflow:visible;height:auto;background:white"><iframe>





### :read-only

:read-only 选择器用于选取设置了 "readonly" 属性的元素。

表单元素可以设置 "readonly" 属性来定义元素只读。

**注意：** 目前，大多数浏览器, :read-only 选择器适用于 input 和 textarea 元素，但是它也适用于设置了 "readonly" 属性的元素。

如果 input 元素设置了 "readonly" 属性，设置输入框样式为黄色:

```html
<style>
input:read-only
{
	background-color: yellow;
}
</style>
<body>

<h3> :read-only 选择器实例演示。</h3>
<p>普通的input元素:<br><input value="hello"></p>
<p>只读的input元素:<br><input readonly value="hello"></p>
<p> :read-write 选择器选取没有设置 "readonly" 属性的元素。</p>
<p> :readonly 择器选取设置 "readonly" 属性的元素。</p>

</body>
```

<h3> :read-only 选择器实例演示。</h3>
<p>普通的input元素:<br><input value="hello" style="border:2px dotted silver"></p>
<p>只读的input元素:<br><input readonly value="hello" style="background:yellow"></p>
<p> :read-write 选择器选取没有设置 "readonly" 属性的元素。</p>
<p> :readonly 择器选取设置 "readonly" 属性的元素。</p>





### :read-write

:read-write 选择器用于匹配可读及可写的元素。

**注意:** 目前, 在大多浏览器中, :read-write 选择器只使用于设置了input 和 textarea 元素。

如果 input 元素不是只读，即没有 "readonly" 属性，设置输入框样式为黄色:

```html
<style>
input:read-write
{
	background-color: yellow;
}
</style>
<body>

<h3> :read-only 选择器实例演示。</h3>
<p>普通的input元素:<br><input value="hello"></p>
<p>只读的input元素:<br><input readonly value="hello"></p>
<p> :read-write 选择器选取没有设置 "readonly" 属性的元素。</p>
<p> :readonly 择器选取设置 "readonly" 属性的元素。</p>

</body>
```

<h3> :read-only 选择器实例演示。</h3>
<p>普通的input元素:<br><input value="hello" style="background:yellow"></p>
<p>只读的input元素:<br><input readonly value="hello" style="border:2px dotted silver" ></p>
<p> :read-write 选择器选取没有设置 "readonly" 属性的元素。</p>
<p> :readonly 择器选取设置 "readonly" 属性的元素。</p>





## *Ⅴ.语言伪类*

### :lang

:lang 向带有指定 lang 属性开始的元素添加样式。

**注意:** 值是整个单词，单独像lang="en"，或者使用连字符(-)如lang ="en-us"。

每个`<p>`元素lang属性值等于"it(Italian)" 的选择的样式：

```html
<style>
p:lang(it)
{ 
	background:yellow;
}
</style>
<body>

<p>I live in Italy.</p>
<p lang="it">Ciao bella!</p>
<p><b>注意:</b> :lang 作用于 IE8, DOCTYPE 必须已经声明.</p>

</body>
```

<p>I live in Italy.</p>
<p lang="it" style="background:yellow">Ciao bella!</p>
<p><b>注意:</b> :lang 作用于 IE8, DOCTYPE 必须已经声明.</p>









# css 选择器（selector）

CSS选择器用于选择你想要的元素的样式的模式。



## *⚪单项选择器*

### .class(类选择器)

*.class*选择器是指定类的所有元素的样式。

```html
<style>
    .renlink{
        color:red;
    }
</style>
<p class="renlink">renlink</p>
<p class="renlink">renlink</p>
```





### #id(id\唯一选择器)

\#*id*选择器指定具有ID的元素的样式。

```html
<style>
    #renlink{
        color:red;
    }
</style>
<p id="renlink">renlink</p>
```





### *(通配符选择器)

\* 选择器选择所有元素。

\* 选择器也可以选择另一个元素内的所有元素:

示例：

选择所有元素，并设置其背景色：

```html
<style>
*
{
background-color:yellow;
}
</style>
<h1>h1</h1>
<p>p</p>
<div>div</div>
```

选择`<div>`元素内的所有元素：

```html
<style>
div *
{
background-color:yellow;
}
</style>
<div>
	<h1>h1</h1>
	<p>p</p>
	<div>div</div>
</div>
```





### element(标签选择器)

*element* 选择器选择指定元素名称的所有元素。

选择所有`<p>` 元素 :

```css
p
{ 
    background-color:yellow;
}
```





### [*attribute*] 属性选择器

所有主流浏览器都支持 [*attribute*] 选择器。

选择所有带有target属性的` <a>`元素：

```css
a[target]
{
background-color:yellow;
}
```



 ### [*attribute*=*value*]

[*attribute*=*value*] 选择器用于选择指定了属性和值的元素。

选择所有使用target="_blank"的a元素

```css
a[target=_blank]
{
background-color:yellow;
}
```



### [*attribute*~=*value*]

[*attribute*~=*value*] 选择器用于选择属性值包含**一个指定单词**的元素。

选择标题属性**包含<span style="color:orange">独立单词</span>**"flower"的所有元素

```css
[title~=flower]
{
background-color:yellow;
}
```

```html
<img src="klematis.jpg" title="klematis flower" width="150" height="113" /><!--包含flower这个独立单词 样式生效-->
<img src="img_flwr.gif" title="flowers" width="224" height="162" /><!--不包含flower这个独立单词 样式不生效-->
<img src="landscape.jpg" title="landscape" width="160" height="120" /><!--不包含flower这个独立单词 样式不生效-->
```







### [*attribute*|=*value*]

[*attribute*|=*value*] 选择器用于选择以指定值开头的属性值的元素。

**注意:**该值是**整个单词**，无论是单独像 lang="en"，或者像连字符 **-** 如 **lang=en-us** 都可以

选择 lang 属性值以 "en" 开头的元素，并设置其样式：

```css
[lang|=en]
{ 
    background-color:yellow;
}
```





### [*attribute*^=*value*]

 [*attribute*^=*value*] 选择器匹配元素属性值带指定的值开始的元素。

```css
div[class^="test"]
{
background:#ffff00;
}
```





### [*attribute*^=*value*] 与 [*attribute*|=*value*] 的区别

**type不同，name不同且name值没有'-(横杠)'，没有class（通过[attribute ^ = value]获取name，input[name^=bob]**

```html
 <input type="month" name="bob1" placeholder="bob"  />
 <input type="number" name="bob2" placeholder="bob"  />
 <input type="text" name="bob3" placeholder="bob"  />
```

**type不同，name不同，没有class（通过[attribute|=value] 获取name，input[name|=kevin]**

```html
 <input type="month" name="kevin-1" placeholder="kevin" />
 <input type="number" name="kevin-2" placeholder="kevin" />
 <input type="text" name="kevin-3" placeholder="kevin" />
```

换言之：

[attribute|=value] 对 bob1、bob2、bob3 这些属性值不起作用，因为没有连接符”-“ bob单词后面纯粹是个数字**不构成完整的单词**





### [*attribute*$=*value*]

[*attribute*$=*value*] 选择器匹配元素属性值带指定的值结尾的元素。

设置class属性值以"test"结尾的所有div元素的背景颜色：

```css
div[class$="test"]
{
background:#ffff00;
}
```







### [*attribute*\*=*value*]

[*attribute*\*=*value*] 选择器匹配元素属性值包含指定值的元素。

设置class属性值包含"test"的所有div元素的背景颜色：

```css
div[class*="test"]
{
    background:#ffff00;
}
```

```html
<!--包含flower 以下两个img的样式都生效-->
<img src="klematis.jpg" title="klematis flower" width="150" height="113" />
<img src="img_flwr.gif" title="flowers" width="224" height="162" />
```













## *⚪组合选择器*



### element,element 多项共用

几个元素具有相同的样式，用逗号分隔每个元素的名称。

选择所有<p>元素和lt;h1>元素:

```css
h1,p
{
background-color:yellow;
}
```



### element element  后代选择器

后代选择器用于选取某元素的所有后代元素。

以下实例选择`<div>`元素内的所有`<span>`元素:

```html
<style>
div p
{
	background-color:yellow;
}
</style>

<body>

<div>
<span>文本 1。 在 div 中。<span>内文本 在div的span中</span></span>
<span>文本 2。 在 div 中。</span>
</div><span>文本 3。不在 div 中。</span><span>文本 4。不在 div 中。</span>

</body>
```

<div>
<span style="background:yellow">文本 1。 在 div 中。<span style="background:yellow">内文本 在div的span中</span></span>
<span style="background:yellow">文本 2。 在 div 中。</span>
</div><span>文本 3。不在 div 中。</span><span>文本 4。不在 div 中。</span>







### element>element 子元素选择器

与后代选择器相比，子元素选择器（Child selectors）只能选择作为某元素子元素的元素。

以下实例选择了`<div>`元素中所有**直接子元素** `<p>` ：

```html
<style>
div>p
{
	background-color:yellow;
}
</style>

<body>
<h1>Welcome to My Homepage</h1>
<div>
<h2>My name is Donald</h2>
<p>I live in Duckburg.</p>
</div>
    
<div>
<span><p>I will not be styled.</p><!--这个p不是div的直接子元素--></span>
</div>

</body>
```

<div>
<h2>My name is Donald</h2>
<p style="background:yellow">I live in Duckburg.</p>
</div>
<div>
<span><p>I will not be styled.</p></span>
</div>









### element+element 相邻兄弟选择器

相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有**相同父元素**。

如果需要选择**紧接**在另一个元素后的元素，而且二者有相同的父元素，可以使用相邻兄弟选择器（Adjacent sibling selector）。

以下实例选取了所有位于` <div>` 元素后的第一个 `<p> `元素:

```html
<style>
div+p
{
	background-color:yellow;
}
</style>
<body>

<h1>文章标题</h1>

<div>
<h2>DIV 内部标题</h2>
<p>DIV 内部段落。</p>
</div>
    
<p>DIV 之后的第一个 P 元素。</p>
    
<p>DIV 之后的第二个 P 元素。</p>

</body>
```

<h1>文章标题</h1>
<div>
<h2>DIV 内部标题</h2>
<p>DIV 内部段落。</p>
</div>
<p style="background:yellow">DIV 之后的第一个 P 元素。</p>
<p>DIV 之后的第二个 P 元素。</p>







### element~element  后续兄弟选择器

后续兄弟选择器选取**所有**指定元素之后的**相邻**兄弟元素。

 以下实例选取了所有`<div>` 元素之后的所有相邻兄弟元素 `<p>` : 

```html
<style>
div~p
{
	background-color:yellow;
}
</style>
<body>
	
<p>之前段落，不会添加背景颜色。</p>
<div>
<p>段落 1。 在 div 中。</p>
<p>段落 2。 在 div 中。</p>
</div>
<p>段落 3。不在 div 中。</p>
<p>段落 4。不在 div 中。</p>
<div><p>阻断相邻</p></div>
<p>段落 5。不在 div 中。</p>

</body>
```

<p>之前段落，不会添加背景颜色。</p>
<div>
<p>段落 1。 在 div 中。</p>
<p>段落 2。 在 div 中。</p>
</div>
<p style="background:yellow">段落 3。不在 div 中。</p>
<p style="background:yellow">段落 4。不在 div 中。</p>
<div><p>阻断相邻</p></div>
<p style="background:yellow">段落 5。不在 div 中。</p>







# css 选择器与伪类参考表格

CSS选择器用于选择你想要的元素的样式的模式。

"CSS"列表示在CSS版本的属性定义（CSS1，CSS2，或对CSS3）。

| 选择器                                                       | 示例                  | 示例说明                                                  | CSS  |
| :----------------------------------------------------------- | :-------------------- | :-------------------------------------------------------- | :--- |
| [.*class*](https://www.runoob.com/cssref/sel-class.html)     | .intro                | 选择所有class="intro"的元素                               | 1    |
| [#*id*](https://www.runoob.com/cssref/sel-id.html)           | #firstname            | 选择所有id="firstname"的元素                              | 1    |
| [*](https://www.runoob.com/cssref/sel-all.html)              | *                     | 选择所有元素                                              | 2    |
| *[element](https://www.runoob.com/cssref/sel-element.html)*  | p                     | 选择所有<p>元素                                           | 1    |
| *[element,element](https://www.runoob.com/cssref/sel-element-comma.html)* | div,p                 | 选择所有<div>元素和<p>元素                                | 1    |
| [*element* *element*](https://www.runoob.com/cssref/sel-element-element.html) | div p                 | 选择<div>元素内的所有<p>元素                              | 1    |
| [*element*>*element*](https://www.runoob.com/cssref/sel-element-gt.html) | div>p                 | 选择所有父级是 <div> 元素的 <p> 元素                      | 2    |
| [*element*+*element*](https://www.runoob.com/cssref/sel-element-pluss.html) | div+p                 | 选择所有紧接着<div>元素之后的<p>元素                      | 2    |
| [[*attribute*\]](https://www.runoob.com/cssref/sel-attribute.html) | [target]              | 选择所有带有target属性元素                                | 2    |
| [[*attribute*=*value*\]](https://www.runoob.com/cssref/sel-attribute-value.html) | [target=-blank]       | 选择所有使用target="-blank"的元素                         | 2    |
| [[*attribute*~=*value*\]](https://www.runoob.com/cssref/sel-attribute-value-contains.html) | [title~=flower]       | 选择标题属性包含单词"flower"的所有元素                    | 2    |
| [[*attribute*\|=*language*\]](https://www.runoob.com/cssref/sel-attribute-value-lang.html) | [lang\|=en]           | 选择 lang 属性以 en 为开头的所有元素                      | 2    |
| [:link](https://www.runoob.com/cssref/sel-link.html)         | a:link                | 选择所有未访问链接                                        | 1    |
| [:visited](https://www.runoob.com/cssref/sel-visited.html)   | a:visited             | 选择所有访问过的链接                                      | 1    |
| [:active](https://www.runoob.com/cssref/sel-active.html)     | a:active              | 选择活动链接                                              | 1    |
| [:hover](https://www.runoob.com/cssref/sel-hover.html)       | a:hover               | 选择鼠标在链接上面时                                      | 1    |
| [:focus](https://www.runoob.com/cssref/sel-focus.html)       | input:focus           | 选择具有焦点的输入元素                                    | 2    |
| [:first-letter](https://www.runoob.com/cssref/sel-firstletter.html) | p:first-letter        | 选择每一个<p>元素的第一个字母                             | 1    |
| [:first-line](https://www.runoob.com/cssref/sel-firstline.html) | p:first-line          | 选择每一个<p>元素的第一行                                 | 1    |
| [:first-child](https://www.runoob.com/cssref/sel-firstchild.html) | p:first-child         | 指定只有当<p>元素是其父级的第一个子级的样式。             | 2    |
| [:before](https://www.runoob.com/cssref/sel-before.html)     | p:before              | 在每个<p>元素之前插入内容                                 | 2    |
| [:after](https://www.runoob.com/cssref/sel-after.html)       | p:after               | 在每个<p>元素之后插入内容                                 | 2    |
| [:lang(*language*)](https://www.runoob.com/cssref/sel-lang.html) | p:lang(it)            | 选择一个lang属性的起始值="it"的所有<p>元素                | 2    |
| [*element1*~*element2*](https://www.runoob.com/cssref/sel-gen-sibling.html) | p~ul                  | 选择p元素之后的每一个ul元素                               | 3    |
| [[*attribute*^=*value*\]](https://www.runoob.com/cssref/sel-attr-begin.html) | a[src^="https"]       | 选择每一个src属性的值以"https"开头的元素                  | 3    |
| [[*attribute*$=*value*\]](https://www.runoob.com/cssref/sel-attr-end.html) | a[src$=".pdf"]        | 选择每一个src属性的值以".pdf"结尾的元素                   | 3    |
| [[*attribute**=*value*\]](https://www.runoob.com/cssref/sel-attr-contain.html) | a[src*="runoob"]      | 选择每一个src属性的值包含子字符串"runoob"的元素           | 3    |
| [:first-of-type](https://www.runoob.com/cssref/sel-first-of-type.html) | p:first-of-type       | 选择每个p元素是其父级的第一个p元素                        | 3    |
| [:last-of-type](https://www.runoob.com/cssref/sel-last-of-type.html) | p:last-of-type        | 选择每个p元素是其父级的最后一个p元素                      | 3    |
| [:only-of-type](https://www.runoob.com/cssref/sel-only-of-type.html) | p:only-of-type        | 选择每个p元素是其父级的唯一p元素                          | 3    |
| [:only-child](https://www.runoob.com/cssref/sel-only-child.html) | p:only-child          | 选择每个p元素是其父级的唯一子元素                         | 3    |
| [:nth-child(*n*)](https://www.runoob.com/cssref/sel-nth-child.html) | p:nth-child(2)        | 选择每个p元素是其父级的第二个子元素                       | 3    |
| [:nth-last-child(*n*)](https://www.runoob.com/cssref/sel-nth-last-child.html) | p:nth-last-child(2)   | 选择每个p元素的是其父级的第二个子元素，从最后一个子项计数 | 3    |
| [:nth-of-type(*n*)](https://www.runoob.com/cssref/sel-nth-of-type.html) | p:nth-of-type(2)      | 选择每个p元素是其父级的第二个p元素                        | 3    |
| [:nth-last-of-type(*n*)](https://www.runoob.com/cssref/sel-nth-last-of-type.html) | p:nth-last-of-type(2) | 选择每个p元素的是其父级的第二个p元素，从最后一个子项计数  | 3    |
| [:last-child](https://www.runoob.com/cssref/sel-last-child.html) | p:last-child          | 选择每个p元素是其父级的最后一个子级。                     | 3    |
| [:root](https://www.runoob.com/cssref/sel-root.html)         | :root                 | 选择文档的根元素                                          | 3    |
| [:empty](https://www.runoob.com/cssref/sel-empty.html)       | p:empty               | 选择每个没有任何子级的p元素（包括文本节点）               | 3    |
| [:target](https://www.runoob.com/cssref/sel-target.html)     | #news:target          | 选择当前活动的#news元素（包含该锚名称的点击的URL）        | 3    |
| [:enabled](https://www.runoob.com/cssref/sel-enabled.html)   | input:enabled         | 选择每一个已启用的输入元素                                | 3    |
| [:disabled](https://www.runoob.com/cssref/sel-disabled.html) | input:disabled        | 选择每一个禁用的输入元素                                  | 3    |
| [:checked](https://www.runoob.com/cssref/sel-checked.html)   | input:checked         | 选择每个选中的输入元素                                    | 3    |
| [:not(*selector*)](https://www.runoob.com/cssref/sel-not.html) | :not(p)               | 选择每个并非p元素的元素                                   | 3    |
| [::selection](https://www.runoob.com/cssref/sel-selection.html) | ::selection           | 匹配元素中被用户选中或处于高亮状态的部分                  | 3    |
| [:out-of-range](https://www.runoob.com/cssref/sel-out-of-range.html) | :out-of-range         | 匹配值在指定区间之外的input元素                           | 3    |
| [:in-range](https://www.runoob.com/cssref/sel-in-range.html) | :in-range             | 匹配值在指定区间之内的input元素                           | 3    |
| [:read-write](https://www.runoob.com/cssref/sel-read-write.html) | :read-write           | 用于匹配可读及可写的元素                                  | 3    |
| [:read-only](https://www.runoob.com/cssref/sel-read-only.html) | :read-only            | 用于匹配设置 "readonly"（只读） 属性的元素                | 3    |
| [:optional](https://www.runoob.com/cssref/sel-optional.html) | :optional             | 用于匹配可选的输入元素                                    | 3    |
| [:required](https://www.runoob.com/cssref/sel-required.html) | :required             | 用于匹配设置了 "required" 属性的元素                      | 3    |
| [:valid](https://www.runoob.com/cssref/sel-valid.html)       | :valid                | 用于匹配输入值为合法的元素                                | 3    |
| [:invalid](https://www.runoob.com/cssref/sel-invalid.html)   | :invalid              | 用于匹配输入值为非法的元素                                | 3    |