# 💥css 变量（*CSS* Variables）

在任何语言中，变量的有一点作用都是一样的，那就是可以降低维护成本，附带还有更高性能，文件更高压缩率的好处。

随着CSS预编译工具Sass/Less/[Stylus](http://www.zhangxinxu.com/wordpress/2012/06/stylus-nodejs-expressive-dynamic-robust-css/)的关注和逐渐流行，CSS工作组迅速跟进[CSS变量的规范](https://drafts.csswg.org/css-variables/)制定，并且，很多浏览器已经跟进，目前这个重要的 CSS 新功能，所有主要浏览器已经都支持了。本节全面介绍如何使用它，你会发现原生 CSS 从此变得异常强大。

![](css3_img\css变量兼容性.png)

Chrome/Firefox/Safari浏览器都是绿油油的，兼容性大大超出预期，于是果断尝鲜记录下语法用法和特性。





## *🔳css变量的定义*

声明变量的时候，变量名前面要加两根连词线（**`--`**）。

```css
body {
  --foo: #7F583F;
  --bar: #F7EFD2;
}
```

上面代码中，`body`选择器里面声明了两个变量：`--foo`和`--bar`。

它们与`color`、`font-size`等正式属性没有什么不同，只是没有默认含义。所以 CSS 变量（CSS variable）又叫做**"CSS 自定义属性"**（CSS custom properties）。因为变量与自定义的 CSS 属性其实是一回事。

你可能会问，为什么选择两根连词线（`--`）表示变量？因为`$foo`被 Sass 用掉了，`@foo`被 Less 用掉了。为了不产生冲突，官方的 CSS 变量就改用两根连词线了。

各种值都可以放入 CSS 变量。

```css
:root{
  --main-color: #4d4e53;
  --main-bg: rgb(255, 255, 255);
  --logo-border-color: rebeccapurple;

  --header-height: 68px;
  --content-padding: 10px 20px;

  --base-line-height: 1.428571429;
  --transition-duration: .35s;
  --external-link: "external link";
  --margin-top: calc(2vh + 20px);
}
```

变量名**大小写敏感**，`--header-color`和`--Header-Color`是两个不同变量。

**不能包含`$`，`[`，`^`，`(`，`%`等字符**，普通字符局限在只要是“数字`[0-9]`”“字母`[a-zA-Z]`”“下划线`_`”和“短横线`-`”这些组合，但是可以是中文，日文或者韩文，例如：

```css
body {
  --深蓝: #369;
  background-color: var(--深蓝);
}
```

所以，我们就可以直接使用中文名称作为变量，即使英语4级没过的小伙伴也不会有压力了，我们也不需要随时挂个翻译器在身边了。

无论是变量的定义和使用只能在声明块`{}`里面，例如，下面这样是无效的：

```css
 --深蓝: #369;
body {
  background-color: var(--深蓝);
}
```





## *🔳var()函数 使用变量*

`var()`函数用于读取变量。

```css
a {
  color: var(--foo);
  text-decoration-color: var(--bar);
}
```



**CSS变量使用完整语法**
CSS变量使用的完整语法为：`var( <custom-property-name> [, <declaration-value> ]? )`，用中文表示就是：`var( <自定义属性名> [, <默认值> ]? )`，

意思就是，如果我们使用的变量没有定义（注意，**仅限于没有定义**），则使用后面的值作为元素的属性值。举个例子：

```css
.box {
  --1: #369;
}
body {
  background-color: var(--1, #cd0000);
}
```

则此时的背景色是`#cd0000`



### CSS变量不合法的缺省特性

请看下面这个例子：

```css
body {
  --color: 20px;
  background-color: #369;
  background-color: var(--color, #cd0000);
}
```

请问，此时`<body>`的背景色是？

```
A. transparent    B. 20px     C. #369      D. #cd0000
```

答案是…………………………**A. transparent**

这是CSS变量非常有意思的一个点，对于CSS变量，只要语法是正确的，就算变量里面的值是个乱七八糟的东西，也是会作为正常的声明解析，如果发现变量值是不合法的，例如上面背景色显然不能是`20px`，则使用背景色的缺省值，也就是默认值代替，于是，上面CSS等同于：

> body {
> --color: 20px;
> background-color: #369;
> background-color: transparent;
> }

千万不能想当然得认为等同于`background-color:20px`，这也是为什么上面要强调CSS默认值的使用**仅限于变量未定义**的情况，并**不包括变量不合法**



### CSS属性名可以用变量定义吗？

类似下面这样：

```css
body {
    --bc: background-color;    
    var(--bc): #369;
}
```

答案是“不可以”，要是可以支持的话，那CSS的压缩可就要逆天了，估计所有的属性都会变成1~2个字符。





### CSS变量的空格尾随特性

请看下面这个例子：

```css
body {
  --size: 20;   
  font-size: var(--size)px;
}
```

请问，此时`<body>`的`font-size`大小是多少？

如果以为是`20px`就太天真了，实际上，此处`font-size:var(--size)px`等同于`font-size:20 px`，注意，`20`后面有个空格，所以，这里的`font-size`使用的是`<body>`元素默认的大小。因此，就不要妄图取消就使用一个数值来贯穿全场，还是使用稳妥的做法：

```css
body {
  --size: 20px;   
  font-size: var(--size);
}
```

或者使用CSS3 `calc()`计算：

```css
body {
  --size: 20;   
  font-size: calc(var(--size) * 1px);
}
```

此时，`<body>`的`font-size`大小才是`20px`





### CSS变量的相互传递特性

就是说，在CSS变量定义的时候可以直接引入其他变量给自己使用，例如：

```css
body {
  --green: #4CAF50;   
  --backgroundColor: var(--green);
}
```

或者更复杂的使用CSS3 `calc()`计算，例如：

```css
body {
  --columns: 4;
  --margins: calc(24px / var(--columns));
}
```









### 变量值的类型

如果变量值是一个字符串，可以与其他字符串拼接。

```css
--bar: 'hello';
--foo: var(--bar)' world';
```

利用这一点，可以 debug（[例子](https://codepen.io/malyw/pen/oBWMOY)）。

```css
body:after {
  content: '--screen-category : 'var(--screen-category);
}
```

如果变量值是数值，不能与数值单位直接连用。

```css
.foo {
  --gap: 20;
  /* 无效 */
  margin-top: var(--gap)px;
}
```

如果变量值带有单位，就不能写成字符串。

```css
/* 无效 */
.foo {
  --foo: '20px';
  font-size: var(--foo);
}

/* 有效 */
.foo {
  --foo: 20px;
  font-size: var(--foo);
}
```







### 变量作用域

同一个 CSS 变量，可以在多个选择器内声明。读取的时候，优先级最高的声明生效。这与 CSS 的"层叠"（cascade）规则是一致的。

下面是一个[例子]。

```html
<style>
  :root { --color: purple; }
  div { --color: green; }
  #alert { --color: red; }
  * { color: var(--color); }
</style>

<p>紫色</p>
<div>绿色</div>
<div id="alert">红色</div>
```

上面代码中，三个选择器都声明了`--color`变量。不同元素读取这个变量的时候，会采用优先级最高的规则，因此三段文字的颜色是不一样的。

![](css3_img\变量作用域.png)

上面这个例子我们可以获得这些信息：

1. **变量也是跟着CSS选择器走的**，如果变量所在的选择器和使用变量的元素没有交集，是没有效果的。例如`#alert`定义的变量，只有`id`为`alert`的元素才能享有。如果你想变量全局使用，则你可以设置在`:root`选择器上；
2. 当存在多个同样名称的变量时候，变量的覆盖规则由CSS选择器的权重决定的，但并无`!important`这种用法，因为没有必要，`!important`设计初衷是干掉JS的`style`设置，但对于变量的定义则没有这样的需求。



这就是说，变量的作用域就是它所在的选择器的有效范围。

```css
body {
  --foo: #7F583F;
}

.content {
  --bar: #F7EFD2;
}
```

上面代码中，变量`--foo`的作用域是`body`选择器的生效范围，`--bar`的作用域是`.content`选择器的生效范围。

由于这个原因，全局的变量通常放在根元素`:root`里面，确保任何选择器都可以读取它们。

```css
:root {
  --main-color: #06c;
}
```





## *🔳如何HTML标签和JS中操作CSS 变量*



 ### HTML标签中设置CSS变量

```html
<div style="--color: #cd0000;">
    <img src="mm.jpg" style="border: 10px solid var(--color);">
</div>
```

直接正常CSS语句一样在`style`属性中设置即可。





### JS对CSS变量的操作

如下，HTML示意：

```html
<div id="box">
    <img src="mm.jpg" style="border: 10px solid var(--color);">
</div>
```

如果要想让`var(--color)`生效，执行下面JavaScript代码即可：

```javascript
box.style.setProperty('--color', '#cd0000');
```



JavaScript 操作 CSS 变量的写法如下。

```javascript
// 设置变量
document.body.style.setProperty('--primary', '#7F583F');

// 读取变量
document.body.style.getPropertyValue('--primary').trim();
// '#7F583F'

// 删除变量
document.body.style.removeProperty('--primary');
```





### 借助content属性显示CSS var变量值



#### 一、变量作为字符动态呈现

CSS var变量（CSS自定义属性）很好用，然后，有时候，需要这些变量能够同时作为字符在页面中呈现，我们想到的是使用`::before/::after`伪元素配合`content`属性，但是，把CSS变量直接作为`content`属性值是没有任何效果的。

例如：

```css
/* 无效 */
.bar::before {
    content: var(--percent);
}
```

那该如何呈现呢？



#### 二、借助CSS计数器呈现CSS var变量值

示意代码如下：

```css
/* 有效 */
.bar::before {
    counter-reset: progress var(--percent);
    content: counter(progress);
}
```

也就是虽然`content`属性本身不支持变量，但是`counter-reset`属性后面的计数器初始值是支持的，于是我们可以来一招移花接木让CSS var变量值作为字符在页面中显示。

关于CSS计数器如果不太了解，可以参考这篇文章：“[CSS counter计数器(content目录序号自动递增)详解](https://www.zhangxinxu.com/wordpress/2014/08/css-counters-automatic-number-content/)”。









### CSS变量局部特性用途

由于CSS变量的局部作用特性，于是，我们可以放心大胆使用CSS变量给伪元素传参了。

例如，一个多图上传页面，有很多loading进度条，此时，就可以使用CSS变量将加载进度传给伪元素，这样，一个标签就能实现完整的进度效果了，代码简洁又高效，而在过去，我们一定要嵌套HTML标签才能实现。HTML代码如下：

```html
<label>图片1：</label>
<div class="bar" style="--percent: 60;"></div>
<label>图片2：</label>
<div class="bar" style="--percent: 40;"></div>
<label>图片3：</label>
<div class="bar" style="--percent: 20;"></div>
```

我们只需要在style属性中更新当前上传进度变量就可以了。

然后，我们就可以把这个变量转换成我们需要的伪元素数值以及宽度大小，CSS代码如下：

```css
.bar {
    height: 20px; width: 300px;
    background-color: #f5f5f5;
}
.bar::before {
    counter-reset: progress var(--percent);
    content: counter(progress) '%\2002';
    display: block;
    width: calc(300px * var(--percent) / 100);
    font-size: 12px;
    color: #fff;
    background-color: #2486ff;
    text-align: right;
    white-space: nowrap;
    overflow: hidden;
}
```

![](css3_img\变量局部的应用.png)

CSS宽度数值可以记住`counter`计数器呈现，对计数器还不了解的可以参见“[CSS counter计数器(content目录序号自动递增)详解](https://www.zhangxinxu.com/wordpress/2014/08/css-counters-automatic-number-content/)”这篇文章，在[《CSS世界》](http://www.epubit.com.cn/book/details/4767)这本书中也有详细介绍。





## *🔳CSS 变量实现响应式布局*

对于复杂布局，CSS变量的这种相互传递和直接引用特性可以简化我们的代码和实现成本，尤其和动态布局在一起的时候，无论是CSS的响应式后者是JS驱动的布局变化。

我们来看一个CSS变量与响应式布局的例子，点击这里：[CSS变量与响应式布局实例demo](http://www.zhangxinxu.com/study/201611/css-var-media-query-layout.html)

![](css3_img\css变量与响应式布局.png)

随着浏览器宽度减小，`4`栏可能就变成`3`栏，`2`栏甚至`1`栏，我们实际开发的时候，显然不仅仅是栏目数量变化，宽度小，往往意味着访问设备尺寸有限，此时我们往往会缩小空白间距以及文字字号大小，这样，有限屏幕才能显示更多内容。

也就是说，当我们响应式变化的时候，改变的CSS属性值不是1个，而是3个或者更多，如果我们有3个响应点，是不是就至少需要9个CSS声明？但是，由于我们有了CSS变量，同时，CSS变量可以传递，当我们遭遇响应点的时候，我们只需要改变一个CSS属性值就可以了。

下面就是本demo核心CSS代码（只需要改变`--columns`这一个变量即可）：

```css
.box {
    --columns: 4;
    --margins: calc(24px / var(--columns));
    --space: calc(4px * var(--columns));
    --fontSize: calc(20px - 4 / var(--columns));
}
@media screen and (max-width: 1200px) {
    .box {
        --columns: 3; /*随着屏幕尺寸变化进行响应*/
    }
}
@media screen and (max-width: 900px) {
    .box {
        --columns: 2; /*随着屏幕尺寸变化进行响应*/
    }
}
@media screen and (max-width: 600px) {
    .box {
        --columns: 1; /*随着屏幕尺寸变化进行响应*/
    }
}
```

于是，我们在2栏下的效果就是这样，字号，间距随着栏目数量的减小也一并减小了，然后每栏之间间距是扩大了：

![](css3_img\css变量与响应式布局2.png)



总结为以下

CSS 是动态的，页面的任何变化，都会导致采用的规则变化。

利用这个特点，就可以在响应式布局的`media`命令里面声明变量，使得不同的屏幕宽度有不同的变量值。

```css

body {
  --primary: #7F583F;
  --secondary: #F7EFD2;
}

a {
  color: var(--primary);
  text-decoration-color: var(--secondary);
}

@media screen and (min-width: 768px) {
  body {
    --primary:  #F7EFD2;
    --secondary: #7F583F;
  }
}
```







## *🔳CSS变量对JS交互组件开发带来的提升与变革*

CSS变量带来的提升绝不仅仅是节约点CSS代码，以及降低CSS开发和维护成本。

更重要的是，把组件中众多的交互开发从原来的JS转移到了CSS代码中，让组件代码更简洁，同时让视觉表现实现更加灵活了。

通过几个案例来说明这一变化。



### CSS变量成为了CSS API接口

![](css3_img\有了css变量之后.jpg)

过去点击提示，点击切换等效果都需要JS针对特定的元素进行样式设置，现在有了CSS变量，我们只需要一段非常简单的通用的全局JS就可以了，然后JS就可以玩耍自己应该玩耍的东西，其他效果，全部交给CSS处理。

这段JS如下：

```javascript
/**
 * js为css变量值，让css运用更加灵活
 * @description 点击页面任意位置，标记坐标位置
 */
document.addEventListener('mousedown', function (event) {
    var target = event.target;
    var body = document.body;
    var html = document.documentElement;

    // 设置自定义属性值
    body.style.setProperty('--pagex', event.pageX);
    body.style.setProperty('--pagey', event.pageY);

    html.style.setProperty('--clientx', event.clientX);
    html.style.setProperty('--clienty', event.clientY);
    html.style.setProperty('--scrolly', window.pageYOffset);

    target.style.setProperty('--offsetx', event.offsetX);
    target.style.setProperty('--offsety', event.offsetY);
    target.parentElement.style.setProperty('--target-width', target.clientWidth);
    target.parentElement.style.setProperty('--target-height', target.clientHeight);
    target.parentElement.style.setProperty('--target-left', target.offsetLeft);
    target.parentElement.style.setProperty('--target-top', target.offsetTop);
});
```

可以看到，JavaScript代码再也不负责任何与交互行为相关的逻辑，直接变成了桥接工具，一个单纯地**传递点击坐标位置**，以及**点击元素偏移和尺寸信息**的工具。



<span style="color:orange">CSS得到了什么呢？</span>

得到了一个巨大的宝藏，一个随时可以拿来使用的宝藏,**css可以直使用坐标信息以及元素偏移和尺寸信息了**

想要点击按钮的时候有什么花哨的反馈，或者点击页面空白也来个创意的交互提示，完全不成问题，随用随取，无比方便，无比自由。

可以说，上面这段JS，或者类似的JS代码是未来web开发的标配。

来看看上面的代码可以实现怎样的效果。



#### 按钮点击水纹涟漪效果

点击按钮的时候有个圈圈放大的效果，圈圈放大的中心点就是点击的位置

效果如下GIF所示：

![](css3_img\涟漪效果.gif)

核心CSS代码如下

```css
.btn:not([disabled]):active::after {
    transform: translate(-50%,-50%) scale(0);
    opacity: .3;
    transition: 0s;
}
.btn::after {
    content: "";
    display: block;
    position: absolute;
    width: 100%; height: 100%;
    left: var(--x, 0); top: var(--y, 0);
    pointer-events: none;
    background: radial-gradient(circle, currentColor 10%, transparent 10.01%) no-repeat 50%;
    transform: translate(-50%,-50%) scale(10);
    opacity: 0;
    transition: transform .3s, opacity .8s;
}
```

`:active`时候隐藏，同时设置过渡时间为0。于是，点击释放的时候，就会有过渡效果。

大家可以访问这个地址进行体验：https://xy-ui.codelabo.cn/docs/#/xy-button







#### 点击浮现文字效果

又例如，点击本文页面任意位置都会出现下图所示的提示信息。

![](css3_img\文字浮现.gif)

就是下面上面那段万能工具人JS加下面这段CSS实现的：

```css
body:active::after {
    transform: translate(-50%, -100%);
    opacity: 0.5;
    transition: 0s;
    left: -999px;
}
body::after {
    content: 'zhangxinxu.com';
    position:fixed;
    z-index: 999;
    left: calc(var(--clientx, -999) * 1px);
    top: calc(var(--clienty, -999) * 1px);
    transform: translate(-50%, calc(-100% - 20px));
    opacity: 0;
    transition: transform .3s, opacity .5s;
}
```







#### 面包屑选项卡切换

以前，下图这种点击选项卡按钮，然后下划线滑来滑去，尺寸还变化效果，使用纯CSS实现很考验功力，几乎99.99%的开发都是借助JS去查询对应DOM元素，然后设置宽高和位置实现的交互效果。

![](css3_img\面包屑动画.gif)

现在，有了工具人JS，只需要一段CSS就可以搞定了，甚至文字的高亮切换都可以纯CSS搞定，就是这么神奇。

下面这里的效果就是实现的实时效果（若没有效果，请访问原文）：

点击任意的选项卡元素，就可以看到下划线滑到对应位置，同时文字有高亮的效果。

相关代码如下：

```html
<div class="yw-tab-tab"> 
  <a href="javascript:" class="yw-tab-a">QQ阅读</a>
  <a href="javascript:" class="yw-tab-a">起点读书</a>
  <a href="javascript:" class="yw-tab-a">红袖读书</a>
  <a href="javascript:" class="yw-tab-a">飞读免费小说</a>
</div>
```

```css
 .yw-tab-tab {
    position: relative;
    display: flex;
    max-width: 414px;
    justify-content: space-between;
    border-bottom: 1px solid #717678;
    background-color: #fff;
    margin: 30px auto;
}
.yw-tab-tab::before,
.yw-tab-tab::after {
    position: absolute;
    width: calc(var(--target-width, 0) * 1px);
    left: calc(var(--target-left, -299) * 1px);
    color: #2a80eb;
}
.yw-tab-tab[style]::before,
.yw-tab-tab[style]::after  {
    content: '';
}
.yw-tab-tab::before {
    background-color: currentColor;
    height: calc(var(--target-height) * 1px);
    mix-blend-mode: overlay;
}
.yw-tab-tab::after {
    border-bottom: solid;    
    bottom: -2px;
    transition: left .2s, width .2s;
}
.yw-tab-a {
    color: #717678;
    padding: 10px 0;
}
```







### css 主打ui交互

web组件的很多API接口可以拜拜了

以前web组件有一个什么功能，就新增一个API接口，看上去很厉害，实际上，加着加着，API越来越多，组件也越来越重，学习成本也越来越高，最后走向了死胡同，变得笨重，迎来了灭亡。

现在，可以改变思路了。

那些与交互表现密切相关的功能，事实上仅仅在组件容器元素上传递CSS自定义属性就可以了，无需负责具体的定位，显隐，或者样式变化，全部交给CSS。

因为设计表现的东西是上层的，灵活的，个性的，应该在CSS层面进行驾驭才是合理的，匹配的。

例如上节提到的进度条组件，无论是条状的还是饼状的都是这样的处理逻辑，只负责传递进度值，样式无需关心。

![](css3_img\进度条.png)

又例如滑条框（如下图Ant Design中的滑条的位置和提示效果）、popup提示框等都可以**通过一个CSS自定义属性完成，JS仅需要把CSS无法获取的数据传递到祖先元素上，不需要负责UI样式。**















# 💥css 函数（functions）

CSS只是一个声明式的语言，主要为标记语言服务。很多程序员鄙视它，有一部分原因是CSS并不像其他程序语言一样，具有一些逻辑能力以及函数功能等特性。随着CSS的不断变革，其慢慢地也变得越来越强大。时至今日，CSS中也有具有函数和运算相关的能力。比如我们今天要聊的CSS函数。在[CSS Values and Units Module Level 4](https://www.w3.org/TR/css-values-4)中把[函数标记（**Functional Natations**）](https://www.w3.org/TR/css-values-4/#functional-notations)单独提取出来做为该规范的一部分。而这部分主要介绍了一些具有数学计算能力相关的属性值，比如大家熟悉的`calc()`和不怎么熟悉的`min()`和`max()`。而这节要聊的是CSS中的函数，其中就包括这些部分。

不要一直以为css没有几个函数，css现在竟然已经有86个函数了，意不意外，惊不惊喜！！！

常用css来解决之前js实现的效果，这样对性能时一种优化，自己也有成就感，希望这些函数能够更多的应用到的项目中

根据w3cplus中可以划分为以下几类：

- **属性函数**：attr()；
- **背景图片函数**：linear-gradient()、radial-gradient()、conic-gradient()、repeating-linear-gradient()、repeating-radial-gradient()、repeating-conic-gradient()、image-set()、image()、url()、element()；
- **颜色函数**：rgb()、rgba()、hsl()、hsla()、hwb()、color-mod()；
- **图形函数**：circle()、ellipse()、inset()、polygon()、path()
- **滤镜函数**：blur()、brightness()、contrast()、drop-shadow()、grayscale()、hue-rotate()、invert()、opacity()、saturate()、sepia()；
- **转换函数**：matrix()、matrix3d()、perspective()、rotate()、rotate3d()、rotateX()、rotateY()、rotateZ()、scale()、scale3d()、scaleX()、scaleY()、scaleZ()、skew()、skewX()、skewY()、translate()、translateX()、translateY()、translateZ()、translate3d()；
- **数学函数**：calc()、min()、max()、mixmax()、repeat()；
- **缓动函数**：cubic-bezier()、steps()；
- **其他函数**：counter()、counters()、toggle()、var()、 symbols()。





## 什么是函数标记（funcitonal）

![](css3_img\函数标记.jpg)

[函数标记](https://www.w3.org/TR/css-values-4/#functional-notations)是[CSS值和单位模块](https://www.w3.org/TR/css-values-4)中的一部分。从Level 4 开始，函数标记中涵盖的内容也越来越多，比如`toggle()`，它允许通过使用`attr()`在值和属性引用之间切换。另外更有趣的是，里面包含了一些具备数学计算能力的表达式，比如`calc()`、`min()`和`max()`。

W3C规范是这样描述函数标记的：

> 函数标记是一种可以表示更复杂类型或调用特殊处理的组件值类型。语法从函数名开始，紧接着是左括号，然后是参数到符号，然后是右括号。

形象一点来说，它是一个函数或函数名，比如`calc`，后面紧跟着`()`。括号之间可以是一个表达式。看上去是不是有点类似于其他编程语言中的函数呢？

从这一点来说，在CSS中只要是带有`()`的属性值，我们都可以把它纳入到CSS中函数标记（或者函数表示法），简单的就称其为**CSS函数**。





## 数学表达式

![](css3_img\数学表达式.jpg)

前面也提到过了，在函数标记中引入了数学表达式。有关于数学表达式，W3C是这样描述的：

> 数学函数`calc()`、`min()`和`max()`允许将带有加法（`+`）、减法（`-`）、乘法（`*`）和除法（`/`）的数学表达式用于`()`之中。

这也就是说，在一些函数中，我们可以通过表达式来计算值。简单点说，表达式一词只是括号内完全允许使用数学逻辑的一种花哨说法。





## 常用函数表

主要介绍主要常用的css函数，其他函数会在后文中使用到

参考以下表格

| 函数                                                         | 描述                                                         | CSS 版本 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| [attr()](https://www.runoob.com/cssref/func-attr.html)       | 返回选择元素的属性值。                                       | 2        |
| [calc()](https://www.runoob.com/cssref/func-calc.html)       | 允许计算 CSS 的属性值，比如动态计算长度值。                  | 3        |
| [cubic-bezier()](https://www.runoob.com/cssref/func-cubic-bezier.html) | 定义了一个贝塞尔曲线(Cubic Bezier)。                         | 3        |
| [hsl()](https://www.runoob.com/cssref/func-hsl.html)         | 使用色相、饱和度、亮度来定义颜色。                           | 3        |
| [hsla()](https://www.runoob.com/cssref/func-hsla.html)       | 使用色相、饱和度、亮度、透明度来定义颜色。                   | 3        |
| [linear-gradient()](https://www.runoob.com/cssref/func-linear-gradient.html) | 创建一个线性渐变的图像                                       | 3        |
| [radial-gradient()](https://www.runoob.com/cssref/func-radial-gradient.html) | 用径向渐变创建图像。                                         | 3        |
| [repeating-linear-gradient()](https://www.runoob.com/cssref/func-repeating-linear-gradient.html) | 用重复的线性渐变创建图像。                                   | 3        |
| [repeating-radial-gradient()](https://www.runoob.com/cssref/func-repeating-radial-gradient.html) | 类似 radial-gradient()，用重复的径向渐变创建图像。           | 3        |
| [rgb()](https://www.runoob.com/cssref/func-rgb-css.html)     | 使用红(R)、绿(G)、蓝(B)三个颜色的叠加来生成各式各样的颜色。  | 2        |
| [rgba()](https://www.runoob.com/cssref/func-rgba.html)       | 使用红(R)、绿(G)、蓝(B)、透明度(A)的叠加来生成各式各样的颜色。 | 3        |
| [var()](https://www.runoob.com/cssref/func-var.html)         | 用于插入自定义的属性值。                                     |          |





### var()

var() 函数用于插入自定义的属性值，如果一个属性值在多处被使用，该方法就很有用。

支持版本：CSS3

定义一个名为 "--main-bg-color" 的属性，然后使用 var() 函数调用该属性：

```css
:root {
  --main-bg-color: coral;
}
 
#div1 {
  background-color: var(--main-bg-color);
}
 
#div2 {
  background-color: var(--main-bg-color);
}
```



**属性值参考**

| 值                     | 描述                                     |
| :--------------------- | :--------------------------------------- |
| *custom-property-name* | 必需。自定义属性的名称，必需以 -- 开头。 |
| *value*                | 可选。备用值，在属性不存在的时候使用。   |









### rgb()

rgb() 函数使用红(R)、绿(G)、蓝(B)三个颜色的叠加来生成各式各样的颜色。

RGB 即红色、绿色、蓝色（英语：Red, Green, Blue）。

- **红色（R）**0 到 255 间的整数，代表颜色中的红色成分。。
- **绿色（G）**0 到 255 间的整数，代表颜色中的绿色成分。
- **蓝色（B）**0 到 255 间的整数，代表颜色中的蓝色成分。

支持版本：CSS2

使用 RGB 颜色：

```css
#p1 {background-color:rgb(255,0,0);} /* 红 */
#p2 {background-color:rgb(0,255,0);} /* 绿 */
#p3 {background-color:rgb(0,0,255);} /* 蓝 */
```

**属性值参考**

| 值      | 描述                                                         |
| :------ | :----------------------------------------------------------- |
| *red*   | 定义红色值，取值范围为 0 ~ 255，也可以使用百分比 0% ~ 100%。 |
| *green* | 定义绿色值，取值范围为 0 ~ 255，也可以使用百分比 0% ~ 100%。 |
| *blue*  | 定义蓝色值，取值范围为 0 ~ 255，也可以使用百分比 0% ~ 100%。 |





### rgba()

rgba() 函数使用红(R)、绿(G)、蓝(B)、透明度(A)的叠加来生成各式各样的颜色。

RGBA 即红色、绿色、蓝色、透明度（英语：Red, Green, Blue、Alpha）。

- **红色（R）**0 到 255 间的整数，代表颜色中的红色成分。。
- **绿色（G）**0 到 255 间的整数，代表颜色中的绿色成分。
- **蓝色（B）**0 到 255 间的整数，代表颜色中的蓝色成分。
- **透明度（A）**取值 0~1 之间， 代表透明度。

支持版本：CSS3



**属性值参考**

| 值               | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| *red*            | 定义红色值，取值范围为 0 ~ 255，也可以使用百分比 0% ~ 100%。 |
| *green*          | 定义绿色值，取值范围为 0 ~ 255，也可以使用百分比 0% ~ 100%。 |
| *blue*           | 定义蓝色值，取值范围为 0 ~ 255，也可以使用百分比 0% ~ 100%。 |
| *alpha - 透明度* | 定义透明度 0（透完全明） ~ 1（完全不透明）                   |







### hsl()

hsl() 函数使用色相、饱和度、亮度来定义颜色。

```css
#p1 {background-color:hsl(120,100%,50%);} /* 绿色 */
#p2 {background-color:hsl(120,100%,75%);} /* 浅绿  */
#p3 {background-color:hsl(120,100%,25%);} /* 暗绿  */
#p4 {background-color:hsl(120,60%,70%);} /* 柔和的绿色 */
```

HSL 即色相、饱和度、亮度（英语：Hue, Saturation, Lightness）。

- **色相（H）**是色彩的基本属性，就是平常所说的颜色名称，如红色、黄色等。
- **饱和度（S）**是指色彩的纯度，越高色彩越纯，低则逐渐变灰，取 0-100% 的数值。
- **亮度（L）**，取 0-100%，增加亮度，颜色会向白色变化；减少亮度，颜色会向黑色变化。

HSL 是一种将 RGB 色彩模型中的点在圆柱坐标系中的表示法。这两种表示法试图做到比基于笛卡尔坐标系的几何结构 RGB 更加直观。

支持版本：CSS3

![](css3_img\hsl 色相  饱和度 亮度.png)

**属性值参考**

| 值                    | 描述                                                         |
| :-------------------- | :----------------------------------------------------------- |
| *hue - 色相*          | 定义色相 (0 到 360) - 0 (或 360) 为红色, 120 为绿色, 240 为蓝色 |
| *saturation - 饱和度* | 定义饱和度; 0% 为灰色， 100% 全色                            |
| *lightness - 亮度*    | 定义亮度 0% 为暗, 50% 为普通, 100% 为白                      |







### hsla()

hsla() 函数使用色相、饱和度、亮度、透明度来定义颜色。

HSLA 即色相、饱和度、亮度、透明度（英语：Hue, Saturation, Lightness, Alpha ）。

- **色相（H）**是色彩的基本属性，就是平常所说的颜色名称，如红色、黄色等。
- **饱和度（S）**是指色彩的纯度，越高色彩越纯，低则逐渐变灰，取 0-100% 的数值。
- **亮度（L）** 取 0-100%，增加亮度，颜色会向白色变化；减少亮度，颜色会向黑色变化。
- **透明度（A）** 取值 0~1 之间， 代表透明度。

支持版本：CSS3

 **属性值参考**

| 值                    | 描述                                                         |
| :-------------------- | :----------------------------------------------------------- |
| *hue - 色相*          | 定义色相 (0 到 360) - 0 (或 360) 为红色, 120 为绿色, 240 为蓝色 |
| *saturation - 饱和度* | 定义饱和度; 0% 为灰色， 100% 全色                            |
| *lightness - 亮度*    | 定义亮度 0% 为暗, 50% 为普通, 100% 为白                      |
| *alpha - 透明度*      | 定义透明度 0（透完全明） ~ 1（完全不透明）                   |







### attr()

attr() 函数返回选择元素的属性值。

支持版本：CSS2

```css
a:after {
    content: " (" attr(href) ")";
}
```

**属性值参考**

| 值               | 描述                                   |
| :--------------- | :------------------------------------- |
| *attribute-name* | 必须。HTML 元素的属性名。              |
| *value*          | 可选。备用值，在属性不存在的时候使用。 |





**Polyfill** attr()新语法

传统的`attr()`语法只能让HTML属性作为字符串使用，且只能使用在伪元素中，例如：

```html
<span data-title="提示">按钮</span>
```

```css
span:hover::after {
    content: attr(data-title);
}
```

但是全新的`attr()`语法那可就完全不得了了，可以让HTML属性值转换成任意的CSS数据类型。

例如如下所示的HTML和CSS语句：

```html
<button bgcolor="skyblue" radius="4">按钮</button>
<button bgcolor="#00000040" radius="1rem">按钮</button>
<button bgcolor="red" radius="50%">按钮</button>
<button bgcolor="orange" radius="100% / 50%" title="by zhangxinxu(.com)">按钮</button>
```

```css
button {
    background-color: attr(bgcolor color);
    border-radius: attr(radius px, 4px);
}
```

也就是几个按钮的样式使用了新的`attr()`语法进行设置，平常的按钮颜色、圆角都是在CSS中设定好的，这个例子中则不一样，颜色和圆角全部都是外部的HTML属性控制的。

理论上，上面的代码会有如下图所示的效果。

![](css3_img\attr 按钮.png)

可以看出，如果浏览器支持了`attr()`函数的值类型新语法，那么我们日常的组件开发就会迎来巨大的颠覆，许多组件的接口可以直接交给CSS完成，以及我们的开发会更灵活，可以节省大量的CSS代码和书写CSS的时间，例如：

```css
[ml] { margin-left: attr(ml px, 0); }
[mt] { margin-top: attr(ml px, 0); }
[mr] { margin-right: attr(mr px, 0); }
[mb] { margin-bottom: attr(mb px, 0); }
[pl] { padding-left: attr(pl px, 0); }
[pt] { padding-top: attr(pt px, 0); }
[pr] { padding-right: attr(pr px, 0); }
[pb] { padding-bottom: attr(pb px, 0); }
```

那么元素的`margin`和`padding`设置就不需要专门写在CSS样式中，直接在HTML中设置即可，支持任意的`margin`和`padding`大小，例如：

```html
<div mt="10">上间距10px</div>
```

喔噢，看上去很厉害哦，HTML和CSS会一下子变得非常灵活与高效。

只可惜，`attr()`函数的新语法目前没有任何浏览器支持，而且在我看来很长一段时间浏览器都不会支持，安全、性能等方面的影响太深远了。

![](css3_img\attr fallback兼容.png)

有没有什么办法让浏览器可以支持CSS `attr()`函数的这个新语法呢？

可以借助CSS变量作为信使实现Polyfill实现。



CSS变量有一个特性，那就是CSS自定义属性值支持各种表达式和函数值，哪怕这个表达式是不知所云的东西。

例如随便自定义一个名叫`keyword()`的函数，然后使用如下所示的CSS调用：

```css
body {
    --keyword: keyword(red, 50%); /*合法*/
    color: var(--keyword);
}
```

结果浏览器认为语法是合法的（比方说下图语句没有出现删除线，说明语句是合法的）：

![](css3_img\keyworld-合法变量.png)

于是我们就可以利用CSS变量的这个特性Polyfill `attr()`函数，原理如下。

1. CSS自定义属性作为信使传递`attr()`函数，保证语法的合法性，例如

   ```css
   button {
       --attr-bg: attr(bgcolor color);
       background-color: var(--attr-bg);
   }
   ```

2. 获取⻚⾯所有的包含`attr()`函数的⾃定义属性

3. 遍历并观察所有DOM，如果设置了对应的⾃定义属性，将`attr()`语法转换成浏览器识别的常规自定义属性语法。

然后页面中有如下所示的HTML和CSS代码：

```html
<button bgcolor="skyblue" radius="4">按钮</button>
<button bgcolor="#00000040" radius="1rem">按钮</button>
<button bgcolor="red" radius="50%">按钮</button>
<button bgcolor="orange" radius="100% / 50%">按钮</button>
<style>
button {
    border: 0;
    padding: .5em 1em;
}
button {
    --attr-bg: attr(bgcolor color);
    background-color: var(--attr-bg);
    --attr-radius: attr(radius px, 4px);
    border-radius: var(--attr-radius);
}
</style>
```

结果就有如下图所示的效果，按钮表现出了符合预期的效果

![](css3_img\attr 按钮.png)

实际开发中，按钮元素往往需要一个默认的主样式，此时可以通过属性选择器进行区分，例如：

```css
button {
    color: #fff;
    background-color: deepskyblue;
}
button[bgcolor] {
    --attr-bg: attr(bgcolor color);
    background-color: var(--attr-bg);
}
button[radius] {
    --attr-radius: attr(radius px, 4px);
    border-radius: var(--attr-radius);
}
```









###  calc()

CSS3 的 calc() 函数允许我们在属性值中执行数学操作。例如，我们可以使用 calc() 指定一个元素宽的固定像素值为多个数值的和。

```css
.foo {
  width: calc(100px + 50px);
}
```

- 需要注意的是，**运算符左右两边要有空格；数字和单位之间不能有空格；括号与减数，被减数之间不能有空格；**，例如：`width: calc(100% - 10px)`；
- 任何长度值都可以使用calc()函数进行计算；
- calc()函数支持 "+", "-", "*", "/" 运算；
- calc()函数使用标准的数学运算优先级规则；



calc() 函数提供了更好的计算方案。首先，我们能够组合不同的单元。特别是，我们可以**混合计算**相对单位（比如百分比与视口单元）与绝对单位（比如像素）。例如，我们可以创造一个表达式，用一个百分比减掉一个像素值。

使用 calc() 函数计算 `<div>` 元素的宽度

```css
#div1 {
    width: calc(100% - 100px);
}
```

本例中，#div1 元素总是小于它父元素宽度 50px。

**属性值参考**

| 值           | 描述                                             |
| :----------- | :----------------------------------------------- |
| *expression* | 必须，一个数学表达式，结果将采用运算后的返回值。 |



calc() 函数可以用来对数值属性执行四则运算。比如，`<length>`，`<frequency>`，`<angle>`，`<time>`，`<number>` 或者 `<integer>` 数据类型。

这里有一些示例：

```css
.foo {
    width: calc(50vmax + 3rem);
    padding: calc(1vw + 1em);
    transform: rotate( calc(1turn + 28deg) );
    background: hsl(100, calc(3 * 20%), 40%);
    font-size: calc(50vw / 3);
}
```



**calc()嵌套**

calc() 函数可以嵌套。在函数里边，会被视为简单的括号表达式，如下例所示。

```css
.foo {
    width: calc( 100% / calc(100px * 2) );
}
```

函数的计算值如下所示：

```css
.foo {
    width: calc( 100% / (100px * 2) );
}
```



**calc()降级方案**

对于不支持 calc() 的浏览器，整个属性值表达式将被**忽略**。不过我们可以对那些不支持 calc() 的浏览器，使用一个固定值作为降级方案。

```css
.foo {
    width: 90%; 
    width: calc(100% - 50px);
}
```





**calc()使用场景**

**元素居中**

使用 calc() 给我们提供另一个垂直居中元素的解决方案。如果我们知道元素的尺寸，一个典型的解决方案是使用负外边距移动自身距离高与宽的一半，如下所示：

```css
.foo {
    position: absolute
    top: 50%;
    left: 50%;
    marging-top: -150px;
    margin-left: -150px;
}
```

使用 calc() 函数，我们仅仅通过 top 与 left 属性便能实现相同的效果：

```css
.foo {
    position: absolute
    top: calc(50% - 150px);
    left: calc(50% - 150px);
}
```





**清晰化计算**

最后，calc()使计算更加清晰。如果你使一组项目为它们父元素容器宽度的 1/6，你可能这么写：

```css
.foo {
    width: 16.666666667%;
}
```

然而，它能够更加清晰并具有可读性：

```css
.foo {
    width: calc(100% / 6);
}
```





**创建栅格尺寸**

使用 rem，calc() 函数能够用来创建一个基于视口的栅格。我们可以设置根元素的字体大小为视口宽度的一部分。

```css
html {  
    font-size: calc(100vw / 30);
}
```

现在，1rem 为视口宽度的 1/30。在页面上的任何文本，将会根据你的视口自动缩放。更进一步，相同比例的视口总会显示相同的文本数量，不管视口的真实尺寸是多少。

如果我们对非文本使用 rem 设置大小，它们同样遵守这个行为。一个 1rem 宽度的元素总是视口元素宽度的 1/30。









### linear-gradient()

linear-gradient() 函数用于创建一个表示两种或多种颜色线性渐变的图片。

创建一个线性渐变，需要指定两种颜色，还可以实现不同方向（指定为一个角度）的渐变效果，如果不指定方向，默认从下到上渐变。

语法：

```css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
```

以下实例演示了从头部开始的线性渐变，从红色开始，转为黄色，再到蓝色:

```css
#grad {
  background-image: linear-gradient(red, yellow, blue);
}
```

<div style="background-image: linear-gradient(red, yellow, blue);height:200px;"></div>

```css
/* 从上到下，蓝色渐变到红色 */
linear-gradient(45deg, blue, red);
 
/* 渐变轴为45度，从蓝色渐变到红色 */
linear-gradient(45deg, blue, red);
 
/* 从右下到左上、从蓝色渐变到红色 */
linear-gradient(to left top, blue, red);
 
/* 从下到上，从蓝色开始渐变、到高度40%位置是绿色渐变开始、最后以红色结束 */
linear-gradient(0deg, blue, green 40%, red);

/*使用了透明度*/
linear-gradient(to right, rgba(255,0,0,0), rgba(255,0,0,1));
```

**属性值参考**

| 值                             | 描述                                         |
| :----------------------------- | :------------------------------------------- |
| *direction*                    | 用角度值指定渐变的方向（或角度）。           |
| *color-stop1, color-stop2,...* | 用于指定渐变的**起止颜色**以及**起始位置**。 |

**渐变角度**

![](css3_img\渐变角度.jpg)















### radial-gradient()

radial-gradient() 函数用径向渐变创建 "图像"。

径向渐变由中心点定义。

为了创建径向渐变你必须设置两个终止色。

语法：

```css
background-image: radial-gradient(shape size at position, start-color, ..., last-color);
```

以下实例演示了径向渐变 - 颜色结点均匀分布:

```css
#grad {
  background-image: radial-gradient(red, green, blue);
}
```

<div style="background-image: radial-gradient(red, green, blue);height:200px"></div>

**属性值参考**

| 值                             | 描述                                                         |
| :----------------------------- | :----------------------------------------------------------- |
| *shape*                        | 确定圆的类型:ellipse (默认): 指定椭圆形的径向渐变。circle ：指定圆形的径向渐变 |
| *size*（x,y）                  | 定义渐变的大小。默认是farthest-corner，closest-side:半径为从圆心到最近边 ，closest-corner:半径为从圆心到最近角 ，farthest-side:半径为从圆心到最远边 ，farthest-corner:半径为从圆心到最远角，**length**:设置数值单位作为**直径** |
| *position（x,y）*              | 定义渐变**圆心**的位置。可能值：**center**（默认）：设置中间为径向渐变圆心的纵坐标值。**top**：设置顶部为径向渐变圆心的纵坐标值。**bottom**：设置底部为径向渐变圆心的纵坐标值。**left**：设置左边为径向渐变圆心的横坐标值。**right**：设置右边为径向渐变圆心的横坐标值。**length**:设置数值单位作为坐标 |
| *start-color, ..., last-color* | 用于指定渐变的**起止颜色**以及**起始位置**。                 |



**坐标参考配置**

![](css3_img\渐变坐标.png)

![](css3_img\渐变坐标2.png)





**size关键字**

**closest-side** 半径为从圆心到最近的边

![](css3_img\圆心到最近边.jpg)



**farthest-side** 半径为从圆心到最远的边

![](css3_img\圆心到最远边.jpg)



**closest-corner** 半径为从圆心到最近的角

![](css3_img\圆心到最近角.jpg)



**farthest-corner** 半径为从圆心到最远的角

![](css3_img\圆心到最远角.jpg)









###  repeating-linear-gradient() 

repeating-linear-gradient() 函数用于创建重复的线性渐变 "图像"。

支持版本：CSS3

语法：

```css
background: repeating-linear-gradient(angle | to side-or-corner, color-stop1, color-stop2, ...);
```

重复的线性渐变:

```css
#grad {
  background-image: repeating-linear-gradient(red, yellow 10%, green 20%);
}
```

<div style=" background-image: repeating-linear-gradient(red, yellow 10%, green 20%);height:200px;"></div>



**属性值参考**

| 值                             | 描述                                                         |
| :----------------------------- | :----------------------------------------------------------- |
| *angle*                        | 定义渐变的角度方向。从 0deg 到 360deg，默认为 180deg。       |
| *side-or-corner*               | 指定线性渐变的起始位置。由两个关键字组成：第一个为指定水平位置(left 或 right)，第二个为指定垂直位置（top 或bottom）。 顺序是随意的，每个关键字都是可选的。 |
| *color-stop1, color-stop2,...* | 指定渐变的起止颜色，由颜色值、停止位置（可选，使用百分比指定）组成。 |







### repeating-radial-gradient()

repeating-radial-gradient() 函数用于创建重复的径向渐变 "图像"。

支持版本：CSS3

语法：

```css
background-image: repeating-radial-gradient(shape size at position, start-color, ..., last-color);
```

重复的线性渐变:

```css
#grad {
  background-image: repeating-radial-gradient(red, yellow 10%, green 15%);
}
```

<div style="background-image: repeating-radial-gradient(red, yellow 10%, green 15%);height:200px;"></div>



**属性值参考**

| 值                             | 描述                                                         |
| :----------------------------- | :----------------------------------------------------------- |
| *shape*                        | 定义渐变的形状。可以是:ellipse (默认)：指定椭圆形的径向渐变circle：指定圆形的径向渐变 |
| *size*                         | 边缘轮廓的具体位置。可以是以下值：farthest-corner (默认)：指定径向渐变的半径长度为从圆心到离圆心最远的角。closest-side：指定径向渐变的半径长度为从圆心到离圆心最近的边。closest-corner：指定径向渐变的半径长度为从圆心到离圆心最近的角。farthest-side：与 closest-side 相反，指定径向渐变的半径长度为从圆心到离圆心最远的边。 |
| *position*                     | 圆心位置，类似 on与 background-position 或者 transform-origin。默认为 "center" |
| *start-color, ..., last-color* | 用于指定渐变的起止颜色，可以使用 长度值或百分比来指定起止色位置，但不允许负值。 |

















# 💥html5 自定义属性

HTML5中我们可以使用`data-`前缀设置我们需要的自定义属性，来进行一些数据的存放，例如我们要在一个文字按钮上存放相对应的id：

```html
<a href="javascript:" data-id="2312">测试</a>
```

这里的`data-`前缀就被称为`data属性`，其可以通过脚本进行定义，也可以应用CSS属性选择器进行样式设置。数量不受限制，在控制和渲染数据的时候提供了非常强大的控制。





## dataset 对象

下面是元素应用data属性的一个例子：

```html
<div id="day2-meal-expense" 
  data-drink="coffee" 
  data-food="sushi" 
  data-meal="lunch">¥20.12
</div>
```

要想获取某个属性的值，可以像下面这样使用dataset对象：

```javascript
var expenseday2 = document.getElementById('day2-meal-expense');  
var typeOfDrink = expenseday2.dataset.drink; /*coffee*/
```



需要注意的是带连字符连接的名称在使用的时候需要**命名驼峰化**，即大小写组合书写，这与应用元素的style对象类似，`dom.style.borderColor`。例如，假设上面的例子中现在有如下data属性，`data-meal-time`，则我们要获取相应的值可以使用：

```javascript
expenseday2.dataset.mealTime
```





## 为何使用dataset

如果使用传统的方法获取属性值应该会类似下面：

```javascript
var typeOfDrink = document.getElementById('day2-meal-expense').getAttribute('data-drink');
```

现在，假设我们要获得多个自定义的属性值，折腾的事情就来了，我们可能要采类似下面的N行代码实现了：

```javascript
var attrs = expenseday2.attributes,
expense = {}, i, j;  
for (i = 0, j = attrs.length; i < j; i++) {
  if(attrs[i].name.substring(0, 5) == 'data-') {
    expense[attrs[i].name.substring(5)] = attrs[i].value;
  }
}
```

而使用`dataset`属性，我们根本不需要任何循环去获取你想要的那个值，直接秒杀：

```javascript
expense = document.getElementById('day2-meal-expense').dataset;
```

`dataset`并不是典型意义上的JavaScript对象，而是个`DOMStringMap对象`，`DOMStringMap`是HTML5一种新的含有多个名-值对的交互变量。







## dataset 的操作

可以像下面这样操作名-值对：

```javascript
chartInput = [];
for (var item in expense) {
  chartInput.push(expense[item]);
}
```

上面这几行代码作用是让所有的自定义的属性值塞到一个数组中。

如果你想删掉一个`data属性`，可以这么做：

```javascript
delete expenseday2.dataset.meal;
```

如果你想给元素添加一个属性，可以这么做：

```javascript
expenseday2.dataset.dessert = 'icecream';
```



每次你使用自定义data属性的时候，使用`dataset`去获取名-值对就是个不错的选择。考虑到现在很多浏览器还是把`dataset`当作不认识的外星生物看待，所以，在实际使用的时候，有必要进行下特征检测，看看是否支持`dataset`，类似下面的使用：

```javascript
if(expenseday2.dataset) {
  expenseday2.dataset.dessert = 'icecream';
} else {
  expenseday2.setAttribute('data-dessert', 'icecream');
}
```





可以基于data属性值对相应的元素设置CSS样式，例如下面这个例子：

HTML代码如下：

```html
<div class="mm" data-name="无版权"></div>
<div class="mm" data-name="undefined"></div>
```

CSS代码如下：

```css
.mm{width:256px; height:200px;}
.mm[data-name='无版权']{background:url(mm1.jpg) no-repeat;}
.mm[data-name='undefined']{background:url(mm3.jpg) no-repeat;}
```











## dataset的获取速度

使用`dataset`操作`data `要比使用`getAttribute`稍微慢些，如下截图：

![](css3_img\dataset的速度.png)

但是，如果我们只是处理少量的data数据，这种速度上差异造成的影响是基本上没有的。反而，我们应该看到，使用`dataset`操作自适应属性要比其他类似`getAttribute`的形式要少很多让人头疼的麻烦，并且更具有可读性。因此，权衡来看，操作自定义属性，`dataset`操作是上选。

















# 💥css 单位

平时我们再制作页面的时候用到的单位也就那么几个，而实际上CSS中的可用单位的数量多得惊人，尤其CSS3的出现更壮大了CSS单位家族。而本文就是简单展示下这些值这些单位。

CSS 的值和单位是 CSS 相关的另一个独立功能模块，到目前为止，该模块已到了 Level 4 阶段（[CSS Values and Units Module Level 4](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.w3.org%2FTR%2Fcss-values-4%2F)）。今天我们就来聊聊这个模块里的内容。

![](css3_img\css所有单位.jpg)

为了便于更好的理解和记住CSS 中单位相关的知识点，下图是根据 W3C 规范重新做的划分：

![](css3_img\css所有单位2.jpg)









## *🔳长度单位*

CSS 有几个不同的单位用于表示长度。

一些设置 CSS 长度的属性有 width, margin, padding, font-size, border-width, 等。

长度有一个数字和单位组成如 10px, 2em, 等。

数字与单位之间不能出现空格。**如果长度值为 0，则可以省略单位。**

对于一些 CSS 属性，长度可以是负数。

有两种类型的长度单位：**相对和绝对**。











**相对长度**

相对长度单位指定了**一个长度相对于另一个长度的属性**。对于不同的设备相对长度更适用。

 

| 单位 | 描述                                                         | 在线实例                                                     |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| em   | 它是描述相对于应用在当前元素的字体尺寸，所以它也是相对长度单位。一般浏览器字体大小默认为16px，则2em == 32px； | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_em) |
| ex   | 依赖于英文字母小 **x** 的高度                                | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_ex) |
| ch   | 数字 0 的宽度                                                |                                                              |
| rem  | rem 是根 em（root em）的缩写，rem作用于非根元素时，相对于根元素字体大小；rem作用于根元素字体大小时，相对于其出初始字体大小。 | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_rem) |
| vw   | viewpoint width，视窗宽度，1vw=视窗宽度的1%                  | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_vw) |
| vh   | viewpoint height，视窗高度，1vh=视窗高度的1%                 | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_vh) |
| vmin | 相对于视窗的宽度或高度，vw和vh取较小的那个。                 | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_vmin) |
| vmax | 相对于视窗的宽度或高度，vw和vh取较大的那个。                 | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_vmax) |
| %    | 相对于父元素长度的百分比。正常情况下是通过属性定义自身或其他元素 |                                                              |

**提示:** rem与em有什么区别呢？区别在于使用rem为元素设定字体大小时，仍然是相对大小，但**相对的只是HTML根元素**。







**绝对单位**

绝对长度单位是一个固定的值，它反应一个真实的物理尺寸。绝对长度单位视输出介质而定，不依赖于环境（显示器、操作系统等） ，也不受任何屏幕大小或字体的影响。这些单位的显示可能会根据不同的屏幕分辨率而有所不同，因为它们取决于屏幕的DPI（每英寸上的点数）。绝对单位常用于一些物理测量上。在环境输出已知的情形下非常有用。

| 单位 | 描述                                   | 在线实例                                                     |
| :--- | :------------------------------------- | :----------------------------------------------------------- |
| cm   | 厘米                                   | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_cm) |
| mm   | 毫米                                   | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_mm) |
| in   | 英寸 (1in = 96px = 2.54cm)             | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_in) |
| px   | 像素 (1px = 1/96th of 1in)             | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_px) |
| pt   | point，大约1/72英寸； (1pt = 1/72in)   | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_pt) |
| pc   | pica，大约6pt，1/6英寸； (1pc = 12 pt) | [尝试一下](https://www.runoob.com/try/tryit.php?filename=trycss_unit_pc) |









### px  css像素

px表示“绝对尺寸”（并非真正的绝对），实际上就是css中定义的像素（*此像素与设备的**物理像素**有一定的区别*，），利用px设置字体大小及元素宽高等比较稳定和精确。px的缺点是其不能适应浏览器缩放时产生的变化，因此一般不用于响应式网站。



**CSS 像素（CSS Pixel）:**
又称为**虚拟像素**、设备独立像素或逻辑像素，也可以理解为直觉像素。CSS 像素是 Web 编程的概念，指的是 CSS 样式代码中使用的逻辑像素。比如 iPhone 6 的 CSS 像素数为 375 x 667px。

虚拟像素，可以理解为“直觉”像素，CSS和JS使用的抽象单位，浏览器内的一切长度都是以CSS像素为单位的，CSS像素的单位是px。

**设备像素（Device Pixels）:**
又称为**物理像素**。指设备能控制显示的最小物理单位，意指显示器上一个个的点。从屏幕在工厂生产出的那天起，它上面设备像素点就固定不变了。比如 iPhone 6 的分辨率为 750 x 1334px

**设备像素比**（DevicePixelRatio）:**DPR** = 设备像素 / CSS 像素

获得设备像素比后，便可得知设备像素与CSS像素之间的比例。当这个比率为1:1时，使用1个设备像素显示1个CSS像素。当这个比率为2:1时，使用4个设备像素显示1个CSS像素，当这个比率为3:1时，使用9（3*3）个设备像素显示1个CSS像素。

![](css3_img\像素比.jpg)

在CSS规范中，长度单位可以分为两类，绝对(absolute)单位以及相对(relative)单位。px是一绝对单位，但是是相对设备像素(device pixel)而言的。

在同样一个设备上，每1个CSS像素所代表的物理像素是可以变化的(即CSS像素的第一方面的相对性);
在不同的设备之间，每1个CSS像素所代表的物理像素是可以变化的(即CSS像素的第二方面的相对性);

px实际是pixel（像素）的缩写，它是图像显示的基本单元，既不是一个确定的物理量，也不是一个点或者小方块，而是一个抽象概念。所以在谈论像素时一定要清楚它的上下文！











### pt 物理像素

pt（point，磅）：是一个物理长度单位，指的是72分之一英寸。

设备像素（物理像素），顾名思义，显示屏是由一个个物理像素点组成的，通过控制每个像素点的颜色，使屏幕显示出不同的图像，屏幕从工厂出来那天起，它上面的物理像素点就固定不变了，单位pt。

pt在css单位中属于真正的绝对单位，1pt = 1/72(inch),inch及英寸，而1英寸等于2.54厘米。

注意，我们通常所说的显示器分辨率，其实是指桌面设定的分辨率，而不是显示器的物理分辨率。只不过现在液晶显示器成为主流，只有在桌面分辨率与物理分辨率一致的情况下，显示效果最佳，所以现在我们的桌面分辨率几乎总是与显示器的物理分辨率一致了







### css像素与物理像素的关系

写样式时一个像素记作1px，但是css的px和物理像素是一一对应的吗，是同样的概念么？在pc端是这样的，因为屏幕足够大，一个css像素用一个物理像素来显示，完全可以，pc端默认情况下一个css像素就对应着一个物理像素，但是有没有发现你把分辨率调小以后，显示的内容变大了，但是显示器的物理像素肯定不会变啊，这时候其实就是一个css像素对应着若干个物理像素了，这个是与用户设置有关。



移动设备大小是有限的，而且分辨率不低，甚至有些比pc端更高，也就是可以显示的物理像素更多，如果和pc端一样，一个css的px和物理像素一一对应，可以想象，显示的内容有多小。这样肯定是不行的，解决这个问题，我们可以很自然的想到，那在移动设备上就别一一对应了，**一个css的px对应多个物理像素**吧，这样就不至于显示的内容过小了，实际上移动设备也是这么做的，你在开发时写的px和最终渲染显示的物理像素数不是一比一的，可能一个px对应2个物理像素，可能3个物理像素，**设备显示的物理像素数和你css的px数的比值就叫做设备像素比（device pixel radio）**，简称dpr。好了，这样显示内容过小的问题就解决了。

有了dpr之后，有一个问题就是同样的一张图片，我设了宽高的px数，那么在dpr为1的设备上，和dpr为2的设备上显示的效果是一样的，1个px在dpr为1的设备上会用1个物理像素来显示，在dpr为2的设备上会用2*2个物理像素来显示，这样dpr高的优势就体现不出来了，我设备比他的好，你给我的体验是一样的，可能有些用户不爽，我们可以区分对待，对于高dpr的设备，用物理像素更多的高清图片来替代，也就是2x图，3x图等等。



**再谈像素的物理（物理像素）和逻辑（CSS像素）之分:**

设备像素比（Device Pixel Ratio，DPR）：一个设备的物理像素与逻辑像素之比。

在移动端浏览器中以及某些桌面浏览器中，window对象有一个**devicePixelRatio**属性，它的官方的定义为：设备物理像素和设备独立像素的比例，也就是 **devicePixelRatio = 物理像素 / 独立像素。**css中的px就可以看做是设备的独立像素，所以通过devicePixelRatio，我们可以知道该设备上一个css像素代表多少个物理像素。devicePixelRatio在不同的浏览器中还存在些许的兼容性问题，所以我们现在还并不能完全信赖这个东西，具体的情况可以看下[这篇文章](http://www.quirksmode.org/blog/archives/2012/06/devicepixelrati.html)。

其实在很久以前，的确是没区别的，CSS里写个1px，屏幕就给你渲染成1个实际的像素点，DPR=1，多么简单自然~
后来**苹果公司为其产品mac、iPhone以及iPad的屏幕配置了Retina高清屏，也就是说这种屏幕拥有的物理像素点数比非高清屏多4倍甚至更多。如果还按照DPR=1进行展示，那么同一张图片在高清屏上面显示的区域面积会是非高清屏的1/4**，这样的话由于图片在屏幕上的展示面积大大缩小，也会导致出现“看不清”的问题。
苹果公司既然推出了Retina技术，那么这种技术带来了高清展示福利的前提下也要解决“DPR=1”的问题。怎么解决呢？DPR!=1。苹果公司经过一系列技术使用4个乃至更多物理像素来渲染1个逻辑像素，这样一来，同样的CSS代码设置的尺寸，在Retina和非Retina屏幕上看起来大小是一样的，但在Retina屏幕上要精细得多。

在Retian屏上，DPR不再是1，而是大于1，比如2（iPhone 5 6 7 8）或3（iPhone 6 Plus等一系列Plus）或者为非整数（一些Android机），说不定还会涨。

举个例子，iPhone 6的物理像素上面已经说了，是`750 * 1334`，那它的逻辑像素呢？我们只需在iPhone 6的Safari里打印一下**screen.width和screen.height就知道了，结果是 `375 * 667`**，这就是它的逻辑像素，据此很容易计算出DRP为2。当然，我们还可以直接通过window.devicePixelRatio这个值来获取DRP，打印结果是2，符合我们的预期。









### css像素的相对性

假设我们用PC浏览器打开一个页面，浏览器此时的宽度为800px，页面上同时有一个400px宽的块级元素容器。很明显此时块状容器应该占页面的一半。

但如果我们把页面放大（通过“Ctrl键”加上“+号键”），放大为200%，**视口变小** **从800px缩小为400px**也就是原来的两倍。此时块状容器则横向占满了整个浏览器。

诡异的是此时我们既没有调整浏览器窗口大小，也没有改变块状元素的css宽度，但是它看上去却变大了一倍——这是因为我们把CSS像素放大为了原来的两倍。

CSS像素与屏幕像素1：1同样大小时：

![](css3_img\css像素放大.png)

CSS像素(黑色边框)开始被拉伸，此时1个CSS像素大于1个屏幕像素

![](css3_img\css像素放大2.png)

就是说默认情况下一个CSS像素应该是等于一个物理像素的宽度的，但是浏览器的放大操作让一个CSS像素等于了两个设备像素宽度。

从上面的例子可以看出，CSS像素从来都只是一个相对值。

再举一个例子：

![](css3_img\相同屏幕下不同分辨率的像素体现.jpg)

因此，px虽然是绝对单位，但也是相对性而言的。







### ppi（dpi）像素密度

ppi 每英寸像素取值，更确切的说法应该是**像素密度**，也就是衡量单位物理面积内拥有像素值的情况。

计算公式

> √(横向分辨率^2+纵向分辨率^2)/英寸=ppi\dpi

比如iphone6的设备分辨为750*1334，屏幕大小为4.7英寸，则√(750^2+1334^2)/4.7 约等于**326**



`PPI` 说的是像素密度，而分辨率说的是块屏幕的像素尺寸，譬如说 1334*750 就是 iPhone（6~7）的分辨率，说 iPhone（6~7）的分辨率是 326 是错误的表述，326 是它的像素密度，单位是 `PPI`。

询问别人一粒像素有多大是一个非常鸡贼的问题（小心面试遇到这样的题），虽然我们说像素是构成屏幕的发光的点，是物理的，但是像素在脱离了屏幕尺寸之后是没有大小可言的，你可以将 1920 * 1080 颗像素放到一台 40 寸的小米电视机里面，也可以将同样多的像素全部塞到一台 5.5 寸的 iPhone7 Plus 手机里面去，那么对于 40 寸的电视而言，每个像素颗粒当然会大于 5.5 寸的手机的像素。

所以光看屏幕的分辨率对于设计师来说是不具备多少实际意义的，通过分辨率计算得出的像素密度（PPI）才是设计师要关心的问题，我们通过屏幕分辨率和屏幕尺寸就能计算出屏幕的像素密度的。

再次使用 iPhone（6~7）作为例子。我们知道该屏幕的横向物理尺寸为 2.3 英寸 ，且横向具有 750 颗像素，根据下面的公式，我们能够算出 iPhone（6~7）的屏幕是 326 PPI，意为每寸存在 326 颗像素。

其实不论我们怎么除，计算得出来的`像素密度（PPI）`都会是这个数，宽存在像素除以宽物理长度，高存在像素除以高物理长度，得数都接近于 326。









### rpx

微信小程序引入rpx（responsive pixel）这个新的尺寸单位

小程序编译后，rpx会做一次px换算。换算是以375个物理像素为基准，也就是在一个宽度为375物理像素的屏幕下，1rpx = 1px。

举个例子：iPhone6屏幕宽度为375px，共750个物理像素，那么1rpx = 375 / 750 px = 0.5px。







### em

在CSS 中，如果没有任何 CSS 规则影响的前提之下，通常情况下：

1em的长度是：1em = 16px = 0.17in = 12pt = 1pc = 4.mm = 0.42cm

```html
<body style=“font-size:1.5em”>

<h3 style=“font-size:1.5em”>哈哈哈<h3>

<body>
```

![](css3_img\em计算方式.jpg)

从上面的简单示例，我们可以得知，随着DOM 元素的嵌套加深，同时不同层级都显式设置font-size的值为em，那将会增加em计算和转换的复杂度。像这样：

![](css3_img\em的继承.jpg)

如果在非font-size的属性上使用em做为`<length>`值的单位时，将会受元素font-size的影响。在众多开发者中有一个比较普遍的语解，认为em单位是相对于父元素的font-size。而事实上呢？**它们是相对于使用em单位元素的font-size。父元素的font-size可以影响em值，但这种情况的发生，纯粹是因为继承。**

em单位除了应用于font-size属性之外，还可以运用于可以使用`<length>`值的其他属性，比如width、margin、padding、border-width和text-shadow等等。





### rem

rem相对于em而言没有那么复杂，他仅仅是相对于根元素<html>的font-size计算

任何值为1rem的元素都等于16px，当然，其前提是浏览器默认的font-size没有被用户重置，或者未显式的给html元素设置font-size值；另外，rem可以不管它的父元素的font-size如何！

![](css3_img\rem计算方式.jpg)

· rem和em在客户端中计算出来的样式值都会以**px显式**

· rem相对于根元素html的font-size计算，em相对于元素font-size计算

· rem可以从浏览器字体设置中继承font-size值，em可能受任何继承过来的父元素font-size的影响

· 使用em应该根据组件的font-size来定，而不是根元素的font-size来定

· 在不需要使用em单位，并且需要根据浏览器的font-size设置缩放时，应该使用rem











### ex 与 ch

ex和ch是排版单取决于元素的**font-family**，也就是说元素的font-family样式对ex和ch单位值的计算有直接关系和影响，因此，它们要求浏览器在计算值和应用样式之前要确定好引用的font-family，但是，这也是ex和ch单位比其它绝对单位更灵活的地方。

ex单位的值来自它们所计算的字体上下文的**x高度**,x高度由两个因素决定：**font-family和font-size**。换句话说，它们等于特定字体在特定font-size下的x高。实例如下图：

![](css3_img\x高度.jpg)



ch单位从字体的**0字形宽度**中提取它们的值，它还随字体而变化。如此一来，就有点随意，而0的宽度通常是对字体的平均字符宽度，这是一个估计值，所以会有点糟糕.实例如下：

![](css3_img\字符0高度.jpg)

ex和ch简单可以用下图理解：

![](css3_img\字符x和0.jpg)











## *🔳视窗单位*

视窗指的是浏览器的可视区域，而在移动端中相对来说更为复杂一些，它包括三个视窗：布局视窗（Layout Viewport）、视觉视窗（Visual Viewport）和理想视窗（Ideal Viewport）：

![](css3_img\视觉视窗.jpg)

而我们要说的视窗单位中的视窗指的是：PC 端指的是浏览器可视区域，移动端的是布局视窗（Layout Viewport）

![](css3_img\pc视窗和移动端视窗.jpg)

· **vw**：视窗宽度的百分比

· **vh**：视窗高度的百分比

· **vmin**：当前较小的vw和vh

· **vmax**：当前较大的vw和vh

![](css3_img\vw和vh.jpg)

简单的来看看视窗单位是如何进行计算的。例如，如果浏览器的高是900px,1vh求得的值为9px。同理，如果显示窗口宽度为750px,1vw求得的值为7.5px。vh和vw总是与视窗的高度和宽度有关，与之不同的，vmin和vmax是与视窗宽度和高度的最大值或最小值有关，取决于哪个更大和更小。例如，如果浏览器设置为1100px宽、700px高，1vmin会是7px,1vmax为11px。







### viewport 深入理解

在移动设备上进行网页的重构或开发，首先得搞明白的就是移动设备上的viewport了，只有明白了viewport的概念以及弄清楚了跟viewport有关的meta标签的使用，才能更好地让我们的网页适配或响应各种不同分辨率的移动设备。



**viewport 的概念**

通俗的讲，移动设备上的viewport就是设备的屏幕上能用来显示我们的网页的那一块区域（上文提到的视觉视窗），在具体一点，就是浏览器上(也可能是一个app中的webview)用来显示网页的那部分区域，但viewport又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域要大，也可能比浏览器的可视区域要小。在默认情况下，一般来讲，**移动设备上的viewport（上文提到布局视窗）都是要大于浏览器可视区域的**，这是因为考虑到移动设备的分辨率相对于桌面电脑来说都比较小，所以为了能在移动设备上正常显示那些传统的为桌面浏览器设计的网站，移动设备上的浏览器都会把自己默认的viewport设为980px或1024px（也可能是其它值，这个是由设备自己决定的），但带来的后果就是浏览器会出现横向滚动条，因为浏览器可视区域的宽度是比这个默认的viewport的宽度要小的。下图列出了一些设备上浏览器的默认viewport的宽度。

![](css3_img\浏览器默认viewport.png)

**而有些浏览器设置了默认viewport后，还会默认根据设置后的viewpor像素与设备物理像素的像素比，来决定1个css像素等于几个物理像素**，如图

![](css3_img\浏览器默认viewport2.png)

像素比为0.765306122448979591,即**1个css像素等于0.765306122448979591个物理像素**

![](css3_img\浏览器默认viewport3.png)

**这也就是为什么，手机上看着网页显小的原因，因为浏览器的默认行为会尽量把pc端的网页在移动端上完整显示**







**三个viewport**

[ppk大神](http://www.quirksmode.org/)对于移动设备上的viewport有着非常多的研究（[第一篇](http://www.quirksmode.org/mobile/viewports.html)，[第二篇](http://www.quirksmode.org/mobile/viewports2.html)，[第三篇](http://www.quirksmode.org/mobile/metaviewport/)），有兴趣的同学可以去看一下，本文中有很多数据和观点也是出自那里。ppk认为，移动设备上有三个viewport。

首先，移动设备上的浏览器认为自己必须能让所有的网站都正常显示，即使是那些不是为移动设备设计的网站。但如果以浏览器的可视区域作为viewport的话，因为移动设备的屏幕都不是很宽，所以那些为桌面浏览器设计的网站放到移动设备上显示时，必然会因为移动设备的viewport太窄，而挤作一团，甚至布局什么的都会乱掉。也许有人会问，现在不是有很多手机分辨率都非常大吗，比如768x1024，或者1080x1920这样，那这样的手机用来显示为桌面浏览器设计的网站是没问题的吧？前面我们已经说了，css中的1px并不是代表屏幕上的1px，你分辨率越大，css中1px代表的物理像素就会越多，devicePixelRatio的值也越大，这很好理解，因为你分辨率增大了，但屏幕尺寸并没有变大多少，必须让css中的1px代表更多的物理像素，才能让1px的东西在屏幕上的大小与那些低分辨率的设备差不多，不然就会因为太小而看不清。所以在1080x1920这样的设备上，在默认情况下，也许你只要把一个div的宽度设为300多px（视devicePixelRatio的值而定），就是满屏的宽度了。回到正题上来，如果把移动设备上浏览器的可视区域设为viewport的话，某些网站就会因为viewport太窄而显示错乱，所以这些浏览器就决定默认情况下把viewport设为一个较宽的值，比如980px，这样的话即使是那些为桌面设计的网站也能在移动浏览器上正常显示了。ppk把这个浏览器默认的viewport叫做 layout viewport。这个layout viewport的宽度可以通过 document.documentElement.clientWidth 来获取。

然而，layout viewport 的宽度是大于浏览器可视区域的宽度的，所以我们还需要一个viewport来代表 浏览器可视区域的大小，ppk把这个viewport叫做 visual viewport。visual viewport的宽度可以通过window.innerWidth 来获取，但在Android 2, Oprea mini 和 UC 8中无法正确获取。

![](css3_img\布局视窗.png)

![](css3_img\可视视窗.png)

现在我们已经有两个viewport了：layout viewport 和 visual viewport。但浏览器觉得还不够，因为现在越来越多的网站都会为移动设备进行单独的设计，所以必须还要有一个能完美适配移动设备的viewport。所谓的完美适配指的是，首先不需要用户缩放和横向滚动条就能正常的查看网站的所有内容；第二，显示的文字的大小是合适，比如一段14px大小的文字，不会因为在一个高密度像素的屏幕里显示得太小而无法看清，理想的情况是这段14px的文字无论是在何种密度屏幕，何种分辨率下，显示出来的大小都是差不多的。当然，不只是文字，其他元素像图片什么的也是这个道理。ppk把这个viewport叫做 ideal viewport，也就是第三个viewport——移动设备的理想viewport。

ideal viewport 并没有一个固定的尺寸，不同的设备拥有有不同的 ideal viewport。

![](css3_img\理想视窗.png)

安卓设备比较复杂,没有像iphone那么统一,有320px的，有360px的，有384px的等等，关于不同的设备ideal viewport的宽度都为多少，可以到 [http://blog.chengyunfeng.com/devices/](http://blog.chengyunfeng.com/devices/) 去查看一下，里面收集了众多设备的理想宽度。











### meta标签对viewport的控制

移动设备默认的viewport是layout viewport，也就是那个比屏幕要宽的viewport，但在进行移动设备网站的开发时，我们需要的是ideal viewport。那么怎么才能得到ideal viewport呢？这就该轮到meta标签出场了。

我们在开发移动设备的网站时，最常见的的一个动作就是把下面这个东西复制到我们的head标签中：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
```

该meta标签的作用是让当前viewport的宽度等于设备的宽度，同时不允许用户手动缩放。也许允不允许用户缩放不同的网站有不同的要求，但让viewport的宽度等于设备的宽度，这个应该是大家都想要的效果，如果你不这样的设定的话，那就会使用那个比屏幕宽的默认viewport，也就是说会出现横向滚动条。

这个name为viewport的meta标签到底有哪些东西呢，又都有什么作用呢？

meta viewport 标签首先是由苹果公司在其safari浏览器中引入的，目的就是解决移动设备的viewport问题。后来安卓以及各大浏览器厂商也都纷纷效仿，引入对meta viewport的支持，事实也证明这个东西还是非常有用的。

在苹果的规范中，meta viewport 有6个属性(暂且把content中的那些东西称为一个个属性和值)，如下：

| width         | 设置***layout viewport*** 的宽度，为一个正整数，或字符串"device-widht(理想宽度)" |
| ------------- | ------------------------------------------------------------ |
| initial-scale | 设置页面的初始缩放值，为一个数字，可以带小数                 |
| minimum-scale | 允许用户的最小缩放值，为一个数字，可以带小数                 |
| maximum-scale | 允许用户的最大缩放值，为一个数字，可以带小数                 |
| height        | 设置***layout viewport*** 的高度，这个属性对我们并不重要，很少使用 |
| user-scalable | 是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许 |

这些属性可以同时使用，也可以单独使用或混合使用，多个属性同时使用时用逗号隔开就行了。

此外，在安卓中还支持 target-densitydpi 这个私有属性，它表示目标设备的密度等级，作用是决定css中的1px代表多少物理像素

| target-densitydpi | 值可以为一个数值或 high-dpi 、 medium-dpi、 low-dpi、 device-dpi 这几个字符串中的一个 |
| ----------------- | ------------------------------------------------------------ |
|                   |                                                              |

特别说明的是，当 target-densitydpi=device-dpi 时， css中的1px会等于物理像素中的1px。

因为这个属性只有安卓支持，并且安卓已经决定要废弃 ~~target-densitydpi~~ 这个属性了，所以这个属性我们要避免进行使用 。





**把当前的viewport宽度设置为 ideal viewport 的宽度**

要得到ideal viewport就必须把默认的layout viewport的宽度设为移动设备的屏幕宽度。因为meta viewport中的width能控制layout viewport的宽度，所以我们只需要把width设为**width-device**这个特殊的值就行了。

```html
<meta name="viewport" content="width=device-width">
```

下图是这句代码在各大移动端浏览器上的测试结果：

![](css3_img\device-width兼容性.png)

可以看到通过width=device-width，所有浏览器都能把当前的viewport宽度变成ideal viewport的宽度，但要注意的是，在iphone和ipad上，无论是竖屏还是横屏，宽度都是竖屏时ideal viewport的宽度。

这样的写法看起来谁都会做，没吃过猪肉，谁还没见过猪跑啊~，确实，我们在开发移动设备上的网页时，不管你明不明白什么是viewport，可能你只需要这么一句代码就够了。

可能你不知道

```html
<meta name="viewport" content="initial-scale=1">
```

这句代码也能达到和前一句代码一样的效果，也可以把当前的的viewport变为 ideal viewport。

呵呵，傻眼了吧，因为从理论上来讲，这句代码的作用只是不对当前的页面进行缩放，也就是页面本该是多大就是多大。那为什么会有 width=device-width 的效果呢？

要想清楚这件事情，首先你得弄明白这个缩放是相对于什么来缩放的，因为这里的缩放值是1，也就是没缩放，但却达到了 ideal viewport 的效果，所以，那答案就只有一个了，**缩放是相对于 ideal viewport来进行缩放的**，当对ideal viewport进行100%的缩放，也就是缩放值为1的时候，不就得到了 ideal viewport吗？事实证明，的确是这样的。下图是各大移动端的浏览器当设置了`<meta name="viewport" content="initial-scale=1">` 后是否能把当前的viewport宽度变成 ideal viewport 的宽度的测试结果。

![](css3_img\initail-scale兼容性.png)

测试结果表明 initial-scale=1 也能把当前的viewport宽度变成 ideal viewport 的宽度，但这次轮到了windows phone 上的IE 无论是竖屏还是横屏都把宽度设为竖屏时ideal viewport的宽度。但这点小瑕疵已经无关紧要了。

但如果 width 和 initial-scale=1 同时出现，并且还出现了冲突呢？比如：

```html
<meta name="viewport" content="width=400, initial-scale=1">
```

width=400表示把当前viewport的宽度设为400px，initial-scale=1则表示把当前viewport的宽度设为ideal viewport的宽度，那么浏览器到底该服从哪个命令呢？是书写顺序在后面的那个吗？不是。当遇到这种情况时，浏览器会取它们两个中较大的那个值。例如，当width=400，ideal viewport的宽度为320时，取的是400；当width=400， ideal viewport的宽度为480时，取的是ideal viewport的宽度。（ps:在uc9浏览器中，当initial-scale=1时，无论width属性的值为多少，此时viewport的宽度永远都是ideal viewport的宽度）

最后，总结一下，要把当前的viewport宽度设为ideal viewport的宽度，既可以设置 width=device-width，也可以设置 initial-scale=1，但这两者各有一个小缺陷，就是iphone、ipad以及IE 会横竖屏不分，通通以竖屏的ideal viewport宽度为准。所以，最完美的写法应该是，两者都写上去，这样就 initial-scale=1 解决了 iphone、ipad的毛病，width=device-width则解决了IE的毛病：

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```









### 关于meta viewport的更多知识



#### 关于缩放以及initial-scale的默认值



首先我们先来讨论一下缩放的问题，前面已经提到过，缩放是相对于ideal viewport来缩放的，缩放值越大，当前viewport的宽度就会越小，反之亦然。例如在iphone中，ideal viewport的宽度是320px，如果我们设置 initial-scale=2 ，此时viewport的宽度会变为只有160px了，这也好理解，放大了一倍嘛，就是原来1px的东西变成2px了，但是1px变为2px并不是把原来的320px变为640px了，而是在实际宽度不变的情况下，1px变得跟原来的2px的长度一样了，所以放大2倍后原来需要320px才能填满的宽度现在只需要160px就做到了。因此，我们可以得出一个公式：

> visual viewport宽度 = ideal viewport宽度  / 当前缩放值
>
> 当前缩放值 = ideal viewport宽度  / visual viewport宽度

**ps:** visual viewport的宽度指的是浏览器可视区域的宽度。

大多数浏览器都符合这个理论，但是安卓上的原生浏览器以及IE有些问题。安卓自带的webkit浏览器只有在 initial-scale = 1 以及没有设置width属性时才是表现正常的，也就相当于这理论在它身上基本没用；而IE则根本不甩initial-scale这个属性，无论你给他设置什么，initial-scale表现出来的效果永远是1。

好了，现在再来说下initial-scale的默认值问题，就是不写这个属性的时候，它的默认值会是多少呢？很显然不会是1，因为当 initial-scale = 1 时，当前的layout viewport宽度会被设为 ideal viewport的宽度，但前面说了，各浏览器默认的 layout viewport宽度一般都是980啊，1024啊，800啊等等这些个值，没有一开始就是 ideal viewport的宽度的，所以 initial-scale的默认值肯定不是1。安卓设备上的initial-scale默认值好像没有方法能够得到，或者就是干脆它就没有默认值，一定要你显示的写出来这个东西才会起作用，我们不管它了，这里我们重点说一下iphone和ipad上的initial-scale默认值。

根据测试，我们可以在iphone和ipad上得到一个结论，就是无论你给layout viewpor设置的宽度是多少，而又没有指定初始的缩放值的话，那么iphone和ipad会自动计算initial-scale这个值，以保证当前layout viewport的宽度在缩放后就是浏览器可视区域的宽度，也就是说不会出现横向滚动条。比如说，在iphone上，我们不设置任何的viewport meta标签，此时layout viewport的宽度为980px，但我们可以看到浏览器并没有出现横向滚动条，浏览器默认的把页面缩小了。根据上面的公式，**当前缩放值 = ideal viewport宽度 / visual viewport宽度**，我们可以得出：

```
当前缩放值 = 320 / 980
```

也就是当前的initial-scale默认值应该是 0.33这样子。当你指定了initial-scale的值后，这个默认值就不起作用了。

总之记住这个结论就行了：在iphone和ipad上，无论你给viewport设的宽的是多少，如果没有指定默认的缩放值，则iphone和ipad会自动计算这个缩放值，以达到当前页面不会出现横向滚动条(或者说viewport的宽度就是屏幕的宽度)的目的。

![](css3_img\intail-scale默认值的影响.png)

![](css3_img\intail-scale默认值的影响2.png)

![](css3_img\intail-scale默认值的影响3.png)











#### 动态设置meta viewport标签

**第一种方法**

可以使用document.write来动态输出meta viewport标签，例如：

```javascript
document.write('<meta name="viewport" content="width=device-width,initial-scale=1">')
```

**第二种方法**

通过setAttribute来改变

```html
<meta id="testViewport" name="viewport" content="width = 380">
<script>
var mvp = document.getElementById('testViewport');
mvp.setAttribute('content','width=480');
</script>
```







### viewport 总结

说了那么多废话，最后还是有必要总结一点有用的出来。

首先如果不设置meta viewport标签，那么移动设备上浏览器默认的宽度值为800px，980px，1024px等这些，总之是大于屏幕宽度的。这里的宽度所用的单位px都是指css中的px，它跟代表实际屏幕物理像素的px不是一回事。

第二、每个移动设备浏览器中都有一个理想的宽度，这个理想的宽度是指css中的宽度，跟设备的物理宽度没有关系，在css中，这个宽度就相当于100%的所代表的那个宽度。我们可以用meta标签把viewport的宽度设为那个理想的宽度，如果不知道这个设备的理想宽度是多少，那么用device-width这个特殊值就行了，同时initial-scale=1也有把viewport的宽度设为理想宽度的作用。所以，我们可以使用

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

来得到一个理想的viewport（也就是前面说的ideal viewport）。

为什么需要有理想的viewport呢？比如一个分辨率为320x480的手机理想viewport的宽度是320px，而另一个屏幕尺寸相同但分辨率为640x960的手机的理想viewport宽度也是为320px，那为什么分辨率大的这个手机的理想宽度要跟分辨率小的那个手机的理想宽度一样呢？这是因为，只有这样才能保证同样的网站在不同分辨率的设备上看起来都是一样或差不多的。实际上，现在市面上虽然有那么多不同种类不同品牌不同分辨率的手机，但它们的理想viewport宽度归纳起来无非也就 320、360、384、400等几种，都是非常接近的，理想宽度的相近也就意味着我们针对某个设备的理想viewport而做出的网站，在其他设备上的表现也不会相差非常多甚至是表现一样的。













### vw 单位适配方案

 不同的设备device-width大小是不一样的
            iphone6 -- 375
            iphone6plus -- 414

由于不同设备视口和像素比不同，所以同样的375个像素在不同的设备下意义是不一样，比如在iphone6中 375就是全屏宽度，而到了plus中375就会缺一块，所以在移动端开发时，就不能再使用px来进行布局了

![](css3_img\px在移动端的设计缺陷.png)

​    vw 表示的是视口的宽度（viewport width）

​	100vw = 一个视口的宽度
​	 1vw = 1%视口宽度

​        vw这个单位永远相当于视口宽度进行计算

​       假设设计图的宽高为
​            750px 1125px

> ​        设计图 
> ​            750px  
>
> ​        使用vw作为单位
> ​            100vw

​        设计图种有一个 48px * 35px 大小的元素

​		在css中设置这个元素大小时就要进行换算，这是一个非常繁琐的操作

```
100vw = 750px(设计图的像素) 0.1333333333333333vw = 1px
6.4vw = 48px(设计图像素)
4.667vw = 35px
```



可以通过下面的方式简化换算

```html
<style>
        *{
            margin: 0;
            padding: 0;
        }

        html{
            /* 
                网页中字体大小最小是12px，不能设置一个比12像素还小的字体
                    如果我们设置了一个小于12px的字体，则字体自动设置为12

                0.1333333vw = 1px

                5.3333vw = 40px
            */
            font-size: 5.3333vw;
        }

        .box1{
            /* 
                rem
                    - 1 rem = 1 html的字体大小
                    - 1 rem = 40 px(设计图)
            */
            width: 1.2rem;
            height: 0.875rem;
            background-color: #bfa;
        }
    </style>
</head>
<body>

    <!-- 
        48 x 35
     -->
     <div class="box1"></div>
    
</body>
```













## *🔳百分比值*

CSS 中百分比%单位也是一个很重要也是很常用的单位，和px、em类似，在CSS 中接受·<length>·值的属性都可以使用%单位。**百分比是一定要有其对应的参照值，也就是说，百分比值是一种相对值**，任何时候要分析它的计算值，都需要正确的找到它的参照值。言外之意，CSS中的百分比单值最终计算出来的值是**可变的**

一个css属性值从定义到最终实际使用，是存在一个过程的。这其中涉及到Specified Values（指定值）、Computed Values（计算值）、 Used Values（使用值）、Actual Values（实际值）等概念，可以想见到，百分比值实际会在这个过程中，根据它的参照计算转化为一个绝对值（比如`100px`），然后再被应用。这就是百分比值的意义。





#### 百分比值的作用

简单地说，就是可变性。这可以衍生出自适应、响应式等看起来很有用的东西。

比如说，一个固定宽高的盒子，然后希望盒子内有一个绝对定位的，宽高和盒子一样的盖板（就这样称呼吧...），下面这样的写法会很合适：

```css
.box{position:relative;width:100px;height:100px;}
.box_cover{position:absolute;width:100%;height:100%;left:0;top:0;}
```

这里使用百分比值的好处的是，如果需要修改盒子的尺寸，只需要修改盒子的宽高，而盖板会自动保持和盒子的尺寸一致。

再一个例子是[Bootstrap](http://getbootstrap.com/)的栅格系统：

![](css3_img\bootstrap栅格.png)

可以看到，栅格系统里会用到百分比值来实现确切的对空间的划分。百分比值是相对的，自适应的，因此栅格系统可以很好地用于响应式设计。







#### width && height

宽和高在使用百分比值时，其参照都是元素的包含块（Containing Block，[详情](http://www.w3help.org/zh-cn/kb/008/)）。`width`参照包含块的宽度，`height`参照包含块的高度。在大部分情况下，包含块就是父元素的内容区（盒模型里的content）。

· height、min/max-height属性的值为百分比时，其相对于包含块的height进行计算

· width、min/max-width属性的值为百分比时，其相对于包含块的width进行计算

很多人写过`width:100%; height:100%;`这样的代码来实现尺寸和父元素一致。但我发现有时候宽度是符合意思（100%）的，但高度却没有效果。请看下面这个示例：

![](css3_img\height100%参照.png)

可以看到，直接父元素（包含块）是否有明确的高度定义，会影响`height`为百分比值时的结果。

关于这一点的详细解释是，**当一个元素的高度使用百分比值，如果其包含块没有明确的高度定义（也就是说，取决于内容高度），且这个元素不是绝对定位，则该百分比值等同于`auto`**。`auto`是初始默认值，所以看起来就像是“失效”了。

如果元素是根元素（`<html>`），它的包含块是视口（viewport）提供的初始包含块（initial containing block），初始包含块任何时候都被认为是有高度定义的，且等于视口高度。所以，`<html>`标签的高度定义百分比总是有效的，而如果你希望在`<body>`里也用高度百分比，就一定要先为`<html>`定义明确的高度。这就是为什么在[固定页脚](http://acgtofe.com/posts/2013/03/sticky-footer/)一文中，有`html, body{height:100%;}`这样的写法。









#### margin && padding

· padding和margin相对来说更为复杂一些，如果书写模式是水平的，则相对于包含块的width进行计算；如果书写模式是垂直的，则相对于包含块的height进行计算

![](css3_img\margin-padding参照.png)

可以分析得到，**对于`margin`和`padding`，其任意方向的百分比值，参照都是包含块的宽度**。

为什么会多个方向都取包含块的宽度作为参照呢？在我看来，包含块的宽度在块布局的排版中是最有用的（想象一下word里输入文字，到宽度边缘后换行的场景），对应的，水平方向的内外边距一定要参照包含块的宽度。再考虑垂直方向的内外边距，它们如果不和水平方向取相同的参照物，就会因为不一致而很难使用。所以，总体来说，统一以包含块的宽度作为参考，会具有相对最好的可用性。

严格地说，参照是包含块的宽度，是在样式属性`writing-mode`为默认值时的情况。不过这个属性极少被用到，所以在此不做考虑。









#### border-radius

在CSS 中border-width属性是不支持%单位的，但在border-radius和border-image-width两个属性上是可以使用百分比为单位的。如果在border-radius中使用百分比单位，也就是说圆角的半径是通过百分比来进行计算的，即：水平方向的半径是相对于元素width计算，垂直方向的半径是相对于元素高度进行计算

![](css3_img\border-radius参照.jpg)





#### background-position

`background-position`的初始值就是百分比值`0% 0%`。

对于background-position中的百分比，相对而言更为复杂一些，需要通过一些数学公式计算：

> （容器尺寸- 背景图像尺寸）* 百分比值：

![](css3_img\background-position参照.jpg)







#### font-size

参照是直接父元素的`font-size`。例如，一个元素的直接父元素的`font-size`是`14px`，无论这个是直接定义的，还是继承得到的，当该元素定义`font-size:100%;`，获得的效果就是`font-size:14px;`。



#### line-height

参照是元素**自身**的`font-size`。例如，一个元素的`font-size`是`12px`，那么`line-height:150%;`的效果是`line-height:18px;`。



#### vertical-align

参照是元素自身的`line-height`（和前面很有关联吧，所以我排在了这里）。例如，一个元素的`line-height`是`30px`，则`vertical-align:10%;`的效果是`vertical-align:3px;`。

对这个属性我还想说，尽管`vertical-align`可以使用数字，百分比值，但浏览器兼容性差异较大，在跨浏览器实现时可能需要较多hack。因此，我个人倾向于使用`middle`等相对来说兼容性差异较小的关键字类型的值。







#### 定位

在CSS 中用来控制position位置的top、right、bottom和left都可以使用百分比作为单位。如果它们的值为百分比时，其对应的参照物是包含块（但不一定是其父容器）同方向的width或height计算。刚才提到过，定位属性中的参照物：包含块并不一定是其父容器。为什么这么说呢？因为在CSS 中position对应的属性值不一样，其对应的包含块也将不同：

· 如果元素为静态（static）或相对定位（relative），包含块一般是其父容器

· 如果元素为绝对定位（absolute），包含块应该是离它最近的position为absolute、relative或fixed的祖先元素

· 如果元素为固定定位（fixed），包含块就是视窗（viewport）





#### 百分比值的继承

当百分比值用于可继承属性时，只有结合参照值计算后的绝对值会被继承，而不是百分比值本身。例如，一个元素的font-size是14px，并定义了line-height:150%;，那么该元素的下一级子元素继承到的line-height就是21px，而不会再和子元素自己的font-size有关。







#### transform && translate

· translateX()的百分比相对于容器的width计算

· translateY()的百分比相对于容器的height计算

· transform-origin中横坐标（x）相对于容器的width计算；纵坐标（y）相对于容器的height计算

在translate还有一个z轴的函数translateZ()。它是不接受百分比为单位的值。





















# 💥css3 动画



CSS3动画相关的几个属性是：**`transition`, `transform`, `animation`**；分别理解为过渡，变换，动画。虽意义相近，但具体角色不一。就像是SHE组合，虽然都是三个女生，都唱同一首歌，但有负责高音，和擅长低音的，具体工作于角色还是有差异的。

`transition`指过渡啦，就是从a点都b点，就像过江坐渡轮，是有时间的，是连续的，一般针对常规CSS属性；`transform`指变换，就那几个固定的属性：旋转啦，缩放啦，偏移啦什么的，常独立于远房亲戚`transition`使用，但是，效果就是很干涩机械的旋转移动。要是配合transition属性，旋转啊什么的，就会很平滑。`animation`最先安家于Safari浏览器，自成一家，与transition和transform有老死不相往来之感，但是要说单挑的话，`animation`要比`transition`厉害些。







## *🔳transition （过渡）*

transition: **平滑的改变CSS的值**。无论是点击事件，焦点事件，还是鼠标`hover`，只要值改变了，就是平滑的，就是动画。于是乎，只要一个整站通用的`class`，就可以很轻松的渐进增强地实现动画效果。

例如下面这个很简单的例子：

<iframe src="html\过渡动画.html" style="height:100px;"></iframe>

CSS3 过渡是元素从一种样式逐渐改变为另一种的效果。

要实现这一点，必须规定两项内容：

- 指定要添加效果的CSS属性
- 指定效果的持续时间。

> 语法:transition: *property duration timing-function delay*;

```css
div
{
    transition: width 2s;
    -webkit-transition: width 2s; /* Safari */
}
```



**指定多项样式改变**

要添加多个样式的变换效果，添加的属性由逗号分隔：

```css
div
{
    transition: width 2s, height 2s, transform 2s;
    -webkit-transition: width 2s, height 2s, -webkit-transform 2s;
}
```



**指定全部样式改变**

要添加全部样式的变换效果，全部属性由all表示：

```css
div
{
    transition: all 2s;
    -webkit-transition: all 2s; /* Safari */
}
```



**属性值参考**

`transiton`属性是下面几个属性的缩写：

- transition-property

  指定过渡的属性值，比如`transition-property:opacity`就是只指定`opacity`属性参与这个过渡。

- transition-duration

  指定这个过渡的持续时间

- transition-delay

  延迟过渡时间

- transition-timing-function

  指定过渡动画缓动类型，有`ease` | `linear` | `ease-in` | `ease-out` | `ease-in-out` | `cubic-bezier()`其中，`linear`线性过度，`ease-in`由慢到快，`ease-out`由快到慢，`ease-in-out`由慢到快在到慢。



**transition**

| 属性                                                         | 描述                                         | CSS  |
| :----------------------------------------------------------- | :------------------------------------------- | :--- |
| [transition](https://www.runoob.com/cssref/css3-pr-transition.html) | 简写属性，用于在一个属性中设置四个过渡属性。 | 3    |
| [transition-property](https://www.runoob.com/cssref/css3-pr-transition-property.html) | 规定应用过渡的 CSS 属性的名称。              | 3    |
| [transition-duration](https://www.runoob.com/cssref/css3-pr-transition-duration.html) | 定义过渡效果花费的时间。默认是 0。           | 3    |
| [transition-timing-function](https://www.runoob.com/cssref/css3-pr-transition-timing-function.html) | 规定过渡效果的时间曲线。默认是 "ease"。      | 3    |
| [transition-delay](https://www.runoob.com/cssref/css3-pr-transition-delay.html) | 规定过渡效果何时开始。默认是 0。             | 3    |



**transition-property**

| 值         | 描述                                                  |
| :--------- | :---------------------------------------------------- |
| none       | 没有属性会获得过渡效果。                              |
| all        | 所有属性都将获得过渡效果。                            |
| *property* | 定义应用过渡效果的 CSS 属性名称列表，列表以逗号分隔。 |





**transition-duration**

| 值     | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| *time* | 规定完成过渡效果需要花费的时间（以秒或毫秒计）。 默认值是 0，意味着不会有效果。 |

**注意：** 如果未指定的期限，transition将没有任何效果，因为默认值是0。





**transition-delay**

| 值     | 描述                                 |
| :----- | :----------------------------------- |
| *time* | 指定秒或毫秒数之前要等待切换效果开始 |





**transition-timing-function**

| 值                            | 描述                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| linear                        | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。 |
| ease                          | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。 |
| ease-in                       | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。  |
| ease-out                      | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。  |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 |

`transition`中的`transition-timing-function`属性让人心存芥蒂，其一堆`ease`相关的值（`linear` | `ease-in` | `ease-out` | `ease-in-out` | `cubic-bezier`），不太容易让人理解与记住。尤其其中cubic-bezier就是指[贝塞尔曲线](http://baike.baidu.com/view/60154.htm)，与复杂的数学扯上的关系，不禁勾起了高中时数学的梦魇。

其实呢，理一理，也还好。首先`cubic-bezier`这个基本上就不用鸟了，90%+的情况都用不到这个东西，所以，难得清闲，直接pass掉。`linear`很好记，线性嘛。至于`ease-in` | `ease-out` | `ease-in-out`，就是指缓动效果啦，说白了就是指开始时候慢慢动呢还是结束的时候慢慢动。那么`in`和`out`那个先慢慢动呢？啊，我们可以联想记忆，很好记的。我们都知道OOXX吧，`ease-in`中的`in`就表示进入，进入的时候显然一开始都是慢的，等瞄准就绪后才能快速冲刺进入，于是`ease-in`表示先慢后快；`ease-out`其`out`表示出来，出来肯定是先快后慢的，因为出来时临近洞口速度肯定要降下来，免得跑出来乱了节奏，于是`ease-out`表示先快后慢；最后，很好理解的，`ease-in-out`表示一进一出，也就是先慢后快再慢。

<iframe src="html\动画过渡缓动类型.html"></iframe>

![](css3_img\过渡缓动.png)



















## *🔳transform 2D（变换）*

`transform`指变换，使用过photoshop的人应该知道里面的Ctrl+T自由变换。`transform`就是指的这个东西，拉伸，压缩，旋转，偏移。见下面示例代码：

```css
.trans_skew { transform: skew(35deg); }
.trans_scale { transform:scale(1, 0.5); }
.trans_rotate { transform:rotate(45deg); }
.trans_translate { transform:translate(10px, 20px); }
```



2D变换方法：

- translate()
- rotate()
- scale()
- skew()
- matrix()

| 函数                            | 描述                                     |
| :------------------------------ | :--------------------------------------- |
| matrix(*n*,*n*,*n*,*n*,*n*,*n*) | 定义 2D 转换，使用六个值的矩阵。         |
| translate(*x*,*y*)              | 定义 2D 转换，沿着 X 和 Y 轴移动元素。   |
| translateX(*n*)                 | 定义 2D 转换，沿着 X 轴移动元素。        |
| translateY(*n*)                 | 定义 2D 转换，沿着 Y 轴移动元素。        |
| scale(*x*,*y*)                  | 定义 2D 缩放转换，改变元素的宽度和高度。 |
| scaleX(*n*)                     | 定义 2D 缩放转换，改变元素的宽度。       |
| scaleY(*n*)                     | 定义 2D 缩放转换，改变元素的高度。       |
| rotate(*angle*)                 | 定义 2D 旋转，在参数中规定角度。         |
| skew(*x-angle*,*y-angle*)       | 定义 2D 倾斜转换，沿着 X 和 Y 轴。       |
| skewX(*angle*)                  | 定义 2D 倾斜转换，沿着 X 轴。            |
| skewY(*angle*)                  | 定义 2D 倾斜转换，沿着 Y 轴。            |





### translate() 偏移

![](css3_img\translate偏移.gif)

```css
div
{
transform: translate(50px,100px);
-ms-transform: translate(50px,100px); /* IE 9 */
-webkit-transform: translate(50px,100px); /* Safari and Chrome */
}
```

translate值（50px，100px）是从左边元素移动50个像素，并从顶部移动100像素，**如果只写第一个值，第二个值默认为0**，允许负值



**单值设置**

| translateX(*n*) | 定义 2D 转换，沿着 X 轴移动元素。 |
| --------------- | --------------------------------- |
| translateY(*n*) | 定义 2D 转换，沿着 Y 轴移动元素。 |









### rotate() 旋转

![](css3_img\rotate旋转.gif)

rotate()方法，在一个给定度数**顺时针旋转**的元素。**负值是允许的，这样是元素逆时针旋转。**

````css
div
{
transform: rotate(30deg);
-ms-transform: rotate(30deg); /* IE 9 */
-webkit-transform: rotate(30deg); /* Safari and Chrome */
}
````

rotate值（30deg）元素顺时针旋转30度。







### scale() 缩放

![](css3_img\scale缩放.gif)

scale()方法，该元素增加或减少的大小，取决于宽度（X轴）和高度（Y轴）的参数：

```css
-ms-transform:scale(2,3); /* IE 9 */
-webkit-transform: scale(2,3); /* Safari */
transform: scale(2,3); /* 标准语法 */
```

scale（2,3）转变宽度为原来的大小的2倍，和其原始大小3倍的高度。**如果只写第一个值，将同时设置水平方向和垂直方向的大小**，允许负值



**单值设置**

| scaleX(*n*) | 定义 2D 缩放转换，改变元素的宽度。 |
| ----------- | ---------------------------------- |
| scaleY(*n*) | 定义 2D 缩放转换，改变元素的高度。 |











### skew() 斜拉

 ![](css3_img\skew拉伸.png)

skew包含两个参数值，分别表示X轴和Y轴倾斜的角度，**如果第二个参数为空，则默认为0**，参数为负表示向相反方向倾斜。

- skewX(`<angle>`);表示只在X轴(水平方向)倾斜。
- skewY(`<angle>`);表示只在Y轴(垂直方向)倾斜。

````css
div
{
transform: skew(30deg,20deg);
-ms-transform: skew(30deg,20deg); /* IE 9 */
-webkit-transform: skew(30deg,20deg); /* Safari and Chrome */
}
````



**单值设置**

| skewX(*angle*) | 定义 2D 倾斜转换，沿着 X 轴。 |
| -------------- | ----------------------------- |
| skewY(*angle*) | 定义 2D 倾斜转换，沿着 Y 轴。 |



**分析**

对于初学者，可能一时半会看不出skewX()方法的本质原理。其实skewX()方法变形原理是这样的：由于我给元素限定了高度为100px，而skewX()方法是沿着X轴方向倾斜。所以，只要倾斜角度不是180°，元素都会保持100px的高度。同时为了保持倾斜，只能沿着X轴拉长本身。

- （1）skewX()方法会保持高度，沿着X轴倾斜；

  ```css
  div{
      transform: skew(30deg);
  }
  ```

  ![](css3_img\skew拉伸2.png)

- （2）skewY()方法会保持宽度，沿着Y轴倾斜；

  ```css
  div{
      transform: skew(0,30deg);
  }
  ```

  ![](css3_img\skew拉伸3.png)

- （3）skew(x,y)方法会先按照skewX()方法倾斜，然后按照skewY()方法倾斜；









### transform对元素的影响

一个普普通通的元素，如果应用了CSS3 transform变换，即便这个transform属性值不会改变其任何表面的变化（如`scale(1)`, `translate(0,0)`），但是，实际上，对这些元素还是造成了写深远的影响。



#### tranform 让元素拥有层叠上下文

我们可能都知道这样一个规则，当遭遇元素`margin`负值重叠的时候，如果没有static以外的position属性值的话，后面的元素是会覆盖前面的元素的。例如下面，后面的妹子覆盖了前面的妹子

![](css3_img\tranform的影响.png)

```html
<img src="mm1"><img src="mm2" style="margin-left:-60px;">
```

在`transform`出现之前，这个规则一直很稳健；但是，自从`transform`降临，这个规则就变了。元素应用了`transform`属性之后，就会变得应用了`position:relative`一个尿性，原本应该被覆盖的元素会雄起，变成覆盖其他元素，修改为如下代码：

```html
<img src="mm1" style="-ms-transform:scale(1);transform:scale(1);"><img src="mm2" style="margin-left:-60px;">
```

![](css3_img\tranform2的影响.png)







#### transform 使position:fixed降级

我们应该都知道，`position:fixed`可以让元素不跟随浏览器的滚动条滚动，而且这种跟随效果连它的兄弟们`position:relative/absolute`都限制不了。但是，真是一物降一物，`position:fixed`固定效果却被小小的`transform`给干掉了，直接降级变成`position:absolute`的蛋疼表现。

例如下面示意代码：

```html
<p style="transform:scale(1);"><img src="mm1.jpg"style="position:fixed;" /></p>
```

结果，本来应该不跟着滚动条滚动的傲娇`fixed`元素，变成`absolute`一样的行为表现，比方说一个元素其`position`属性值1000%是`fixed`，但是，却大失所望跟着滚动条混了，归根结底就是父元素加了个小小的`transform`属性值。











#### tranform 改变 overflow对 absolute元素的限制

在以前，`overflow`与`absolute`之间的限制规范内容大致是这样的：

> absolute绝对定位元素，如果含有overflow不为visible的父级元素，同时，该父级元素以及到该绝对定位元素之间任何嵌套元素都没有position为非static属性的声明，则overflow对该absolute元素不起作用。

比方说如下示意代码：

```html
<p style="width:96px; height:96px; border:2px solid #beceeb; overflow:hidden;">
    <img src="mm1.jpg"style="position:absolute;" />
</p>
```

结果是这样子，图片溢出的右侧还是可见的。

![](css3_img\tranform3的影响.png)

但是，一旦我们给`overflow`容器或者与图片有嵌套关系的子元素使用`transform`声明，呵呵呵，估计`absolute`元素就要去领便当了！

比方说，下面这个嵌套一层`block`水平标签应用`transform`属性后的效果：

![](css3_img\tranform4的影响.png)

Chrome浏览器已经调整了渲染表现，已经和IE9+/Firefox浏览器保持了一致，也就是“无论是overflow容器还是嵌套子元素，只要有`transform`属性，就会`hidden`溢出的`absolute`元素。”







#### tranform 限制对 absolute元素的 100% 宽度

以前，我们设置`absolute`元素宽度100%, 则都会参照第一个非`static`值的`position`祖先元素计算，没有就`window`. 现在，诸位，需要把`transform`也考虑在内了。

![](css3_img\tranform5的影响.png)

结果，无论是IE9+，还是Chrome还是FireFox浏览器，所有绝对定位图片`100%`宽度，都是相对设置了`transform`的容器计算了，于是，上面的图片拉长到了西伯利亚；下面的图片被限制成了小胖墩。















## *🔳transform 3D（变换）*



### 3D变换坐标系

既然是3D变换，自然要在三维空间中进行，二维空间坐标系仅有x轴与y轴。

三为空间则多出了一个z轴，简单图示如下：

他跟我们平常的 x 和 y 不太一样，因为， 他是倒着的。

![](css3_img\2d坐标.png)

![](css3_img\3d坐标.png)

图示简单说明如下：

（1）.默认状态下，X轴是水平的，左侧为正。

（2）.默认状态下，y轴是垂直的，向下为正。

（3）.默认状态下，z轴垂直于x轴和y轴形成的平面，正对着我们的方向为正。



**3D效果的产生：**

现实生活中，我们所处的环境毫无疑问是3D立体空间。

然而要展示我们3D效果的显示器却是二维的，所以做到视觉上的3D效果是关键所在。

首先通过三个简单的图片感受一下3D变换效果。

图示简单说明如下：

（1）.虚线是X轴，红色元素围绕x轴旋转。

（2）.离我们越近的部分也大，反之越小，在视觉上符合3D效果。

![](css3_img\x轴旋转.png)



图示简单说明如下：

（1）.虚线是y轴，红色元素围绕y轴旋转。

（2）.同样，离我们越近的部分也大，反之越小，在视觉上符合3D效果。

![](css3_img\y轴旋转.png)



图示简单说明如下：

（1）.黄色点是垂直的z轴，所以我们只能看到一个点。

（2）.红色的元素围绕z轴旋转，元素在视觉上不会产生变形

![](css3_img\z轴旋转.png)

很简单，通过改变元素响应地方的尺寸就产生了视觉效果，距离我们近的就大一点，远的就小一点。

不过这一切都是依据透视原理，下面透视**小节**专门做一下介绍。





**3D 转换方法**

| 函数                          | 描述                                      |
| :---------------------------- | :---------------------------------------- |
| translate3d(*x*,*y*,*z*)      | 定义 3D 转化。                            |
| translateX(*x*)               | 定义 3D 转化，仅使用用于 X 轴的值。       |
| translateY(*y*)               | 定义 3D 转化，仅使用用于 Y 轴的值。       |
| translateZ(*z*)               | 定义 3D 转化，仅使用用于 Z 轴的值。       |
| scale3d(*x*,*y*,*z*)          | 定义 3D 缩放转换。                        |
| scaleX(*x*)                   | 定义 3D 缩放转换，通过给定一个 X 轴的值。 |
| scaleY(*y*)                   | 定义 3D 缩放转换，通过给定一个 Y 轴的值。 |
| scaleZ(*z*)                   | 定义 3D 缩放转换，通过给定一个 Z 轴的值。 |
| rotate3d(*x*,*y*,*z*,*angle*) | 定义 3D 旋转。                            |
| rotateX(*angle*)              | 定义沿 X 轴的 3D 旋转。                   |
| rotateY(*angle*)              | 定义沿 Y 轴的 3D 旋转。                   |
| rotateZ(*angle*)              | 定义沿 Z 轴的 3D 旋转。                   |
| perspective(*n*)              | 定义 3D 转换元素的透视视图。              |







### rotateX\Y\Z() 旋转



以3d的角度再认识一下rotate 

rotate:旋转该元素，配合着**transform-origin**属性，transform-origin 是设置旋转点的。(没有设置transform-origin 属性也可以，只不过是根据该元素的中心点旋转，也就是center center)

![](css3_img\旋转动图.gif)

加上 transform-origin 设置旋转点。transform-origin 是根据自己而定位的，所以 0px 0px 就是左上角那个点。

![](css3_img\旋转动图2.gif)







**rotateX\Y\Z()**

rotateX\Y\Z()**)方法，围绕其在一个给定度数X轴旋转的元素。

```css
div
{
    transform: rotateX\Y\Z(120deg);
    -webkit-transform: rotateX\Y\Z(120deg); /* Safari 与 Chrome */
}
```

rotateX 的X呢，可以写成大写的，也可以写成小写的x， 没有影响，这个属性呢，你加上rotateX 之后，这个元素，就会以 X 轴 旋转，里面填的是角度。

![](css3_img\旋转动图3.gif)

这样看起来，好像不是那么直观，毕竟是2D 的图， 来给他加了3D 的效果看看，(由于设置了 transform-origin：0 0，所以并不会在元素的中间旋转，而是以 0 0 点的那条x 轴旋转)

![](css3_img\旋转动图4.gif)





由于我设置的 transform-origin：center center ；定的点在中心，那么两条轴，是会成这样子的。

![](css3_img\视点居中.gif)

然后，看下，结果，是不是如我们所示？

![](css3_img\旋转动图5.gif)

最后，加上rotateZ

![](css3_img\旋转动图6.gif)









### rotate3D()

设置一条主轴，然后根据这条轴旋转

　　这个呢，可以设置4个参数， 前三个是，x y z 最后一个是 角度deg 但是，此 x y z ，可不是上面那几个，不一样的。这三个值，设置的是矢量的方向，填什么无所谓，主要是比值很重要。举个例子  1，1，0，0deg  那么就是  1：1：0  = 100：100：0  对吧，拿这个值来图解比较好。

![](css3_img\寻找主轴.png)

![](css3_img\旋转动图7.gif)









### translateZ() 、translate3d()

向Z轴平移，这个可能有点难理解，想像一个场景，你现在和电脑屏幕的距离，这就是Z轴的距离，电脑屏幕离你越近，那么translateZ() 里面的值 越大， 电脑屏幕离你越远， translateZ() 的值就越小。 所以说，Z 增加，那么这个电脑屏幕，离你就越近，

下面要用到旋转，rotate

首先Z 轴是朝向我们的，所以 看不出效果，但是，我们把它转个身，让Z轴 面对 右边，就可以了。

![](css3_img\偏移动图.gif)



**translate() 和  translate3d()**

translate 是同时设置 translateX 和 translateY， 所以里面可以填两个参数， 第一个值 X 第二个 Y

translate3d 是同时设置 translateX ，translateY 和 translateZ 所以里面可以填三个参数

只不过有点不同的是， translate 如果第二个参数不填的话，默认是0， 不过translate3d的话，人家就不同意你不填了，你三个参数，必须都给我填。

![](css3_img\偏移动图.gif)









### ➕透视

很多画作具有3D效果，我们说它采用三点透视画法，在电脑上绘图也是同样的道理。

根据灭点数量的不同，可以将透视划分为如下三种：

（1）.只有一个灭点，那就是一点透视。

（2）.具有两个灭点，那就是两点透视。

（3）.具有三个灭点，那就是三点透视。

所谓灭点就是立体图形延伸后相交的地方，下面是来自百度百科一个关于透视的示意图：

![](css3_img\三点透视.jpg)

利用透视原理实现了3D效果，生活现实中视觉效果上也是如此。

假设有一个长廊，距离我们越远的地方看起来越小，如果足够的长，尽头会在视觉上相交。

简单图示如下：

![](css3_img\一点透视.jpg)











#### perspective（透视距离）

前面所做的一切都是为了介绍perspective属性做知识铺垫。

```css
div
{
    perspective: 500;
    -webkit-perspective: 500; /* Safari and Chrome */
}
```

**属性值参考**

| 值       | 描述                            |
| :------- | :------------------------------ |
| *number* | 元素距离视图的距离，以像素计。  |
| none     | 默认值。与 0 相同。不设置透视。 |

简单来说，就是设置这个属性后，那么，就可以模拟出像我们人看电脑上的显示的元素一样。比如说， perspective：800px  意思就是，我在离屏幕800px 的地方观看这个元素

![](css3_img\透视距离概念.png)

先来看看， 加了perspective 和 没有加是什么区别， 第一个小方块，是有加的效果，能明显的看到空间感了有没有， 感觉他是真的像在旋转， 而第二个呢，像是在伸缩。

![](css3_img\perspective的效果动图.gif)

那么思考一个问题，transform：translateZ 呢，可以增加 Z轴的距离， 那么Z轴越大，是不是也就代表着，这个元素，离我们的距离越近？ 那么，你把一张图片，贴到你脸上，有什么效果？ 是不是非常大？有人可能会问，这两者之间有什么关系吗？ 肯定是有的，这个 perspective 配合 transform：translateZ 就有这种效果， 我们来看看。(先记着，我们设置了perspective：800px，那么来看看 Z到800px 有什么效果)

![](css3_img\perspective的效果动图2.gif)

有没有发现，临近 800px 的时候， 图片突然变黑了， 然后到800px的时候， 图片消失了。 这又是为啥呢？  其实很像我们现实中的例子一样，一张远处的图片，慢慢的移动到你脸上， 你会看见图片越来越大，贴到你脸上的时候，是不是 你就看不见了？ 到800px 的时候，图片贴你脸上了，你被蒙蔽了双眼， 如果801px 是不是你都穿过这张图片了？这很好理解：我们是看不见眼睛后面的东西的！



那么transform：translateZ， 到负数的时候， 是不是值越小，图片离我们越远，同理的 图片也就越小。

![](css3_img\perspective的效果动图3.gif)

但是！如果你真的认为 perspective 这个属性这么简单的话，那么你就太天真了。按照我们的思路继续，如果 perspective： 这个值，越小，是不是我们就离屏幕越近， 那么 图片也会越大，(translateZ 是移动图片， perspective是移动 人 和屏幕的距离，这么想也是没问题的哈。对吧，那么把translateZ(0px)。然后增加 perspective 试试看。 )

![](css3_img\perspective的效果动图4.gif)

然后，你会惊奇的发现，咦？ 好像无论是增加，还是减少，图片都没有任何变化。 这个时候，先卖个关子，接着看下个案例，把 translateZ(-100px) 设置成 负值。(正常，按照我们的想法，是不是 Z的值是正数，说明这个图片，离我们越近，那么反之，负值，离我们越远对吧) 那么这次我们不移动 translateZ 了， 设置好Z 值为-100px 之后，移动perspective的值，把他的值变小，(正常来说，值越小，是不是就代表 我们离屏幕越近， 看的东西也就越大对吧)

![](css3_img\perspective的效果动图5.gif)

然后，你又会惊奇的发现， 怎么图片不是越来越大呢? 我们离屏幕越大，图片应该越大才对啊， 怎么变小了呢？ 

其实把。这里我们一直误导一个情况，我们看到的，并不是图片本身，而是图片的投影。 是不是有点晕了，投影是什么鬼， 没事，看下面的图解。



第一个情况，translateZ 的值越大，图片越大。

![](\css3_img\视像投影.png)



第二个情况，translateZ 的值越小，图片越小。

![](\css3_img\视像投影2.png)



第三个情况，translateZ 为0的时候，为什么移动我们perspective 的值，图片的大小没有改变呢？

![](\css3_img\视像投影3.png)



第四个情况，为什么translateZ 为负数之后，增加 perspective 的值后，图片不是变大， 反而变小呢？

![](\css3_img\视像投影4.png)





好了，最后补充一点，还有一个属性**perspective-origin**

这个属性呢，**默认值是 center center**，也就是 居中。这两个参数呢，是根据自身来定位的， 0px 0px 代表着元素的左上角，center center代表着元素的中间点。可以设置像素 50px 也可设置百分比 50%，还可以设置 top right left bottom center 等。

这个属性有什么用呢？ 这个属性是相当于人 的眼睛看哪里。你没有设置，也就是默认看父元素 中间的地方。看下面两张图的例子，就知道什么意思啦。



这个属性也是设置在父级身上的顶点。

![](css3_img\视觉中心设置.png)

![](css3_img\视觉中心设置2.png)











#### perspective属性的两种书写

`perspective`属性有两种书写形式，一种用在舞台元素上（动画元素们的共同父辈元素）；第二种就是用在当前动画元素上，与transform的其他属性写在一起。如下代码示例：

```css
.stage {
    perspective: 600px;
}
```

```css
#stage .box {
    transform: perspective(600px) rotateY(45deg);
}
```

![](css3_img\perspective两种书写.png)

从上图我们貌似可以看到，虽然书写的形式，属性名称不一致，但是，效果貌似是一样的~~果真是这样吗？？？

实际上不然，上面的demo上下两个效果之所以会一样，是因为舞台上只有一个元素，因此，发生了巧合，其正好表现一样了。如果，如果舞台上有**很多个元素**，则两种书写形式的表现差异就会立马显示出来了！

![](css3_img\perspective两种书写2.png)

好吧，图中的效果其实不难理解。上面舞台整个作为透视元素，因此，显然，我们看到的每个子元素的形体都是不一样的；而下面，每个元素都有一个自己的视点，因此，显然，因为rotateY的角度是一样的，因此，看上去的效果也就一模一样了！



#### **透视盲区**

当我们改变第一个`range`控件值为200的时候，您会发现右侧第三个元素看不见了：

![](css3_img\透视盲区.png)

这不难理解，前面一排门，每个门都是1米，你距离门2米，显示，当所有门都开了45°角的时候，此时，距离中间门右侧的第二个门正好与你的视线平行，这个门的门面显然就什么也看不到。这就是为什么上面右侧第三个门一片空白的元素——特定的视角以及距离形成的视觉盲区。















#### perspective-origin（灭点位置）

perspective-origin属性是3D变形中另一个重要属性，主要用来决定perspective属性的源点角度。它实际上设置了X轴和Y轴位置(或者说基点)，在该位置观看者好像在观看该元素的子元素。

> perspective-origin: x-axis y-axis;

x-axis:
定义视图在x轴上的位置。默认值：50%。可能的参数值形式:left、center、right、length和%。
y-axis:
定义视图在y轴上的位置。默认值：50%。可能的参数值形式:top、center、bottom、length和%。
看了上面的介绍可能还是不够清晰，没有能在大脑中形成一个清晰的概念，那么看下面这张图片:

![](css3_img\透视点位置.jpg)



下面这张截在W3C官网的图可以更好的阐述这一观点：

![](css3_img\透视点位置2.jpg)



为了加深印象，请看以下例子：

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
        .wrapper {   
            width: 50%;   
            float: left;   
        }   
        .cube {   
            font-size: 4em;   
            width: 2em;   
            margin: 1.5em auto;   
            transform-style: preserve-3d;   
        }   
        .side {   
            position: absolute;   
            width: 2em;   
            height: 2em;   
            background: rgba(255, 99, 71, 0.6);   
            border: 1px solid rgba(0, 0, 0, 0.5);   
            color: white;   
            text-align: center;   
            line-height: 2em;   
        }   
        .front {   
            transform: translateZ(1em);   
        }   
        .top {   
            transform: rotateX(90deg) translateZ(1em);   
        }   
        .right{   
            transform: rotateY(90deg) translateZ(1em);   
        }   
        .left {   
            transform: rotateY(-90deg) translateZ(1em);   
        }   
        .bottom{   
            transform: rotateX(-90deg) translateZ(1em);   
        }   

        .back {   
            transform: rotateY(-180deg) translateZ(1em);   
        }   
        .w1 {   
            perspective: 1000px; 
        }   
        .w2{   
            perspective: 1000px;    
        }  
    </style>
</head>
<body>
    <div class="wrapper w1">  
        <div class="cube">  
            <div class="side  front">1</div>  
            <div class="side   back">6</div>  
            <div class="side  right">4</div>  
            <div class="side   left">3</div>  
            <div class="side    top">5</div>  
            <div class="side bottom">2</div>  
        </div>  
    </div>  
    <div class="wrapper w2">  
        <div class="cube">  
            <div class="side  front">1</div>  
            <div class="side   back">6</div>  
            <div class="side  right">4</div>  
            <div class="side   left">3</div>  
            <div class="side    top">5</div>  
            <div class="side bottom">2</div>  
        </div>  
    </div>
</body>
</html>
```

<iframe src="html\3d透视.html" style="height:350px"><iframe>

下面的代码和上面的那个例子基本一样，只是改变了.w1,.w2,.cube三个css样式



**1.源点向左偏**

```css
.w1 {   
         perspective: 1000px; 
 }   
.w2{   
        perspective: 1000px;
        perspective-origin: left center;
 } 
```

![](css3_img\透视点位置3.jpg)





**2.源点向上偏**

```css
.w1 {   
         perspective: 1000px; 
 }   
.w2{   
        perspective: 1000px;
        perspective-origin: center -100px;
 } 
```

![](css3_img\透视点位置4.jpg)





**3.源点向左偏的同时向上偏**

```css
w1 {   
         perspective: 1000px; 
 }   
.w2{   
        perspective: 1000px;
        perspective-origin: left -100px;
 } 
```

![](css3_img\透视点位置5.jpg)

可以看到，我们只是给第二个盒子加了个perspective-origin属性,其表现结果却和第一个盒子有很大不同，这就是perspective-origin改变基点的作用。



**属性值参考**

| 值       | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| *x-axis* | 定义该视图在 x 轴上的位置。默认值：50%。可能的值：left、center、right、*length*、*%* |
| *y-axis* | 定义该视图在 y 轴上的位置。默认值：50%。可能的值：top、center、bottom、*length*、*%* |









### transform-style

transform--style属性指定嵌套元素是怎样在三维空间中呈现。

**注意：** 使用此属性必须先使用 [transform](https://www.runoob.com/cssref/css3-pr-transform.html) 属性.



语法

> transform-style: flat|preserve-3d;

**属性值参考**

| 值          | 描述                           |
| :---------- | :----------------------------- |
| flat        | 表示所有子元素在2D平面呈现。   |
| preserve-3d | 表示所有子元素在3D空间中呈现。 |



`transform-style`属性也是3D效果中经常使用的，其两个参数，`flat|preserve-3d`. 前者`flat`为默认值，表示平面的；后者`preserve-3d`表示3D透视。

基本上，我们想要根据现实经验实现一些3D效果的时候，`transform-style: preserve-3d`是少不了的。一般而言，该声明应用在3D变换的兄弟元素们的**父元素**上，也就是**舞台元素**。















### transform-origin（基点位置）

CSS3 transform里面有一个属性transform-origin，该属性可以改变元素的原点位置，这篇文章：[CSS揭秘之沿着环形路径运动的动画](http://caibaojian.com/css3-animate-spin.html)，正是巧妙的运用了原点位置，从而实现了围绕圆心运动。transform-origin里面的百分比没有详细了解是以什么为标准的。今天看看这篇文章详细了介绍了这个值跟left、right、top、bottom之间的关系。 默认情况，变形的原点在元素的中心点，或者是元素X轴和Y轴的50%处，如下图所示：

![](css3_img\基点位置.jpg)



语法

> transform-origin: *x-axis y-axis z-axis*;

**不同写法**

```css
/*只设置一个值的语法*/
transform-origin: x-offset
transform-origin:offset-keyword
/*设置两个值的语法*/
transform-origin：x-offset  y-offset
transform-origin：y-offset  x-offset-keyword
transform-origin：x-offset-keyword  y-offset
transform-origin：x-offset-keyword  y-offset-keyword
transform-origin：y-offset-keyword  x-offset-keyword
/*设置三个值的语法*/
transform-origin：x-offset  y-offset  z-offset
transform-origin：y-offset  x-offset-keyword  z-offset
transform-origin：x-offset-keyword  y-offset  z-offset
transform-origin：x-offset-keyword  y-offset-keyword  z-offset
transform-origin：y-offset-keyword  x-offset-keyword  z-offset
```

`transform-origin`属性值可以是百分比、em、px等具体的值，也可以是top、right、bottom、left和center这样的关键词。 **2D的变形中的`transform-origin`属性可以是一个参数值，也可以是两个参数值。如果是两个参数值时，第一值设置水平方向X轴的位置，第二个值是用来设置垂直方向Ｙ轴的位置。** 3D的变形中的`transform-origin`属性还包括了Ｚ轴的第三个值。其各个值的取值简单说明如下：

- x-offset：用来设置`transform-origin`水平方向Ｘ轴的偏移量，可以使用`<length>`和`<percentage>`值，同时也可以是正值（从中心点沿水平方向Ｘ轴向右偏移量），也可以是负值（从中心点沿水平方向Ｘ轴向左偏移量）。
- offset-keyword：是`top`、`right`、`bottom`、`left`或`center`中的一个关键词，可以用来设置`transform-origin`的偏移量。
- y-offset：用来设置`transform-origin`属性在垂直方向Ｙ轴的偏移量，可以使用`<length>`和`<percentage>`值，同时可以是正值（从中心点沿垂直方向Ｙ轴向下的偏移量），也可以是负值（从中心点沿垂直方向Ｙ轴向上的偏移量）。
- x-offset-keyword：是`left`、`right`或`center`中的一个关键词，可以用来设置`transform-origin`属性值在水平Ｘ轴的偏移量。
- y-offset-keyword：是`top`、`bottom`或`center`中的一个关键词，可以用来设置`transform-origin`属性值在垂直方向Ｙ轴的偏移量。
- z-offset：用来设置3D变形中`transform-origin`远离用户眼睛视点的距离，默认值`z=0`，其取值可以`<length>`，不过`<percentage>`在这里将无效。





看上去`transform-origin`取值与`background-position`取值类似。为了方便记忆，可以把关键词和百分比值对比起来记：

- top = top center = center top = 50% 0
- right = right center = center right = 100%或(100% 50%)
- bottom = bottom center = center bottom = 50% 100%
- left = left center = center left = 0或(0 50%)
- center = center center = 50%或（50% 50%）
- top left = left top = 0 0
- right top = top right = 100% 0
- bottom right = right bottom = 100% 100%
- bottom left = left bottom = 0 100%





为了能一眼看明白，下面截图以transform中的旋转rotate()为例，并transform-origin取值不一样时的效果：

 **transform-origin取值为center（或center center或50% 或50% 50%）：**

![](css3_img\基点位置2.jpg)



**transform-origin取值为top（或top center或center top或50％ 0）：** 

![](css3_img\基点位置3.jpg)



**transform-origin取值为right(或right center 或center right 或 100％ 或 100％ 50％)：**

![](css3_img\基点位置4.jpg)

 

**transform-origin取值为bottom(或bottom center 或center bottom 或 50％ 100％)：** 

![](css3_img\基点位置5.jpg)



**transform-origin取值为left(或left center或center left或0或0 50％):**

![](css3_img\基点位置6.jpg)



 **transform-origin取值为top left(或left top或0 0)：** 

![](css3_img\基点位置7.jpg)



**transform-origin取值为right top（或top right或100％ 0）：** 

![](css3_img\基点位置8.jpg)



**transform-origin取值为bottom right（或right bottom或100％ 100％）:**

![](css3_img\基点位置9.jpg)



**transform-origin取值为left bottom（或bottom left 或 0 100％）:** 

![](css3_img\基点位置10.jpg)









CSS3变形中旋转、缩放、倾斜都可以通过`transform-origin`属性重置元素的原点，但其中的位移**`translate()`始终以元素 *中心点* 进行位移。**例如下面的两段代码的演示过程：

```css
div {
  transform-origin: 50% 50%;
  transform: translate(40px, 40px) translate(-50px, 35px)
}
```

接下来通过`transform-origin`将变形原点设置为100% 100%:

```css
div {
  transform-origin: 100% 100%;
  transform: translate(40px, 40px) translate(-50px, 35px)
}
```

![](css3_img\基点位移.jpg)

虽然元素的变形原点通过`transform-origin`从50% 50%变成100% 100%，但元素位移`translate()`始终是依元素中心点进行位移









### backface-visibility

backface-visibility 属性定义当元素背面向屏幕时是否可见。

如果在旋转元素不希望看到其背面时，该属性很有用。

语法

> backface-visibility: visible|hidden

**属性值参考**

| 值      | 描述             |
| :------ | :--------------- |
| visible | 背面是可见的。   |
| hidden  | 背面是不可见的。 |

![](css3_img\翻转可见.png)









### **趣味动画**

<iframe src="html\骰子动画.html" style="height: 380px;background:white"></iframe>



<iframe src="html\转针动画.html" style="background: white;height:200px"></iframe>



















## *🔳@keyframes（动画）*

要创建 CSS3 动画，你需要了解 @keyframes 规则。

@keyframes 规则是创建动画。

@keyframes 规则内指定一个 CSS 样式和动画将逐步从目前的样式更改为新的样式。

CSS3 可以创建动画，它可以取代许多网页动画图像、Flash 动画和 JavaScript 实现的效果。



使一个div元素逐渐移动200像素：

```css
@keyframes mymove
{
from {top:0px;}
to {top:200px;}
}

@-webkit-keyframes mymove /* Safari and Chrome */
{
from {top:0px;}
to {top:200px;}
}
```

使用@keyframes规则，你可以创建动画。

创建动画是通过逐步改变从一个CSS样式设定到另一个。

在动画过程中，您可以更改CSS样式的设定多次。

指定的变化时发生时使用％，或关键字**"from"和"to"**，这是**和0％到100％相同**。

**0％是开头动画，100％是当动画完成。**

**为了获得最佳的浏览器支持，您应该始终定义为0％和100％的选择器。**

**注意:** 使用animation属性来控制动画的外观，还**使用选择器绑定动画。**



**语法**

> @keyframes *animationname* {*keyframes-selector* {*css-styles;}*}

**属性值参考**

| 值                   | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| *animationname*      | 必需的。定义animation的名称。                                |
| *keyframes-selector* | 必需的。动画持续时间的百分比。合法值：0-100% from (和0%相同) to (和100%相同)**注意：** 您可以用一个动画keyframes-selectors。 |
| *css-styles*         | 必需的。一个或多个合法的CSS样式属性                          |















### animation

使用简写属性把 动画 绑定到一个元素：

```css
div
{
    animation:mymove 5s infinite;
    -webkit-animation:mymove 5s infinite; /* Safari 和 Chrome */
}
```

**语法**

> ```
> animation: name duration timing-function delay iteration-count direction fill-mode play-state;
> ```

**属性值参考**

| 值                                                           | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| *[animation-name](https://www.runoob.com/cssref/css3-pr-animation-name.html)* | 指定要绑定到选择器的关键帧的名称                             |
| *[animation-duration](https://www.runoob.com/cssref/css3-pr-animation-duration.html)* | 动画指定需要多少秒或毫秒完成                                 |
| *[animation-timing-function](https://www.runoob.com/cssref/css3-pr-animation-timing-function.html)* | 设置动画将如何完成一个周期                                   |
| *[animation-delay](https://www.runoob.com/cssref/css3-pr-animation-delay.html)* | 设置动画在启动前的延迟间隔。                                 |
| *[animation-iteration-count](https://www.runoob.com/cssref/css3-pr-animation-iteration-count.html)* | 定义动画的播放次数。                                         |
| *[animation-direction](https://www.runoob.com/cssref/css3-pr-animation-direction.html)* | 指定是否应该轮流反向播放动画。                               |
| [animation-fill-mode](https://www.runoob.com/cssref/css3-pr-animation-fill-mode.html) | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 |
| *[animation-play-state](https://www.runoob.com/cssref/css3-pr-animation-play-state.html)* | 指定动画是否正在运行或已暂停。                               |
| initial                                                      | 设置属性为其默认值。 [阅读关于 *initial*的介绍。](https://www.runoob.com/cssref/css-initial.html) |
| inherit                                                      | 从父元素继承属性。 [阅读关于 *initinherital*的介绍。](https://www.runoob.com/cssref/css-inherit.html) |







### animation-name

使用指定的 @keyframes 动画：

```css
animation-name:mymove;
-webkit-animation-name:mymove; /* Safari 和 Chrome */
```

**语法**

> animation-name: *keyframename*|none;

**属性值参考**

| 值             | 说明                                     |
| :------------- | :--------------------------------------- |
| *keyframename* | 指定要绑定到选择器的关键帧的名称         |
| none           | 指定有没有动画（可用于覆盖从级联的动画） |









### animation-duration

animation-duration属性定义动画完成一个周期需要多少秒或毫秒。

```css
animation-duration:2s;
-webkit-animation-duration:2s; /* Safari 和 Chrome */
```

语法

> animation-duration: *time*;

| 值     | 说明                                                         |
| :----- | :----------------------------------------------------------- |
| *time* | 指定动画播放完成花费的时间。默认值为 0，意味着没有动画效果。 |









### animation-timing-function

animation-timing-function指定动画将如何完成一个周期。

速度曲线定义动画从一套 CSS 样式变为另一套所用的时间。

速度曲线用于使变化更为平滑。



从开始到结束以相同的速度播放动画:

```css
animation-timing-function:linear;
-webkit-animation-timing-function:linear; /* Safari and Chrome */
```

**语法**

> animation-timing-function: *value*;

**属性值参考**

| 值                            | 描述                                                         | 测试                                                         |
| :---------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| linear                        | 动画从头到尾的速度是相同的。                                 | [测试](https://www.runoob.com/try/playit.php?f=animation-timing-function&preval=linear) |
| ease                          | 默认。动画以低速开始，然后加快，在结束前变慢。               | [测试](https://www.runoob.com/try/playit.php?f=animation-timing-function&preval=ease) |
| ease-in                       | 动画以低速开始。                                             | [测试](https://www.runoob.com/try/playit.php?f=animation-timing-function&preval=ease-in) |
| ease-out                      | 动画以低速结束。                                             | [测试](https://www.runoob.com/try/playit.php?f=animation-timing-function&preval=ease-out) |
| ease-in-out                   | 动画以低速开始和结束。                                       | [测试](https://www.runoob.com/try/playit.php?f=animation-timing-function&preval=ease-in-out) |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值。 |                                                              |













### animation-delay

animation-delay 属性定义动画什么时候开始。

animation-delay 值单位可以是秒（s）或毫秒（ms）。

**提示:** 允许负值，-2s 使动画马上开始，但跳过 2 秒进入动画。



等待两秒，然后开始动画：

```css
animation-delay:2s;
-webkit-animation-delay:2s; /* Safari 和 Chrome */
```

**语法**

| 值     | 描述                                                    | 测试                                                         |
| :----- | :------------------------------------------------------ | :----------------------------------------------------------- |
| *time* | 可选。定义动画开始前等待的时间，以秒或毫秒计。默认值为0 | [测试 »](https://www.runoob.com/try/playit.php?f=animation-delay&preval=1s) |











### animation-iteration-count

animation-iteration-count属性定义动画应该播放多少次。



播放三次动画：

```css
animation-iteration-count:3;
-webkit-animation-iteration-count:3; /*Safari and Chrome*/
```

**语法**

> animation-iteration-count: *value*;

**属性值参考**

| 值       | 描述                             | 测试                                                         |
| :------- | :------------------------------- | :----------------------------------------------------------- |
| *n*      | 一个数字，定义应该播放多少次动画 | [测试 »](https://www.runoob.com/try/playit.php?f=animation-iteration-count&preval=1) |
| infinite | 指定动画应该播放无限次（永远）   | [测试 »](https://www.runoob.com/try/playit.php?f=animation-iteration-count&preval=infinite) |







### animation-direction

animation-direction 属性定义是否循环交替反向播放动画。

**注意：**如果动画被设置为只播放一次，该属性将不起作用。



先执行一遍动画，然后再反向执行一遍动画：

```css
animation-direction:alternate;
-webkit-animation-direction:alternate; /* Safari 和 Chrome */
```

**语法**

> animation-direction: normal|reverse|alternate|alternate-reverse|initial|inherit;

| 值                | 描述                                                         | 测试                                                         |
| :---------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| normal            | 默认值。动画按正常播放。                                     | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_animation-direction&preval=normal) |
| reverse           | 动画反向播放。                                               | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_animation-direction&preval=reverse) |
| alternate         | 动画在奇数次（1、3、5...）正向播放，在偶数次（2、4、6...）反向播放。 | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_animation-direction&preval=alternate) |
| alternate-reverse | 动画在奇数次（1、3、5...）反向播放，在偶数次（2、4、6...）正向播放。 | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_animation-direction&preval=alternate-reverse) |
| initial           | 设置该属性为它的默认值。请参阅 [*initial*](https://www.runoob.com/cssref/css-initial.html)。 |                                                              |
| inherit           | 从父元素继承该属性。请参阅 [*inherit*](https://www.runoob.com/cssref/css-inherit.html)。 |                                                              |









### animation-play-state

animation--play-state属性指定动画是否正在运行或已暂停。

**注意：**在JavaScript中使用此属性在一个周期中暂停动画。



暂停动画：

```css
animation-play-state:paused;
-webkit-animation-play-state:paused; /* Safari 和 Chrome */
```



**语法**

> animation-play-state: paused|running;

| 值      | 描述               | 测试                                                         |
| :------ | :----------------- | :----------------------------------------------------------- |
| paused  | 指定暂停动画       | [测试 »](https://www.runoob.com/try/playit.php?f=animation-play-state&preval=paused) |
| running | 指定正在运行的动画 | [测试 »](https://www.runoob.com/try/playit.php?f=animation-play-state&preval=running) |











### animation-fill-mode

animation-fill-mode 属性规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。

默认情况下，CSS 动画在第一个关键帧播放完之前不会影响元素，在最后一个关键帧完成后停止影响元素。animation-fill-mode 属性可重写该行为。

**语法**

> animation-fill-mode: none|forwards|backwards|both|initial|inherit;

**属性值参考**

| 值        | 描述                                                         |
| :-------- | :----------------------------------------------------------- |
| none      | 默认值。动画在动画执行之前和之后不会应用任何样式到目标元素。 |
| forwards  | 在动画结束后（由 animation-iteration-count 决定），动画将应用该属性值。 |
| backwards | 动画将应用在 animation-delay 定义期间启动动画的第一次迭代的关键帧中定义的属性值。这些都是 from 关键帧中的值（当 animation-direction 为 "normal" 或 "alternate" 时）或 to 关键帧中的值（当 animation-direction 为 "reverse" 或 "alternate-reverse" 时）。 |
| both      | 动画遵循 forwards 和 backwards 的规则。也就是说，动画会在两个方向上扩展动画属性。 |
| initial   | 设置该属性为它的默认值。请参阅 [*initial*](https://www.runoob.com/cssref/css-initial.html)。 |
| inherit   | 从父元素继承该属性。请参阅 [*inherit*](https://www.runoob.com/cssref/css-inherit.html)。 |



仅仅看上面，很难理解。下面详细解释。
首先我们要了解动画按照执行时间可以分为三个状态：动画执行前（动画延迟）、动画执行中、动画执行后。了解了状态，我们逐个值看。

示例代码：

```html
<div class="test">
    <span class="mode1">none</span>
    <span class="mode2">forwards</span>
    <span class="mode3">backwards</span>
    <span class="mode4">both</span>
</div>
```

```css
.test span{
    display: block;
    width: 100px;
    height: 100px;
    font-size: 14px;
    color: #000;
    line-height: 100px;
    text-align: center;
    border-radius: 100%;
    background: #ac2aef;
    animation-name: move;
    animation-duration: 3s;
    animation-delay: 1s;
    animation-timing-function: ease-in;
}
.mode1{animation-fill-mode: none;}
.mode2{animation-fill-mode: forwards;}
.mode3{animation-fill-mode: backwards;}
.mode4{animation-fill-mode: both;}
@keyframes move{
    0%{
        background: #FFFA90;
        transform: translateX(0) scale(1);
    }
    100%{
        background: #4cd826;
        transform: translateX(200px) scale(0.5);
    }
}
```

上述动画我们有延迟1s，可以观察动画执行前的状态。
注意：动画执行方向是0%到100%执行

![](css3_img\逐帧分析.png)

![](css3_img\逐帧分析2.png)

![](css3_img\逐帧分析3.png)

![](css3_img\逐帧分析4.png)

**当值为none时**

`none`为默认值，在动画执行前和动画执行后，对元素的样式不会产生影响。
从上图可以看到，值为none时，**动画执行前**和**执行后**的状态和**无动画**的状态是**一致**的，动画执行前和执行后对元素没有产生任何样式影响。动画执行后跳到无动画状态。

**当值为forwards时**

`forwards`当使用这个值时，告诉浏览器：动画结束后，元素的样式将设置为动画的最后一帧的样式。
从上图可以看出，值为forwards时，**动画执行前**的状态和**无动画状态**一致，但是**动画执行后**状态却不一样，查看动画效果，你会发现他的状态和**动画最后一帧**的状态相同。

**当值为backwards时**

从CSS样式可以看到，动画我们有设置延迟1s,为了便于观察动画执行前的状态。
`backwards`当使用这个值时，告诉浏览器：动画开始前，元素的样式将设置为动画的第一帧的样式。
从上图可以看出，值为backwards时，**动画执行前**的状态和**无动画状态** 不一致，可以看出，它的状态和**动画第一帧**相同，但是**动画执行后**状态却和**无动画**时的状态相同。

**当值为both时**

`both`当使用这个值时，告诉浏览器：同时使用**forwards**和**backwards**两个属性值的效果。
从上图可以看出，**动画执行前**是**动画第一帧**的效果，**动画执行后**是**动画最后一帧**的效果。



**最后`animation-fill-mode`的状态和`animation-direction`的值有关。**
1、当`animation-direction`为`normal` 或 `alternate`时，和上面的状态相同。
2、当`animation-direction`为`alternate-reverse` 或`reverse` 时，状态刚好和上面相反。从100%到0%执行。









## *🔳动画函数*



### ➕matrix() 矩阵

实现炫酷的网页动画效果，自然少不了css3中transform的属性，此属性功能丰富且强大，比如实现元素的位移translate(x,y)，缩放scale(x,y)，2d旋转rotate(angle)，倾斜变换skew(x-angle,y-angle)等，利用这些属性可以实现基本的动画效果，如果你要实现自定义和像素级别控制的高级动画效果，我们还需要深入了解它的另外一个属性——matrix，matrix就是矩阵的意思，听起来是不是很高级，你没听错实现更高级的效果，你需要了解“矩阵”，听到“矩阵”，是不是很惊慌，本节文章，笔者将从以下几个方面进行介绍:向量与矩阵基础知识一个matrix()的例子matrix()参数详细介绍





#### 向量

向量被用于许多科学领域用来描述方向和大小，我们一般用笛卡尔坐标系进行向量的描述，向量简单的来说就是把数排成一列或一行进行展示，比如向量(2,1)和(3,3)在坐标系中表示如下：

![](css3_img\向量坐标.png)

假设我们现在有两个向量AB(8,2)和向量AC(2,6)，我们对其进行加、减、乘运算，示例如下

向量加法：

AB+AC=AD(8,2)+(2,6)=(8+2,2+6)=(10,8)

![](css3_img\向量坐标2.jpeg)



向量减法：

AB-AC= AD(8,2)-(2,6)=(8-2,2-6)=(6,-4)

![](css3_img\向量坐标3.jpeg)



向量乘法：

ABAC = AD(8,2)(2,6)=(82,26)=(16,12)

![](css3_img\向量坐标4.jpeg)

从上述例子我们可以看出，向量的运算即为每个向量对应的位置进行两两相加、两两相减或两两相乘。







#### 矩阵

简单来说把数排列成长方形就是矩阵，我们一般用几行几列来描述矩阵，比如 2\*2 矩阵，2\*3 矩阵等，乘号左边代表行数，乘号右边代表列数，如下图所示表示2\*2 的矩阵：

![](css3_img\矩阵.jpg)

矩阵相加：

相同规模（行数列数都相等）的矩阵之间的加法如下图所示：

![](css3_img\矩阵2.jpg)





我们可以看出是对应的位置两两相加而得。

矩阵相乘

1、矩阵与向量相乘,示意图如下：

![](css3_img\矩阵3.jpg)

从图可以看出矩阵与向量相乘结果为向量，矩阵每行的数字分别与向量每行对应的位置分别相乘最后将结果相加，得到结果向量。







2、矩阵与矩阵相乘

比如 2\*4 的矩阵与 4\*3 的矩阵相乘我们得到一个 2\*3 的矩阵，如下图所示：

![](css3_img\矩阵4.jpg)

从图示中我们可以看出，我们左边矩阵的每行与右边矩阵的每列分别相乘，相乘规则如矩阵与向量相乘的规则一样，最终得到矩阵的行数等于左边矩阵的行数，列数等于右边矩阵的列数。











#### matrix() 与 矩阵

一个matrix()的例子

介绍完基本向量和矩阵的知识后，我们来看看一个matrix()的例子，transform:matrix(a,b,c,d,ex,fy)一共六个参数，用矩阵表示如下图所示：

![](css3_img\矩阵5.jpg)

注：参数书写的方向是竖着写的。

这六个参数代表什么意思，这里先不做介绍，稍后会详细介绍，现在我们有这样一个元素，其对应的CSS属性如下：

```css
#transformedObject { position: absolute; left: 0px; top: 0px; width: 200px; height: 80px;}
```

此段代码，对应的页面效果如下：

![](css3_img\元素的四个顶点.jpeg)

从上图我们可以看出，此长方形的四个顶点从左顶点开始，顺时针方向对应的坐标分别为：(0,0)、(200,0)、(200,80)、(0,80),我们对其进行transform:matrix(0.9, -0.05, -0.375, 1.375, 220, 20)变换，css代码如下：

\#transformedObject { position: absolute; left: 0px; top: 0px; width: 200px; height: 80px; transform: matrix(0.9, -0.05, -0.375, 1.375, 220, 20); transform-origin: 0 0;}

注：transform-origin是变形原点，也就是该元素围绕着那个点变形或旋转，该属性只有在设置了transform-origin属性的时候起作用。

应用这个属性后的效果如下图：

![](css3_img\元素的四个顶点2.jpeg)





好奇的你，你一定困惑这四个点的值是怎么得出来的呢，其实有了前面的向量和矩阵知识，我们很容易算出，matrix(0.9, -0.05, -0.375, 1.375, 220, 20)用矩阵表示如下图所示：

![](css3_img\矩阵6.jpg)

元素最初的每个顶点相当一个向量，例如(200,0)可用下图表示：

![](css3_img\矩阵7.jpg)

变换后的四个点的坐标值，其实是matrix(0.9, -0.05, -0.375, 1.375, 220, 20)对应的矩阵与原始四个点对应的向量分别相乘而得，具体的运算过程如下图：

与(200, 80)相乘的运算过程得到点(370,120)：

![](css3_img\矩阵换算.jpg)

与(200, 0)相乘的运算过程得到点(400,10)：

![](css3_img\矩阵换算2.jpg)

与(0, 80)相乘的运算过程得到点(190,130)：

![](css3_img\矩阵换算3.jpg)

与(0,0)相乘的运算过程得到点(220,20)：

![](css3_img\矩阵换算4.jpg)

经过运算后，我们得到最终变换后的四个顶点坐标: (220,20)，(400,10)，(370,120)，(190,130)







#### matrix()参数详解

通过前面的学习，我们学到了向量和矩阵的基础知识，一个matrix()变换的例子，我们通过矩阵运算算出了变换后的结果。接下来我们一起学习下transform:matrix(a,b,c,d,ex,fy)这六个参数代表的意义，其实这六个参数，对应的是**translate(x,y)，scale(x,y)，rotate(angle)，skew(x-angle,y-angle)**这些效果,每种变换效果对应的参数不同,先看一下公式

<img src="css3_img\矩阵公式.jpg" style="background:white">

其中，`x`, `y`表示转换元素的所有坐标（变量）了。

很简单，3\*3矩阵每一行的第1个值与后面1*3的第1个值相乘，第2个值与第2个相乘，第3个与第3个，然后相加，如下图同色标注：

![](css3_img\矩阵公式2.png)

`ax+cy+e`为变换后的水平坐标，`bx+dy+f`表示变换后的垂直位置。



**每个参数与之对应的transform效果**

![](css3_img\matrix与transform.jpg)

缩放：scale(x,y) => matrix(x,0,0,y,0,0)

偏移：translate(x,y) => matrix(1,0,0,1,x,y)

倾斜：skew(x,y) => matrix(1,tan(θy),tan(θx),1,0,0)

旋转：rotate(deg) => matrix(cosθ,sinθ,-sinθ,cosθ,0,0)









#### matrix3d()

3D变换中的矩阵

3D变换虽然只比2D多了一个D，但是复杂程度不只多了一个。从二维到三维，是从4到9；而在矩阵里头是从3\*3变成4\*4, 9到16了。

其实，本质上很多东西都与2D一致的，只是复杂度不一样而已。这里就举一个简单的3D缩放变换的例子。

初始化的变换矩阵:

<img src="css3_img\3d矩阵.jpg" style="background:white">

初始化的变换乘法后的结果:

<img src="css3_img\3d矩阵2.jpg" style="background:white">

所以matrix3d的默认值:

```css
div {
  transform: matrix3d(1, 0, 0, 0, 0,  1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1)
}
```



**旋转**

![](css3_img\旋转矩阵.png)



**偏移**

![](css3_img\移动矩阵.png)

`dx`: x轴移动的距离
`dy`: y轴移动的距离
`dz`: z轴移动的距离



**缩放**

![](css3_img\缩放矩阵.png)



**倾斜**

斜切是最不好理解的，符合右手定则，如果y轴斜切角度，是指垂直Y轴逆时针旋转一定的角度后的坐标

![](css3_img\倾斜矩阵.png)









### ➕cubic-bezier() 贝塞尔曲线

cubic-bezier() 函数定义了一个贝塞尔曲线(Cubic Bezier)。

贝塞尔曲线曲线由四个点 P0，P1，P2 和 P3 定义。P0 和 P3 是曲线的起点和终点。P0是（0,0）并且表示初始时间和初始状态，P3是（1,1）并且表示最终时间和最终状态。

![](css3_img\贝塞尔曲线坐标.png)

![](css3_img\贝塞尔曲线2.jpg)

![](css3_img\贝塞尔曲线.jpg)

贝塞尔曲线通过控制曲线上的四个点（起始点、终止点以及两个相互分离的中间点）来创造、编辑图形，绘制出一条光滑曲线并以曲线的状态来反映动画过程中速度的变化。

从上图我们需知道的是 cubic-bezier 的取值范围:

```
P0：默认值 (0, 0)
P1：动态取值 (x1, y1)
P2：动态取值 (x2, y2)
P3：默认值 (1, 1)
```



CSS3动画速度的控制通过三次贝塞尔曲线函数实现，定义规则为
            **cubic-bezier (x1,y1,x2,y2)**

分别用A,B,C,D表示这四个点，其中起始点固定值为**A(0,0)**,终止点固定为**D(1,1)**剩下的中间点**B(x1,y1),C(x2,y2)**也就是所要动态操控的两个点了,对应**cubic-bezier (x1,y1,x2,y2)**中的四个参数,通过改变B,C两点的坐标值来动态生成一条贝塞尔曲线表示动画中的速度变化。

![](css3_img\贝塞尔曲线3.jpg)

 x的取值区间是**[0,1]**,取值超过该区间cubic-bezier即无效,y的的取值区间没有限制[-0.5,0.5]也是可以的,但不应该超出**[0,1]**范围太大。







CSS3提供的几个预设速度：

1. **ease** 对应自定义cubic-bezier(.25,.01,.25,1),效果为先慢后快再慢；

   ![](css3_img\ease.jpg)

2. **linear**  对应自定义cubic-bezier(0,0,1,1)，效果为匀速直线；

   ![](css3_img\linear.jpg)

3. **ease-in**  对应自定义cubic-bezier(.42,0,1,1),效果为先慢后快；

   ![](css3_img\ease-in.jpg)

4. **ease-out**  对应自定义cubic-bezier(0,0,.58,1),效果为先快后慢；

   ![](css3_img\ease-out.jpg)

5. **ease-in-out**  对应自定义cubic-bezier(.42,0,.58,1),效果为先慢后快再慢。

   ![](css3_img\ease-in-out.jpg)















### ➕steps() 定格动画

`steps()`功能符可以让动画不连续。

`animation`默认以`ease`方式过渡，会以在每个关键帧之间插入补间动画，所以动画效果是连贯性的。`ease`，`linear`等之类的过渡函数都会为其插入补间。但有些效果不需要补间，只需要关键帧之间的跳跃，这时应该使用`steps`过渡方式。

steps()是一个timing function，允许我们将动画或者过渡分割成段，而不是从一种状态持续到另一种状态的过渡。这个函数有两个参数——第一个参数是一个正值，指定我们希望动画分割的段数。

第二个参数定义了这个要点 在我们的@keyframes中申明的动作将会发生的关键。这个值是可选的，在没有传递参数时，默认为”end”

> ```
> steps(n,[start | end])
> ```

简单地说，就是原本一个状态向另一个状态的过渡是平滑的，`steps`可以实现分布过渡。`steps`用法 :

n是一个自然数，`steps`函数把动画分成`n`段。

**关于timing-function**:

- `step-start`

  ```
  等同于 `steps(1,start)` ，动画分成 1 步，动画执行时以左侧端点为开始 ；
  ```

- `step-end`

  ```
  等同于 steps(1,end) ，动画分成 1 步，动画执行时以结尾端点为开始 。
  ```





#### 了解end 和 start

CSS规范中对于`start`和`end`的定义是基于数学函数来的，函数这东西，多少人的噩梦，因为过于抽象，与现实难以关联，所以，如果我们盯着定义去理解`start`和`end`，那就是死胡同，不归路，就算现在弄懂了，过段时间再遇到，得了，全忘光光了，函数图哪个是哪个，鬼才记得。下面这张图就出自[规范文档](https://www.w3.org/TR/css-timing-1/#step-timing-functions)：

<img src="css3_img\end 和 start.svg" style="background:white">

按照规范图再细化解释就是：

- `start`：表示直接开始。也就是时间才开始，就已经执行了一个距离段。于是，动画执行的5个分段点是下面这5个，起始点被忽略，因为时间一开始直接就到了第二个点：

![](css3_img\start.png)

+ `end`：表示戛然而止。也就是时间一结束，当前距离位移就停止。于是，动画执行的5个分段点是下面这5个，结束点被忽略，因为等要执行结束点的时候已经没时间了：

![](css3_img\end.png)





万物具有相对性。例如，苍蝇眼中的人类动作都是慢动作，但是人类眼中的苍蝇却非常敏捷。

同样的，`start`和`end`这里的开始和结束是相对于时间而言的，但是，如果站在人类可感知的具体事物而言，`start`和`end`却是相反的含义。

所以，我们可以这么理解：

+ `start`：表示结束。分段结束的时候，时间才开始走。于是，动画执行的5个分段点是后5个点

![](css3_img\start.png)

+ `end`：表示开始。分段开始的时候，时间跟着一起走。于是，动画执行的5个分段点是前5个点

![](css3_img\end.png)





#### end 和 start 的 实际演示

先看一个效果。两个小球，从0px运动到400px，分为了4个动画步骤，有5个关键帧。第一个是start模式，第二个是end模式。

```html
<p>start模式</p>
<div class="wrap">
    <div id="ball"></div>
</div>
<p>end模式</p>
<div class="wrap">
    <div id="ball1"></div>
</div>
```

```css
body{
  font:1em "microsoft Yahei";
  color:#333;
}
.wrap{
width:500px;
height:140px;
background:url(http://www.mrszhao.com/zb_users/upload/2018/06/20180625162728152991524879459.jpg) no-repeat 0 bottom;
margin-bottom:30px;}
#ball,#ball1{
width:100px;
height:100px;
background-color:#F63;
border-radius:50%;
}
#ball{
animation: move 2s steps(4,start) ;}
#ball1{
animation: move 2s steps(4,end) ;}
@keyframes move{
0%{
transform:translateX(0px);}

100%{
transform:translateX(400px);}
}
```

![](css3_img\小球逐帧.png)

发现start模式是时间一开始，直接进入第二个关键帧状态，然后顺利顺利走完全程。

end模式有点傻，时间一开始，从第一个关键帧开始跑，结果时间结束了，才走完第四个关键帧，没有完成全部路程就over了。

所以start和end的名字和它所表示的含义刚好相反

为上面的小球加上infinite，可以看出start模式第二次开始的运动都是从第二个关键帧开始的。

<iframe src="html\小球逐帧动画.html" style="background:white;height:500px;"><iframe>



加上forwards模式则变得不一样了，forwards是向前的单词意思，但是表示的则是保留动画最后的运动状态，意思和功能也是相反的。

可以看到，第二个end模式的小球终于有机会走完全程了。

![](css3_img\小球逐帧2.png)

所以，当为end模式设置了forwards的时候要小心，因为它其实多走了一步。











#### steps() 案例

1、这头熊的原始素材一共有8个步骤。

![](css3_img\熊.jpg)

所以使用`steps(8,end)`是最好的方式，因为如果使用`steps(8,start)`，则第一帧不能执行，最后一帧会闪白，图片消失。

因为要一直运动，所以需要加上infinite，当执行完最后一张图的时候，再返回到第一张图，形成一个连贯的完步。

通过背景图片的`background-position`的改变，形成熊的运动。

```html
<div id="bear"></div>
```

```css
body{
background:#000;}
#bear{
width:200px;
height:200px;
position:absolute;
left:50%;
top:50%;
margin-left:-100px;
margin-top:-100px;
background:url(http://www.mrszhao.com/zb_users/upload/2018/06/stepsanimation/images/bear.png) no-repeat 0 0;
animation:move 1.5s steps(8,end) infinite ;
}
@keyframes move{
0%{
background-position:0 0;}
100%{
background-position:-1600px 0;}}
```

<iframe src="html\熊奔跑动画.html" style="height:300px;"><iframe>







2、这个logo一共有24张图片

![](css3_img\logo胶卷.jpg)

但是logo只运动一次，并且停在结束状态，根据end模式的特征，如果加上forwards的话，会多运动一步。

所以，这里是`steps(23,end)`，为什么是23步，而不是24步，因为forwards模式对它的影响。要想最后一步还要看到图片，那么背景图片的挪动就要少挪动一个图片的宽度。图片整个宽度4800px，每一张图200px的宽高。所以背景图片只需要往左边挪动-4600px即可。

```html
<div id="logo"></div>
```

```css
body{
background:#000;}
#logo{
width:200px;
height:200px;
position:absolute;
left:50%;
top:50%;
margin-left:-100px;
margin-top:-100px;
background:url(http://www.mrszhao.com/zb_users/upload/2018/06/stepsanimation/images/bg-logo-long.png) no-repeat 0 0;
animation:move 2s steps(23,end) forwards ;
}
@keyframes move{
0%{
background-position:0 0;}
100%{
background-position:-4600px 0;}}
```

<iframe src="html\logo动画.html" style="height:300px;"><iframe>

通过上面的案例可以看出，对象的运动效果由逐帧绘制的图片的间距所影响。间距一样，则速度一样。利用steps不能去改变现成的图片帧与帧之间的速度。









#### step-end的用处

其实`step-end`等价于`steps(1,end)`，`step-start`等价于`steps(1,start)`。

只有一个步骤的逐帧动画，一样可以利用好时间的改变和距离的改变做出速度不同的效果来。

```html
<style>
      div {
        width: 100px;
        height: 177px;
        background: url(https://static.runoob.com/images/mix/space-to-top.png);
        background-repeat: no-repeat;
        background-position: -2px;
        animation: renlink 0.5s infinite step-end;
      }
      @keyframes renlink {
        0% {
          background-position: -2px;
        }
        14.28571428571429% {
          background-position: -144px;
        }
        28.57142857142857% {
          background-position: -287px;
        }
        42.85714285714286% {
          background-position: -431px;
        }
        57.14285714285714% {
          background-position: -573px;
        }
        71.42857142857143% {
          background-position: -716px;
        }
        85.71428571428571% {
          background-position: -860px;
        }

        100% {
          background-position: 0;
        }
      }
    </style>
 
  <body>
    <div></div>
  </body>
```

<iframe src="html\B站动画.html" style="background:white;height:200px"></iframe>



















# 💥响应式布局



## *🔳flex box弹性盒子*

弹性盒子是 CSS3 的一种新的布局模式。

CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。

引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器中的子元素进行排列、对齐和分配空白空间。

弹性盒子由弹性容器(Flex container)和弹性子元素(Flex item)组成。

弹性容器通过设置 display 属性的值为 flex 或 inline-flex将其定义为弹性容器。

弹性容器内包含了一个或多个弹性子元素。

**注意：** 弹性容器外及弹性子元素内是正常渲染的。弹性盒子只定义了弹性子元素如何在弹性容器内布局。

弹性子元素通常在弹性盒子内一行显示。默认情况每个容器只有一行。



总结一句话就是

给div这类块状元素元素设置`display:flex`或者给span这类内联元素设置`display:inline-flex`，flex布局即创建！其中，直接设置`display:flex`或者`display:inline-flex`的元素称为flex容器，里面的子元素称为flex子项。

flex和inline-flex区别在于，inline-flex容器为inline特性，因此可以和图片文字一行显示；flex容器保持块状特性，宽度默认100%，不和内联元素一行显示。



Flex布局相关属性正好分为两拨，一拨作用在flex容器上，还有一拨作用在flex子项上，参见下表

|                       作用在flex容器上                       |                       作用在flex子项上                       |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| [flex-direction](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-direction)、[flex-wrap](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-wrap)、[flex-flow](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-flow)、[justify-content](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#justify-content)、[align-items](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#align-items)、[align-content](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#align-content) | [order](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-order)、[flex-grow](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-grow)、[flex-shrink](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-shrink)、[flex-basis](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-basis)、[flex](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#flex-flex)、[align-self](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/?shrink=1#align-self) |

无论作用在flex容器上，还是作用在flex子项，都是控制的flex子项的呈现，只是前者控制的是整体，后者控制的是个体。







## *🔳作用在flex容器上的css 属性*

下面的所有案例HTML结构为：

```
container(flex容器)
  div(flex子项) > img
  div(flex子项) > img
  div(flex子项) > img
```

同时，为了便于区分，flex容器区域使用虚框标示，flex子项增加了白蓝径向渐变背景色，图片上显示了原始序号。

Flex布局中还有主轴和交叉轴的概念，为避免过多概念干扰，本教程省略相关措辞，而是使用水平方向和垂直方向代替。

writing-mode属性可以改变文档流方向，此时主轴是垂直方向，但实际开发很少遇到这样场景，因此，初学的时候，直接使用水平方向和垂直方向理解不会有任何问题，反而易于理解。





### flex-direciton

`flex-direction`用来控制子项整体布局方向，是从左往右还是从右往左，是从上往下还是从下往上。和CSS的[direction属性](https://www.zhangxinxu.com/wordpress/2016/03/css-direction-introduction-apply/)相比就是多了个`flex`。

语法如下：

```
flex-direction: row | row-reverse | column | column-reverse;
```

其中：

- row

  默认值，显示为行。方向为当前文档水平流方向，默认情况下是从左往右。如果当前水平文档流方向是`rtl`（如设置`direction:rtl`），则从右往左。

- row-reverse

  显示为行。但方向和`row`属性值是反的。

- column

  显示为列。

- column-reverse

  显示为列。但方向和`column`属性值是反的。

眼见为实，点击下面对应单选项，可以看到实时的布局效果：

![](css3_img\flex-direction-row.png)

![](css3_img\flex-direction-row-reverse.png)

![](css3_img\flex-direction-column.png)

![](css3_img\flex-direction-column-reverse.png)











### flex-wrap

`flex-wrap`用来控制子项整体单行显示还是换行显示，如果换行，则下面一行是否反方向显示。这个属性比较好记忆，在CSS世界中，只要看到单词wrap一定是与换行显示相关的，`word-wrap`属性或者`white-space:nowrap`或者`pre-wrap`之类。

语法如下：

```
flex-wrap: nowrap | wrap | wrap-reverse;
```

其中：

- nowrap

  默认值，表示单行显示，不换行。于是很容易出现宽度溢出的场景，其渲染表现比较复杂，需要对CSS3宽度有一定了解，可以阅读“[理解CSS3 max/min-content及fit-content等width值](https://www.zhangxinxu.com/wordpress/?p=5392)”这篇文章。具体表现如下（以水平布局举例）：flex子项最小内容宽度`min-content`之和大于flex容器宽度，则内容溢出，表现和`white-space:nowrap`类似。如果flex子项最小内容宽度`min-content`之和小于flex容器宽度，则：flex子项默认的`fit-content`宽度之和大于flex容器宽度，则flex子项宽度收缩，正好填满flex容器，内容不溢出。flex子项默认的`fit-content`宽度之和小于flex容器宽度，则flex子项以`fit-content`宽度正常显示，内容不溢出。在下面案例中，示意的图片默认有设置`max-width:100%`，flex子项div没有设置宽度，因此，flex子项最小宽度是无限小，表现为图片宽度收缩显示。如果我们取消`max-width:100%`样式，则此时flex子项最小宽度就是图片宽度，就可以看到图片溢出到了flex容器之外。

- wrap

  宽度不足换行显示。

- wrap-reverse

  宽度不足换行显示，但是是从下往上开始，也就是原本换行在下面的子项现在跑到上面。

眼见为实，点击下面对应单复选框，可以看到实时的布局效果：

![](css3_img\flex-nowrap.png)

![](css3_img\flex-nowrap2.png)

![](css3_img\flex-wrap.png)

![](css3_img\flex-wrap-reverse.png)













### flex-flow

`flex-flow`属性是`flex-direction`和`flex-wrap`的缩写，表示flex布局的flow流动特性，语法如下：

```
flex-flow: <‘flex-direction’> || <‘flex-wrap’>
```

当多属性同时使用的时候，使用空格分隔。

举个例子，容器元素如下设置：

```css
.container {
    display: flex;
    flex-flow: row-reverse wrap-reverse;
}
```

![](css3_img\wrap-reverse、row-reverse.png)

可以看到水平排序从右往左（`row-reverse`属性值的作用），以及换行的那一行在上面（`wrap-reverse`属性值的作用）。









### justify-content

`justify-content`属性决定了水平方向子项的对齐和分布方式。CSS `text-align`有个属性值为`justify`，可实现[两端对齐](https://www.zhangxinxu.com/wordpress/2015/08/chinese-english-same-padding-text-justify/)，所以，当我们想要控制flex元素的水平对齐方式的时候，记住`justify`这个单词，`justify-content`属性也就记住了。

`justify-content`可以看成是`text-align`的远房亲戚，不过前者控制flex元素的水平对齐外加分布，后者控制内联元素的水平对齐。

语法如下：

```
justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
```

其中：

- flex-start

  默认值。逻辑CSS属性值，与文档流方向相关。默认表现为左对齐。

- flex-end

  逻辑CSS属性值，与文档流方向相关。默认表现为右对齐。

- center

  表现为居中对齐。

- space-between

  表现为两端对齐。between是中间的意思，意思是多余的空白间距只在元素中间区域分配。使用抽象图形示意如下：

  ![](css3_img\flex-between.svg)

+ space-around

  around是环绕的意思，意思是每个flex子项两侧都环绕互不干扰的等宽的空白间距，最终视觉上边缘两侧的空白只有中间空白宽度一半。使用抽象图形示意如下：

  ![](css3_img\flex-around.svg)

+ space-evenly

  evenly是匀称、平等的意思。也就是视觉上，每个flex子项两侧空白间距完全相等。使用抽象图形示意如下

  ![](css3_img\flex-evenly.svg)

眼见为实，点击下面对应单复选框，可以看到实时的布局效果：

![](css3_img\justify-start.png)

![](css3_img\justify-end.png)

![](css3_img\justify-center.png)

![](css3_img\justify-space-between.png)

![](css3_img\justify-space-around.png)

![](css3_img\justify-space-evenly.png)













### align-items

`align-items`中的`items`指的就是flex子项们，因此`align-items`指的就是flex子项们相对于flex容器在垂直方向上的对齐方式，大家是一起顶部对齐呢，底部对齐呢，还是拉伸对齐呢，类似这样。

语法如下：

```
align-items: stretch | flex-start | flex-end | center | baseline;
```

其中：

- stretch

  默认值。flex子项拉伸。在演示中我们可以看到白蓝径向渐变背景区域是上下贯穿flex容器的，就是因为flex子项的高度拉伸到容器高度导致。如果flex子项设置了高度，则按照设置的高度值渲染，而非拉伸。

- flex-start

  逻辑CSS属性值，与文档流方向相关。默认表现为容器顶部对齐。

- flex-end

  逻辑CSS属性值，与文档流方向相关。默认表现为容器底部对齐。

- center

  表现为垂直居中对齐。

- baseline

  表现为所有flex子项都相对于flex容器的基线（[字母x的下边缘](https://www.zhangxinxu.com/wordpress/2015/06/about-letter-x-of-css/)）对齐。

眼见为实，点击下面对应单选框，可以看到实时的布局效果：

![](css3_img\align-strech.png)

![](css3_img\align-start.png)

![](css3_img\align-end.png)

![](css3_img\align-center.png)

![](css3_img\align-baseline.png)



















### align-content

`align-content`可以看成和`justify-content`是相似且对立的属性，`justify-content`指明水平方向flex子项的对齐和分布方式，而`align-content`则是指明垂直方向每一行flex元素的对齐和分布方式。如果所有flex子项只有一行，则`align-content`属性是没有任何效果的。

语法如下：

```
align-content: stretch | flex-start | flex-end | center | space-between | space-around | space-evenly;
```

其中：

- stretch

  默认值。每一行flex子元素都等比例拉伸。例如，如果共两行flex子元素，则每一行拉伸高度是50%。

- flex-start

  逻辑CSS属性值，与文档流方向相关。默认表现为顶部堆砌。

- flex-end

  逻辑CSS属性值，与文档流方向相关。默认表现为底部堆放。

- center

  表现为整体垂直居中对齐。

- space-between

  表现为上下两行两端对齐。剩下每一行元素等分剩余空间。

- space-around

  每一行元素上下都享有独立不重叠的空白空间。

- space-evenly

  每一行元素都完全上下等分。

眼见为实，我们给flex容器设置高度500像素，然后点击下面对应单选框，可以看到实时的布局效果：

![](css3_img\align-content-strech.png)

![](css3_img\align-content-start.png)

![](css3_img\align-content-end.png)

![](css3_img\align-content-center.png)

![](css3_img\align-content-between.png)

![](css3_img\align-content-around.png)

![](css3_img\align-content-evenly.png)

















## *🔳作用在flex子项上的css属性*

### order

我们可以通过设置`order`改变某一个flex子项的排序位置。

语法：

```
order: <integer>; /* 整数值，默认值是 0 */
```

所有flex子项的默认`order`属性值是0，因此，如果我们想要某一个flex子项在最前面显示，可以设置比0小的整数，如`-1`就可以了。

眼见为实，下面flex容器有3个子元素，现在，我们给第2个子元素设置`order`属性值，看看其排列位置有何变化。点击下面的单选框，可以看到实时的交互效果：

![](css3_img\order-1.png)

![](css3_img\order0.png)

![](css3_img\order1.png)











### flex-grow

`flex-grow`属性中的grow是扩展的意思，扩展的就是flex子项所占据的宽度，扩展所侵占的空间就是除去元素外的剩余的空白间隙。

具体的扩展比较复杂。在展开之前，我们先看下语法。

语法：

```
flex-grow: <number>; /* 数值，可以是小数，默认值是 0 */
```

`flex-grow`不支持负值，默认值是0，表示不占用剩余的空白间隙扩展自己的宽度。如果`flex-grow`大于0，则flex容器剩余空间的分配就会发生，具体规则如下：

- 所有剩余空间总量是1。

- 如果只有一个flex子项设置了flex-grow属性值：

  - 如果`flex-grow`值小于1，则扩展的空间就总剩余空间和这个比例的计算值。
  - 如果`flex-grow`值大于1，则独享所有剩余空间。

  具体可参见下面“grow案例1”。

  

- 如果有多个flex设置了flex-grow属性值：

  - 如果`flex-grow`值总和小于1，则每个子项扩展的空间就总剩余空间和当前元素设置的`flex-grow`比例的计算值。
  - 如果`flex-grow`值总和大于1，则所有剩余空间被利用，分配比例就是`flex-grow`属性值的比例。例如所有的flex子项都设置`flex-grow:1`，则表示剩余空白间隙大家等分，如果设置的`flex-grow`比例是1:2:1，则中间的flex子项占据一半的空白间隙，剩下的前后两个元素等分。

  具体可参见下面“grow案例2”。

**grow案例1：**
flex容器有3个子元素，现在，我们仅给第2个子元素设置`flex-grow`属性值，看看其占据尺寸有何变化。点击下面的单选框，可以看到实时的交互效果：

![](css3_img\flex-grow.png)

![](css3_img\flex-grow2.png)

![](css3_img\flex-grow3.png)

![](css3_img\flex-grow4.png)

此实例演示中，仅一个flex子项设置了`flex-grow`属性值，当我们选择`0.5`的时候，值小于1，剩余空间用不完，因此，扩展的宽度是总剩余宽度是0.5，也就是一半；当我们选择`1`的时候，正好所有空间都使用；当我们选择`2`的时候，效果一样，因为没有其他参与分配的子项，因此渲染表现和`1`一样。





**grow案例2：**
flex容器有3个子元素，默认所有子项都设置了`flex-grow:0.25`，现在我们点击下面的单选框，改变第2个子元素的`flex-grow`属性值，看看其占据尺寸有何变化：

![](css3_img\flex-grow5.png)

![](css3_img\flex-grow6.png)

![](css3_img\flex-grow7.png)

![](css3_img\flex-grow8.png)

此实例演示中，因为3个子项都是0.25，因此默认还剩余25%的剩余空间；如果我们选择`flex-grow:0`，则加起来的`flex-grow`是`0.5`，因此剩余50%空间；如果我们选择`flex-grow:0.5`，则加起来的`flex-grow`是`1`，因此没有剩余空间，同时空间占用比例为1:2:1，最终效果符合此预期；如果我们选择`flex-grow:1`，则加起来的`flex-grow`大于`1`，剩余空间按比例分配，为1:4:1，最终效果也确实如此。

以上就是`flex-grow`属性的作用规则。











### flex-shrink

shrink是“收缩”的意思，`flex-shrink`主要处理当flex容器空间不足时候，单个元素的收缩比例。

语法如下：

```
flex-shrink: <number>; /* 数值，默认值是 1 */
```

`flex-shrink`不支持负值，默认值是1，也就是默认所有的flex子项都会收缩。如果设置为0，则表示不收缩，保持原始的`fit-content`宽度。

`flex-shrink`的内核跟`flex-grow`很神似，`flex-grow`是空间足够时候如何利用空间，`flex-shrink`则是空间不足时候如何收缩腾出空间。总有点CP的味道。

两者的规则也是类似。已知flex子项不换行，且容器空间不足，不足的空间就是“完全收缩的尺寸”：

- 如果只有一个flex子项设置了flex-shrink：
  - `flex-shrink`值小于1，则收缩的尺寸不完全，会有一部分内容溢出flex容器。
  - `flex-shrink`值大于等于1，则收缩完全，正好填满flex容器。
- 如果多个flex子项设置了flex-shrink：
  - `flex-shrink`值的总和小于1，则收缩的尺寸不完全，每个元素收缩尺寸占“完全收缩的尺寸”的比例就是设置的`flex-shrink`的值。
  - `flex-shrink`值的总和大于1，则收缩完全，每个元素收缩尺寸的比例和`flex-shrink`值的比例一样。下面案例演示的就是此场景。

眼见为实，flex容器有4个子元素，现在，我们给第2个子元素设置不同的`flex-shrink`属性值，看看其占据尺寸有何变化。点击下面的单选框，可以看到实时的交互效果：

![](css3_img\flex-shrink.png)

![](css3_img\flex-shrink2.png)

![](css3_img\flex-shrink3.png)

![](css3_img\flex-shrink4.png)

此实例演示中，因为4个子项都是1，和远大于1，因此，完全收缩，不会有内容溢出。如果我们选择`flex-shrink:0`，则第2个flex子项不收缩，剩下3个flex子项等比例收缩；如果我们选择`flex-shrink:1`，则4个子项1:1:1:1收缩；如果我们选择`flex-shrink:2`，则完全收缩尺寸比例分配为1:2:1:1，第2个flex子项收缩的宽度最大，是其他元素的2倍。

以上就是`flex-shrink`属性的作用规则。











### flex-basis

`flex-basis`定义了在分配剩余空间之前元素的默认大小。相当于对浏览器提前告知：浏览器兄，我要占据这么大的空间，提前帮我预留好。

语法如下：

```
flex-basis: <length> | auto; /* 默认值是 auto */
```

默认值是`auto`，就是自动。有设置`width`则占据空间就是`width`，没有设置就按内容宽度来。

如果同时设置`width`和`flex-basis`，就渲染表现来看，会忽略`width`。flex顾名思义就是弹性的意思，因此，实际上不建议对flex子项使用`width`属性，因为不够弹性。

当剩余空间不足的时候，flex子项的实际宽度并通常不是设置的`flex-basis`尺寸，因为flex布局剩余空间不足的时候默认会收缩。

**实例一则：**
flex容器有3个子元素，现在，我们给第2个子元素设置不同的`flex-basis`属性值，看看其占据尺寸有何变化。点击下面的单选框，可以看到实时的交互效果：

![](css3_img\flex-basis.png)

![](css3_img\flex-basis2.png)

![](css3_img\flex-basis3.png)

![](css3_img\flex-basis4.png)

选择最后一个`flex-basis:256px`会发现flex子项的宽度并不是`256px`，这是因为此时剩余空间不足，3个子项1:1:1收缩的缘故。











### ➕flex

`flex`属性是`flex-grow`，`flex-shrink`和`flex-basis`的缩写。

语法

```
flex: none | auto | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
```

上面这种CSS语法被称为格式化语法，大部分CSS属性值都比较简单，格式化语法看不出有什么优势。但是如果CSS属性值比较的复杂，规则较多，例如CSS渐变或者这里的`flex`属性，则格式化语法就比较重要了，可以知道一些平时没有注意到的语法上的细节，也能一看就知道语法规则。





语法解构

CSS语法中的特殊符号的含义绝大多数就是正则表达式中的含义，例如单管道符`|`，方括号`[]`，问号`？`，个数范围花括号`{}`等。具体说明：

首先是单管道符`|`。表示排他。也就是这个符号前后的属性值都是支持的，且不能同时出现。因此，下面这些语法都是支持的：

```
flex: auto;
flex: none;

flex: [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
```

接下来是`[ ... ]`这一部分。其中方括号`[]`表示范围。也就是支持的属性值在这个范围内。我们先把方括号`[]`内其他特殊字符去除，可以得到下面的语法：

```
flex: auto;
flex: none;

flex: [ <'flex-grow'> <'flex-shrink'> <'flex-basis'> ]
```

这就是说，flex属性值支持空格分隔的3个值，因此，下面的语法都是支持的。

```
flex: auto;
flex: none;
/* 3个值 */
flex: 1 1 100px;
```

然后我们再看方括号`[]`内的其他字符，例如问号`?`，表示0个或1个。也就是`flex-shrink`属性可有可无。因此，flex属性值也可以是2个值。因此，下面的语法都是支持的。

```
flex: auto;
flex: none;
/* 2个值 */
flex: 1 100px;
/* 3个值 */
flex: 1 1 100px;
```

然后我们再看双管道符`||`，是或者的意思。表示前后可以分开独立合法使用。也就是`flex: flex-grow flex-shrink?`和`flex-basis`都是合法的。于是我们又多了2种合法的写法：

```
flex: auto;
flex: none;
/* 1个值，flex-grow */
flex: 1;
/* 1个值，flex-basis */
flex: 100px;
/* 2个值，flex-grow和flex-basis */
flex: 1 100px;
/* 2个值，flex-grow和flex-shrink */
flex: 1 1;
/* 3个值 */
flex: 1 1 100px;
```





文字表述

- **1个值**

  如果flex的属性值只有一个值，则：如果是数值，例如`flex: 1`，则这个`1`表示`flex-grow`，此时`flex-shrink`和`flex-basis`的值分别是`1`和`0%`，注意，这里的`flex-basis`的值是`0%`，而不是默认值`auto`。如果是长度值，例如`flex: 100px`，则这个`100px`显然指`flex-basis`，因为3个缩写CSS属性中只有`flex-basis`的属性值是长度值。此时`flex-grow`和`flex-shrink`都是`1`，注意，这里的`flex-grow`的值是`1`，而不是默认值`0`。

- **2个值**

  如果flex的属性值有两个值，则第1个值一定指`flex-grow`，第2个值根据值的类型不同表示不同的CSS属性，具体规则如下：如果第2个值是数值，例如`flex: 1 2`，则这个`2`表示`flex-shrink`，**此时`flex-basis`计算值是`0%`，并非默认值`auto`。如果第2个值是长度值，例如`flex: 1 100px`，则这个`100px`指`flex-basis`，此时`flex-shrink`使用默认值`0`。

- **3个值**

  如果`flex`的属性值有3个值，则这长度值表示`flex-basis`，其余2个数值分别表示`flex-grow`和`flex-shrink`。下面两行CSS语句的语法都是合法的，且含义也是一样的：

  ```css
  /* 下面两行CSS语句含义是一样的 */ 
  flex: 1 2 50%； 
  flex: 50% 1 2;
  ```

  





#### flex属性值深入

**initial**

初始值。`flex:initial`等同于设置`"flex: 0 1 auto"`。可以理解为flex属性的默认值。任意打开一个页面，我们打开控制台执行下面的代码：

```
window.getComputedStyle(document.body).flex;
// 结果就是"flex: 0 1 auto"
```

也就是`flex`属性默认值是`"0 1 auto"`，等同于`initial`关键字的计算值。

该默认值图形示意如下：

![](css3_img\flex-initial.png)

其行为表现文字描述为：

*不会增长变大占据flex容器中额外的剩余空间（flex-grow:0），会收缩变小以适合容器（flex-shrink:1），尺寸根据自身宽高属性进行调整（flex-basis:auto）。*

**案例**

于是如果我们只给容器设置`display:flex`，同时子项元素内容都很少，就会有下图所示的效果（用我最爱的深天蓝色高亮下轮廓）：

![](css3_img\flex案例.png)

如果子项内容很多，由于`flex-shrink:1`，因此，会缩小，表现效果就是文字换行。如下：

![](css3_img\flex案例2.png)

关键CSS代码和HTML代码如下：

```html
<style>
.container {
    display: flex;
}
</style>
<div class="container">
    <item>范张</item>
    <item>范鑫</item>
    <item>范旭</item>
    <item>范帅</item>
    <item>范哥</item>
</div>
```









**auto**

`flex:auto`等同于设置`"flex: 1 1 auto"`。

图示如下：

![](css3_img\flex-auto.png)

其行为表现文字描述为：

*子项会增长变大占据flex容器中额外的剩余空间（flex-grow:1），会收缩变小以适合容器（flex-shrink:1），尺寸根据自身宽高属性进行调整（flex-basis:auto）。*

**案例**

HTML不变，子项元素的CSS增加`flex:auto`的设置：

```css
.container {
    display: flex;
}
.container item {
    flex: auto;
}
```

结果就是下面这样，子项宽度变大填满了剩余空间，由于每一个子项元素的`flex-grow`的值都是`1`，因此，5等分填充。

![](css3_img\flex案例3.png)















**none**

`flex:none`等同于设置`"flex: 0 0 auto"`。

图示如下：

![](css3_img\flex-none.png)

其行为表现文字描述为：

*子项会不会增长变大占据flex容器中额外的剩余空间（flex-grow:0），也不会收缩变小以适合容器（flex-shrink:0），尺寸根据自身宽高属性进行调整（flex-basis:auto）。*

**案例**

HTML不变，每个子项内容很多，同时子项元素的CSS增加`flex:none`的设置：

```css
.container {
    display: flex;
}
.container item {
    flex: none;
}
```

结果就是下面这样，子项宽度超过了容器的尺寸，由于`flex-shrink`的值都是`0`，因此，不会收缩变小导致子项的内容撑爆了容器。

![](css3_img\flex案例4.png)















#### flex分配实践

最后再说说一开始提到的`flex-grow`，`flex-shrink`和`flex-basis`。

**一定要牢记这3个属性默认值，**不然遇到只有1~2个参数时候，根本不知道什么意思。

- **flex-grow**

  `flex-grow`指定了容器剩余空间多余时候的分配规则，默认值是`0`，多余空间不分配。

- **flex-shrink**

  `flex-shrink`指定了容器剩余空间不足时候的分配规则，默认值是`1`，空间不足要分配。

- **flex-basis**

  `flex-basis`则是指定了固定的分配数量，默认值是`auto`。如会忽略设置的同时设置`width`或者`height`属性。`flex-basis`包含大量的细节知识，这个可以专门开一篇文章深入探讨。

首先解释下，为什么会有容器剩余空间多余和不足的情况出现？

我们还是要分配财产的例子去说明。

范闲的财产分配遗嘱是自己50岁时候制定的。由于范张，范鑫和范旭都已经成家立业，自己在外独立生活，因此，给这三人分配了固定数目的财产，每人100万；而范帅和范哥尚未成年，和范闲还住在一起，所以，遗嘱就是剩下的财产两人评分，按照50岁时候的资产，也是人均100万的样子。

但是世事难料，谁知没过几年，家道中落，范闲总资产已经不足300万，此时，扣除答应范张，范鑫和范旭的300万，已经没有都与家产了，范帅和范哥就要喝西北风了，这就是容器剩余空间不足的情况。

为了应对各种状况出现，因此，财产分配规则制定的时候，一定要明确好基本财产数量`flex-basis`，财产有多余时候的分配规则`flex-grow`，以及财产不足时候的分配规则`flex-shrink`。

**案例**

请实现：

> 范张，范鑫和范旭每人100万固定家产，范帅和范哥则20万保底家产。如果范闲归西那天家产还有富余，范帅和范哥按照3:2比例分配；如果没有剩余财产，则范张，范鑫和范旭三位兄长按照2:1:1的比例给两人匀20万保底家产。

HTML结构如下：

```html
<div class="container">
    <item clas="zhang">范张</item>
    <item clas="xin">范鑫</item>
    <item clas="xu">范旭</item>
    <item clas="shuai">范帅</item>
    <item clas="ge">范哥</item>
</div>
```

大家可以想想CSS代码该怎么写……

拿出纸和笔，自己写写看……

假设你自己已经写好了，可以对比看一下是不是下面我写的一样：

```css
.container {
    /* 范闲：来，家产分配开始了~ */
    display: flex;  
}
.zhang {
    /* 老大不会争夺多余财产，但是会在财产不足时候分出老二老三分出的2倍的财产，这是作为老大应有的姿态 */
    flex: 0 2 100px;    
}
.xin,
.xu {
    /* 老二和老三不会争夺多余财产，但是会在财产不足时候分出部分财产，照应老四和老幺 
    这里也可以直接写成：flex: 100px*/
    flex: 0 1 100px;    
}
.shuai {
    /* 老四会争夺多余财产，且会在财产不足时候享用哥哥们分出的财产，确保能够活下去，感谢三位哥哥的照顾 */
    flex: 3 0 20px;    
}
.ge {
    /* 老五会争夺多余财产，不过比例比哥哥少一点，且会在财产不足时候享用哥哥们分出的财产，感谢哥哥们的照顾 */
    flex: 2 0 20px;    
}
```

最终的效果参见下面的GIF录屏+实时标注说明：

![](css3_img\flex实践.gif)

极端情况下，几个哥哥会财产小到几乎饿死，而依然保证两位弟弟有20万的保底财产，多么感人的兄弟情啊！

![](css3_img\flex实践.png)







### ➕flex-basis的一些细节

实际上在Flex布局中，一个flex子项的最终尺寸是基础尺寸、弹性增长或收缩、最大最小尺寸限制共同作用的结果。

其中：

- 基础尺寸由CSS `flex-basis`属性，`width`等属性以及`box-sizing`盒模型共同决定；
- 弹性增长指的是`flex-grow`属性，弹性收缩指的是`flex-shrink`属性；
- 最大最小尺寸限制指的是`min-width`/`max-width`等CSS属性，以及`min-content`最小内容尺寸。





**flex-basis与盒模型**

`flex-basis`的尺寸是作用在`content-box`上的，这个和`width`属性是一样的。

例如：

```css
.flex-basis {
    padding: 1em;
    border: 1em solid deepskyblue;
    color: deepskyblue;
    flex-basis: 100px;
}
```

和

```css
.width {
    padding: 1em;
    border: 1em solid deepskyblue;
    color: deepskyblue;
    width: 100px;
}
```

在外部尺寸够大，内容尺寸够小的情况下，两者的表现是一样的。

如下图示意：

![](css3_img\flex-basis与width.png)



我们可以通过设置`box-sizing`属性改变元素的盒模型，例如。

```css
.flex-basis {
    padding: 1em;
    border: 1em solid deepskyblue;
    color: deepskyblue;
    flex-basis: 100px;
    box-sizing: border-box;
    word-break: break-all;
}
```

此时就可以看到内容展示宽度变小了，外部尺寸表现为`100px`：

![](css3_img\flex-basis与width2.png)





#### flex-basis 与 width

大家一定要搞清楚这样一个事实，**在Flex布局中，子项设置width是没有直接效果的。**

此时一定会有人反驳，我明明设置了`width:100px`就有效果啊！

对是有效果，但并不是`width`直接生效的，而是`flex-basis`的作用。





**深入理解flex-basis:auto**

`flex-basis`的默认值是`auto`，表示自动，也就是完全根据子列表项自身尺寸渲染。

自身尺寸渲染优先级如下：

```
min-width > || max-width > width > Content Size
```

同时，在Flex布局中，**flex-basis优先级是比width高的**（可以理解为覆盖）。

所以，`flex-basis`和`width`同时设置了具体的数值，则`width`属性值直接被打入冷宫，在样式表现上完全被忽略。

例如：

```html
<by-zhangxinxu>
    <item-basis-width>项目1</item-basis-width>
    <item-basis-width>项目2</item-basis-width>
    <item-basis-width>项目3</item-basis-width>
    <item-basis-width>项目4</item-basis-width>
</by-zhangxinxu>
```

CSS如下：

```css
by-zhangxinxu {
    display: flex;
}
item-basis-width {
    padding: 1em;
    border: 1px solid deepskyblue;
    color: deepskyblue;
    box-sizing: border-box;
    width: 200px;
    flex-basis: 100px;
}
```

结果`width:200px`完全是一个盒饭角色，所谓盒饭角色，就是明明参与了这部电视剧，也吃了剧组的盒饭，但是观众完全就没有意识到他的存在。这里的`width`属性就是这种性质，代码中确实出现过，但是样式渲染却完全体现不出来。

如下图所示，宽度表现为`100px`。

![](css3_img\flex-basis与width3.png)

回到主线这里，实际上，`flex-basis`值是`auto`的时候，`width`属性值也是被打入冷宫的，只是，这个时候，由于后宫放权，所以，冷宫也是有影响力的。什么意思呢？

`flex-basis:auto`的含义是，子项的基本尺寸根据其自身的尺寸决定。而这个自身尺寸与下面这几个方面有关：

- `box-sizing`盒模型（这个已经介绍过了）；
- `width`/`min-width`/`max-width`等CSS属性设置；
- `content`内容（min-content最小宽度）；

所以下面两段Flex布局中的CSS就不难理解了：

```css
item-width {
    width: 100px;
}
```

最终尺寸 = 自身尺寸 = basis尺寸(auto)。

此时，自身尺寸为`100px`，basis尺寸按照自身尺寸来算（因为此时属性值是`auto`），因此最终尺寸是`100px`。





```css
item-basis {
    flex-basis: 100px;
}
```

最终尺寸 = 自身尺寸 < basis尺寸(length) < content。

此时，自身尺寸为content内容最小宽度，basis尺寸100px，因此最终尺寸是：如果content内容最小宽度不超过`100px`，则最终尺寸是`100px`，否则就是内容最小宽度。

记住上面的解释，我们再看下面几个例子，就会豁然开朗了。





**案例1：相同表现**

一个元素最小宽度超过100像素场景并不多，因此大部分场景下，`flex-basis`和`width`表现是一致的。

例如下面这个对比案例：

```html
<h4>1. flex-basis:100px</h4>
<by-zhangxinxu>
    <item-basis>项目1</item-basis>
    <item-basis>项目2</item-basis>
    <item-basis>项目3</item-basis>
    <item-basis>项目4</item-basis>
</by-zhangxinxu>
<h4>2. width:100px</h4>
<by-zhangxinxu>
    <item-width>项目1</item-width>
    <item-width>项目2</item-width>
    <item-width>项目3</item-width>
    <item-width>项目4</item-width>
</by-zhangxinxu>
```

一个设置`width:100px`，一个设置`flex-basis:100px`：

```css
item-width {
    width: 100px;
}
item-basis {
    flex-basis: 100px;
}
```

默认都是`100px`的展示，例如我的PC电脑显示器下：

![](css3_img\flex-basis与width4.png)

由于默认`flex-shrink`属性值是1，因此，容器宽度不足时候的弹性收缩效果也是一样的，如下GIF示意：

![](css3_img\flex-basis与width4.gif)







**案例2：不同表现**

什么时候`width:100px`和`flex-basis:100px`表现不一样呢？就是最小内容宽度较大的时候，例如出现连续英文单词demo_by_zhangxinxu。

还是上面那个[demo页面](https://www.zhangxinxu.com/study/201912/flex-basis-vs-width-demo.php)，点击任意某个子项，会改变元素里面的文字内容为demo_by_zhangxinxu。

此时就可以看到两者的差异了，如下图所示：

![](css3_img\flex-basis与width5.png)

因为此时content内容最小宽度超过了`100px`，于是`flex-basis:100px`按照了最小内容宽度显示了；但是`width:100px`把元素的尺寸限制得死死的，字符直接溢出容器之外。

要想两者表现一致，可以使用`word-break:break-all`使单词断开。







再来看看

```html
<style>
by-zhangxinxu {
    display: flex;    
}
item-zxx {
    padding: 1em;
    border: 1em solid deepskyblue;
    color: deepskyblue;
    outline: 1px dashed #fff;
}
item-zxx:nth-child(1) {
    width: 100px;    
}
item-zxx:nth-child(2) {
    width: 100px;    
    box-sizing: border-box;
}
item-zxx:nth-child(3) {
    flex-basis: 100px;
}
item-zxx:nth-child(4) {
    flex-basis: 100px;    
    box-sizing: border-box;
}
</style>
HTML代码：
<by-zhangxinxu>
    <item-zxx>我设置了width:100px</item-zxx>
    <item-zxx>我设置了width:100px，同时box-sizing: border-box</item-zxx>
    <item-zxx>我设置了flex-basis:100px</item-zxx>
    <item-zxx>我设置了flex-basis:100px，同时box-sizing: border-box</item-
```

![](css3_img\flex-basis与width6.png)

上面代码demo就足够展现`flex-basis`和`width`属性的区别了。例如第4项设置的`flex-basis`值是`100px`，同时`box-sizing`是`border-box`，但是最终表现出来的尺寸却不是`100px`，却是个更大的宽度值，如下图所示：

![](css3_img\flex-basis与width7.png)

主要是因为这一项里面的内容，`'basis:100px'`会当做一个独立的单词，并作为此时`flex-basis`默认就有的最小宽度，这个宽度值等同于`min-content`的计算尺寸。

我们可以对比一下隔壁`width:100px`的效果，可以看到文字内容`'width:100px'`直接溢出到了容器之外。

![](css3_img\flex-basis与width8.png)











#### **更深入的案例：最小内容宽度、width和flex-basis同时存在的情况**

根据上面的分析（不考虑容器尺寸不足或溢出），我们可以得到结论：

- `width:100px` + `flex-basis:auto` = 元素自身100px
- content + `flex-basis:100px` = max(content, flex-basis) = 大于等于100px

再加上**flex-basis优先级是比width高**的结论，我们就可以得到：

- content + `width:100px` + `flex-basis:100px` = content + `flex-basis:100px` = max(content, flex-basis) = 大于等于100px

事实究竟是不是这样呢？我们看一个实际的例子。

CSS和HTML代码分别如下：

```css
by-zhangxinxu {
    display: flex;    
}
item-zxx {
    padding: 1em;
    border: 1em solid deepskyblue;
    color: deepskyblue;
    box-sizing: border-box;
}
item-zxx:nth-child(1) {
    width: 100px;    
}
item-zxx:nth-child(2) {
    flex-basis:100px;
}
item-zxx:nth-child(3) {
    width: 100px;
    flex-basis: 100px;
}
```

```html
<by-zhangxinxu>
    <item-zxx>我仅设置了width:100px，没有设置了flex-basis:100px。</item-zxx>
    <item-zxx>我没有设置width：100px，但是设置了flex-basis:100px。</item-zxx>
    <item-zxx>我设置了width：100px，同时设置了flex-basis:100px</item-zxx>
</by-zhangxinxu>
```

如果`flex-basis`优先级是比`width`高，则第3个子项的`width:100px`应该是忽略的，也就是项目2和项目3的渲染表现是一样的。

我们把上面的代码在各个浏览器下跑一下，结果发现出现了尴尬的事情：

在Chrome浏览器下，项目3和项目1表现是一样的，也就是`width:100px`依然起到了一些作用。

![](css3_img\flex-basis与width9.png)

但是，在Firefox浏览器以及IE Edge浏览器下都是符合预期的项目3和项目2表现一样。

![](css3_img\flex-basis与width10.png)

![](css3_img\flex-basis与width11.png)

按照历史的经验，大概率Chrome浏览器会在N年之后的什么时候修改这个表现，和Firefox等浏览器一致。

至于谁对谁错啊，这个是没有定论的，规范中估计没有对这个细节进行特别详细的描述。

那给我们的启示是什么东西呢？那很简单，**flex-basis数值属性值和width数值属性值不要同时使用**，就不会遇到浏览器发现差异的问题。说的更极端一点，在Flex布局中，除非万不得已，否则没有使用`width`属性的理由，请使用`flex-basis`代替。







#### flex-basis与min-width/max-width

很多前端在Flex布局的时候会使用`min-width`或者`max-width`限制列表上的最大尺寸和最小尺寸。

实际上很多时候都是没有必要的，活用`flex-basis`属性和`flex`属性，可以实现类似的效果。

例如我希望一个列表中的每一项最小宽度是100像素，尽可能填满整个容器，请问该如何实现？

无需使用`min-width`属性，可以试试下面的CSS代码：

```css
.container {
    display: flex;
    flex-wrap: wrap;
}
.item {
    flex-basis: 100px;
    flex-grow: 1;
}
```

![](css3_img\flex-basis与max-width.gif)





#### flex-basis 与属性关键字

以前以为`flex-basis`只支持数值，最近才发现，`flex-basis`还支持一大波关键字属性值，包括：

```css
/* 根据flex子项的内容自动调整大小 */
flex-basis: content;

/* 内部尺寸关键字 */
flex-basis: fill;
flex-basis: max-content;
flex-basis: min-content;
flex-basis: fit-content;
```

但是我要提前先泼一波冷水，目前，在Chrome浏览器下，上面任何一个关键字属性值都不支持。这是很罕见的场景。

其中，`flex-basis:content`是Edge 12以及Firefox浏览器支持：

![](css3_img\flex-basis与属性关键字.png)







#### 总结

内容最后的总结一下：

- `flex-basis`默认作用在content box上，IE11会忽略box-sizing；
- `width`只是`flex-basis`为auto时候间接生效，其余时候使用优先级更高的`flex-basis`属性值；
- flex子项元素尺寸还与元素内容自身尺寸有关，即使设置了`flex-basis`属性值；
- `flex-basis`数值属性值和`width`数值属性值不要同时使用；
- `flex-basis`还支持很多关键字属性，只不过目前兼容性不太好；
- `flex-basis`使用得当可以实现类似`min-width`/`max-width`的效果。



















### ➕关于flex子项

- 在Flex布局中，flex子元素的设置`float`，`clear`以及`vertical-align`属性都是没有用的。
- Flexbox布局最适合应用程序的组件和小规模布局（一维布局），而Grid布局则适用于更大规模的布局（二维布局），关Grid布局请参见“[写给自己看的display: grid布局教程”一文](https://www.zhangxinxu.com/wordpress/?p=8144)。
- 已经2020年了，Flex老语法不用在管了，舒爽弃之，然后私有前缀也不用再加了，看到就烦。











## *🔳@media 多媒体查询* 

使用@media查询，你可以针对不同的媒体类型定义不同的样式。
@media可以针对不同的屏幕尺寸设置不同的样式，特别是如果你需要设计响应式的页面，@media是非常有用的。
当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面。
媒体类型还支持not和only关键字，它们可以用来更方便的定位某个媒体设备：

```
not:排除某种制定的媒体类型。
@media not print and(color){}

only:指定某种特定的媒体类型，可以用来排除不支持媒体查询的浏览器；
@media only screen and(color){}
```



### CSS3 多媒体类型

基本语法：

```
@media +（ not | only） + 媒体类型 +（and+ 媒体查询）
```

| 值     | 描述                             |
| :----- | :------------------------------- |
| all    | 用于所有多媒体类型设备           |
| print  | 用于打印机                       |
| screen | 用于电脑屏幕，平板，智能手机等。 |
| speech | 用于屏幕阅读器                   |









### @media 的引入方法

**准备工作1：设置Meta标签**

首先我们在使用Media的时候需要先设置下面这段代码，来兼容移动设备的展示效果：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

这段代码的几个参数解释：

- **width = device-width：**宽度等于当前设备的宽度
- **initial-scale：**初始的缩放比例（默认设置为1.0）  
- **minimum-scale：**允许用户缩放到的最小比例（默认设置为1.0）  
- **maximum-scale：**允许用户缩放到的最大比例（默认设置为1.0）  
- **user-scalable：**用户是否可以手动缩放（默认设置为no，因为我们不希望用户放大缩小页面） 





**准备工作2：加载兼容文件JS**

因为IE8既不支持HTML5也不支持CSS3 Media，所以我们需要加载两个JS文件，来保证我们的代码实现兼容效果：

```html
<!--[if lt IE 9]>
  <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
  <script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script>
<![endif]-->
```



**准备工作3：设置IE渲染方式默认为最高(这部分可以选择添加也可以不添加)**

现在有很多人的IE浏览器都升级到IE9以上了，所以这个时候就有又很多诡异的事情发生了，例如现在是IE9的浏览器，但是浏览器的文档模式却是IE8:

为了防止这种情况，我们需要下面这段代码来让IE的文档模式永远都是最新的：

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge，chrome=1">
```

怎么这段代码后面加了一个**chrome=1**，这个[**Google Chrome Frame（谷歌内嵌浏览器框架GCF）**](http://zh.wikipedia.org/wiki/Google_Chrome_Frame)，如果有的用户电脑里面装了这个chrome的插件，就可以让电脑里面的IE不管是哪个版本的都可以使用**Webkit**引擎及**V8**引擎进行排版及运算，无比给力，不过如果用户没装这个插件，那这段代码就会让IE以最高的文档模式展现效果。这段代码我还是建议你们用上，不过不用也是可以的。





**CSS3 Media写法**

我们先来看下下面这段代码，估计很多人在响应式的网站CSS很经常看到类似下面的这段代码：

```css
@media screen and (max-width: 960px){
    body{
        background: #000;
    }
}
```

这个应该算是一个**media**的一个标准写法，上面这段CSS代码意思是：当页面**小于960px**的时候执行它下面的CSS.这个应该没有太大疑问。

很多网站都会直接省略screen,因为你的网站可能不需要考虑用户的设备时，你可以直接这样写：



**CSS2 Media用法**

其实并不是只有CSS3才支持**Media**的用法，早在CSS2开始就已经支持Media，具体用法，就是在HTML页面的**head**标签中插入如下的一段代码：

```html
<link rel="stylesheet" type="text/css" media="screen" href="style.css">
```

既然CSS2可以实现CSS的这个效果为什么不用这个方法呢，很多人应该会问，但是上面这个方法，最大的弊端是他**会增加页面http的请求次数**，增加了页面负担，我们用CSS3把样式都写在一个文件里面才是最佳的方法。





**组合：**

```css
@media screen and (min-width:960px) and (max-width:1200px){
    body{background:yellow;}
}
```









### @media参数汇总

| **值**                  | **描述**                                                     |
| ----------------------- | ------------------------------------------------------------ |
| aspect-ratio            | 定义输出设备中的页面可见区域宽度与高度的比率                 |
| color                   | 定义输出设备每一组彩色原件的个数。如果不是彩色设备，则值等于0 |
| color-index             | 定义在输出设备的彩色查询表中的条目数。如果没有使用彩色查询表，则值等于0 |
| device-aspect-ratio     | 定义输出设备的屏幕可见宽度与高度的比率。                     |
| device-height           | 定义输出设备的屏幕可见高度。                                 |
| device-width            | 定义输出设备的屏幕可见宽度。                                 |
| grid                    | 用来查询输出设备是否使用栅格或点阵。                         |
| height                  | 定义输出设备中的页面可见区域高度。                           |
| max-aspect-ratio        | 定义输出设备的屏幕可见宽度与高度的最大比率。                 |
| max-color               | 定义输出设备每一组彩色原件的最大个数。                       |
| max-color-index         | 定义在输出设备的彩色查询表中的最大条目数。                   |
| max-device-aspect-ratio | 定义输出设备的屏幕可见宽度与高度的最大比率。                 |
| max-device-height       | 定义输出设备的屏幕可见的最大高度。                           |
| max-device-width        | 定义输出设备的屏幕最大可见宽度。                             |
| max-height              | 定义输出设备中的页面最大可见区域高度。                       |
| max-monochrome          | 定义在一个单色框架缓冲区中每像素包含的最大单色原件个数。     |
| max-resolution          | 定义设备的最大分辨率。                                       |
| max-width               | 定义输出设备中的页面最大可见区域宽度。                       |
| min-aspect-ratio        | 定义输出设备中的页面可见区域宽度与高度的最小比率。           |
| min-color               | 定义输出设备每一组彩色原件的最小个数。                       |
| min-color-index         | 定义在输出设备的彩色查询表中的最小条目数。                   |
| min-device-aspect-ratio | 定义输出设备的屏幕可见宽度与高度的最小比率。                 |
| min-device-width        | 定义输出设备的屏幕最小可见宽度。                             |
| min-device-height       | 定义输出设备的屏幕的最小可见高度。                           |
| min-height              | 定义输出设备中的页面最小可见区域高度。                       |
| min-monochrome          | 定义在一个单色框架缓冲区中每像素包含的最小单色原件个数       |
| min-resolution          | 定义设备的最小分辨率。                                       |
| min-width               | 定义输出设备中的页面最小可见区域宽度。                       |
| monochrome              | 定义在一个单色框架缓冲区中每像素包含的单色原件个数。如果不是单色设备，则值等于0 |
| orientation             | 定义输出设备中的页面可见区域高度是否大于或等于宽度（横竖屏）。 |
| resolution              | 定义设备的分辨率。如：96dpi, 300dpi, 118dpcm                 |
| scan                    | 定义电视类设备的扫描工序。                                   |
| width                   | 定义输出设备中的页面可见区域宽度。                           |

![](css3_img\media参数.png)

![](css3_img\media参数2.png)

```
1. (orientation:portrait)  竖屏
2. (orientation:landscape)  横屏
```









### max(min)-device-width和max(min)-width区别

1.max-device-width是设备整个显示区域的宽度，例如，真实的设备屏幕宽度。

2.max-width是目标显示区域的宽度，例如，浏览器宽度。
3.如果使用max-device-width，那么在PC浏览器上浏览网页时，缩小或放大浏览器时是不执行CSS的，因为‘PC设备’没有变化。但如果使用max-width，缩小或放大浏览器时是执行CSS的，因为显示区域即浏览器大小发生了变化。
4.如果使用max-device-width，那么当手机由竖变横时，CSS是执行的，因为显示区域发生了变化。
5.通常，面向移动设备用户使用max-device-width；面向PC设备用户使用max-width。
看看下面的写法：

```css
/*移动设备*/
@media screen and (max-device-width:480px){
    /*CSS代码*/
}
/*PC*/
@media screen and(max-width:1024px){
    /*CSS代码*/
}
```

min-device-width和min-width的区别，跟max-device-width和max-width的区别是一样的。





### 简单示例

使用多媒体查询可以在指定的设备上使用对应的样式替代原有的样式。

以下实例中在屏幕可视窗口尺寸小于 480 像素的设备上修改背景颜色:

实例

```css
@media screen and (max-width: 480px) {
    body {
        background-color: lightgreen;
    }
}
```



以下实例在屏幕可视窗口尺寸大于 480 像素时将菜单浮动到页面左侧：

实例

```css
@media screen and (min-width: 480px) {
    #leftsidebar {width: 200px; float: left;}
    #main {margin-left:216px;}
}
```





