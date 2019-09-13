# stylus

> `CSS` 预处理器：`stylus`



## 缩排

可**省略**的 `{}` `:` `;` `,`，利用**缩进**表征嵌套关系：

```css
textarea,
input {
  border: 1px solid #eee;
}
```

`=>`

```stylus
textarea
input
  border 1px solid #eee
```

> tip：选择器有歧义（可能看作属性），不省略 `,`，以提高阅读性。



## 层级嵌套

```css
textarea,
input {
  color: #a7a7a7;
}
textarea:hover,
input:hover {
  color: #000;
}
```

`=>`

```stylus
textarea
input
  color #A7A7A7
  &:hover
    color #000
```

`&`：父级引用，用于后面也可以：`html.ie6 &`



## 消除歧义

```stylus
pad(n)
  padding (- n)

body
  pad(5px)
```

`-` 可理解为减，也可理解为负，添加 `()` 消除歧义。



## 变量

```
$font-size = 14px
$font = $font-size "Lucida Grande", Arial

body
  font $font sans-serif
  
$font[1]  // "Lucida Grande"
```

- 为了提高阅读性，变量名最好使用 `$` 开头；

- 变量有多个值时，可通过 `[index]` 取得具体值。



## 学习参考

官网：http://stylus-lang.com/

中文文档 - 张鑫旭：https://www.zhangxinxu.com/jq/stylus/

