# 基本结构标签







### 





## 结构标签

| 标签名          | 定义         | 说明                                                         |
| --------------- | ------------ | ------------------------------------------------------------ |
| `<!DOCTYPE>`    | 文档类型声明 | 告诉浏览器使用什么版本的HTML，处于<html>标签之前             |
| <html></html>   | HTML标签     | 页面中最大的标签，称为根标签<br />指定lang字符集例如：en，zh-CN，作用于浏览器和搜索引擎 |
| <head></head>   | 文档的头部   | <meta charset="UFT-8">指定HTML文档的字符编码<br /><title></title>在文档的头部我们必须要设的标签是title<br /><meta name="viewport"> 视口标签 |
| <title></title> | 文档的标题   | 网页标题                                                     |
| <body></body>   | 文档的主体   | 文档的内容，页面的内容                                       |





















  





### 表单标签

**输入**`<input /> `

- 属性`type`必须属性。`button`, `checkbox`, `file`, `hidden`, `image`, `password`, `radio`, `reset`, `submit`, `text`
- HTML5新增`type`属性`email` `url` `date` `time` `month` `week` `number` `tel` `search` `color`
- 属性：`name`, `value`, `checked`, `maxlength`
- HTML5新增属性 `require` `placeholder` `autofocus` `autocomlete` `mutiple`
- 去除表单默认轮廓线`input { outline: none;}`

```html
<input type="text" name="text1" />
<input type="password" name="password" />

<input type="radio" name="radio1" id="radio1-1" />
<label >选项一</label>
<input type="radio" name="radio1" id="radio1-2" />
<label for="radio1-2">选项二</label>
```



# 元素分类

- **块级元素 `block`**：独占一行，自动换行， 可设置高宽

  `div`, `p`，`section`, `article`, `aside`, `h1-h6`, `pre`, `ul`, `ol`, `li`, `form`, `table`, `label`

- **行内元素 `inline`**：不会独占一行，无法设置宽高

  `span`, `em`, `strong`, `a`, `img`, `i`, `sub`,`sup`

- **行内块元素 `inline-block`**：对外，和`inline`元素一样不会独占一行。对内，可设置高宽

  `select`, `input`, `button`, `textarea`, `img`



# 元素嵌套关系

- ##### 块级元素可以包含行内元素 

- ##### 块级元素不一定能包含块级元素

  `<p>`中就不能包含`<div>`

- ##### 行内元素一般不能包含块级元素

  `<a>`元素是可以包含块级元素









# HTML文档结构优化

大纲算法工具 https://h5o.github.io/ 



# SEO优化

`title` `description` `keyword`三大标签

**网站标题**`title`

- 网站名(产品名)-网站的介绍(尽量不超过30汉字)

**网站说明**`description`

- `<meta name="description" content="网站说明" />`

**关键字**`keywords`

- 最好限制为6~8个关键词，关键词之间用英文逗号隔开，采用关键词1，关键词2的形式
- `<meta name="keywords" content="关键字1,关键字2" />`

**LOGO**

1. logo里面首先放一个`h1`标签，目的为了提权，告诉搜索引擎，这个地方很重要
2. `h1`里面再放一个链接，可以返回首页，把logo的背景图片给链接
3. 为了让搜索引擎收录，链接里面要放文字(网站名称),但是文字不要显示出来
   - 方法1：`text-indent`移到盒子外面(`text-indent:-9999px`)，然后`overflow:hidden`
   - 方法2：直接给`font-size: 0`
4. 最后给链接一个`title`属性

```html
<h1 class="logo">
  <a href="#" title="网易云音乐">网易云音乐</a>
</h1>
```

```css
.logo {
  background-image: url(./images/topbar_sprite.png);
}
.logo a {
  display: block;
  width: 157px;
  padding-right: 20px;
  text-indent: -9999px;
  overflow:hidden;
}
```



