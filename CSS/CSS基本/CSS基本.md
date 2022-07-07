# 语法

- CSS规则两部分构成：选择器，一条或多条声明



# 引入方式

- 行内样式(内联样式)

  `style`属性

- 内部样式

  一般放在`<head>`中<style></style>

- 外部样式

  写在css文件，引入 `<link rel="stylesheet" href="文件路径" >`








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

 





# CSS预处理器

- 嵌套 反应层级和约束
- 变量和计算 减少重复代码
- Extend和Mixin 代码片段
- 循环 适用于复杂有规矩的样式
- import CSS文件模块化





