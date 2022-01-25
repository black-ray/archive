# 语法

- CSS规则两部分构成：选择器，一条或多条声明



# 引入方式

- 行内样式(内联样式)

  `style`属性

- 内部样式

  一般放在`<head>`中<style></style>

- 外部样式

  写在css文件，引入 `<link rel="stylesheet" href="文件路径" >`



# 特性

- 层叠性

  给相同选择器设置相同的样式，第一个样式就会被覆盖另一个冲突的样式

  - 样式冲突，遵循的原则是就近原则，哪个样式离结构近，就执行哪个样式
  - 样式不冲突，不会层叠

- 继承性

  子标签会继承父标签的某些样式，如文本样式和字号

  - `text-`,`font-`,`line-`这些开头的元素可以继承，以及`color`




# CSS初始化

消除不同浏览器对HTML文本呈现的差异，照顾兼容，需要对CSS初始化

- yui css reset

- 移动端初始化推荐 normalize.css
- 偷懒版本 `*{ marigin: 0; padding: 0 }`



# 浏览器私有前缀

- `-moz-` Firefox
- `-ms-` IE
- `-webkit-` Safari Chrome
- `-o-` Opera
- 先写私有前缀，再写正常写法



# CSS属性书写顺序

- 布局定位属性`display`,`position`,`float`,`clear`,`visibility`,`overfloat`（建议`display`第一个写）
- 自身属性`width`,`height`,`margin`,`padding`,`border`,`background`
- 文本属性`color`,`font`,`text-decoration`,`text-align`,`vertical-align`,`white-space`,`break-word`
- 其他属性(CSS3)`content`,`cursor`,`border-radius`,`box-shadow`,`text-shadow`,`background:liner-gradient`

 

# Emmet语法

## 生成html标签

- 生成标签 ： 直接输入标签名按tab键
- 生成多个标签 ： div*10
- 生成父子关系 ： ul>li
- 生成兄弟关系 ：div+p
- 生成带有类名 ： div.nav
- 生成带有id的 ： div#banner
- 生成自增排序 : div.demo$*5
- 生成标签的内部写内容 ： div{内容写这里}

## 生成CSS样式

- `text-align: center` => `tac`
- `width: 100px`=> `w100`
- `height: 200px` => h200
- `text-indent: 2em`=> `ti2em`
- `line-height: 26px`=> `lh26`
- `text-decoration: none`=> `tdn`
- ....



# CSS预处理器

- 嵌套 反应层级和约束
- 变量和计算 减少重复代码
- Extend和Mixin 代码片段
- 循环 适用于复杂有规矩的样式
- import CSS文件模块化





