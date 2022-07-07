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

















### 语义化标签

HTML5新增，有兼容性问题。在IE9中需要将这些元素转换为块级元素

**头部标签**`header`

**导航标签**`nav`

**内容标签**`article`

**定义文档某个区域**`section`

**侧边栏标签**`aside`

**尾部标签**`footer`





  





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



# 多媒体标签

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



# 浏览器运行原理

1. ##### 构建DOM树

   渲染引擎解析HTML文档，首先将**标签**转换成DOM树中的**DOM节点**（包括js生成的标签）生成**内容树**(content tree/DOM tree)

2. ##### 构建渲染树

   解析对应的**CSS样式**信息（包括js生成的样式，外部css文件，带有样式的HTML标签）构建**渲染树**(rendering tree)

   渲染树中每个节点都有自己的style

   渲染树**不包含隐藏的节点**(display: none的节点)和head节点，因为这些节点不会显现

3. ##### 布局渲染树

   从根节点递归调用，计算每一个元素的**大小，位置**等

   给出每个节点所应该在屏幕上出现的精准**坐标**

4. ##### 绘制渲染树

   遍历渲染树，使用UI层来绘制每个节点

<img src="HTML.assets/image-20220629152425616.png" alt="image-20220629152425616" style="zoom: 50%;" /> 



# 重绘重排

## 重绘

当**盒子的位置**，**大小**以及其他属性，例如**颜色**，**字体大小**等都确定下来以后，浏览器会按照各自的特性绘制一遍，将内容呈现

重绘是指**元素外观的改变**，触发了浏览器根据元素的新属性**重新绘制**的行为，使元素呈现新的外观

**触发重绘的条件**：**改变元素外观属性**。如：color，background-color等

**table**及其内部元素可能需要**多次计算才能确定**好其在渲染树中节点的属性值，比同等元素要多花两倍时间，尽量避免使用table布局



## 重排

重排又称为回流，重构

当渲染树的一部分或全部，因为元素的**规模尺寸**，**布局**，**隐藏**等改变而需要重新构建，这称为重排

每个页面至少需要一次重排，就是页面第一次加载的时候

- ##### 重绘和重排的关系

  在回流的时候，浏览器会使渲染树中受到影响的部分失效，并重新构建这部分的渲染树

  完成回流后，浏览器会重新绘制受影响的部分到屏幕，该过程称为重绘

**重排必定会引发重绘，但重绘不一定会引发重排**

- ##### 触发重排的条件

  任何**页面布局**和**几何属性**的改变都会触发重排

  1. 页面渲染**初始化**（无法避免）

  2. 添加或删除可见的**DOM元素**

  3. 元素**位置改变**，或者使用**动画**

  4. **元素尺寸改变**——大小，外边距，边距

  5. 浏览器**窗口**尺寸的变化（resize事件发生时）

  6. 填充**内容改变**，比如文本数量和图片大小改变而引起的计算值宽度和高度的改变

  7. 查询读取某些元素属性

     例如：offsetLeft/Top/Height/Width，clientTop/Left/Height/Width，scrollTop/Left/Width/Height，width/height，getComputedStyle()，currentStyle

     当用js操作DOM的时候，浏览器并不是立马执行的，而是将操作存储在一个队列中。

     当到达一定数量或者时间时，浏览器才会统一执行队列中的操作。

     当查询这些属性时，浏览器就会强制刷新队列。如果不立马执行队列中的操作，有可能得到的结果是错误的。

     如果之前没有改变几何元素，获取offset等属性也不会产生重排



## 优化

重绘重排的代价：耗时，浏览器卡慢

- ##### 浏览器的优化

  浏览器会维护一个队列，会把所有会引起回流、重绘的操作放入这个队列，等队列中的操作到了一定的数量或者到了一定时间后，浏览器就会自动flush队列，进行一个批处理。这样就能让多次回流、重绘变成一次回流重绘

- ##### 代码的优化

  要减少对渲染树的操作。可以合并多次的DOM和样式的修改。减少对style样式的请求

  - 直接**改变元素的className**

  - 先设置元素为display: none; 然后进行页面布局等操作

    完成操作后将元素设置为display: block;

    这样只会引发两次重绘和重排

  - 使用cloneNode和replaceChild，引发一次回流和重绘

  - 将需要多次重排的元素，**position属性设置成absolute或fixed**。元素脱离了文档流，它的变化不会影响到其他元素

  - 如果需要创建多个DOM节点，可以使用DocumentFragment创建完后一次性的加入document

    ```javascript
    // 不要循环渲染，建议使用DocumentFragment
    var fragment = document.createDocumentFragment();
    for(let i = 0; i< 1000; i++) {
        var li = document.createElement('li');
        li.innerHtml = 'apple' + i;
        fragment.appendChild(li);
    }
    document.getElementById('fruit').appendChild(fragement);
    ```

  - offset之类的属性读取注意顺序

    ```javascript
    // 产生了四次重排
    var div = document.querySelector("#test");
    div.style.left = '200px';
    console.log(div.offsetLeft);
    div.style.left = '100px';
    console.log(div.offsetLeft);
    div.style.left = '200px';
    console.log(div.offsetLeft);
    div.style.left = '100px';
    console.log(div.offsetLeft);
    ```

    ```javascript
    // 只发生了一次重排
    var div = document.querySelector("#test");
    div.style.left = '200px';
    div.style.left = '100px';
    div.style.left = '200px';
    div.style.left = '100px';
    console.log(div.offsetLeft);
    console.log(div.offsetLeft);
    console.log(div.offsetLeft);
    console.log(div.offsetLeft);
    ```

    

 
