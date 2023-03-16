# CSS 设计模式

## OOCSS

面向对象的 CSS

**原则一：容器与内容分离**

- 不要在 CSS 中模仿 HTML 的结构，避免使用后代选择器

- 禁止使用和位置相关的样式还有标签选择器、ID选择器，应该让容器和内容有各自的样式，应用描述相关标签使用的类

  ```html
  <div class="menu">
    <h2 class="menu-title"></h2>
  </div>
  ```

  ```css
  .menu {
    width: 200px;
    height: 200px;
  }
  .menu-title {
    font-size: 20px;
  }
  ```

**原则二：结构（基础对象）与皮肤分离**

- 在基础类中保留结构和位置；在扩展类中保留视觉特征，创建可重用的皮肤

- 皮肤是视觉属性，例如：颜色、字体、阴影、渐变、背景等
- 结构是不可见的视觉属性，例如：高度、宽度、位置、内边距、外边距、隐藏等

```html
<div class="box-border box-1">Learn OOP</div>
<div class="box-border box-2">Learn CSS</div>
```

```css
.box-border{
  border: 1px solid #CCC;
  border-radius: 10px;
}
.box-1 {
  width: 200px;
  height: 200px;
}
.box-2 {
  width: 120px;
  height: 120px;
}
```

```css
/* 不好的方式 
.box-1 {
  border: 1px solid #ccc;
  width: 200px;
  height: 200px;
  border-radius: 10px;
}
.box-2 {
  border: 1px solid #ccc;
  width: 120px;
  height: 120px
  border-radius: 10px;
}
*/
```

核心就是**编写可复用和可维护的样式**



## BEM

BEM 是一个分层系统，将网站分为三层：**块层 Block、元素层 Element、修饰符层 Modifier** 

**BEM 的优点**：保持块的独立性，可以将整个块添加、移动、删除而不影响整体页面

**命名规则**：

<img src="CSS 设计模式.assets/image-20220722155000857.png" alt="image-20220722155000857" style="zoom: 60%;" /> 

- 块名称为其元素和修饰符定义了命名空间

  ```css
  .block {}
  ```

  `.block` 代表了更高级别的抽象或组件，每个块的**块名必须是唯一**的，要**明确描述**出是哪个块，例如 `.head`

  在使用块时，块不应影响其环境，**不应设置块的外部几何形状或位置**

  ```html
  <div class="top">
    <form class="search-form">搜索</form>
  </div>
  <div class="bottom">底部</div>
  ```

  块**应该是独立的**，当在页面中添加，删除，或者是移动某个块时，不需要对块进行修改

  ```css
  /* 错误写法 */
  /* 
  	.top .search-form {}
  */
  /* 正确写法 */
  .top {}
  .search-form {}
  ```

- 块名称与元素名称之间用双下划线  `__` 分隔

  ```css
  .block__element {}
  ```

  **`.block__element` 代表 `.block` 的后代**

  **元素是块的组成部分**，是依赖上下文的，元素之间可以彼此嵌套，元素的**名称用于描述它是什么**，而不是它的状态

  元素在所属的**块中指定位置**时，**才能表现出应有的功能**

  ```html
  <div class="top">
    <form class="search-form">
      <input class="search-form__input"> 
      <button class="search-form__button">搜索按钮</button>
    </form>
  </div>
  ```

  ```css
  /* css代码中，元素可以放到块中 */
  .search-form .search-form__input {}
  .search-form .search-form__button {}
  ```

- 块名称与修饰符或元素与修饰符之间用双连字符 `-- `分隔

  ```css
  .block--modifier {}
  .block--modifier-value {}
  .block__element--modifier {}
  .block__element--modifier-value {}
  ```

  **`.block--modifier` 和 `.block__element--modifier` 代表  `.block`  和 `.block__element` 的不同状态或不同版本**

  修饰符与块、元素一起工作，**不能单独使用**，而且必须绑定在对应的块或元素上，不能混搭

  通常是已定义的**块或者元素上的外观或行为**要有些许改变，这时使用修饰符来处理

  ```html
  <div class="banner__btn">
    <button class=".button .banner__btn--red"></button>  
    <button class=".button .banner__btn--green"></button>
    <button class=".button .banner__btn--blue"></button>   
    <button class=".button .banner__btn--yellow"></button>       
  </div>
  ```

- 命名一般使用小写字母
- 单词之间可以使用 `-` 分隔

通过混合的方式**把位置样式从块中剥离**，**适时拆分元素为独立的块**，解耦样式并形成新的命名空间

```html
<!-- top 块 -->
<div class="top">
  <!-- search-form块混合top块的search-form元素 -->
  <!-- 在top__search-form元素中设置位置浮动等样式，保持了search-form块的样式独立 -->
  <form class="search-form top__search-form">搜索</form>
</div>
```

在 SCSS 中使用 `@at-root` 获得非嵌套的 CSS，编写时保持嵌套格式

```scss
.block {
  @at-root #{&}__element { }
  @at-root #{&}--modifier { }
}
/* 编译后
.block {}
.block__element {}
.block--modifier {}
*/
```



## SMACSS

SMACSS 的核心是分类，具体把项目的样式分为了五类：Base 、Layout 、Module 、State 、Theme 

- Base 基础

  基础（Base）规则里一般**放置默认样式**，基础样式定义了元素在页面的任何位置应该是怎么样的

  一般都是使用 Normalize.css 来实现的

  ```css
  html, body, form { 
    margin: 0;
    padding: 0; 
  }
  ```

- Layout 布局

  **类名都是 `.l-` 开头**

  布局（Layout）定义网站的骨架、**布局样式**

  ```css
  .l-header {}
  .l-primary-nav {}
  .l-main-content {}
  ```

  项目开发一般用的**布局组件**可以当成 Layout ，例如 `<Header />` 

- Module 模块

  **类名使用模块本身的名字即可**

  模块（Module）定义**可重用、可模块化**的部分，例如插图、列表等。（等同于 component  的理解）

  ```html
  <div class="l-container-12">
    <div class="l-grid-06">
      <div class="box">box</div>
    </div>
  </div>
  ```

- State 状态

  **类名以 `.is-` 开头**

  state（状态）用来操作**动态变化的部分**，例如禁用 disable、激活 active、展开 expand 等

  ```css
  .is-collapsed {}
  .is-expanded {}
  .is-error {}
  .is-success {}
  ```

- Theme 主题

  类名以 `.theme-` 开头

  theme 定义**公共类名**部分，例如颜色、形状、边框、阴影等



## ITCSS

ITCSS 的分层分的更细，分为七层：Settings、Tools、Generic、Elements、Objects、Components、Trumps

<img src="CSS 设计模式.assets/tnaq5slf6vq5disuxuqy.jpg" alt="ITCSSとBEMを使用して大規模なCSSのボトルネックを解決する方法 - 開発者ドキュメント" style="zoom:67%;" /> <img src="CSS 设计模式.assets/image-20220722175358957.png" alt="image-20220722175358957" style="zoom: 33%;" />

从倒三角形的平顶到底部尖端的定向流动象征着特异性的增加，即**后面的样式可以影响前面的**

三角形的每层都可以被视为一个单独的文件或一组文件，**每层都是可选的**

- Settings ：包含字体、颜色定义等，通常定义可以**自定义模板的变量**

  主要定义字体以及将在整个主题中使用的颜色变量，以及自定义的所有变量

  <img src="CSS 设计模式.assets/webp.webp" alt="img" style="zoom:50%;" /> 

- Tools：定义预处理器的 mixin、function

  主要定义工具

  <img src="CSS 设计模式.assets/webp-16584811089395.webp" alt="img" style="zoom:50%;" /> 

- Generic：重置或标准化样式、例如 normalize.css

- Elements：定义网站 HTML 元素的样式，也可写为 Base

  <img src="CSS 设计模式.assets/webp-16584811462778.webp" alt="img" style="zoom:50%;" /> 

- Objects：结构布局骨架样式，不包括任何装饰性

  <img src="CSS 设计模式.assets/webp-165848122613111.webp" alt="img" style="zoom: 67%;" /> 

- Components：UI 组件样式

  <img src="CSS 设计模式.assets/webp-165848126487614.webp" alt="img" style="zoom:67%;" /> 

- Trumps：辅助类，唯一可以添加 `important!` 的地方，也可写为 Utilities

  <img src="CSS 设计模式.assets/webp-165848127414317.webp" alt="img" style="zoom:67%;" /> 



## ACSS

原子化 CSS 是一种 CSS 的架构方式，它倾向于**小巧且用途单一的 class**，并且会以视觉效果进行命名

**一个样式属性一个类**

```css
.m-0 {
  margin: 0;
}
.text-red {
  color: red;
}
```

**常用框架**：tailwindcss、windicss



## 参考架构模式

整体采用 ITCSS 架构思想，分为 Tools 层、Base 层、Settings 层、Theme 层、Object 层、Components 层

<img src="CSS 设计模式.assets/image-20220722183617319.png" alt="image-20220722183617319" style="zoom:80%;" /> 

- Base 层：融合了 ITCSS 的 Generic 层和 Base 层

  引入 normalize.css，重置浏览器样式

  对元素基础样式进行补充

- Object 层：融合了 Accs，单一属性样式**采用属性选择器**

  ```css
  [circle] {
    border-radius: 100rem;
  }
  ```

  ```html
  <avatar circle></avatar>
  ```

- Settings 层：定义公共变量

- Theme 层：主题样式

- Components 层：项目的组件就是 Components 层

















