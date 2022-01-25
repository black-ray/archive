# HTML

## 基本结构标签

| 标签名          | 定义         | 说明                                                         |
| --------------- | ------------ | ------------------------------------------------------------ |
| `<!DOCTYPE>`    | 文档类型声明 | 告诉浏览器使用什么版本的HTML，处于<html>标签之前             |
| <html></html>   | HTML标签     | 页面中最大的标签，称为根标签<br />指定lang字符集例如：en，zh-CN，作用于浏览器和搜索引擎 |
| <head></head>   | 文档的头部   | <meta charset="UFT-8">指定HTML文档的字符编码<br /><title></title>在文档的头部我们必须要设的标签是title<br /><meta name="viewport"> 视口标签 |
| <title></title> | 文档的标题   | 网页标题                                                     |
| <body></body>   | 文档的主体   | 文档的内容，页面的内容                                       |

- 视口标签

  ```html
  <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  ```

  - `width` 设置`viewport`宽度，可以设置`device-width`特殊值
  - `initial-scale` 初始缩放比。大于0的数字
  - `maximum-scale` 最大缩放比。大于0的数字
  - `minimum-scale`最小缩放比。大于0的数字
  - `user-scalable`用户是否可以缩放。yes 或 no (1或0)
  
- 当前网页使用IE浏览器最高版本的内核来渲染

  ```html
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  ```



## 常用标签

### 标题标签

**标题标签**`<h1>`-`<h6>`

-  文字加粗，字号依次加大
- 一个标题独占一行



### 段落和换行标签

**段落标签**`<p></p>`

-  可以把HTML文档分割为若干段落
- 文本在一个段落中会根据浏览器的窗口的大小自动换行

**换行标签**`<br />`

- 单标签
- 只是简单的开始新的一行，而段落标签会在段落之间插入一些垂直间距



### 文本格式化标签

**加粗**`<strong></strong>` `<b></b>`

- 推荐使用`<strong>` 语义更强烈

**倾斜**`<em></em>` `<i></i>`

- 推荐使用`<em>`语义更强烈
- HTML5中`<i>`主要用来做图标icon

**删除线**`<del></del>` `<s></s>`

- 推荐使用`<del>`语义更强烈

**下划线**`<ins></ins>` `<u></u>`

- 推荐使用`<ins>`语义更强烈



### 内容标签

**内容标签**`<div></div>` `<span></span>`

- 没有语义，是一个盒子，用来装内容
- `<div>`标签用来布局，一行只能放一个
- `<span>`标签用来布局，一行可以多个



### 语义化标签

HTML5新增，有兼容性问题。在IE9中需要将这些元素转换为块级元素

**头部标签**`header`

**导航标签**`nav`

**内容标签**`article`

**定义文档某个区域**`section`

**侧边栏标签**`aside`

**尾部标签**`footer`



### 图像标签

**图像标签**`<img />`

- `src`是`<img>`的必须属性，用于指定图像文件的路径和文件名

- 其他属性 `src` `alt` `title` `width` `height` `border`
- 相对路径：下一级`/` 上一级`../` 



### 超链接标签

**超链接标签**`<a>`

- `href` 跳转目标

  ```html
  <a href="http://www.qq.com">腾讯网</a>
  ```

- `target` 链接页面的打开方式，`_self`为默认值，`_blank`在新窗口打开

  ```html
  <a href="http://www.taobao.com" target="_blank">淘宝网</a>
  ```

- 外部链接 ，内部链接，空链接`#`，锚点链接`#name`

- `href`的地址是文件或者压缩包，会下载这个文件

- 各种网页元素，如文本，图像，表格，音频，视频都可以添加超链接

-  阻止链接跳转 `javascript:void(0);` 或者` javascript:;`



### 表格标签

**表格标签**`<table></table>`

- 属性`align`, `border`,`cellpadding`,`cellspacing`,`width`

**表头标签**`<thead></thead>`

**表格主体标签**`<tbody></tbody>`

**表格中的行**`<tr></tr>`

- 必须嵌套在`<table></table>`中

**单元格**`<td></td>`

- 必须嵌套在`<tr></tr>`中
- 跨行合并`rowspan`，最上侧的单元格为目标单元格
- 跨列合并`colspan`，最左侧的单元格为目标单元格

**表头单元格**`<th></th>`

- 一般位于表格的第一行或者第一列，文本内容会加粗居中显示

```html
<table border="1">
    <thead>
        <tr>
            <th>表头1</th>
            <th>表头2</th>
            <th>表头3</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>数据1</td>
            <td>数据2</td>
            <td>数据3</td>
        </tr>
        <tr>
            <td colspan="2">数据1</td>
            <td>数据3</td>
        </tr>
        <tr>
            <td rowspan="2">数据1</td>
            <td>数据2</td>
            <td>数据3</td>
        </tr>
        <tr>
            <td>数据2</td>
            <td>数据3</td>
        </tr>
    </tbody>
</table>
```



### 列表标签

**无序列表**`<ul></ul>` `<li></li>`

- `ul`中只能嵌套`li`,`li`里面可以嵌套任何元素
- 会带有自己的样式属性

**有序列表**`<ol>` `<li>`

- `ol`中只能嵌套`li`,`li`里面可以嵌套任何元素
- 会带有自己的样式属性
- 去除li前面的小圆点`list-style: none`

**自定义列表**`<dl></dl>` `<dt></dt>` `<dd></dd>`

- ```html
  <dl>
  	<dt>关注我们</dt>
      <dd>新浪微博</dd>
      <dd>官方微信</dd>
      <dd>联系我们</dd>
  </dl>
  ```

- `dl`用于定义描述列表，只能包含`dt`和`dd`
- `dt`定义项目，一个`dt`可以对应多个`dd`
- `dd`描述每一个项目



### 表单标签

**表单域**`<form></form>`

- 会把范围内的表单元素信息提交给服务器
- 属性：`action` `method` `name`

```html
<form method="GET" action="http://www.qq.com">
</form>
```

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

```html
<button type="button">普通按钮</button>
<button type="submit">提交按钮一</button>
<input type="submit" value="提交按钮二"/>
<button type="reset">重置按钮</button>
```

**标签**`<lable></lable>`

- 属性`for`可以绑定一个表单元素的`id`属性，点击`lable`光标跳转到表单元素

**下拉列表**`<select></select>` `<option></option>`

```html
<select name="select1">
    <option value="1">一</option>
    <option value="2" selected>二</option>
</select>
```

**文本域**`<textarea></textarea>`

- 属性`col`每行的字符数,`rows`显示的行数
- 去除文本域拖拽`textarea { resize: none;}`



## 元素分类

- **块级元素 `block`**：独占一行，自动换行， 可设置高宽

  `div`, `p`，`section`, `article`, `aside`, `h1-h6`, `pre`, `ul`, `ol`, `li`, `form`, `table`, `label`

- **行内元素 `inline`**：不会独占一行，无法设置宽高

  `span`, `em`, `strong`, `a`, `img`, `i`, `sub`,`sup`

- **行内块元素 `inline-block`**：对外，和`inline`元素一样不会独占一行。对内，可设置高宽

  `select`, `input`, `button`, `textarea`, `img`



## 元素嵌套关系

- ##### 块级元素可以包含行内元素 

- ##### 块级元素不一定能包含块级元素

  `<p>`中就不能包含`<div>`

- ##### 行内元素一般不能包含块级元素

  `<a>`元素是可以包含块级元素



## 多媒体标签

**视频**`video`

- `<video src="文件地址" controls="contrils"></video>`
- 属性`autoplay` `controls` `width` `height` `loop` `perload` `src` `poster` `muted`
- 谷歌浏览器设置自动播放需要添加`muted`

- 支持MP4,WebM,Ogg格式文件，大部分浏览器都支持MP4，建议使用MP4

**音频**`audio`

- `<audio src="文件地址" controls="contrils"></audio>`
- 属性`autoplay` `controls` `loop` `src`
- 支持MP3,Wav,Ogg格式文件，大部分浏览器都支持MP3，建议使用MP3
- 谷歌把自动播放禁止了



## 自定义属性

H5规定自定义属性data-开头做为属性名并且赋值。

```html
<div data-index=“1”></div>
```

或者使用 JS 设置

```javascript
element.setAttribute(‘data-index’, 2)
```



## 网站favicon图标

目前主要浏览器都支持favicon.ico图标。

favicon.ico图标放到根目录

在`<head></head>`元素之间引入代码

```html
<link ref="shortcut icon" href="favicon.ico" type="image/x-icon"/>
```



## 特殊字符

| 特殊字符 | 描述     | 代码       |
| -------- | -------- | ---------- |
|          | 空格     | `&nbsp;`   |
| <        | 小于     | `&lt;`     |
| >        | 大于     | `&gtl;`    |
| &        | 和       | `&amp;`    |
| ￥       | 人民币   | `&yen;`    |
| ©        | 版权     | `&copy;`   |
| ®        | 注册商标 | `&reg;`    |
| ±        | 正负     | `&plusmn;` |
| ×        | 乘       | `&times;`  |
| ÷        | 除       | `&divide;` |
| ²        | 平方     | `&sup2;`   |
| ³        | 立方     | `&sup3;`   |
| °        | 度       | `&deg;`    |



## HTML文档结构优化

大纲算法工具 https://h5o.github.io/ 



## SEO优化

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



