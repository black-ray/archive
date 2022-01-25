**less是一门css预处理语言，扩展了css的动态特性**



# 变量

**@变量名:值;**

必须有@前缀，不能包含特殊字符，不能数字开头，大小写敏感

```less
@color: pink;
body {
    background-color: @color;
}
```

- ##### 选择器变量

  ```less
  // 选择器变量
  @btn: a;
  @{btn} {
  	background-color: @color;
  }
  @info: warn;
  .@{info} {
      background-color: @color;
  }
  ```

  ```html
  <button class="warn">按钮</button>
  ```

- ##### 字符串变量

  ```less
  @images:"./assets/img/";
  #d1 {
      background-image: url("@{images}picture.jpg");
  }
  ```

- ##### 样式对象变量

  ```less
  @smallBtn: {
      width: 100px;
      height: 50px;
      color: @color;
      background-color: orangered;
  }
  .small-success-btn {
  	@smallBtn();
      background-color: green;
  }
  ```

  ```html
  <button class="small-success-btn">成功按钮</button>
  ```

- ##### 变量参与运算

  ```less
  .input {
      // 变量是有作用域的
  	@height: 30px;
      @width: @height*15;
      width: @width;
      heigh: @height;
      background-color: @color - #378;
  }
  ```



# 嵌套

```less
/* CSS */
#header .logo {
    width: 300px;
}

/* LESS */
#header {
    .logo {
        width: 300px;
    }
}
```

- ##### 交集，伪类，伪元素选择器

  &符号表示平级关系

  使用&符号，就会被解析为父元素自身或父元素的伪类

  内层选择器前面没有&符号，则被解析为父选择器的后代
  
  ```less
  a {
      color: red;
      &:hover {
          color: blue;
          &::before {
              color: blue;
          }
      }
      &::before {
          content: "";
          color: red;
      }
  }
  ```
  
- ##### 套用现有样式

  ```less
  .card {
      width: 400px;
      height: 300px;
      border-raduis: 20px;
      background-color: salmon;
  }
  .cardYellow {
      // 调用已有的。不加:{} 
      // 等价于.card();
      // 如果需要提升权重 .card !important
      .card;
      background-color: yellow;
  }
  ```

  ```html
  <div class="card"></div>
  <div class="cardYellow"></div>
  ```



# 运算

数字，样式，变量都可以参入运算。提供加减乘除运算

运算符两侧必须用一个空格隔开

```less
div {
    width: 200px - 50;
    height: 200px * 2;
}
img {
    width: 82 / 50rem;
}
div {
    width: (@width + 5) * 2;
}
```

- 两个数运算，只有一个数有单位，最后结果就按该单位

  两个数运算，都有单位，结果是第一个单位



# 函数

```less
// 可以设置默认参数.card(@bgColor: salmon){}
.card(@bgColor,@width:400px) {
    width: @width;
    height: 300px;
    border-raduis: 20px;
    background-color: @bgColor;
}
.cardYellow {
    // 传参调用
    .card(yellow,600px);
}
```

- ##### 方法匹配模式

  ```less
  // 制作三角形
  .triangle(top,@width:20px,@color:#000){
      border-color:transparent transparent @color transparent;
  }
  .triangle(right,@width:20px,@color:#000){
      border-color:  @color transparent transparent;
  }
  .triangle(bottom,@width:20px,@color:#000){
      border-color:@color transparent transparent;
  }
  .triangle(left,@width:20px,@color:#000){
      border-color:transparent transparent transparent @color;
  }
  .triangle(@_,@width,@color){
      border-style:solid;
      border-width:@width;
  }
  .top-triangle{
      .triangle(top,20px,pink);
      width:0;
      height:0;
  }
  ```

  ```html
  <div class="top-triangle"></div>
  ```

- ##### 嵌套函数调用

  ```less
  #card{
      background-color:pink;
      .d(@w:300px){
          width:@w;
          #a(@h:300px){
              height:@h
          }
      }
  }
  #wrap{
      #card > .d > #a(400px);
  }
  #wrap2{
      #card > .d(500px);
  }
  ```

- ##### 函数条件筛选

  ```less
  #card{
      // and 运算符
      .border(@width,@color,@style) when(@width > 100px) and (@color=#999){
          border:@width @style @color;
      }
      // not 运算符
      .bg(@color,@width) when not (@width > 100px){
          background-color:@color;
          width:@width;
      }
      // 或运算，用逗号分隔
      .font(@size) when (@size<50px) , (@size>100px){
          font-size: @size;
      }
  }
  
  #main{
      #card>.border(200px,#999,solid);
      #card>.bg(green,200px);
      #card>.font(120px);
      width:200px;
      height:200px;
  }
  ```

- ##### 不定参数

  ```less
  .boxshadow(...){
      box-shadow:@arguments;
  }
  #main{
      .boxShadow(0,0,10px,#333)
  }
  ```

- ##### 循环函数

  ```less
  .generate-columns(4);
  
  .generate-columns(@n,@i:1) when (@i<@n){
      .column-@{i}{
          width: (@i * 100% / n);
      }
      .generate-columns(@n, (@i + 1));
  }
  ```

  ```html
  <div class="columns-1 box"></div>
  <div class="columns-2 box"></div>
  <div class="columns-3 box"></div>
  <div class="columns-4 box"></div>
  ```



# 继承

```less
.linkBtn{
    width:200px;
    height:50px;
}
.linkBtn{
	line-height:50px;
    text-align:center;
    margin:0 auto;
    display:block;
    background:blue;
    color:#fff;
    &:hover{
        background-color:pink;
    }
}
// 继承所有
.linlAll:extend(.linkBtn all){
    text-decoration: none;
}
```

```html
<a href="" class="linkBtn">连接按钮</a>
<a href="" class="linkAll">继承所有的按钮</a>
```

- ##### 继承和调用函数的区别

  ```less
  // 继承方式
  .linlAll:extend(.linkBtn all){
      text-decoration: none;
  }
  // 调用函数方式
  .linkAll2 {
      .linkBtn;
  }
  ```
  
  ```html
  <a href="" class="linkAll">继承</a>
  <a href="" class="linkAll2">调用</a>
  ```
  
  编译结果
  
  ```css
  /* 继承方式 */
  /* 样式合并，减少代码重复 */
  .linkAll {
      text-decoration: none;
  }
  .linkBtn .linkAll {
      width:200px;
      height:50px;
  	line-height:50px;
      ......
  }
  
  /* 调用函数方式 */
  /* 重写了一份 */
  .linkAll2 {
      width:200px;
      height:50px;
  	line-height:50px;
      ......
      text-decoration: none;
  }
  ```



# 导入

`@import "common"` 在其他less文件中导入common.less文件

-  使用关键字reference，导入的代码不进行编译，只满足调用

  `@import (reference) "./assets/style.less";`
  
- 变量是可以跨文件使用的



# 编译

- VSCode插件easy less 
- 脚手架命令行`lessc style.less > style.css`



# 框架

-  LessHat / EST
- 提供现成的mixin
- 类似js类库 封装常用功能
