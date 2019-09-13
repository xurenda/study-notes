# CSS Reset

## 什么是 CSS Reset 及为什么 CSS Reset
CSS Reset 简单地说就是重置浏览器的 CSS 默认属性。<br />不同的浏览器都有一些自带的或者共有的默认样式，会造成一些布局上的困扰。也就是说同样的 CSS 代码在不同的浏览器中会有不同的样式表现。<br />css Reset 的作用就是重置这些默认样式，让各个浏览器的 CSS 样式有一个统一的基准，即：**样式表现一致**。

## CSS Reset 参考示例

使用前请参见：[CSS Reset 的重新审视](#8wabin)

```css
/* CSS Reset */
html,body {
    margin: 0;
    padding: 0;
}
body {
    background-color: #fff;
    color: #333;
    font-size: 14px;
    font-family: "Arial", "Microsoft YaHei", "黑体", "宋体", sans-serif;
    line-height: 1.2;
}
ul, ol {
    margin: 0;
    padding: 0;
    list-style: none;
}
h1, h2, h3, h4, h5, h6 {
    font-weight: normal;
    font-size: 100%;
}
a {
    color: #333;
    text-decoration: none;
}
img, input {
    border: none;
    vertical-align: middle;
}
table {
    border-collapse: collapse;
    border-spacing: 0;
}
caption, em, strong, th { /* 不常用的标签：var, address, cite, dfn, code，有需要则加入 */
    font-style: normal;
    font-weight: normal;
}
hr {
    display: block;
    height: 1px;
    border: 0;
    border-top: 1px solid #333;
    margin: 1em 0;
    padding: 0;
}
:focus {
	outline-style: none;
}
::selection {
    background-color: #b3d4fc;
    text-shadow: none;
}

/* 公共类 */
.pull-right {
    float: right;
}
.pull-left {
    float: left;
}
.text-right {
    text-align: right;
}
.text-left {
    text-align:left;
}
.text-center {
    text-align: center;
}
.hide {
    display: none;
}
.show {
    display: block;
}
.text-hide {
    font-size: 0;
    line-height: 0;
    color: transparent;
    background-color: transparent;
    text-shadow: none;
    border: none;
}
.center-block {
    display: block;
    margin: 0 auto;
}
.text-ellipsis {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
.clear-fix::before, /* before为了防止上下margin叠加 */ 
.clear-fix::after {
    content: "";
    display: table;
}
.clear-fix::after {
    clear: both;
}
```

## CSS Reset 的重新审视
### CSS Reset 的初衷
最初出现 CSS Reset 的目的是解决浏览器在默认样式上的诸多差异和问题。而在实际应用中，人们常常使用 CSS Reset 把所有的浏览器默认样式**全部****“清零”**！<br />同时，随着时间的推移，现代浏览器在这方面的差异已经小了很多，所以 CSS Reset 的必要性就不那么高了。

### CSS Reset 的滥用
物极必反，CSS Reset 提供了很大的便利这点毋庸置疑，但是 CSS Reset 的滥用情况也实在是比比皆是，Eric 在其 reset 代码页面中提到：**要根据您自己的要求做修改**。<br />然而，目前的状态是（尤其一些中小型网站），CSS Reset 代码直接拷贝过去，也不做一番思考，实在是有违原意。<br />如下 CSS Reset 代码以作**反面**示例：
```css
body, div, dl, dt, dd, ul, ol, li, h1, h2, h3, h4, h5, h6, pre, code, form, fieldset, legend, input, button, textarea, p, blockquote, th, td {
    margin: 0;
    padding: 0;
}
```

1. `div` `li` `form` `input` `button` `textarea` 等标签默认无 `margin` 值和 `padding` 值。应用 `margin:0; padding:0;` 实属画蛇添足，多此一举。

2. `dt` 标签有默认的 `margin` 与 `padding` 值就是 0，亦是多此一举。

3. `code` 标签是个属于 `inline` 水平的元素，居然也扯到 `margin` 与 `padding` 的重置。

4. `fieldset` `legend` 这种使用概率不足 1% 的标签也拿来重置，实在无语。


修改如下：
```css
body, dl, dd, h1, h2, h3, h4, h5, h6, p, form {
    margin: 0;
}
ol,ul {
    margin: 0;
    padding: 0;
}
```

### CSS Reset 的不足

1. 不当的破坏了所有浏览器的基本样式。最典型的做法就是将所有元素的 `margin/padding` 设为 0，及去掉 `ol/ul` 的列表样式，去掉 `h1~h6` 的字体大小样式。这导致 `blockquote`、`ol`、`ul`、`hn` 等语义元素在没有赋以其他合理的样式时（常常如此），缺乏恰当的样式展现。而因为视觉上无法区分，这进一步导致许多开发人员忽视或误用这些语义元素，并严重破坏团队内其他希望使用语义元素的开发人员的工作流程。


2. 浏览器为配合控件风格，或出于可用性和无障碍性考虑，会有一些特定的默认样式。如一些浏览器对于 `input[type=radio]` 默认有 `margin`，以确保单选框的外观比较协调。粗暴的 reset 导致控件外观可能在特定浏览器下失调。又如大部分交互控件在获得焦点时有虚线 `outline`。去掉这样式而又不提供其他对于焦点的视觉反馈机制（常常如此）可能导致可用性和无障碍性问题。


3. 某些 use case 中，网页中有有一些较高独立性的部分，比如内嵌编辑器、widget、第三方内容等，CSS Reset 导致这部分的样式常需要完全重新覆写，而这种覆写本来可能大部分是不必要的，令人烦躁且可能遗漏。


4. CSS Reset 通常会增加浏览器进行样式计算的成本（即有一定的性能负担），因为它引入了许多的针对元素的全局规则，网页中几乎所有元素都会匹配一条乃至几条的reset规则，且往往规则中的属性设定其实会被更 specific 的规则所覆盖（比如 `padding` 和 `margin` ）。极端情况下，可能某条 reset 规则中的所有属性设置实际上并没有在任何元素上生效（因为全部被更 specific 的规则给覆盖了），所有针对此规则的级联计算全都是浪费。


### 解决方法

1. 不要使用将浏览器默认样式全部“清零”的，有违 reset 初衷的 CSS Reset，而是使用那些谨慎的，只是用来消除浏览器默认样式不一致的真正的 CSS Reset。（参考 Normalize.css：非常温和的 CSS Reset 替代方案，具体可以看 normalize.css 的作者 Nicolas 写的一篇文章：[About normalize.css](http://nicolasgallagher.com/about-normalize-css/) ，另外也可以参考网友译文：[Jerry的乐园 | 来，让我们谈一谈 Normalize.css](http://jerryzou.com/posts/aboutNormalizeCss/) ）


2. 对于特定的 reset 需求，只在真正需要的时候，单独设置。若要提高代码复用程度，可以引入 CSS 预处理，以 mixin 来做例子：

```
@mixin reset-box() {
  list-style: none
  padding: 0
  margin: 0
}
@mixin reset-font() {
  font-size: 1em
  font-weight: normal
  font-style: normal
}

//真正用的场景
html#示例网站 {
  nav#全站导航条 ul {
    border: 1px solid
    +reset-box()
  }
  section.侧栏模块 {
    border: 1px solid
    h1 {
      background: navy
      color: white
      border-bottom: 1px solid
      +reset-box()
      +reset-font()
    }
  }
}
```
以上以伪代码书写，请自行脑补转换为 SASS/Stylus/LESS 等。

3. 限制 CSS Reset 的作用全局性。可以籍由支持局部 import 的 CSS 预处理器（比如 stylus）的帮助，从限定其在特定 selector 之下开始，逐步控制其影响范围。如：

```
html#示例网站 {
  
  ... // 不依赖 CSS reset 的部分

  .legacy-part { // 页面中仍依赖 CSS reset 的部分
    @import 'reset.css'
  }
}
```

4. 另外，CSS 3 已加入 unset 关键字和 all 快捷属性（目前仅 Firefox 支持），可以更好的支持必要的 reset 需求。


### 总结
**最少的 CSS 代码，最少的渲染，最少的重置就是最好的 CSS 样式代码，这反应了您的 CSS 层次。**

## CSS Reset 参考附录
### 腾讯QQ官网样式初始化:
```css
body,ol,ul,h1,h2,h3,h4,h5,h6,p,th,td,dl,dd,form,fieldset,legend,input,textarea,select{margin:0;padding:0}
body{font:12px"宋体","Arial Narrow",HELVETICA;background:#fff;-webkit-text-size-adjust:100%;}
a{color:#2d374b;text-decoration:none}
a:hover{color:#cd0200;text-decoration:underline}
em{font-style:normal}
li{list-style:none}
img{border:0;vertical-align:middle}
table{border-collapse:collapse;border-spacing:0}
p{word-wrap:break-word}
```

### 新浪官网样式初始化:
```css
body,ul,ol,li,p,h1,h2,h3,h4,h5,h6,form,fieldset,table,td,img,div{margin:0;padding:0;border:0;}
body{background:#fff;color:#333;font-size:12px; margin-top:5px;font-family:"SimSun","宋体","Arial Narrow";}
ul,ol{list-style-type:none;}
select,input,img,select{vertical-align:middle;}
a{text-decoration:none;}
a:link{color:#009;}
a:visited{color:#800080;}
a:hover,a:active,a:focus{color:#c00;text-decoration:underline;}
```

### 淘宝官网样式初始化:
```css
body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
h1, h2, h3, h4, h5, h6{ font-size:100%; }
address, cite, dfn, em, var { font-style:normal; }
code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
small{ font-size:12px; }
ul, ol { list-style:none; }
a { text-decoration:none; }
a:hover { text-decoration:underline; }
sup { vertical-align:text-top; }
sub{ vertical-align:text-bottom; }
legend { color:#000; }
fieldset, img { border:0; }
button, input, select, textarea { font-size:100%; }
table { border-collapse:collapse; border-spacing:0; }
```

### 网易官网样式初始化:
```css
html {overflow-y:scroll;}
body {margin:0; padding:29px00; font:12px"\5B8B\4F53",sans-serif;background:#ffffff;}
div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,form,fieldset,input,textarea,blockquote,p{padding:0; margin:0;}
table,td,tr,th{font-size:12px;}
li{list-style-type:none;}
img{vertical-align:top;border:0;}
ol,ul {list-style:none;}
h1,h2,h3,h4,h5,h6{font-size:12px; font-weight:normal;}
address,cite,code,em,th {font-weight:normal; font-style:normal;}
```

