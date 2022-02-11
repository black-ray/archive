# 基础架构

<img src="C:\Users\shawe\AppData\Roaming\Typora\typora-user-images\image-20210908135822699.png" alt="image-20210908135822699" style="zoom:50%;" /> 



# 环境搭建

## 脚手架

- ##### 安装Node

- ##### 安装脚手架

  CLI的版本号和Angular是一致的

  ```bash
  #安装脚手架
  npm install -g @angular/cli
  ```

- ##### 新建工程 

  ```bash
  #新建项目
  ng new newDemo
  #新建项目（添加参数：不要下载安装；样式表要css；不要路由支持）
  ng new newDemo --skip-intall css --routing false
  #新建项目（不需要单元测试文件，保留html模板文件，不保留css文件）
  ng new newDemo --minimal --inlineTemplate false
  #查询新建项目指令支持的参数
  ng new --help
  ########## 主要参数 #############
  # 精简的Angular项目 (没有单元测试关联文件)
  --minimal=true
  # 不需要初始化Git仓库
  --skipGit=true
  # 不需要安装项目依赖
  --skip-install
  # 样式选择
  --style=css
  # 不创建路由文件
  --routing=false
  # 内联模板
  --inlineTemplate
  # 行内样式
  --inlineStyle
  # 修改名称的前缀‘App‘
  --prefix
  #不创建单元测试
  -S
  ```

- ##### 安装依赖

  ```bash
  #软件依赖
  npm i --save 包名
  #开发依赖
  npm i --save-dev 包名
  ```

- ##### 启动开发服务器

  ```bash
  #启动开发服务器，项目编译
  ng serve
  # 编译后在浏览器打开
  ng serve --open
  # 开启热更新
  --hmr=true
  # 禁用热更新警告
  hmrWarning=false
  # 更改运行端口
  --port
  ```

- ##### 代码规范扫描

  ```bash
  #代码规范扫描
  ng lint
  #自动修改部分代码规范问题
  ng lint -fix
  ```
  
- ##### 编译

  ```bash
  #开发环境编译
  ng build
  #生产环境编译
  ng build --prod
  ```
  
- ##### cli命令查询

  ```bash
  #查询ng命令的子命令
  ng help
  ```
  
- ##### 添加组件

  ```bash
  #添加组件
  ng generate component 组件名（驼峰形式）
  ng g c 组件名（驼峰形式）
  #添加组件 指定路径
  ng generate component 文件夹名/组件名（驼峰形式）
  #添加组件 不要测试文件
  ng generate component 组件名 --spec=false
  ```
  
- ##### 真机调试

  1. 手机和电脑在同一网络
  2. `ng serve --host 0.0.0.0`
  3. 手机打开http://电脑IP:4200



## 项目文件

### 项目目录

```bash
newDemo
    │
    ├─e2e							# end to end 自动化集成测试目录
    │  │  
    │  └─src
    │  │       app.e2e-spec.ts
    │  │       app.po.ts
    │  │
    │  │  protractor.conf.js
    │  │  tsconfig.json
    │          
    └─src								# 源代码目录
    │    ├─app							# 工程源码目录
    │    │      app.component.css       # 根组件
    │    │      app.component.html      # 根组件
    │    │      app.component.spec.ts
    │    │      app.component.ts        # 根组件
    │    │      app.module.ts			# 根模块
    │    │      
    │    ├─assets						# 静态资源目录
    │    │      .gitkeep
    │    │      
    │    └─environments					# 环境配置
    │    │        environment.prod.ts	# 生产环境
    │    │        environment.ts		# 开发环境
    │    │  favicon.ico					# 收藏图标
    │    │  index.html					# 单页应用的宿主HTML
    │    │  main.ts						# 入口ts文件
    │    │  polyfills.ts				# 填充库 用于不同浏览器的兼容脚本加载
    │    │  styles.css					# 整个项目的全局CSS
    │    │  test.ts						# 测试入口
    │    
    │  .browserslistrc					# 浏览器兼容性
    │  .editorconfig					# 在不同的代码编辑器中统一编码风格
    │  .gitignore						# git中忽略文件列表
    │  angular.json						# angular配置文件
    │  karma.conf.js					# 自动化测试karma配置文件
    │  package-lock.json				# package.json中依赖的软件包所依赖的软件包
    │  package.json						# npm描述文件，包含脚本，项目依赖
    │  README.md						# 说明文档
    │  tsconfig.app.json				# typescritp配置 针对项目开发
    │  tsconfig.base.json				# typescritp配置 全局性
    │  tsconfig.json					# 项目引用的typescritp配置文件列表
    │  tsconfig.spec.json				# typescritp配置 针对测试
    │  tslint.json						# 静态代码扫描 定义编码规则
```

### main.ts

入口文件

```typescript
// enableProdMode 方法调用后会开启生产模式
import { enableProdMode } from '@angular/core';
// Angular  应用程序的启动在不同平台上是不一样的
// 在浏览器中启动时需要用到 platformBrowserDynamic 方法，返回平台实例对象
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
// 根模块 用于启动应用程序
import { AppModule } from './app/app.module';
// 引入环境变量对象
import { environment } from './environments/environment';

// 如果是生产环境，开启生产模式
if (environment.production) {
  enableProdMode();
}
// 启动应用程序
platformBrowserDynamic()
  .bootstrapModule(AppModule)
  .catch(err => console.error(err));
```

### environment.ts

环境设置

```typescript
// 字典型的变量
export const environment = {
  production: false,
  // 可以定义api接口地址等
  baseUrl: 'https://yourcompany.apis',
};

// environment.prod.ts 是开发用环境设置
// ng build --prod 编译使用开发环境设置
```

### app.module.ts

Angular 根模块，告诉Angular如何组装应用

```typescript
// CommonModule 提供各种服务和指令，如ngif，ngfor
// BrowserModule 已经导入并导出了 CommonModule 模块
import { BrowserModule } from '@angular/platform-browser';  // 浏览器解析的模块
import { NgModule } from '@angular/core';					// Angular模块装饰器
import { AppComponent } from './app.component';				// 根组件
import { NewsComponent } from './components/news/news.component';
import { HomeComponent } from './components/home/home.component';
import { HeaderComponent } from './components/header/header.component';
/* @NgModule装饰器, @NgModule接受一个元数据对象，告诉 Angular 如何编译和启动应用 */
@NgModule({
  declarations: [    /* 配置当前项目运行的的组件 */
    AppComponent, NewsComponent, HomeComponent, HeaderComponent
  ],
  imports: [  /* 配置当前模块运行依赖的其他模块 */
    BrowserModule
  ],
  providers: [],  /* 配置项目所需要的服务，该服务只能在当前模块的组件中使用 */
  bootstrap: [AppComponent]  // 指定应用的主视图通过引导根AppModule来启动应用，一般写的是根组件
})
//根模块不需要导出任何东西，因为其它组件不需要导入根模块
export class AppModule { }
```

### app.component.ts

```typescript
import { Component } from '@angular/core';		//引入核心模块里面的Component
@Component({
  /* 指定这个组件的使用方式
  	 app-home   ->  <app-home></app-home>
  	 [app-home] ->  <div app-home></div>
  	 .app-home  ->  <div class="app-home"></div>
  */
  // 根组件app-root在index.html中被调用
  selector: 'app-root',
  /* 关联组件的模板文件
  	 templateUrl: '组件模板文件路径'
  	 template: `组件模板字符串`
  */
  templateUrl: './app.component.html',
  /* 关联组件的样式文件
  	 styleUrls: ['组件样式文件路径']
  	 styles: [`组件样式`]
  */
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'angulardemo01';   //定义属性
  constructor(){ }  //构造函数
}
```

### angular.json

```json
// 整个angular项目的定义文件
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {
    // projects中可以放多个项目
    "pinduoduo": {              // 项目名
      "projectType": "application",
      "schematics": {},
      "root": "",
      "sourceRoot": "src",      // 源码目录
      "prefix": "app",
      "architect": {
        // ng 命令的详细配置
        "build": {},
        "serve": {},
        "extract-i18n": {},
        "test": {},
        "lint": {},
        "e2e": {}
      }
    }},
  "defaultProject": "pinduoduo"
}
```

### package.json

```json
{
  "name": "pinduoduo",
  "version": "0.0.0",
  // scripts指定脚本任务
  // 使用定义的scripts脚本命令的好处是，可以使用自己定义的(这个文件里的)版本的CLI去运行
  "scripts": {
    "ng": "ng",           // npm run ng
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
  "private": true,
  // 依赖 项目里直接使用的依赖
  // 安装直接依赖 npm i --save @angular/form
  "dependencies": {
    "@angular/router": "~10.0.0",
    "rxjs": "~6.5.5",
  },
  // 开发阶段依赖 不跟随发布
  // 安装开发依赖 npm -i --save-dev typescript
  "devDependencies": {
    "typescript": "~3.9.5"
  }

  // 版本号："~3.9.5" 大版本号.小版本号.补丁版本号 major.minor.patch
  // ~代表着 前两个版本号一致，后面的补丁版本不锁定
  // ^代表着 锁定大版本号
  // 没有符号代表着 严格锁定某一个版本
}
```



# 组件 Component

## 创建组件

```bash
ng g component 文件夹名/组件名
```



## 组件结构

组件控制屏幕的一部分（视图）

```typescript
import { Component, OnInit } from '@angular/core';

// @Component注解标记一个Class为Angular Component
// 模板，样式和组件一起决定视图的外观和行为
@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',		// templateUrl或者template将模板和组件联系起来
  /* 内联模板写法
  template: `<div></div>`, */
  styleUrls: ['./header.component.scss']		// styleUrl或者styles将样式和组件联系起来
})
// 组件的逻辑定义在class中，class通过属性和方法与视图进行交互
export class HeaderComponent implements OnInit {
  constructor() { }
  ngOnInit() {}
  // public 默认 可以在这个类里面使用、也可以在类外面使用
  // protected 只有在当前类和它的子类里面可以访问
  // private 只有在当前类才可以访问这个属性
  public title = "我是一个组件";
}
```

组件扩展html标签

```html
<!-- 当html标签使用 -->
<app-header></app-header>
```

组件本身的样式

```css
:host {
}
```

```css
/* 限定组件本身只能在哪个父级内才生效 */
:host-context(父级标签名) {
}
```



## 组件注解

@Component注解

- ##### selector

  类型: string

  css选择器名，用于在模板中标记出该指令（组件），并触发其实例化

  继承自@Directive装饰器

- ##### template

  类型: string

  组件的内联模板。如果提供了它，就不要再用 templateUrl 提供模板

- ##### templateUrl

  类型: string

  组件模板文件的 URL。如果提供了它，就不要再用 template 来提供内联模板

- ##### styles

  类型: string[]

  组件用到的一个或多个内联的CSS 样式

- ##### styleUrls

  类型: string[]

  一个或多个 URL，指向组件CSS样式表的文件

- ##### providers

  类型: Provider[]

  使用一个令牌配置组件的注入器

  注入的服务是按组件实例化的，并且可以在组件及其子树中的所有子组件中访问

  服务不是单例的，每次都会获得所提供服务的新实例，服务实例将与组件一起销毁

  子组件会逐级向上寻找provider，直到找到为止，否则就会抛出错误

  - @NgModule中的providers

    服务将是全局单例的，即使重复声明，也不会重新创建实例，最终都会注册到根级注入器

    懒加载模块中提供的服务实例会在子注入器（懒加载模块）上创建

  - @Injectable的provideIn

    服务的@Injectable装饰器的provideIn属性可以视为以反向方式指定依赖关系，服务本身宣布它应该提供给哪些模块使用

    申明的模块可以是 `root` 或其他任何可用模块。`root` 实际上是 `AppModule` 的别名

    如果服务仅被注入到懒加载模块，它将捆绑在懒加载包中

    如果服务又被注入到正常模块中，它将捆绑在主包中

  继承自@Directive装饰器

- ##### viewProviders

  类型: Provider[]

  注入的服务只能在视图的各个子节点中可用

  viewProviders与providers的区别

  ```typescript
  // 服务
  @Injectable()
  export class MyService{
    // 打印在哪里调用了该服务
    testIfGetService(where){
      console.log('Got My Service in ' + where);
    }
  }
  
  // 投射用子组件
  @Component({
    selector: 'vp-child',
    template: `<div>This is child!!!</div>`
  })
  export class VPChild{
    constructor(private service: MyService) {
      this.service.testIfGetService('child');
    }
  }
  
  // 模板调用子组件
  @Component({
    selector: 'vp-viewchild',
    template: `<div>This is viewChild!!!</div>`
  })
  export class ViewVPChild{
    constructor(private service: MyService){
      this.service.testIfGetService('viewChild');
    }
  }
  ```

  providers形式注册服务

  ```typescript
  // 父组件
  @Component({
    selector: 'vp-parent',
    template: `<div>This is parent!!!</div>
    			 <ng-content></ng-content>
    			 <vp-viewchild></vp-viewchild>`,
    // providers形式注册服务
    providers: [MyService]
  })
  export class VPParent{
    constructor(private service: MyService){
      this.service.testIfGetService('parent');
    }
  }
  ```

  在父组件用使用providers注册的服务，对viewChildren和contentChildren都可见

  ```html
  <vp-parent>
    <vp-child></vp-child>
  </vp-parent>
  
  <!--
  运行结果
  Got My Service in parent
  Got My Service in child
  Got My Service in viewchild
  -->
  ```

  viewProviders形式注册服务

  ```typescript
  // 父组件
  @Component({
    selector: 'vp-parent',
    template: `<div>This is parent!!!</div>
    			 <ng-content></ng-content>
    			 <vp-viewchild></vp-viewchild>`,
    // providers形式注册服务
    viewProviders: [MyService]
  })
  export class VPParent{
    constructor(private service: MyService){
      this.service.testIfGetService('parent');
    }
  }
  ```

  在父组件用viewProviders注册的服务，对contentChildren是不可见的

  ```html
  <vp-parent>
    <vp-child></vp-child>
  </vp-parent>
  
  <!--
  运行结果报错
  Error: StaticInjectError[Myservice]
  -->
  
  <vp-parent>
    <vp-child></vp-child>
  </vp-parent>
  <!--
  运行结果
  Got My Service in parent
  Got My Service in viewchild
  -->
  ```

- 









- ##### exportAs

  类型: string

  定义一个名字，用于在模板中把该组件赋值给一个变量。即给组件定义一个别名。

  当一个组件绑定于一个元素时，那么声明的模板引用变量将会被解析为当前元素上所绑定的组件

  ```html
  // app.component.html
  <toggle-on #toggleOn></toggle-on>
  // toggleOn is the ToggleOnComponent
  ```

  当有多个名字时，使用逗号分隔

  继承自@Directive装饰器

- ##### changeDetection

  类型: ChangeDetectionStrategy

  变更检测策略

  当组件实例化之后，Angular会创建一个变更检测器，它负责传播组件各个绑定值的变化

  `ChangeDetectionStrategy.OnPush` 检测策略为CheckOnce按需

  `ChangeDetectionStrategy.Default` 检测策略为CheckAlways

- ##### encapsulation

  类型: ViewEncapsulation

  模板和 CSS 样式使用的样式封装策略

  - `ViewEncapsulation.ShadowDom` 

    使用 Shadow DOM封装样式，并为组件的宿主元素创建一个ShadowRoot

    它只在原生支持 Shadow DOM 的平台上才能工作

  - `ViewEncapsulation.Emulated`

    样式有范围封装，父组件不影响子组件的样式。

    如果非要让父组件的样式覆盖子组件的样式，使用`::ng-deep`

  - `ViewEncapsulation.None`

    使用全局 CSS，不做任何封装，样式直接应用到整个document

    向下影响自己的子组件，向上影响自己的父组件

  - `ViewEncapsulation.Native`

    在angular10.0中已经去除

  如果没有设置，该值就会从CompilerOptions中获取。默认的编译器选项是`ViewEncapsulation.Emulated`

  如果设置为`ViewEncapsulation.Emulated`，并且该组件没有指定styles或styleUrls，就会自动切换到`ViewEncapsulation.None`

- ##### animations

  类型: any[]

  一个或多个动画 trigger() 调用，包含一些 state() 和 transition() 定义。参考[动画]目录

- ##### preserveWhitespaces

  类型: boolean

  为 true 则保留，为 false 则从编译后的模板中移除可能多余的空白字符，默认为 false

  空白字符就是指那些能在 JavaScript 正则表达式中匹配 \s 的字符

- ##### interpolation

  类型:[string, string]

  改写默认的插值表达式起止分界符 [ '{{', '}}']

  ```typescript
  interpolation: ['~~','~~']
  ```

  ```html
  <div>~~title~~</div>
  ```

- 







## 组件生命周期

<img src="E:\app\PotPlayer\Capture\1 组件生命周期（1）.mp4_20210909_203116.700.jpg" alt="1 组件生命周期（1）.mp4_20210909_203116.700" style="zoom:33%;" /> 

- ##### ngOnChanges

  参数SimpleChanges，字典型的对象，每一个key值是输入的参数属性的名字，value值是simpleChange对象，包含 当前值，前一个值，是否是第一次改变

  SimpleChanges参数只存储发生变化的数据，没发生变化的数据不存储

  SimpleChanges的子属性类型为SimpleChange

  对于基本数据类型来说，只要值发生变化就可以被检测

  对于引用类型来说，可以检测引用地址的变化，但是检测不到对象内属性值得变化，但是不影响组件模板更新数据

  ```typescript
  ngOnChanges(changes: SimpleChanges) {
      console.log(changes)
  }
  ```

  ![image-20220109212747774](Angular.assets/image-20220109212747774.png) 

  组件的输入属性@Input变化时调用，可以被触发调用多次。第一次是在组件初始化之前

  主要用在父子组件传值中，父组件给子组件传值的时候，以及父组件改变传值的数据时调用

- ##### ngOninit

  首次接收到输入属性值后执行。组件初始化完成，在这个函数中可以安全的使用组件的属性和方法。只装载一次

  主要用来处理请求数据

- ##### ngDoCheck

  脏值检测时被调用

  ngOnChanges和ngDoCheck被认为不应该在同一个组件之中，同时被订阅

  ngOnChanges是监听自己组件本身的变化，ngDoCheck是Angular做整个框架性检查

  只要输入属性发生变化，无论是基本类型还是引用类型，还是引用类型的属性发生变化，都会执行

  可以被调用多次

- ##### ngAfterContentInit

  组件内容初始化。ng-content投影内容完成时调用

- ##### ngAfterContentChecked

  组件内容的脏值检测。可以被调用多次

- ##### ngAfterViewInit

  组件的视图初始化完成。一个组件和它的子组件都初始化完成。

  DOM操作都应该在这里处理

- ##### ngAfterViewChecked

  组件的视图的脏值检测。可以被调用多次

- ##### ngOnDestory 

  组件销毁。做一些清理工作
  
  清理定时函数
  
  可以用来在组件销毁时进行数据保存工作



## 组件输入输出

- ##### 父组件给子组件传值

  ```html
  <!-- 父组件调用子组件的时候传入数据 -->
  <app-header [msg]="msg" [run]='run' [home]='this'></app-header>
  ```

  ```typescript
  // 子组件引入Input
  import { Component, OnInit ,Input } from '@angular/core';
  // 子组件中@Input 接收父组件传过来的数据
  export class HeaderComponent implements OnInit {
      // 如果变量要和属性名不一样，可以在@Input()内写属性的名字
      // @Input('msg') myMsg: string;
  	@Input() msg:string;
      // 可以接受父组件的方法作为参数
      @Input() run:any;
      // 甚至可以直接把父组件传过来
      @Input() home: HomeComponent; 
  	constructor() { }
      ngOnInit() { }
  }
  
  // 父组件
  export class HomeComponent implements OnInit {
  	public msg:string='我是父组件的msg';
      run(){
          alert('我是父组件的run方法');
      }
  }
  ```

- ##### 子组件触发父组件的方法

  ```typescript
  // 子组件
  // 子组件引入Output 和EventEmitter
  import { Component, OnInit ,Input, Output, EventEmitter} from '@angular/core';
  
  // EventEmitter 事件发射器
  // 子组件中实例化EventEmitter
  @Output() private outer = new EventEmitter<string>();
  
  // 子组件通过事件驱动EventEmitter对象outer 实例广播数据
  sendParent(){
  	this.outer.emit('msg from child')
  }
  ```

  ```html
  <!-- 父组件调用子组件时 定义接收广播事件 -->
  <!-- outer就是子组件的EventEmitter对象outer -->
  <!-- $event是事件所携带的数据 -->
  <app-header (outer)="runParent($event)"></app-header>
  ```

  ```typescript
  // 父组件接收子组件传递过来的数据
  runParent(msg:string){
  	alert(msg);
  }
  ```

- ##### 子组件获取父组件元素节点

  ```typescript
  // 子组件
  export class ChildComponent implements OnInit {
      constructor (private el: ElementRef) { }
      ngOnInit() {
          // 获取父元素节点名
      	console.log(this.elRef.nativeElement.parentElement.nodeName);
          // 查找祖先节点 IE不支持
          console.log(this.elRef.nativeElement.closest('.parent-element-class'));
      }
  }
  ```

- ##### 非父子组件之间的通信

  公共的服务，Localstorage (推荐)，Cookie

<img src="Angular.assets/7 【ngIf指令 组件的输入输出】组件的输入和输出属性.mp4_20210909_170849.188-16407575502101.jpg" alt="7 【ngIf指令 组件的输入输出】组件的输入和输出属性.mp4_20210909_170849.188" style="zoom: 33%;" />  



## 脏值检测

### 脏值检测基本概念

- ##### 什么是脏值检测

  当数据改变时更新视图(DOM)

- ##### 什么时候触发脏值检测

  1. 浏览器事件(click, mouseover, keyup 等)
  2. 异步函数(setTimeout, setInterval 等）
  3. HTTP请求

- ##### 如何进行脏值检测

  检查两个状态值，当前状态和新状态

- ##### 脏值检测执行顺序

  检查的方向是**单向**的。会从根组件开始，逐一检测每一个组件。

  检测基于组件树的单向数据流，组件树+单项数据流使Angular在检测每一个组件时不需要考虑当前组件会修改父组件的数据

  检查是同步的操作。

  <img src="Angular.assets/image-20220107151758583.png" alt="image-20220107151758583" style="zoom:50%;" /> <img src="Angular.assets/image-20220107163530024.png" alt="image-20220107163530024" style="zoom:50%;" /> 

- ##### 开发模式下脏值检测会进行两遍

  在开发模式下，Angular 会有意的进行两次脏值检测，目的是提示开发者：开发的代码违背了单向数据流的策略。

  在脏值检测之后会进行**AfterViewChecked**和**AfterViewInit**生命周期方法。

  在**更新视图**之后的生命周期方法里，是不能改变属性值(绑定的属性)的，因为会再次引起新的一轮脏值检测。会带来无限循环问题。

  这种做法是不符合单向数据流的，因为父组件已经被检测过。虽然界面也会正常的被修改，但是会打印出错误。

  进行两次脏值检测是就为了防止无限检测的问题，第一遍脏值检测结束以后，会马上进行第二遍的检测，检测这次的值和上一次的值是否是一致。如果不一致会抛出异常。

  ```typescript
  @Component({
    selector: 'app-child',
    template: `<span [textContent]="title"></span>`
  })
  export class ChildComponent implements OnInit {
    _title = 'hi';
    @Input()
    public get title(): string {
      console.log('脏值检测');
      return this._title;
      /* 执行结果：
  	脏值检测
  	脏值检测
  	*/
    }
    ngAfterViewChecked(): void {
    	// 在此处会抛出异常，不要在 ngAfterViewChecked 或者 ngAfterViewInit 中去改变属性值
    	// this.title = 'hello';
    	// 异常为 ExpressionChangedAfterItHasBeenCheckedError
    }
  }
  ```
  
- ##### 使用NgZone改变属性值

  ```typescript
  import {NgZone} from '@angular/core';
  export class ChildComponent {
      _title;
      public get time(): number {
  		/**如果写成下面的代码就会出现异常
      	 * 这是由于脏值检测是一个单向的同步的检测过程，而且会进行两次检查
      	 * 这会导致下面的结果在两次检查中值是不一样的，所以会抛出异常 */
      	// return Date.now();
          return this._time;
      }
      constructor(ngZone: NgZone) { }
      ngAfterViewChecked() {
          // zone.js 提供了在一个浏览器应用中使用不同的运行时上下文的能力
          ngZone.runOutsideAngular(() => {
              // 异步改变值
              setInterval(() => {
           		this._time = Date.now();
         		}, 1);
          });
      }
  }
  ```

- ##### 使用DOM避免脏值检测的无限循环

  ```html
  <!--
  <span [textContent]="time | date: 'HH:mm:ss:SSS'"></span>
  -->
  <span #timeRef></span>
  ```

  ```typescript
  export class ChildComponent {
      @ViewChild('timeRef') timeRef: ElementRef;
      constructor(rd: Renderer2) {
       	/* 引发脏值检测的无限循环，当然如果不是性能出现很大的问题，这样做也可以
       	setInterval(() => {
       	  this.time = Date.now();
       	  cd.markForCheck();
       	}, 100);
       	*/
          
          // 如果不使用绑定，采用直接写 DOM 的形式可以解决脏值检测无限循环的带来的性能问题
  		setInterval(() => {
  		 rd.setProperty(
  		   this.timeRef.nativeElement, 'innerText',
  		   // 管道使用的变换函数也可以直接在类中使用
  		   // 下面的 formatDate 就是 DatePipe 的变换函数
  		   formatDate(Date.now(), 'HH:mm:ss:SSS', 'zh-Hans')
  		 );
  		}, 100);
      }
  }
  ```



### NgZone

Angular 引入 Zone.js 以处理变更检测，具体来说，Zone.js 通过对所有常见的异步 API 打上了“补丁” 以追踪所有的异步操作，进而使 Angular 可以决定何时刷新 UI。

- ##### 什么是 Zone

  Zone 是一种用于拦截和跟踪异步工作的机制。

- ##### 什么是 NgZone

  Zone.js 将会对每一个异步操作创建一个 task。一个 task 运行于一个 Zone 中。通常来说， 在 Angular 应用中，每个 task 都会在 Angular Zone 中运行，这个 Zone 被称为 NgZone。一个 Angular 应用中只存在一个 Angular Zone，而变更检测只会由 运行于这个 NgZone 中的异步操作触发。

- ##### runOutsideAngular

  函数 `runOutsideAngular` 用于确保代码于 NgZone 之外运行，即保证 Angular 的变更检测不会因为相关代码而触发

  ```typescript
  // setInterval 定时器不会触发变更检测
  constructor(private ngZone: NgZone) {
    this.ngZone.runOutsideAngular(() => {
      setInterval(() => doSomething(), 100)
    });
  }
  ```

- ##### run

  `run` 方法的目的与 `runOutsideAngular` 正好相反，任何写在 run 里的方法，都会进入 Angular Zone 的管辖范围

  ```typescript
  // 通过 run() 方法使得在 Zone 之外的操作重新又进入了 Zone 的管辖范围
  num = 0;
  constructor(private zone: NgZone) {
    this.zone.runOutsideAngular(() => {
      let i = 0;
      const token = setInterval(() => {
        this.zone.run(() => {
          this.num = ++i;
        })
  
        if (i == 10) {
          clearInterval(token);
        }
      }, 1000);
    })
  }
  ```

- ##### NgZone和rxjs结合使用，在 Zone 外创建数据流、Zone 内订阅数据流

  ```typescript
  notify$ = new Subject();
  
  ngOnInit() {
    this.notify$.subscribe(() => {
        this.message = 'timeout';
    })
  }
  
  constructor(private zone: NgZone) {
    // 将过期时间保存在 localStorage 中
    localStorage.setItem('expiredDate', addMinutes(new Date(), 1).getTime().toString());
    // 一旦时间过期，runOutsideAngular 中的定时器便会通知 Zone 中的 message 更新并同时清除自己
    this.zone.runOutsideAngular(() => {
      const i = setInterval(() => {
        const expiredDate = +localStorage.getItem('expiredDate');
        if (new Date().getTime() - expiredDate > 0) {
          this.zone.run(() => {
            this.notify$.next();
          })
          clearInterval(i);
        };
      }, 1000)
    })
  }
  ```



###  OnPush变更检测策略

- ##### 默认策略`ChangeDetectionStrategy.Default`

  不管在任何一个地方发生了一个变化，脏值检测都会把整个树跑一遍。

  不会有遗漏，但是树过大，性能降低

- ##### 变更检测策略`ChangeDetectionStrategy.OnPush`

  只对组件当中的有**@Input**注解的属性进行检测。

  这个属性发生了改变，就会引发一次脏值检测；不改变，不检测。

  如果没有检测到组件的变化，那就没有必要检测其子组件树的变化了，因为开发者说了：如果我没变，我的子组件是不会变的。

  检测也只检查有脏值检测发生的节点和它的子组件。

  有时需要手动通知框架进行脏值检测。

  如果子组件设置了OnPush策略，**子组件所有的状态的改变，都依赖于父组件的@Input**。

  ```typescript
  @Component({
    selector: 'app-parent',
    templateUrl: './parent.component.html'
    // OnPush策略
    changeDetection: ChangeDetectionStrategy.Default
  })
  export class ParentComponent implements OnInit { }
  ```




### ChangeDetectorRef

#### 配置使用

引入ChangeDetectorRef模块

```typescript
import { ChangeDetectorRef } from “angular”;
```

声明

```typescript
constructor(private cd:ChangeDetectorRef) 
```

使用

```typescript
this.cd.detectChanges();
```



#### markForCheck()

通知框架执行脏值检测，忽视当前组件或是父组件中OnPush的影响

<img src="Angular.assets/webp.webp" alt="img" style="zoom:33%;" /> 

- ##### 手动通知框架进行脏值检测

  如果设置了OnPush策略，没有Input参数，但是有其他（例如路径，http获取数据等）参数会发生变化，这些变化会被系统忽略掉。

  需要手动通知系统属性的变化。

  ```typescript
  @Component({
    changeDetection: ChangeDetectionStrategy.OnPush
  })
  export class HomeDetailComponent implements OnInit {
      constructor(private cd: ChangeDetectorRef) { }
      selectedTabLink;
      ngOnInit() {
          // 路径参数发生变化
          this.route.paramMap.subscribe(params => {
        		console.log('路径参数: ', params);
        		this.selectedTabLink = params.get('tabLink');
              // 发生变化，通知系统检查
        		this.cd.markForCheck();
      	});
      }
  }
  ```



#### detectChanges()

该方法会从当前组件到各个子组件开始触发一次变化检测

与detach()结合，可以实现局部变更检测。

<img src="Angular.assets/webp-16431272730502.webp" alt="img" style="zoom:33%;" /> 



#### checkNoChanges( )

该方法会从当前组件到各个子组件开始触发一次变化检测，如果有检测某个组件发生了变化，抛出异常并停止检测。

<img src="Angular.assets/webp-16431274889744.webp" alt="img" style="zoom:33%;" /> 



#### detach( )，reattach()

手动将所在组件与其子组件分离或是重新连接

<img src="Angular.assets/webp-16431828607696.webp" alt="img" style="zoom:33%;" /> 



## 组件工厂





# 模板 Template 

## 数据绑定

使用方括号[]是数据绑定，如果带方括号，等号后面就是一个对象或表达式

不使用方括号是字符串，但如果此时在等号后使用{{}}就是和方括号等效的（如果绑定的数据有. 如class. active则不能使用{{}}）

- ##### 插值表达式

  ```html
  <h1>{{title}}</h1>
  <h1>{{'Hello World'}}</h1>
  <h1>{{getInfo()}}</h1>
  <h1>{{a == b ? '相等' : '不等'}}</h1>
  ```

- ##### html绑定

  ```typescript
  this.h="<h2>这是一个h2 用[innerHTML]来解析</h2>"
  ```

  ```html
  <div [innerHTML]="h"></div>
  ```

- ##### 数据绑定容错处理

  ```html
  <!-- 
  interface Task {
  	person?:{ name: string }
  }
  -->
  <span *ngIf="task.person">{{ task.person.name }}</span>
  <span>{{ task.person?.name }}</span>
  ```



## 属性绑定

- ##### DOM属性绑定

  ```html
  <div [id]="id" [title]="msg">我的属性</div>
  ```

- ##### 标记属性绑定

  ```html
  <!-- [attr.属性名称] 标记属性或者自定义的HTML属性绑定-->
  <td [attr.colspan]="colSpan"></td>
  ```

- ##### 单个样式绑定

  ```html
  <div class="title" [class.className]="条件表达式">
      对于单个样式的条件绑定最为合适
  </div>
  <a href="#" [class.active]="selectedIndex === i" (click)="selectedIndex = i">
  	{{ menu.title }}
  </a>
  ```

- ##### class属性绑定

  ```html
  <div [ngClass]="{'active': true, 'error': true}"></div>
  ```

- ##### style属性绑定

  ```html
  <button [style.backgroundColor]="isActive ? 'blue' : 'red'">按钮</button>
  <button [ngStyle]="{'backgroundColor': 'red'}">按钮</button>
  ```



## 事件绑定

圆括号()用于事件绑定，等号后可以接表达式也可以是一个定义在类中的函数

可以传入事件对象$event

```html
<button class="button" (click)="getData()">
	点击按钮触发事件
</button>
```

```html
<!-- (事件名)="模板语句"  事件对象：$event -->
<input type="text" (keyup)="keyUpFn($event)"/>
<input type="text" (keyup.enter)="keyUpFn($event)"/>
```

```typescript
// 事件对象 event
keyUpFn(event: Event){
	console.log(event)
}
```



## 双向数据绑定

### 双向绑定

应用于表单

```html
<input [(ngModel)]="username"/>
```

- ##### ngModel

  - **FormsModule**中提供的指令

    注意引入：FormsModule

    ```typescript
    import { FormsModule } from '@angular/forms';
    @NgModule({
    declarations: [AppComponent,],
    imports: [BrowserModule,FormsModule],
    providers: [],
    bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

  - 使用**[(ngModel)]="变量"** 形式进行双向绑定

  - 其实是一个语法糖

    ```html
    <!-- 等同于 -->
    <input [ngModel]="username" (ngModelChange)="username = $event"/>
    ```

- ##### 双向绑定就是属性绑定加事件绑定

  ```html
  <!-- 等同于 -->
  <input [value]="username" (input)="username = $event.target.value"/>
  ```

  - [value]="username" 绑定username值到input的value
  - (input)=“表达式” 绑定表达式到input的input事件
  - username = $event.traget.value 在input事件触发时执行
  - $event 表达式，提供事件的数据

- ##### 其他表单标签

  ```html
  <input type="radio" value="1" name="sex" id="sex1" [(ngModel)]="usersex"/><label for="sex1">男</label>
  <input type="radio" value="2" name="sex" id="sex2" [(ngModel)]="usersex"/><label for="sex2">女</label>
  ```

  ```html
  <select name="city" id="city" [(ngModel)]="usercity">
  	<option [value]="item" *ngFor="let item of cityList">{{item}}</option>
  </select>
  ```

  ```html
  <span *ngFor="let item of hobbies;let key=index;">
  	<input type="checkbox"  [id]="'check'+key" [(ngModel)]="item.checked"/> 
      <label [for]="'check'+key"> {{item.title}}</label>
  </span>
  ```

  ```html
  <textarea name="mark" id="mark" cols="30" rows="10" [(ngModel)]="usermark"></textarea>
  ```



### 自定义双向绑定

```typescript
export class HorizonGridComponent{
	@Output() usernameChange = new EventEmitter();
    
	private _username = '';   
	@Input()
	public get username (): string {
	  return this._username;
	}
	public set username ( value: string ) {
	  this._username = value;
	  // 在完成属性绑定的同时，也进行了事件绑定
	  this.usernameChange.emit( value );
	}
}
```

```html
<app-horizon-grid [(username)]="username"></app-horizon-grid>
<!-- 等同于 -->
<app-horizon-grid (username)="username" (usernameChange)="username = $event"></app-horizon-grid>
```



## 模板引用

### 获取DOM节点

**ViewChild**用来在类中引用模板中的视图节点

- 可以是Angular组件，也可以是HTML元素
- 在**AfterViewInit**中可以安全的使用@ViewChild引用的元素
- 推荐使用Renderer2操作DOM元素，防止注入攻击
- 真正的DOM节点是nativeElement里

```html
<!-- #号后面是给模板或者DOM元素一个引用名字，以便可以在组件类或模板中进行引用 -->
<!-- #id是唯一标识，注意在ngfor循环中的使用 -->
<div #helloDiv>
    你好
</div>
```

```typescript
import { Component, ViewChild, ElementRef} from '@angular/core';
export class AppComponent{
    // ViewChild是一个选择器，用来查找要引用的DOM元素或组件
    // ElementRef是DOM元素的一个包装类。
    // 因为DOM元素不是Angular中的类，所以需要一个包装类以便在Angular中使用和标识其类型
    // ElementRef可以加泛型形式 例如 ElementRef<HTMLParagraphElement>
    @ViewChild('helloDiv') helloDivRef: ElementRef;
}
```

使用Renderer2操作DOM元素

```html
<img
     #img
     *ngFor="let slider of sliders"
     [src]="slider.imgUrl"
     [alt]="slider.caption"
/>
```

静态选项

```typescript
// 如果元素没有在ngif或ngfor的包含之下static就是true；反之是false
@ViewChild('imageSlider', { static: true }) imgSlider: ElementRef;
```



### 获取多个DOM节点

ViewChildren引用多个模板元素

```typescript
// 引用多个模板元素
@ViewChildren('img') imgs: QueryList<ElementRef>;
```

输出的QueryList对象

![image-20220109154325466](Angular.assets/image-20220109154325466.png) 

```typescript
// 获取ElementRef集合，其中有nativeElement
this.imgs.toArray();
```

```typescript
// 可以直接操作DOM，但是不提倡，会造成注入攻击
this.imgs.ForEach(item => item.nativeElement.style.height = '100px');
// 提倡的方式是使用Renderer2
this.imgs.ForEach(item => {
    this.rd2.setStyle(item.nativeElement, 'height', '100px');
});
```



### 获取组件实例

父组件获取子组件实例

```html
<app-image-slider [sliders]="imageSliders"></app-image-slider>
```

```typescript
// 引用模板中的Angular组件时，viewChild 可以使用引用名，也可以使用组件类型
@ViewChild(ImageSliderComponent) imageSlider: ImageSliderComponent;
```

使用引用名获取

```html
<app-footer #footerChild></app-footer>
```

```typescript
@ViewChild('footerChild') footer;

// 调用子组件方法
run(){
	this.footer.footerRun();
}
```



## 内容投影

投影组件简单来说就是动态内容

使用场景：动态内容；容器组件；有冗余的事件传递

`<ng-content select="样式类/HTML标签/指令"><ng-content>`

select添加约束，也可以省略

```html
<!-- 只选取标签 -->
<ng-content select="span"></ng-content>
<!-- 只选取样式类 -->
<ng-content select=".special"></ng-content>
<!-- 只选取指令 -->
<ng-content select="[appGridItem]"></ng-content>  
<!-- 可以写多个ng-content，以及带有不同的约束 -->
```

投影组件适合做容器

ng-content在浏览器中会被`<div class="heading"></div>`所替代，如果不想要这个额外的div，可以使用ng-container替代这个div

```html
<bootstrap-panel>
    <!-- <ng-container class="heading">Heading</ng-container> -->
    <div class="heading">Heading</div>
    <div class="body">Body</div>
</bootstrap-panel>
```

```html
<!-- panel.component.html -->
<div class="panel panel-default">
    <div class="panel-heading">
         <!-- 显示类名为heading的部分 -->
        <ng-content select=".heading"></ng-content>
    </div>
    <div class="panel-body">
        <!-- 显示类名为body的部分 -->
        <ng-content select=".body"></ng-content>
    </div>
</div>
```



## 表单

### 模板驱动

表单的控制逻辑写在组件模板中，适合简单的表单类型

引入FormsModule模块

```typescript
import { FormsModule } from '@angular/forms';
```

将DOM表单转换为Angular表单

Angular表单提供了表单相关的很多信息，比如当前表单内容，当前表单是否操作过，当前表单有没有通过验证等

- ##### 转换Angular表单

  ```html
  <!-- 添加模板变量用来代表DOM对象,为这个模板变量赋值为ngForm(固定).这样就变成了Angular的表单对象 -->
  <!-- 表单提交时，把表单对象作为参数传递到组件中 -->
  <form #f="ngForm" (submit)="onSubmit(f)">
      <!-- 绑定到表单 添加属性 ngModel -->
      <!-- 表单项需要name属性，获取表单对象时，name属性的值会作为对象的属性存在 -->
      <input type="text" name="username" ngModel />
      <button>提交</button>
  </form>
  ```

  ```typescript
  import { NgForm } from '@angular/forms';
  export class AppComponet {
      onSubmit(form: NgForm) {
          // 在表单填写的数据
          console.log(form.value)
          // NgForm 中有很多属性 valid(验证是否通过), invalid(验证是否没有通过)
      }
  }
  ```


- ##### 表单分组

  ```html
  <form #f="ngForm" (submit)="OnSubmit(f)">
      <!-- 为表单项分组 赋值为分组的名字 -->
      <div ngModelGroup="user">
          <input type="text" name="username" ngModel />
      </div>
      <div ngModelGroup="contact">
          <input type="text" name="photo" ngModel />
      </div>
      <button>提交</button>
  </form>
  ```

- ##### 表单验证

  ```html
  <!--
  required 必填字段
  minlength 字段最小长度
  maxlength 字段最大长度
  pattern 验证正则 pattern="\d" 匹配一个数值
  -->
  <form #f="ngForm" (submit)="OnSubmit(f)">
      <!-- 为表单项设置模板变量 赋值为ngModel(固定) -->
  	<input #username="ngModel" type="text" name="username" ngModel required minlength="2" maxlength="6" />
      <!-- 验证错误信息 -->
      <!-- touched 用户是否获取焦点 -->
      <!-- 用户操作过表单项 && 没有通过验证 && 有验证错误 -->
      <div *ngIf="username.touched && !username.valid && username.errors">
          <div *ngIf="username.errors.required">请填写用户名</div>
          <div *ngIf="username.errors.minlength">
              用户名最少{{ username.errors.minlength.requiredLength }}个字符
          </div>
      </div>
      <!-- 表单整体未通过禁止提交表单 -->
      <button type="submit" [disabled]="f.invalid">提交</button>
  </form>
  ```

  ```typescript
  onSubmit(form: NgForm) {
      // 表单整体验证是否通过
      console.log(form.valid)
  }
  ```

  验证错误样式

  ```css
  /*
  .ng-touched 用户获取过焦点
  .ng-dirty 用户更改过表单值
  */
  input .ng-touched .ng-invalid {
      border: 2px solid red;
  }
  ```




### 模型驱动/响应式

表单的逻辑控制写在组件类中，对验证逻辑有更多的控制权，适合复杂的表单

在模型驱动表单中，表单中的每一个表单项，都必须是FormControl类的实例。实例对象可以验证表单字段中的值是否被修改过等等

整个表单都是FromGroup类的实例，可以对表单进行整体验证

FromControl 表单组中的一个表单项

<img src="Angular.assets/image-20220109235148992.png" alt="image-20220109235148992" style="zoom: 67%;" /> 

FromGroup 表单组 表单至少是一个FromGroup

<img src="Angular.assets/image-20220109235210162.png" alt="image-20220109235210162" style="zoom:67%;" /> 

FromArray 用于复杂表单，可以动态添加表单项或表单组，在表单验证时，FromArray 中有一项没有通过，整体就没通过。

- ##### 表单控制以及配置验证规则

  引入ReactiveFormsModule

  ```typescript
  import { ReactiveFormsModule } from '@angular/forms';
  ```

  创建表单控制对象

  ```typescript
  import { FormControl, FromGroup, Validators } from '@angular/forms';
  export class AppComponet {
      contactForm: FormGroup = new FormGroup({
          // 参数1 设置默认值 参数2 验证规则
          name: new FormControl("", {
              Validators.required,
              Validators.minLength(2)
          }),
          phone: new FormControl()
      })
      
  	onSubmit() {
          // 获取表单值
  	    console.log(this.contactForm.value);
          // 是否通过验证
          console.log(this.contactForm.valid);
  	}
  }
  ```

  关联模板表单

  ```html
  <form [fromGroup]="contactForm" (submit)="onSubmit()">
      <!-- formControlName 绑定的是字符串 -->
      <input type="text" formControlName="name" />
      <div *ngIf="name.touched && name.invalid && name.errors">
          <div *ngIf="name.errors.required">
              请填写姓名
          </div>
          <div *ngIf="name.errors.maxlength">
              姓名长度不能大于{{ name.errors.maxlength.reqiuredLength }}
              实际填写长度为{{ name.errors.maxlength.actualLength }}
          </div>
      </div>
      <input type="text" formControlName="phone" />
      <button [disabled]="contactForm.invalid">提交</button>
  </form>
  ```


- ##### 表单分组

  ```typescript
  contactForm: FormGroup = new FormGroup({
      fullName: new FormGroup({
          firstName: new FormControl(),
          lastName: new FormControl()
      }),
      phone: new FormControl()
  })
  onSubmit() {
      console.log(this.contactForm.value.fullName.firstName);
      console.log(this.contactForm.get(["fullName", "firstName"])?.value)
  }
  ```

  ```html
  <form [fromGroup]="contactForm" (submit)="onSubmit()">
      <div fromGroupName="fullName">
  		<input type="text" formControlName="firstName" />
  		<input type="text" formControlName="lastName" />      
      </div>
  	<input type="text" formControlName="phone" />
      <button>提交</button>
  </form>
  ```


- ##### FromArray 动态创建表单

  ```typescript
  import { FormControl, FromGroup, FormArray } from '@angular/forms';
  
  contactForm: FormGroup = new FormGroup({
      // FormArray可以动态的向其中添加FormControl和FormGroup
      contacts: new FormArray([])
  })
  
  get contacts() {
      return this.contactForm.get("contacts") as FormArray
  }
  
  // 动态添加联系方式
  addContact() {
      // 创建联系方式组
      const myContact: FormGroup = new FormGroup({
          name: new FormControl(),
          address: new FormControl(),
          phone: new FormControl()
      })
      // 向联系方式数组添加联系方式
      this.contacts.push(myContact);
  }
  
  ngOnInit() {
      // 默认显示一组联系方式
      this.addContact();
  }
  
  // 删除联系方式
  removeContact(index: number) {
      this.contacts.removeAt(index);
  }
  ```

  ```html
  <form [fromGroup]="contactForm">
      <div formArrayName="contacts">
          <!-- FormArray中的属性controls里绑定的是FormGroup的集合 -->
          <div *ngFor="let contact of contacts.controls; let i = index" [formGroupName]="i">
              <input type="text" formControlName="name" />
              <input type="text" formControlName="address" />
              <input type="text" formControlName="phone" />
              <button (click)="removeContact(i)">删除联系方式</button>
          </div>
      </div>
      <button (click)="addContact()">添加联系方式</button>
  </form>
  ```


- ##### 自定义表单验证器

  自定义验证器类型是TypeScript类。类中包含具体的验证方法，验证方法必须为静态方法

  验证方法有一个类型为 AbstractControl的参数，是 FromControl 类的实例对象的类型

  如果验证成功，返回null。

  如果验证失败，返回对象，对象中的属性即为验证标识，值为 true，标识该项验证失败

  验证方法的返回值为ValidationErrors | null

  ```typescript
  export class MyValidators {
      // 参数 验证表单项的实例对象
      // 返回类型 ValidationErrors | null
      static cannotContainSpace(control: AbstractControl): ValidationErrors | null{
          if(/\s/.test(control.value)){
              return {
                  cannotContainSpace: true
              }
          }
          return null;
      }
      
      // 异步的验证规则
      static shouldBeUnique(control: AbstractControl): Promise<ValidationErrors | null>{
          return new Promise((resolve) => {
             setTimeout(() => {
                 if(control.value === 'admin'){
                     resolve({shouldBeUnique: true})
                 }else{
                     resolve(null)
                 }
             },2000);                
  		});
      }
  }
  ```

  ```typescript
  contactForm: FormGroup = new FormGroup({
      name: new FormControl(
          // 参数1 默认值
          "",
   	    // 参数2 同步的验证规则
          { 
           Validators.required,
           Validators.minLength(2),
       	 // 如果不需要传参，就不用调用方法
       	 MyValidators.cannotContainSpace
   	    },
   		// 参数3 异步的验证规则
   		MyValidators.shouldBeUnique
  	),
      phone: new FormControl()
  })
  ```

  ```html
  <form [fromGroup]="contactForm" (submit)="onSubmit()">
      <!-- formControlName 绑定的是字符串 -->
      <input type="text" formControlName="name" />
      <div *ngIf="name.touched && name.invalid && name.errors">
          <div *ngIf="name.errors.cannotContainSpace">名字中不能包含空格</div>
          <div *ngIf="name.errors.shouldBeUnique">名字已经存在</div>
      </div>
      <!-- 异步方法验证 -->
      <div *ngIf="name.pending">正在验证...</div>
      <button [disabled]="contactForm.invalid">提交</button>
  </form>
  ```

- ##### FormBulider 创建模型表单快捷方式

  ```typescript
  import { FormBuilder, FromGroup, Validators } from '@angular/forms';
  
  export class AppComponet {
      contactForm: FormGroup;
      // 实现FormBulider的实例对象
  	constructor(private fb: FormBulider){ 
          // fb.group 创建表单组
          // fb.array 创建FromArray
      	this.contactForm = this.fb.group({
              fullName: this.fb.group({
                  // 数组第一项 默认值， 第二项 验证
                  firstName: ["", [Validators.required]],
                  lastName: [""]
              }),
              phone: {}
          })
      }
  }
  ```

- ##### 模型驱动表单常用方法

  ```typescript
  form: FormGroup = new FormGroup({
      firstName: new FormControl(),
      lastName: new FormControl()
  })
  
  // patchValue 设置表单控件的值
  onPatchValue(){
      this.form.patchValue({
          firstName: "test"
      })
  }
  
  // setValue 设置表单控件的值，只能设置所以，不能排除任何一个
  onSetValue(){
      this.form.setValue({
          firstName: "test",
          lastName: "test123"
      })
  }
  
  // 表单内容清空
  onReset(){
      this.form.reset()
  }
  
  ngOnInit(){
      // valueChanges 表单控件值发生变化时触发的事件
      this.form.get('lastName')?.valueChanges()
          .subscribe(value => {
          // value 最新变化的值
          console.log(value)
      })
  }
  
  ```

  ```html
  <form [fromGroup]="form" (submit)="onSubmit()">
      <!-- formControlName 绑定的是字符串 -->
      <input type="text" formControlName="firstName" />
      <input type="text" formControlName="lastName" />
      <button (click)="onPatchValue()">patchValue</button>
      <button (click)="onSetValue()">setValue</button>
      <button (click)="onReset()">reset</button>
      <button>提交</button>
  </form>
  ```

- ##### 表单使用双向绑定

  模型驱动表单也可以使用required，minlength等指令

  使用了双向绑定就不能使用 formControlName

  ```html
  <form>
      <input #username="ngModel" type="text" name="username" [(ngModel)]="value" required minlength="2" maxlength="6" />
  </form>
  ```

  ```typescript
  value: string = '';
  ```

  使用表单项模板变量ngModel对象，修改FromControl对象

  ```typescript
  // 表单项模板变量,通过#username="ngModel" 形式指定
  @ViewChild('username') model: NgModel;
  // FromControl对象
  get fromControl (): FormControl {
      return this.model.control;
  }
  // 添加验证方法
  ngAfterViewInit() {
      this.fromControl.addValidators([Validators.required]);
      // 清除验证 clearValidators
      // 更新验证 updateValueAndValidity
  }
  ```



## 全局样式

```css
/* 方式一 在 style.css 文件中 */
@import "~bootstrap/dist/css/bootstrap.css";
/* ~ 相对于node_module文件夹 */
```

```html
<!-- 方式二 在 index.html 文件中-->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
```

```json
// 方式三 在 angular.json 文件中
"style": [
    "./node_module/bootstrap/dist/css/bootstrap.min.css",
    "src/styles.css"
]
```



# 指令 Directive

指令没有模板，指令要寄宿在一个宿主之上。指令是提供操作DOM的途径

结构性指令：会改变宿主文档DOM结构。

属性型指令：会改变宿主行为

组件就是带模板的指令。

## 结构性指令 

增加，删除DOM节点以修改布局，使用*作为指令前缀

### *ngFor

- ##### 变量的应用范围

  ```html
  <!-- menu变量的作用范围是所在的标签的闭合区间之内 -->
  <ul>
    <li *ngFor="let menu of menus">
      <a href="#">{{ menu.title }}</a>
    </li>
  </ul> 
  ```

- ##### 索引的获取

  ```html
  <ul>
    <li *ngFor="let menu of menus; let i = index">
      <a href="#">{{ menu.title }}</a>
    </li>
  </ul> 
  ```

- ##### 是否是第一个元素，最后一个元素

  ```html
  <ul>
    <!-- 属性是bool类型 -->
    <li *ngFor="let menu of menus; let first = first; let last = last">
      <a href="#">{{ menu.title }}</a>
    </li>
  </ul> 
  ```

- ##### 奇和偶

  ```html
  <ul>
    <!-- 数组索引的奇偶 -->
    <li *ngFor="let menu of menus; let odd = odd; let even = even">
      <a href="#">{{ menu.title }}</a>
    </li>
  </ul> 
  ```

- ##### 提升性能

  ```html
  <ul>
    <!-- 使用数组中唯一能表达某个元素的值，利用这个值进行渲染排序 -->
    <!-- trackBy 后可以接函数名和表达式 -->
    <li *ngFor="let menu of menus; trackBy: trackElement">
      <a href="#">{{ menu.title }}</a>
    </li>
  </ul> 
  ```

  ```typescript
  // trackBy函数
  identity (index: number, item: List){
      return item.id
  }
  ```

- ##### template 写法

  ```html
  <ul>
  	<li template="ngFor let item of list">
  		{{item}}
  	</li>
  </ul>
  ```



### *ngIf

- ##### 条件表达式

  ```html
  <!-- 节点只有在条件表达式为真的时候才会显示出来 -->
  <div *ngIf="条件表达式">
      在条件为真的时候需要显示的内容
  </div>
  ```

- ##### If..else

  ```html
  <div *ngIf="条件表达式 else elseContent">
      在条件为真的时候需要显示的内容
  </div>
  <ng-template #elseContent>
  	在条件为假的时候需要显示的内容
  </ng-template>
  ```

  ```html
  <div *ngIf="条件表达式;then thenTemplate else elseTemplate">
  </div>
  <ng-template #thenTemplate>
  	在条件为真的时候需要显示的内容
  </ng-template>
  <ng-template #elseTemplate>
  	在条件为假的时候需要显示的内容
  </ng-template>
  ```

- ##### template 写法

  ```html
  <p template="ngIf list.length > 3">判断是否显示</p>
  ```



### *ngSwitch

```html
<ul [ngSwitch]="score">
	<li *ngSwitchCase="1">已支付</li>
	<li *ngSwitchCase="2">订单已经确认</li>
	<li *ngSwitchCase="3">已发货</li>
	<li *ngSwitchDefault>无效</li>
</ul>
```



## 属性型指令

修改现有元素的外观或行为，使用[]包裹

### ngClass

```html
<!-- flag是定义的布尔值 -->
<div [ngClass]="{'red': flag, 'blue': !flag}">
	这是一个div
</div>

<li *ngFor="let item of arr, let i = index">
	<span [ngClass]="{'red': i==0 }">{{ item }}</span>
</li>
```

- ##### 样式绑定

  ```html
   <div [class.className]="条件表达式">
       对于单个样式的条件绑定最为合适
   </div>
  ```



### ngStyle

```html
<div [ngStyle]="{'color':someColor, 'font-size':fontSize}">
    ngStyle由于是嵌入式样式，会覆盖掉其他样式，使用时要谨慎
</div>

<!-- 已定义 public attr='red'; -->
<div [ngStyle]="{'background-color':attr}">你好ngStyle</div>
```



### hidden

```html
<!-- 根据条件显示DOM节点或隐藏DOM节点（display）true隐藏，false隐藏 -->
<div [hidden]="data.length == 0">课程列表</div>
<div [hidden]="data.length > 0">没有更多数据</div>
```



## 指令结构

使用指令操作DOM元素

```typescript
// 网格布局模版
@Directive({
  // 推荐使用方括号[]指定Selector，可以当成宿主元素属性
  selector: '[appGridItem]'
})
export class GridItemDirective implements AfterViewInit {
  // 注入 ElementRef 获取宿主的实例对象
  constructor(private elr: ElementRef, private renderer: Renderer2) {}
  // 组件视图初始完成
  ngAfterViewInit() {
    this.setStyle('display', 'grid');
    this.setStyle('grid-template-areas', `'image' 'title'`);
    this.setStyle('place-items', 'center');
    this.setStyle('width', '4.3rem');
  }
  private setStyle(styleName: string, styleValue: string | number) {
    this.renderer.setStyle(this.elr.nativeElement, styleName, styleValue);
  }
}
```

指令传入参数

```typescript
// 模版image区块
@Directive({
  selector: '[appGridItemImage]'
})
export class GridItemImageDirective implements AfterViewInit {
  @Input() appGridItemImage = '2rem';
  constructor(private elr: ElementRef, private renderer: Renderer2) {}
  ngAfterViewInit() {
    this.setStyle('grid-area', 'image');
    this.setStyle('width', this.appGridItemImage);
    this.setStyle('height', this.appGridItemImage);
    this.setStyle('object-fit', 'cover');
  }
  private setStyle(styleName: string, styleValue: string | number) {
    this.renderer.setStyle(this.elr.nativeElement, styleName, styleValue);
  }
}
```

指令传入对象参数

```typescript
interface Option {
    fontSize?: string;
}

// 模版title区块
@Directive({
  selector: '[appGridItemTitle]'
})
export class GridItemTitleDirective implements AfterViewInit {
  @Input() appGridItemTitle: Option = {};
  constructor(private elr: ElementRef, private renderer: Renderer2) {}
  ngAfterViewInit(): void {
    this.setStyle('grid-area', 'title');
    this.setStyle('font-size', this.appGridItemTitle.fontSize || '0.5rem');
  }
  private setStyle(styleName: string, styleValue: string | number) {
    this.renderer.setStyle(this.elr.nativeElement, styleName, styleValue);
  }
}
```

模板引用

```html
<!-- 指令所在的元素叫宿主 -->
<span appGridItem *ngFor="let item of channels">
  <img appGridItemImage="2rem" [src]="item.icon" alt="" />
  <span [appGridItemTitle]="{fontSize: '0.6rem'}" class="title">{{ item.title }}</span>
</span>
```



## 指令注解





## 指令样式绑定

@HostBinding 绑定宿主的属性或者样式

```typescript
@Directive({
  selector: '[appGridItemTitle]'
})
export class GridItemTitleDirective {
  // 绑定宿主的样式
  // 多个注解可以修饰一个变量
  @HostBinding('style.font-size') @Input() appGridItemTitle = '0.5rem';
  @HostBinding('style.grid-area') area = 'title';
}
```



## 指令事件绑定

@HostListener 绑定宿主的事件

```typescript
@Directive({
  selector: '[appGridItemImage]'
})
export class GridItemImageDirective implements OnInit {
  // 参数1：事件名 参数2：事件所依赖数据
  // 监听宿主元素的点击事件
  @HostListener('click', ['$event.target'])
  handleClick(ev) {
    console.log(ev);
  }
}
```

组件的样式也可使用:host这样一个伪类选择器

:host 伪类所应用的样式不是模板中的样式，而是组件本身



# 管道 Pipe

管道的作用就是在视图上提供便利的值变换的方法

## 内置管道

- ##### json序列化

  ```html
  {{ obj | json }}
  <!-- 换行的格式 -->
  <pre>{{ obj | json }}</pre>
  ```

- ##### 日期

  ```html
  {{ date | date: 'yyyy-MM-dd HH:mm:ss' }}
  ```

- ##### 小数位

  ```html
  <!-- 4.0-2 (最小几位整数.小数部分最小几位-小数部分最大几位) -->
  {{ price | currency: 'CNY' : 'symbol' : '4.0-2' }}
  ```

- ##### 截取

  ```html
  <!-- 取集合索引1到3的元素 -->
  {{ data | slice:1:3 }}
  <!-- 截取字符串 结果：sem -->
  {{ 'semlinker' | slice:0:3 }}
  ```

- ##### 金钱单位

  ```html
  <!-- 使用中文的钱单位 -->
  <!-- 输出结果 CN¥123.32 -->
  {{ price | currency:'CNY' }}
  <!-- ¥123.32 -->
  {{ price | currency:'¥' }}
  ```

  - 去掉CN的方法，注册中文相关的数据

    ```typescript
    import localzh from '@angular/common/locales/zh-Hans';
    import { registerLocaleData } from '@angular/common';
    
    @NgModule( {
      declarations: [ AppComponent ],
      imports: [ BrowserModule, FormsModule ],
      providers: [
        {
          provide: LOCALE_ID,
          useValue: 'zh-Hans'
        }
      ],
      bootstrap: [ AppComponent ]
    } )
    export class AppModule {
      constructor () {
        registerLocaleData( localzh, 'zh' );
      }
    }
    ```

- ##### 大小写

  ```html
  {{ title | uppercase }}
  {{ title | lowercase }}
  ```

- ##### 管道链

  ```html
  {{ 'semlinker' | slice:0:3 | uppercase }}
  ```



## Async管道

```html
<!--
<div *ngIf="selectedTabLink === 'hot'">
    ...
</div>
-->
<!-- 在模板中进行subscribe -->
<div *ngIf="(selectedTabLink$ | async) === 'hot'">
    ...
</div>
<!-- 还可以把流定义成一个变量 -->
<div *ngIf="(selectedTabLink$ | async) as tab">
    <div *ngIf="tab === 'hot'">
      ...  
    </div>
</div>
```

```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class HomeDetailComponent implements OnInit, OnDestroy {
    
    // sub: Subscription;
    
    // selectedTabLink;
    selectedTabLink$: Observable<string>;
    
    ngOnInit() {
        /*
        this.sub = this.route.paramMap.pipe(
      		filter(params => params.has('tabLink')),
      		map(params => params.get('tabLink'))
        ).subscribe(params => {
			console.log('路径参数: ', params);
			this.selectedTabLink = params.get('tabLink');
			this.cd.markForCheck();
    	});
    	*/
        // 直接使用数据流
        // 不需要再写脏值检测的部分，Angular会自动处理
		this.selectedTabLink$ = this.route.paramMap.pipe(
		  filter(params => params.has('tabLink')),
		  map(params => params.get('tabLink'))
		);
    }
    
    ngOnDestroy() {
        // 不需要再清理订阅
    	// this.sub.unsubscribe();
	}
}
```



## 创建管道

```bash
ng g pipe pipe/formattime
```



## 自定义管道

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'appAgo' })
export class AgoPipe implements PipeTransform {
  // 参数1 要处理的数据 参数2 向管道传递的参数
  transform(value: any): any {
    if (value) {
      const seconds = Math.floor((+new Date() - +new Date(value)) / 1000);
      if (seconds < 29) {
        // 小于 30 秒
        return '刚刚';
      }
      const intervals = {
        年: 3600 * 24 * 365,
        月: 3600 * 24 * 30,
        周: 3600 * 24 * 7,
        天: 3600 * 24,
        小时: 3600,
        分钟: 60,
        秒: 1
      };
      let counter = 0;
      for (const unitName in intervals) {
        if (intervals.hasOwnProperty(unitName)) {
          const unitValue = intervals[unitName];
          counter = Math.floor(seconds / unitValue);
          if (counter > 0) {
            return counter + ' ' + unitName + '前';
          }
        }
      }
    }
    return value;
  }
}
```

```typescript
@NgModule({
  declarations: [
      ...
      AgoPipe
  ]
    ....
})
```

```html
<p>{{ date | appAgo }}</p>
```

管道传递参数

```typescript
// 参数1 要处理的数据 参数2 向管道传递的参数
transform(value: any, ...args: number[]): any { }
transform(value: any, num?: number): any { }
```

```html
<!-- 可以传递多个参数 -->
<div>{{ paragraph | summary: 100:200 }}</div>
<div>{{ paragraph | summary: 20 }}</div>
```



# 服务 Service

组件之间没法相互调用，可以把公共的方法放到服务中，实现方法的跨组件共享

服务是由Injectable装饰器装饰的类

## 配置服务

- ##### 创建服务

  ```bash
  ng g service 文件夹名/服务名
  ```


- ##### 模块配置

  ```typescript
  import { Injectable } from '@angular/core';
  
  // @Injectable()标记为可供注入的服务 
  @Injectable()
  export class StorageService {
  }
  ```

  ```typescript
  import { StorageService } from './services/storage.service';
  @NgModule({
    declarations: [AppComponent,SearchComponent,TodolistComponent   ],
    imports: [BrowserModule,FormsModule],
    providers: [StorageService],			//引入并且配置服务
    bootstrap: [AppComponent]
  })
  export class AppModule { }
  ```

  ```typescript
  import { StorageService } from '../../services/storage.service';
  export class TodolistComponent implements OnInit {
      // 构造函数中直接声明 Angular框架帮助完成依赖注入
  	constructor(public storage: StorageService) {}
  }
  ```

- ##### providedIn 配置

  可以不在providers数组中声明，而是在服务上使用Injectable声明 

  优点：在Module中声明，import Module的时候都需要导入，不管用还是没有，编译出来的JS大

  在服务上面声明，直到真正注入了，才会编译到JS中

  ```typescript
  import { Injectable } from '@angular/core';
  
  // @Injectable()标记为可供注入的服务 
  @Injectable({
      // 不需要再在模块中注册
      providedIn: 'root'				// 根目录注册
      // providedIn: HomeModule		// 注入到模块
  })
  export class StorageService { }
  ```

  

## 依赖注入

服务的实例对象由Angular框架中内置的依赖注入系统创建和维护。服务是依赖需要被注入到组件中。

组件需要通过constructor构造函数的参数来获取服务的实例对象

<img src="Angular.assets/image-20220107105456646.png" alt="image-20220107105456646" style="zoom:50%;" /> 

注入器也是可以分级别的，应用级，模块级，组件级。应用范围不同。大部分只需要注册到应用级别

不同的注入器返回不同的实例对象

服务实例的查找类似函数作用域链，当前级别找到就用当前级别，找不到就去父级查找

服务默认是单例模式。这也是为什么服务可以用来在组件之间共享数据和逻辑的原因

- ##### 模块中注入

  在需要注入的服务上标记@Injectable()

  ```typescript
  // @Injectable()标记为可供注入的服务
  @Injectable()
  class Product {
    constructor(private name: string, private color: string) { }
  }
  @Injectable()
  class PurchaseOrder {
      constructor(private product: Product) { }
  }
  ```

  标识符中注入

  ```typescript
  // 直接通过标识符
  @NgModule( {
    providers: [
        // useClass模式的直接写入就可以
        PurchaseOrder
        // { provider: PurchaseOrder, useClass: PurchaseOrder }
    ],
  })
  ```
  
  工厂模式注入
  
  ```typescript
  // 工厂模式
  @NgModule( {
    providers: [ 
  	{
  		provide: Product,
  		useFactory: () => {
  		    return new Product( "大米手机" );
  		  },
  		deps: []
  	}
   ],
  })
  ```
  
  Token注入
  
  ```typescript
  // 使用Token作为标识
  const token = new InjectionToken<string>('BaseUrl');
  @NgModule( {
    providers: [ 
  	{
  		provide: token,
          useValue:'http://localhost'
          // useValue 也可以是对象
          /*
          useValue: Object.freeze({
          	APIKEY: "API123456",
          	APISCRET: "100-222-333"
      	})
      	*/
  	}
   ],
  } )
  
  // 使用Token依赖注入
  constructor(@Inject(token) private baseUrl:string){ }
  ```


- ##### 使用Injector手动注入

  ```typescript
  import { Injector } from '@angular/core';
  
  // @Injectable()标记为可供注入的服务
  @Injectable()
  class Product {
    constructor(private name: string, private color: string) { }
  }
  @Injectable()
  class PurchaseOrder {
      constructor(private product: Product) { }
  }
  
  export class HomeGrandComponent implements OnInit {
      ngOnInit() {
          // 使用Injector依赖注入
          // 默认是单例模式 所有的对象都已经创建好了
   		const injector = Injector.create({
              // providers数组里去描述服务怎么去创建
   		     providers: [
   		       {
                   // provide 设置标识符 需要依赖这个服务的时候用来匹配到这里
   		         provide: Product,
                   // useClass 直接new这个类
                   // useClass: Product,
                   // useExisting 使用现存的已经实例化的
                   // useValue 不想注入类，只想注入一个字符串等
                   // useFactory 工厂模式
   		         useFactory: () => {
   		           return new Product('大米手机', '黑色');
   		         },
                   // deps 描述要创建的这个对象的依赖性
                   //（不是要完全实现对象的构造函数，而是如果还依赖providers里面的其他东西，则在这里去提供）
   		         deps: []
   		       },
   		        { 
                   provide: PurchaseOrder,
                   deps: [Product]
                 }
   		     ]
   		});
          console.log(injector.get(PurchaseOrder).getProduct);
      }
  }
  ```



## Http数据交互

### HTTP方法配置

app.module.ts 中导入**HttpClientModule** 

- 只在根模块中导入
- 整个应用只需导入一次，不要再其他模块导入

```typescript
import {HttpClientModule} from '@angular/common/http';

imports: [
	HttpClientModule
]
```

使用到的地方引入**HttpClient** 并在构造函数声明

```typescript
// 当做一个服务
import { HttpClient } from '@angular/common/http';

// 依赖注入
constructor(public http: HttpClient) { }
```



### 请求方法

get/post/put/delete 方法对应于Http方法

```typescript
this.http.get(url [, options]);
this.http.post(url, data [, options]);
this.http.delete(url [, options]);
this.http.put(url, data [, options]);
```

这些方法是泛型的，可以直接把返回的JSON转换成对应类型

不规范的请求，使用request方法

返回的值是Observable

```typescript
this.http.get<Post[]>('/getAllPosts').subscribe(response => console.log(response))
```

必须订阅，才会发生请求，否则不会发送 

- ##### 数据请求

  ```typescript
  public list:any[]=[];
  getData() {
  	// 服务器必须允许跨域
  	let api="http://a.itying.com/api/productlist";
      // 使用rxjs
  	this.http.get<Product[]>(api, params: { category: '手机' })
      .subscribe((response:any)=>{
  	    console.log(response);
  	    this.list = response.result;
  	})
  }
  ```

- ##### 数据提交

  ```typescript
  import {HttpClient,HttpHeaders} from "@angular/common/http";
  
  constructor(public http:HttpClient) { }
  
  doLogin(){
      //手动设置请求的类型
  	const httpOptions = {
  		headers: new HttpHeaders({ 'Content-Type': 'application/json' })
  	};
  	var api = "http://127.0.0.1:3000/doLogin";
  	this.http.post(api,{username:'张三',age:'20'},httpOptions).subscribe(response => {
  		console.log(response);
  	});   
  }
  ```

- ##### Jsonp请求数据

  - Jsonp配置

    在app.module.ts 中引入HttpClientModule、HttpClientJsonpModule 并注入

    ```typescript
    import {HttpClientModule,HttpClientJsonpModule} from '@angular/common/http';
    ```

    ```typescript
    imports: [
    	BrowserModule,
    	HttpClientModule,
    	HttpClientJsonpModule
    ]
    ```

  jsonp 请求数据

  ```typescript
  import {HttpClient} from "@angular/common/http";
  
  constructor(public http:HttpClient) { }
  
  getJsonpData(){
  	//jsonp请求 服务器必须得支持jsonp
  	let api="http://a.itying.com/api/productlist";
  	this.http.jsonp(api,'callback').subscribe((response)=>{
  	  console.log(response);
  	})
  }
  ```

- ##### axios 请求数据

  安装axios

  ```bash
  npm install axios --save
  ```

  ```typescript
  // 引入axios
  import axios from 'axios';
  
  axiosGet(api:string){
  	return new Promise((resolve,reject)=>{
  		axios.get(api).then(function (response:any) {
  			resolve(response)
          });
  	})
  }
  
  getAxiosData(){
  	console.log('axios获取数据');
  	let api="http://a.itying.com/api/productlist";
  	this.httpService.axiosGet(api).then((data)=>{
  	    console.log(data)
  	})
  }
  ```



### 请求参数

- ##### HttpParams 类

  ```typescript
  import { HttpParams } from '@angular/common/http';
  
  // 请求参数以对象的形式传递
  let params = new HttpParams({ fromObject: { name: "zhangsan", age: "20" } });
  // HttpParams内的append、set等方法的返回值都是HttpParams
  params = params.append("sex", "male");
  // 以查询参数的形式传递
  let params = new HttpParams({ fromString: "name=zhangsan&age=20" })
  
  this.http.get("/users", {
      // params: params 的简写
      params
  }).subscribe(console.log)
  ```



### 请求头

- ##### HttpHeaders

  ```typescript
  import { HttpHeaders } from '@angular/common/http';
  
  let headers = new HttpHeaders({
      test: "Hello"
  })
  
  this.http.get("/users", {
      // headers: headers 的简写
      headers
  }).subscribe(console.log)
  ```



### 响应内容

- ##### HttpObserve

  ```typescript
  // type HttpObserve = 'body' | 'response'
  // response 读取完整响应体
  // body 读取服务器端返回的数据
  
  this.http.get("/users", {
      // 默认是 body
      // observe: 'body'
      observe: 'response'
  }).subscribe(console.log)
  ```
  
  <img src="Angular.assets/image-20220111230356008.png" alt="image-20220111230356008" style="zoom:50%;" /> 
  
  <img src="Angular.assets/image-20220111230423949.png" alt="image-20220111230423949" style="zoom: 67%;" /> 





## 拦截器

拦截器是Angular应用中全局捕获和修改HTTP请求和响应的方式（添加Token，捕获Error）

拦截器将只拦截HttpClientModule模块发出的请求

创建拦截器

```bash
ng g interceptor <name>
```

<img src="Angular.assets/image-20220111231714345.png" alt="image-20220111231714345" style="zoom:50%;" /> 

- ##### 对请求进行截断

  ```typescript
  import { Injectable } from '@angular/core';
  import { HttpInterceptor, HttpHandler, HttpRequest } from '@angular/common/http';
  
  @Injectable()
  export class ParamInterceptor implements HttpInterceptor {
      // 拦截方法 参数1: 请求(泛型指定的是请求体的类型) 参数2: 下一步的处理
      intercept(req: HttpRequest<any>, next: HttpHandler) {
      	// 对请求消息进行处理
          // 请求不能直接修改 克隆去修改请求
      	const modifiedReq = req.clone({
            // 统一添加请求路径前缀
      	  setParams: { icode: environment.icode },
            // 统一添加Token
            setHeaders: { Authorization: "Bearer XXXX" }
      	});
          // 把修改后的请求回传给应用
          return next.handle(modifiedReq);
      }
  }
  ```

  根模块引入

  ```typescript
  import { HTTP_INTERCEPTORS } from '@angular/common/http';
  @NgModule({
      providers: [
          // 多个令牌 multi: true
  		{ provide: HTTP_INTERCEPTORS, useClass: ParamInterceptor, multi: true }
      ]
  })
  ```

- ##### 对响应进行截断

  ```typescript
  import { Injectable } from '@angular/core';
  import { HttpEvent, HttpInterceptor, HttpHandler, HttpRequest } from '@angular/common/http';
  import { tap } from 'rxjs/operators';
  
  @Injectable()
  export class ParamInterceptor implements HttpInterceptor {
      // 拦截方法 参数1: 请求 参数2: 下一步的处理
      intercept(req: HttpRequest<any>, next: HttpHandler) {
      	// 对响应消息进行处理
      	return next.handle(req).pipe(
      	  tap((event: HttpEvent<any>) => {
      	    if ( event instanceof HttpResponse && event.status >= 200 && event.status < 300 ) {
      	      console.log('[此处假装弹出消息] 请求成功！');
      	    }
      	  })
      	);
      }
  }
  ```

  根模块引入

  ```typescript
  import { HTTP_INTERCEPTORS } from '@angular/common/http';
  @NgModule({
      providers: [
          // 多个令牌 multi: true
  		{ provide: HTTP_INTERCEPTORS, useClass: NotificationInterceptor, multi: true }
      ]
  })
  ```




## 代理

解决跨域问题

1. 在项目的根目录下创建 proxy.conf.json 文件并加入如下代码

   ```json
   {
       "/api/*": {
           "target": "http://localhost: 3070",
           "secure": false,
           "changeOrigin": true
       }
   }
   ```

   - /api/*：在应用中发出的以/api/开头的请求走此代理

     应用中的http请求api地址也不需要写前缀，直接以/api开头

     ```typescript
     // this.http.get("http://localhost:3005/api/hello")
     this.http.get("/api/hello")
     ```

   - target：服务器端URL

   - secure：如果服务器端URL的协议是https，此项需要为true

   - changeOrigin：如果服务器端不是localhost，此项需要为true

2. 指定proxy 配置文件

   方式一 package.json 指定

   ```json
   "scripts": {
       "start": "ng serve --proxy-config proxy.conf.json"
   }
   ```

   方式二 angular.json 指定

   ```json
   "serve": {
       "options": {
           "proxyConfig": "proxy.conf.json"
       }
   }
   ```



# 模块 Module

模块就是提供相对独立功能的一组代码。主要的作用就是让程序更有序，独立的功能放到独立的模块中去，让模块更好的维护。

模块的组成部分可以有：组件，服务，指令，管道等、从某种角度说，像一个小型应用

Angular应用中至少需要一个根模块，用于启动

## 内置模块

- ##### 需要在每个需要的模块中进行导入的模块

  1. CommonModule：提供绑定。*ngIf 和 *ngFor等基础指令，基本上每个模块都需要导入它。
  2. FormsModule/ReactiveFormsModule：表单模块需要在每个需要的模块导入
  3. 提供组件，指令，管道的模块

- ##### 只需要的在根模块导入一次的模块

  1. HttpClientModule/BrowserAnimationsModule/NoopAnimationsModule/BrowserModule

     (在根模块不需要单独导入CommonModule，因为BrowserModule已经顺带导出了)

  2. 只提供服务的模块

<img src="Angular.assets/image-20220103154139291.png" alt="image-20220103154139291" style="zoom: 67%;" /> 



## 创建模块

```bash
ng g module 文件夹名/模块名
# 同时创建路由
ng g module 模块 --routing
# 创建模块直接放到已有文件夹中，不新建文件夹
ng g module 模块 --flat=true
```



## 模块结构

```typescript
@NgModule({
  // 模块拥有的组件，指令或管道。注意每个组件，指令，管道只能在一个模块中声明
  // 如果想要导入的模块已经通过其所在的模块被导入了，就不需要单独再导入这个模块
  declarations: [UserComponent],
  // 导入本模块需要的依赖模块，只能是模块
  imports: [CommonModule],
  // 暴露给其他模块使用的模块，组件，指令，管道等。
  exports:[UserComponent],
  // 模块中需要使用的服务
  providers:[CommonService],
  // 引导组件 进入这个模块后需要显示的是什么 只有根组件有这个属性
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- ##### 模块导入注意点

  - 如果是组件，那么需要在每一个需要模块都进行导入
  - 如果是服务，那么一般来说在根模块中导入一次即可



## 自定义模块

项目非常庞大的时候把所有的组件都挂载到根模块里面，会导致项目加载很慢。这个时候可以自定义模块来组织项目

- ##### 自定义模块文件结构

  ```bash
  #自定义user模块
  #整体结构和根模块类似
  /app
  - /module
  -- /user
  --- /components
  ---- /address
  ----- address.component.html
  ----- address.component.ts
  ---- /order
  ----- order.component.html
  ----- order.component.ts
  --- /services
  ---- common.service.ts
  --- user.component.html
  --- user.component.ts
  --- user.module.ts
  ```

- ##### 自定义模块例

  自定义模块user.module.ts

  ```typescript
  import { NgModule } from '@angular/core';
  import { CommonModule } from '@angular/common';
  // 导入子组件
  import { CommonService } from './services/common.service';
  import { AddressComponent } from './components/address/address.component';
  import { OrderComponent } from './components/order/order.component';
  import { UserComponent } from './user.component';
  
  @NgModule({
    // 声明user模块里面的组件
    declarations: [ProfileComponent, AddressComponent, OrderComponent, UserComponent],
    // 暴露组件 让其他模块里面可以使用暴露的组件
    exports:[UserComponent],
    providers:[CommonService],
    imports: [CommonModule]
  })
  export class UserModule { }
  ```

  根模块app.module.ts

  ```typescript
  //引入自定义模块
  import { UserModule } from './module/user/user.module';
  
  @NgModule({
    declarations: [AppComponent, HomeComponent, NewsComponent],
    imports: [
      BrowserModule,
      FormsModule,
      HttpClientModule,
      // 在根模块引入
      UserModule
    ],
    providers: [],
    bootstrap: [AppComponent]
  })
  export class AppModule { }
  ```

  其他模块调用

  ```html
  <app-user></app-user>
  ```

  <img src="Angular.assets/image-20220106202113397.png" alt="image-20220106202113397" style="zoom: 33%;" /> 



# 路由 Route

路由本质上是切换视图的一种机制

通过路由插座<router-oulet></router-oulet>

切换页面的时候，路由显示的内容是插入到 router-outlet 的同级的下方节点，而不是在 router-outlet 中包含

Angular是单页程序，路由显示的路径不过是一种保存路由状态的机制，这个路径在web服务器上不存在。如果刷新服务器可能会找不到，需要重定向对应。

路由是以模块为单位的，每个模块都可以有自己的路由

## 路由配置

根路由 app-routing.module.ts

```typescript
// 路由是按顺序执行的
const routes: Routes = [
    //{path: 'home', component: HomeComponent},
	{path: 'news', component: NewsComponent},
	{path: 'newscontent/:id', component: NewscontentComponent},
    // 重定向
    // pathMatch：prefix 以定义的path开头；full 定义的path完全匹配
	{path: '', redirectTo: 'home', pathMatch: 'full'}
    // 匹配任意的路由（以上都不存在的情况）
    {path:'**', redirectTo: 'home'}
];
@NgModule({
  // 根路由使用 RouterModule.forRoot(routes)
  // 启用路由的 debug 跟踪模式，需要在根模块中设置 `enableTracing: true`
  // 启用Hash路由  forRoot(routes, { useHash: true })
  imports: [RouterModule.forRoot(routes, { enableTracing: true })],
  // 要导出
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

根组件 app.component.html

```html
<h1>
    <!-- routerLinkActive默认选中，参数是.active类选择器css -->
    <a routerLink="/home" routerLinkActive="active">首页</a>
	<a routerLink="/news" routerLinkActive="active">新闻</a>
</h1>

<!-- 配置router-outlet 显示动态加载的路由 匹配到的路由组件会显示在这个地方 -->
<router-outlet></router-outlet>
```

子路由

```typescript
/**
 * 在功能模块中定义子路由后，只要导入该模块，等同于在根路由中直接定义
 * 也就是说在 AppModule 中导入 HomeModule 的时候，
 * 由于 HomeModule 中导入了 HomeRoutingModule
 * 在 HomeRoutingModule 中定义的路由会合并到根路由表
 * 相当于直接在根模块中定义下面的数组。
 */
const routes: Routes = [
  { path: 'home', component: HomeContainerComponent}
];
@NgModule({
  // 子路由 forChild
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class HomeRoutingModule {}
```



## 路由跳转

### 无参数

**URL结果：/home**

```typescript
{ path: 'home', component: HomeComponent },
```

```html
<a routerLink="/home">首页</a>
<a [routerLink]="['/home']">首页</a>
```

```typescript
// 引入
import { Router} from '@angular/router';
// 依赖注入
constructor(public router:Router) { }
// 以根路由跳转/home
this.router.navigate(['home']);
```



### 查询参数

**URL结果：/home?name=val1**

```html
<a [routerLink]="['/home']" [queryParams]="{name: 'val1'}">...</a>
```

```typescript
// 引入
import { Router} from '@angular/router';
// 依赖注入
constructor(public router:Router) { }
// 传入查询参数
this.router.navigate(['home'], { queryParams: {name:'val1'} });
```

查询参数读取

```typescript
import { ActivatedRoute } from '@angular/router';

// 获取查询参数传值
this.route.queryParamMap.subscribe((data)=>{
	console.log(data);
    console.log(data.get('name');
}
```

可以使用NavigationExtras配置传参

```typescript
// 引入NavigationExtras
import { Router, NavigationExtras} from '@angular/router';
// 用NavigationExtras 配置传参
goNewsContent(){
	let navigationExtras: NavigationExtras = {
		queryParams: { 'session_id': '123' },
		fragment: 'anchor'
	};
	this.router.navigate(['/news'], navigationExtras);
}
```



### 路径参数(必选)

路径参数是以:开头的，作为url的一部分

路径参数的变化在默认的路由策略之下，不会销毁这个组件，会重用这个组件，ngOnInit只走一遍

**URL结果：/newscontent/123**

```typescript
{ path:'newscontent/:key',component:NewscontentComponent }
```

```html
<a [routerLink]="['/newscontent/', key ]">...</a>
```

```typescript
// 注意路径参数传值, 参数写在[]里面
this.router.navigate(['newscontent', key]);
```

```typescript
// 获取动态路由传值
this.route.paramMap.subscribe((data)=>{
  console.log(data);
  console.log(data.get('key');
});
```



### 路径参数(可选)

**URL结果：/home/sports;name=val1**

```typescript
{ path: ':tabLink', component: HomeDetailComponent }
```

```html
<!-- 有必须参数，也有可选参数. 可选参数用{}包起来-->
<a [routerLink]="['/home', tab.link, {name: 'val1'}]">...</a>
<!-- 只有必须参数
<a [routerLink]="['/home', tab.link]">...</a>
-->
```

```typescript
// 可选参数用{}包起来
this.router.navigate(['home', tab.link, {name: 'val1'}]);
/* 只有必须参数
this.router.navigate(['home', tab.link]);
*/
```

```typescript
// 获取动态路由传值
this.route.paramMap.subscribe((data)=>{
  console.log(data);
  console.log(data.get('tabLink');
  console.log(data.get('name');
});
```



### 多种参数同时存在

**URL结果：/home/mobile;name=zhangshan?productId=2&productImg=aaa.jpg**

```typescript
{ path: ':tabLink', component: HomeDetailComponent }
```

```html
<a [routerLink]="['/home', tab.link, {name: 'zhangshan'}]" [queryParams]="{productId: '2', productImg: 'aaa.jpg'}">...</a>
```

```typescript
this.router.navigate(['home', tab.link, {name: 'zhangshan'}], { queryParams: {productId:'2', productImg: 'aaa.jpg'} });
```

```typescript
// 路径参数和查询参数读取可以同时存在
this.route.paramsMap.subscribe(params => {
    console.log('路径参数', params);
});
this.route.queryParamMap.subscribe(params => {
    console.log('查询参数', params);
});
```



## 路由插座

主要插座只能有一个，辅助插座可以有多个

```html
<!-- 没有名字的只能有一个，带名字的可以有多个 -->
<!-- 没有名字的默认名是primary -->
<route-outlet></route-outlet>
<route-outlet name="second"></route-outlet>
```

主要插座

```html
<a [routerLink]="['grand']" routerLinkActive="active">Link to grand</a>
```

```typescript
{ path: 'grand', component: HomeGrandComponent }
```

辅助插座

```html
<!-- URL：/home/hot/(grand//second:aux) -->
<a [routerLink]="[{ outlets: { second: ['aux'] } }]">link to second</a>
```

```typescript
// outlet 定义匹配的路由插座名
{ path: 'aux', component: HomeAuxComponent, outlet: 'second' }
```

同时将两个链接结果都显示出来

```typescript
{
    path: 'news', component: NewsComponent, children:[
        { path: 'company', component: CompanyComponent, outlet: 'left' }
		{ path: 'industry', component: IndustryComponent, outlet: 'right' }
    ]
}
```

```html
<a [routerLink]="['news', { outlets: { left: ['company'], right: ['industry'] } }]"></a>
```

```html
<route-outlet name="left"></route-outlet>
<route-outlet name="right"></route-outlet>
```



## 获取路由参数

- ##### 配置

  ```typescript
  // 引入
  import { ActivatedRoute } from '@angular/router';
  // 依赖注入
  constructor(public route:ActivatedRoute) { }
  ```

- ##### 路由快照

  快照方式获取的参数不是Observable

  使用快照方式时要**保证url只会用一次**，即不会发生从当前url导航到当前url的情况。因为这种方式获取的参数是不会变动的。

  因为组件的ngOnInit方法只会调用一次，而如果检测到路由相同而参数不同时，是不会重新初始化组件的

  <img src="Angular.assets/SouthEast.jpeg" alt="这里写图片描述" style="zoom: 50%;" /> <img src="Angular.assets/SouthEast-16411806115693.jpeg" alt="这里写图片描述" style="zoom:50%;" />

  ```typescript
  // 普通对象
  this.route.snapshot.params['title'];
  this.route.snapshot.queryParams['name'];
  
  // 有get,has,getAll 等方法
  this.route.snapshot.paramMap.get('title');
  this.route.snapshot.queryParamMap.get('name');
  ```

- ##### 参数订阅

  ```typescript
  this.route.params.subscribe((params)=>{
  	console.log(params.title)
  });
  this.route.queryParams.subscribe((params)=>{
  	console.log(params.name)
  });
  
  // 有get,has,getAll 等方法
  this.route.paramMap.subscribe((params)=>{
  	console.log(params.get('id')
  });
  this.route.queryParamMap((params)=>{
  	console.log(params.get('name'))
  });
  ```



## 父子路由

<img src="Angular.assets/image-20220106230142760.png" alt="image-20220106230142760" style="zoom: 67%;" /> 

- ##### 配置路由

  ```typescript
  {
     path:'home',component:HomeComponent,
     children:[
       {path:'welcome',component:WelcomeComponent},
       {path:'setting',component:SettingComponent},
       /* 动态路由
       {path:':id',component:ChildComponent},
       */
       {path:'**',redirectTo:'welcome'}
     ]
  }
  ```
  
  父组件中定义router-outlet
  
  ```html
  <div class="content">
    <div class="left">
        <a [routerLink]="[ '/home/welcome']" routerLinkActive="active">欢迎首页</a>
        <a [routerLink]="[ '/home/setting']" routerLinkActive="active">系统设置</a>
    </div>
    <div class="right">
      <!-- 显示对应子组件的内容 -->
      <router-outlet></router-outlet>
    </div>
  </div>
  ```
  
  ```typescript
  this.route.firstChild.paramsMap.subscribe(params => {
      console.log('路径参数', params);
  });
  ```




## 路由守卫

Angular 提供了一些钩子帮助控制进入或离开路由。这些钩子就是路由守卫

可以通过这些钩子，控制进入离开路由时的操作

路由守卫方法可以返回boolean 或 Observable<boolean> 或 Promise<boolean>，它们在将来的某个时间点解析为布尔值

### 创建守卫

```bash
ng g guard service/login
```



### CanActivate

导航到某路由时触发

可以用来判断用户只有在满足一定的条件的情况下才能打开这个路径对应的页面

```typescript
import { CanActivate } from "@angular/router";
// 实现CanActivate 接口
export class LoginGuard implements CanActivate{
    constructor(private router: Router) {}
    // 返回true 或false
	canActivate(){
		var userinfo=this.storage.get('userinfo');
		if(!userinfo || !userinfo.username){
            // 跳转
			return this.router.createUrlTree(['/login']);
		} else{
			return true;
		} 
	}
}
```

canActivate 可以指定多个守卫路由，所有守卫方法都允许，路由才被允许访问

```typescript
{ path:"", component:DefaultComponent, canActivate:[LoginGuard], 
    children:[
        { path:"home", component:HomeComponent },
        { path:"report", component:ReportComponent, canActivate:[LoginGuard] }
    ]
}

@NgModule({
  providers: [LoginGuard]  
})
```



### CanActivateChild

是否可访问子路由

```typescript
export class AdminGuard implements CanActivateChild{
    canActivateChild(): boolean | UrlTree {
        return true;
    }
}
```

```typescript
{ path:"about", component:AboutComponent, canActivateChild:[AdminGuard], 
    children:[
        { path:"introduce", component:IntroduceComponent },
    ]
}
```



### CanDeactivate

从当前路由离开时触发

决定用户是否能够离开。比如用户输入的表单没有保存，用户离开路由时提示

```typescript
import { CanDeactivate } from "@angular/router";
import { ProductComponent } from "../product/product.component";
// CanDeactivate接口有一个范型，指定当前组件的类型
export class UnsaveGuard implements CanDeactivate<ProductComponent>{
    //第一个参数 范型类型的组件
    //根据当前要保护组件 的状态 判断当前用户是否能够离开
    canDeactivate(component: ProductComponent){
        if (component.canLeave()){
           return true; 
        } else {
            if(window.confirm('你还没有保存，确定要离开吗？')){
                return true;
            }else {
                return false;
            }
        }
        
    }
}
```

```typescript
 { path: 'product/:id', component: ProductComponent, children:[
    { path: '', component : ProductDescComponent },
    { path: 'seller/:id', component : SellerInfoComponent }
  ] ,canActivate: [LoginGuard], canDeactivate: [UnsaveGuard] }

@NgModule({
  providers: [LoginGuard,UnsaveGuard]
})
```



### Resolve

路由激活之前触发

http请求数据返回有延迟，导致模版无法立刻显示。

允许在进入路由之前去服务器读数据，把需要的数据都读好以后，带着这些数据进到路由里，立刻就把数据显示出来

```typescript
@Injectable()
// 实现Resolve接口
// Resolve要声明一个范型，范型就是resolve要解析出来的数据的类型
export class ProductResolve implements Resolve<Product>{
    resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<any> | Promise<any> | any {
        // 进入商品信息路由之前，准备好商品信息再进入路由
        // 拿不到信息，或者拿信息出问题了，直接跳到错误信息页面，不再进入目标路由
        let productId: number = route.params["id"];
        if (productId == 2) { 
            //正确id
            return new Product(1, "iPhone7");
        } else { 
            //id不是1导航回首页
            this.router.navigate(["/home"]);
            return undefined;
        }
    }
}
```

路由配置

```typescript
{ path: 'product/:id', component: ProductComponent, children:[
    { path: '', component : ProductDescComponent },
    { path: 'seller/:id', component : SellerInfoComponent }
  ],
    // canActivate: [LoginGuard],
    // canDeactivate: [UnsaveGuard],
    // resolve是一个对象
    resolve:{
      // 传入product
      // product由ProductResolve生成
      product : ProductResolve
    }
}
```

组件配置

```typescript
export class ProductComponent implements OnInit {
	private productId: number;
	private productName: string;
    constructor(private routeInfo: ActivatedRoute) { }
	ngOnInit() {
	  // this.routeInfo.params.subscribe((params: Params)=> this.productId=params["id"]);
	  this.routeInfo.data.subscribe((data:{product:Product}) => {
	      this.productId=data.product.id;
	      this.productName=data.product.name;
      });
	}
}
```















## 懒加载

通过路由来动态挂载模块，来实现模块的懒加载

子模块路由配置

```typescript
{
  path:'', component: ProductComponent,
  children: [
    // 子路由的组件 根模块访问不到，需要在父组件中设置router-outlet以显示子组件内容
    { path: 'cart', component: CartComponent },
    { path: 'pcontent', component: PcontentComponent }
  ]
},
// 根组件可以直接访问到plist的组件内容
{ path: 'plist', component: PlistComponent }
```

子模块挂载路由

```typescript
@NgModule({
  declarations: [ProductComponent, PlistComponent, CartComponent, PcontentComponent],
  // 引入子模块路由
  imports: [ CommonModule, ProductRoutingModule]
})
export class ProductModule { }
```

根模块路由配置

```typescript
{
  path:'user',
  loadChildren: () => import('./module/user/user.module').then(m => m.UserModule)
},
{
  path:'article',
  loadChildren: () => import('./module/article/article.module').then(m => m.ArticleModule)
},
{
  path:'product',
  loadChildren: () => import('./module/product/product.module').then(m => m.ProductModule)
},
{
  path:'**',redirectTo:'user'
}

/* Angular10.x之前配置写法
{
  path:'user',
  loadChildren:'./module/user/user.module#UserModule'   
}
*/ 
```

根模块不需要再引入子模块和组件

```typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```



# 动画

## 基础概念

- ##### 状态

  状态表示的是要进行运动的元素在运动的不同时期所呈现的样式

  <img src="Angular.assets/image-20220112213408626.png" alt="image-20220112213408626" style="zoom:50%;" /> 

- ##### 状态种类

  **void**：当元素在内存中创建好但尚未被添加到DOM中或将元素从DOM中删除时会发生此状态

  *****：元素被插入到DOM树之后的状态，或者已经是在DOM树中的元素的状态，也叫默认状态

  **custom**：自定义状态，元素默认就在页面之中，从一个状态运动到另一个状态，比如面板的折叠和展开

  <img src="Angular.assets/image-20220112213753631.png" alt="image-20220112213753631" style="zoom:50%;" /> 

- ##### 进出场动画

  进场动画是指元素被创建后以动画的形式出现在用户面前，进场动画的状态用 **`void => * `** 表示，别名为`:enter`

  <img src="Angular.assets/image-20220112214301662.png" alt="image-20220112214301662" style="zoom:50%;" /> 

  出场动画是指元素在被删除前执行的一段告别动画，出场动画的状态用 **`* => void `** 表示，别名为`:leave`

  <img src="Angular.assets/image-20220112214437416.png" alt="image-20220112214437416" style="zoom:50%;" /> 



## 创建动画

- ##### 动画模块 BrowserAnimationsModule

  ```typescript
  import { BrowserAnimationsModule } from '@angular/platform-browser/animations'
  
  @NgModule({
      imports: { BrowserAnimationsModule }
  })
  export class AppModule {}
  ```

- ##### 创建动画

  ```typescript
  @Component({
      animations: [
          // 创建动画，参数一：动画名称 参数二：数组 定义动画的运动状态、状态下的运动样式、动画运动的形式
          trigger("slider", [
              // 指定入场动画，注意字符串两边不能有空格，箭头两边可以有也可以没有空格
              // void => * 可以替换为 :enter
              transition("void => *", [
                  // 指定元素未入场前的样式 void状态的样式
                  style({ opacity: 0, transform: "translateY(40px)" }),
                  // 指定元素入场后的样式及运动参数
                  animate(250, style({ opacity: 1, transform: "translateY(0)" }))
              ]),
              // 指定出场动画
              // * => void 可以替换为 :leave
              transition("* => void", {
                  // *的样式就是页面中定义的样式，所以不用定义
                  // 指定元素出场后的样式和运动参数
                  animate(600, style({ opacity: 0, transform: "translateX(100%)" }))
              })
          ])
      ]
  })
  ```

  - **trigger** 用于创建动画，指定动画名称，用名称去调用运行

    参数1：动画名称，参数2：数组，定义动画的运动状态

  - **transition** 用于指定动画的运动状态，出场动画或者入场动画，或者自定义状态动画

    参数1：状态，参数2：数组，定义入场前样式、出入场动画

  - **style** 用于设置元素在不同的状态下所对应的样式

  - **animate** 用于设置运动参数，比如动画运动时间，延迟事件，运动形式

    参数1：是数字，就是动画时间；是字符串，就是运动参数，参数2：出入场后样式

    ```typescript
    // 字符串：动画运动参数 -> 动画执行总时间 延迟时间(可选) 运动形式(可选)
    animate("600ms 1s ease-out", style({ opacity: 0, transform: "translateX(100%)" }))
    // 数字：动画运行时间
    animate(600, style({ opacity: 0, transform: "translateX(100%)" }))
    ```

    入场动画中可以不指定元素的默认状态，Angular会将void状态清空作为默认状态

    ```typescript
    trigger("slider", [
        translation(":enter", [
            style({ opacity: 0, transform: "translateY(40px)" }),
            // 可以不指定animate的第二参数
            animate(250)
        ]),
        translation(":leave", [
            animate(600, style({ opacity: 0, transform: "translateX(100%)" }))
        ])
    ])
    ```

- ##### 创建可重用动画

  动画的定义放置在单独的文件中，方便多组件调用

  抽取具体的动画定义，方便多动画调用。调用动画时传递运动总时间、延迟时间、运动形式参数

  ```typescript
  // 创建 animations.ts 文件
  import { animate, keyframes, style, transition, trigger, animation, useAnimation } from '@angular/animations'
  
  // 使用animation方法，定义动画形式
  let sliderAnimationEnter = animation([
      style({ opacity: 0, transform: "translateY(40px)" }),
      animate(250)
  ])
  
  // animation的参数2是一个固定对象，对象中有属性params，用来设置默认参数
  let sliderAnimationLeave = animation([
      animate(
          "{{ duration }} {{ delay }} {{ easing }}",
          keyframes([
              style({ offset: 0.3, transform: "translateX(-80px)" }),
          	style({ offset: 1, transform: "translateX(100%)" }),
      	]) 
      )
  ], {
      // 设置默认参数
      params: {
          duration: "1s",
          delay: "0s",
          easing: "ease-out"
      }
  })
  
  export const slideAnimation = trigger("slide", [
      // 入场动画
      transition(":enter", useAnimation(sliderAnimationEnter)),		    // useAnimation调用动画
      // 出场动画
      // useAnimation方法也可以设置参数 会覆盖默认参数
      translation(":leave", useAnimation(sliderAnimationLeave), { params: { delay: "1s" } })
  ])
  ```

  组件调用

  ```typescript
  import { slideAnimation } from './animations'
  
  @Component({
      animations: [ slideAnimation ]
  })
  ```

- ##### 调用动画

  ```html
  <div class="form-group">
      <input (keyup.enter)="addItem(input)" #input type="text" class="form-control" placeholder="add todos" />
  </div>
  <ul class="list-group">
      <!-- 指定动画 @动画名 -->
      <li @slide
          (click)="removeItem(i)" *ngFor="let item of todos; let i = index" class="list-group-item">
      	{{ item }}
      </li>
  </ul>
  ```

  入场动画<img src="Angular.assets/7acos-letyb.gif" alt="7acos-letyb" style="zoom:50%;" /> 离场动画<img src="Angular.assets/tg0ye-tdrr9.gif" alt="tg0ye-tdrr9" style="zoom: 33%;" /> 




## 关键帧动画

使用keyframes方法，传入style方法的数组，去定义帧位置以及样式

```typescript
transition(":leave", {
    animate(
    	600,
		// animate方法的第二个参数
        keyframes([
         	// style方法中，指定帧的位置，以及在这个帧的位置上元素的样式
         	// 先向左移动
			style({ offset: 0.3, transform: "translateX(-80px)" }),
    		// 再向右移动
    		style({ offset: 1, transform: "translateX(100%)" }),
        ])
    )
})
```

<img src="Angular.assets/bbtbw-q23kl.gif" alt="bbtbw-q23kl" style="zoom: 67%;" /> 



## 回调动画

Angular 提供了和动画相关的两个回调函数，分别为动画开始执行时和动画执行完成后

```html
<!-- 动画开始时执行的回调函数 (@slide.start)  -->
<!-- 动画完成后执行的回调函数 (@slide.done)  -->
<li @slide (@slide.start)="start($event)" (@slide.done)="done($event)"></li>
```

```typescript
import { AnimationEvent } from '@angular/animations'

start(event: AnimationEvent) {
    console.log(event)
}
done(event: AnimationEvent) {
    console.log(event)
}
```

<img src="Angular.assets/image-20220113103037251.png" alt="image-20220113103037251" style="zoom:67%;" /> 



## 查询元素执行动画

- ##### query 方法查找元素并为元素创建动画

  query 方法在查找元素获取元素时，查找的是子元素，所以要放到父级进行动画调用

  ```html
  <div class="container" @todoAnimations>
      <h2>Todos</h2>
      <ul class="list-group">
      	<li @slide
      	    (click)="removeItem(i)" *ngFor="let item of todos; let i = index" class="list-group-item">
      		{{ item }}
      	</li>
  	</ul>
  </div>
  ```

  ```typescript
  export let todoAnimations = trigger("todoAnimations", [
      transition(":enter", [
          // 查阅元素，然后执行动画
          query("h2", [
              // 运动之前的样式
              style({ transform: "translateY(-30)", opacity: 0 }),
              animate(300)
          ]),
          // 查询子级动画，使其执行
          query("@slide", animateChild())
      ])
  ])
  
  @Component({
      animations: [ slideAnimation, todoAnimations ]
  })
  ```

  <img src="Angular.assets/hkf5z-jcj1i.gif" alt="hkf5z-jcj1i" style="zoom: 67%;" /> 

- ##### group 对多个动画进行编组

  默认情况下，父级动画和子级动画按照顺序执行，先执行父级动画，再执行子级动画，可以使用 group 方法让其并行

  ```typescript
  transition(":enter", [
      group([
          query("h2", [
          	style({ transform: "translateY(-30)", opacity: 0 }),
          	animate(300)
      	]),
      	query("@slide", animateChild())  
      ])
  ])
  ```

- ##### stagger 交错动画

  在多个元素同时执行同一个动画时，让每个元素动画的执行依次延迟。

  stagger 方法只能在 query 方法内部使用。在query的第二个参数上，可以调用stagger方法

  ```typescript
  transition(":enter", [
      group([
          query("h2", [
              style({ transform: "translateY(-30)", opacity: 0 }),
              animate(300)
          ]),
          // stagger 参数1 延迟时间，参数2 动画的执行
          query("@slide", stagger(200, animateChild()))
      ])
  ])
  ```

  <img src="Angular.assets/pngz7-mvjv9.gif" alt="pngz7-mvjv9" style="zoom:67%;" /> 



## 自定义状态动画

state 方法定义元素的自定义状态，以及元素在这个状态下的样式

两个参数。参数1：状态名；参数2：在这个状态下元素的样式

```typescript
trigger("expandCollapse", [
    // 使用 state 方法定义折叠状态元素对应的样式
    state("collapsed", style({ height: 0, overflow: "hidden", paddingTop: 0, PaddingBottom: 0 })),
    // 使用 state 方法定义展开状态元素对应的样式
    state("expanded", style({ height: "*", overflow: "auto" })),
    // 定义运动轨迹
    // 定义展开动画
    transition("collapsed => expanded", animate("400ms ease-out")),
    // 定义折叠动画
    transition("expanded => collapsed", animate("400ms ease-in"))
])
```

```html
<div class="container">
    <div class="panel panel-defalut">
        <!-- 点击事件 改变状态 -->
        <div class="panel-heading" (click)="toggle()">
            标题
        </div>
        <!-- 调用动画 可以指定动画状态 -->
        <div class="panel-body" [@expandCollapse]="isExpanded ? 'expanded' : 'collapsed'">
            <p>段落1</p>
            <p>段落2</p>
            <p>段落3</p>
            <p>段落4</p>
        </div>
    </div>
</div>
```

```typescript
isExpanded = false;
toggle() {
   this.isExpanded = !this.isExpanded 
}
```

<img src="Angular.assets/8jrki-0t4bu.gif" alt="8jrki-0t4bu" style="zoom:67%;" /> 



## 路由动画

为路由添加自定义数据，用来表示该路由的动画状态

```typescript
const routes: Routes = [
    { path: "", component: HomeComponent, pathMatch: "full", data: { animation: "one" } },
    { path: "about", component: AboutComponent, data: { animation: "two" } },
    { path: "news", component: NewsComponent, data: { animation: "three" } }
]
```

根组件添加动画

```typescript
@Component({
    selector: "app-root",
    templateUrl: "./app.component.html",
    styles: [],
    animations: [trigger("routerAnimation", [
        // 从右向左
        transition("one => two, one => three, two => three", [
            // 查找进场的路由 设置进场前的样式
            query(":enter", style({ transform: "translateX(100%)", opacity: 0 })),
            // 进场和离场
            group([
                query(
                    ":enter", 
                    animate("0.4s ease-in", style({ transform: "translateX(0)", opacity: 1 }))
                ),
                query(
                    ":leave",
                    animate("0.4s ease-out", style: ({ transform: "translateX(-100%)", opacity: 0 }))
                )
            ])
        ]),
        // 从左往右
        transition("three => two, three => one, two => one", [
            query(":enter", style({ transform: "translateX(-100%)", opacity: 0 })),
            group([
                query(
                    ":enter", 
                    animate("0.4s ease-in", style({ transform: "translateX(0)", opacity: 1 }))
                ),
                query(
                    ":leave",
                    animate("0.4s ease-out", style: ({ transform: "translateX(-100%)", opacity: 0 }))
                )
            ])
        ])
    ])]
})
export class AppComponent {
    
    prepare (outlet: RouterOutlet) {
        if (outlet && outlet.activatedRouteData && outlet.activatedRouteData.animation) {
            return outlet.activatedRouteData.animation
        }
    }
}
```

在模板调用

```html
<!-- 添加到父级上 -->
<!-- 路由动画不能是文档流，要改成绝对定位 -->
<div [@routerAnimation]="prepare(outlet)" class="routerContainer">
    <!-- 通过路由的实例对象取得 -->
    <router-outlet #outlet></router-outlet>
</div>
```

```css
.routerContainer {
    position: relative;
}
/* 要找第一层子级 */
.routerContainer > * {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
}
```



# TypeScript相关

## interface

- ##### 类型命名

  ```typescript
  interface Topinterface TopMenu {
    title: string;
    link: string;
  }
  ```

- ##### 可选属性，只读属性

  ```typescript
  interface Topinterface TopMenu {
    title: string;
    link?: string;
    readonly name: string;
  }
  ```

- ##### 函数类型，索引类型

  ```typescript
  interface AddFunc{
      (x: number, y: number): number
  }
  ```

  ```typescript
  interface Dict {
      [key: string]: string
  }
  ```



## 装饰品(注解)

装饰品/注解就是一个函数，但它是一个返回函数的函数

是TypeScript的一个特性，不是Angular的特性   

- ##### 应用于属性的注解

  ```typescript
  export function Emoji () {
      // target就是使用的类，key是类中的属性
      return ( target: object, key: string ) => {
          let val = target[ key ];
  
          // 新定义读写方法
          const getter = () => {
              return val;
          }
  
          const setter = ( value: string ) => {
              val = `@@ ${ value } @@`;
          }
  
          // 重新设置 替换读写
          Object.defineProperty( target, key, {
              get: getter,
              set: setter,
              enumerable: true,
              configurable: true
          } );
      }
  }
  
  export class HorizonGridComponent{
      @Emoji() result = 'Hello';
  }
  ```

  ```html
  {{ result }}
  <!-- 结果为 @@ Hello @@ -->
  ```


- ##### 应用于函数的注解

  ```typescript
  // 有参数的注解
  export function Confirmable ( message: string ) {
      return ( traget: object, key: string, descriptor: PropertyDescriptor ) => {
          const orginal = descriptor.value;
          descriptor.value = function ( ...args: any ) {
              const allow = window.confirm( message );
              if ( allow ) {
                  const result = orginal.apply( this, args );
                  return result;
              }
              return null;
          }
          return descriptor;
      }
  }
  export class HorizonGridComponent{
    @Confirmable( '您确认要执行吗?' )
    handleClick () {
      console.log( '点击已执行' );
    }
  }
  ```

  ```html
  <span (click)="handleClick()">点击</span>
  ```




# Rxjs响应式编程

rxjs要把事件或数据看成一个流

响应式编程就是随着事件流中的元素的变化随之做出相应的动作

流的种类：无限，有限，单个，空

所有的操作都是异步的

```typescript
import {Observable} from 'rxjs';
import {map,filter} from 'rxjs/operators';

let stream = new Observable<any>(observer => {
    let count = 0;
	setInterval(() => {
		observer.next(count++);
	}, 1000);
});

stream.pipe(
	filter(val => val%2==0)
).subscribe(value => console.log("filter>"+value));

stream.pipe(
	filter(val => val%2==0),
	map(value => {
		return value * value
	})
).subscribe(value => console.log("map>"+value));
```

- Angular6 以后使用以前的rxjs 方法，必须安装rxjs-compat 模块才可以使用map、filter方法。



## 基本概念

可观察对象 Observable ：类比 Promise 对象，内部可以用于执行异步代码，通过调用内部提供的方法将异步代码执行的结果传递到可观察对象外部

观察者 Observer：类比 then 方法中的回调函数，用于接收可观察对象中传递出来数据

订阅 Subscribe：类比 then 方法，通过订阅将可观察对象和观察者连接起来，当可观察对象发出数据时，订阅者可以接收到数据

<img src="Angular.assets/image-20220110230109796.png" alt="image-20220110230109796" style="zoom: 80%;" /> <img src="Angular.assets/image-20220110230123802.png" alt="image-20220110230123802" style="zoom:50%;" />



## 可观察对象

### Observable

可观察对象是惰性的，只有被订阅后才会执行

可观察对象可以有n多订阅者，每次被订阅时都会得到执行

```typescript
const observable = new Observable(() => {
    console.log("执行");
})
observable.subscribe();  // 执行
observable.subscribe();  // 执行
observable.subscribe();  // 执行
```

三种状态状态：next，error，complete

<img src="Angular.assets/image-20220108142409251.png" alt="image-20220108142409251" style="zoom:50%;" /> 

当调用了complete 或 error 方法以后，就不能再次调用 next方法

```typescript
const observable = new Observable((observer) => {
    let index = 0;
    let timer = setInterval(() => {
        //  在Observable对象内部可以多次调用 next方法向外发送数据
        observer.next(index++);
        if(index === 3) {
            // 当所有数据发送完成以后，可以调用complete方法终止数据发送
            // observer.complete();
            // 当内部逻辑发送错误时，可以调用 error 方法将失败信息发送给订阅者，Observable 终止
            observer.error("error");
            clearInterval(timer);
        }
    }, 1000);
});

const observer = {
    next: (value) => {
        console.log(value)
    },
    complete: () => {
        console.log("complete")
    },
    error: (error) => {
        console.log(error)
    }
}

observable.subscribe(observer);
```



### Subject

用于创建空的可观察对象。在订阅后不会立即执行，next方法可以在可观察对象外部调用

可当做广播去使用。创建一个空的可观察对象，让很多的订阅者去订阅，当订阅的时候并不会立刻执行。当有数据要发出的时候，统一调用一次next方法，所有的订阅者可以同时接受到数据

```typescript
import { Subject } from 'rxjs';

const demoSubject = new Subject();

demoSubject.subscribe(next: (value) => console.log(value));
demoSubject.subscribe(next: (value) => console.log(value));

setTimeout(() => {
    demoSubject.next("hahaha");
}, 3000)
```



### BehaviorSubject

拥有 Subject 全部功能，但是在创建 Observable 对象时可以传入默认值，观察者订阅后可以直接拿到默认值

```typescript
import { BehaviorSubject } from 'rxjs';

const demoBehavior = new BehaviorSubject("默认值");
demoBehavior.subscribe(next: (value) => console.log(value))
demoBehavior.next("Hello");
```



### ReplaySubject

功能类似Subject，但有新订阅者时两者处理方式不同，Subject 不会广播历史结果，而 ReplaySubject 会广播所有历史结果

```typescript
import { ReplaySubject } from 'rxjs';

const rSubject = new ReplaySubject();

rSubject.subscribe(value => {
    console.log(value);
})

rSubject.next("Hello 1");	// Hello 1
rSubject.next("Hello 2");	// Hello 2

setTimeout(() => {
    rSubject.subscribe(value => {
    	console.log(value);
        // 输入
        // Hello 1
        // Hello 2
	})
}, 3000)
```



## 辅助方法

### range

range(start, length) , 调用方法后返回 observable 对象，被订阅后会发出指定范围的数据

方法内部并不是一次发出length个数值，而是发送了length次，每次发送一个数值。也就是内部调用了length次next方法

<img src="Angular.assets/image-20220111085845329.png" alt="image-20220111085845329" style="zoom:67%;" /> 

```typescript
import { range } from 'rxjs'
range(0, 5).subscribe(n => console.log(n))

// 1
// 2
// 3
// 4
```



### from

将 Array，Promise，lterator 转换为observable 对象
<img src="Angular.assets/image-20220111103250677.png" alt="image-20220111103250677" style="zoom:67%;" /> 

```typescript
from(["a", "b", "c"]).subscribe(v => console.log(v))
// a
// b
// c
```

```typescript
import { from } from 'rxjs';

p(){
    return new Promise(resolve => {
        setTimeout(() => {
			resolve([100, 200])
        }, 2000)
    })
}

from(p()).subscribe(v => console.log(v));
// [100, 200]
```



### forkjoin

是Rx版本的 Promise.all() ，即表示等到所有的Observable 都完成后，才一次性返回值

<img src="Angular.assets/image-20220111121037815.png" alt="image-20220111121037815" style="zoom:67%;" /> 

```typescript
import axios from 'axios';
import { from, forkJoin } from 'rxjs';

axios.interceotor.response.use(response => response.data)

forkJoin({
    goods: from(axios.get("http://localhost:3005/goods")),
    category: from(axios.get("http://localhost:3005/category"))
}).subscribe(console.log)


```



### fromEvent

将事件转换为 Observable

```typescript
import { fromEvent } from 'rxjs'

const btn = document.getElementById("btn")
fromEvent(btn, 'clilck').subscribe(e => e.console.log(e))
// 获取事件元
fromEvent(btn, 'clilck')
    .pipe(map(event => event.traget))
    .subscribe(e => e.console.log(e))
```



### interval、timer

interval：每隔一段事件发出一个数值，数值递增

<img src="Angular.assets/image-20220111133745862.png" alt="image-20220111133745862" style="zoom:67%;" /> 

```typescript
import { interval } from 'rxjs'

interval(1000).subscribe(n => console.log(n))
```

timer：间隔时间过去以后发出数值，行为终止，或间隔时间发出数值后，继续按第二个参数的时间间隔继续发出

<img src="Angular.assets/image-20220111134041713.png" alt="image-20220111134041713" style="zoom:67%;" /> 

```typescript
import { timer } from 'rxjs'

timer(2000).subscribe(n => console.log(n))
timer(0, 1000).subscribe(n => console.log(n))
```



### of

将参数列表作为数据流返回

<img src="Angular.assets/image-20220111150119202.png" alt="image-20220111150119202" style="zoom:67%;" /> 

```typescript
of("a", "b", [], {}, true, 20).subscribe(v => console.log(v))
```

 

### distinctUntilChanged

检测数据源当前发出的数据流是否和上次发出的相同，如相同，跳过，不相同，发出

<img src="Angular.assets/image-20220111151038124.png" alt="image-20220111151038124" style="zoom:67%;" /> 



```typescript
import { of } from 'rxjs'
import { distinctUntilChanged } from 'rxjs/operators'

of(1, 1, 2, 2, 1, 1, 2, 3, 3, 4)
.pipe(distinctUntilChanged())
.subscribe(x => console.log(x)) // 1, 2, 1, 2, 3, 4
```





## 操作符

数据流：从可观察对象内部输出的数据就是数据流，可观察对象内部可以向外部源源不断的输出数据

操作符：用于操作数据流，可以将对象数据流进行转换，过滤等操作

### map、mapTo

map： 对数据流进行转换，基于原有值进行转换

<img src="Angular.assets/image-20220111085615028.png" alt="image-20220111085615028" style="zoom:67%;" /> 

```typescript
import { interval } from 'rxjs';
import { map } from 'rxjs/operators';

interval(1000)
.pipe(map(n => n * 2))
.subscribe(n => console.log(n));
```

mapTo：对数据流进行转换，不关心原有值，可以直接传入要转换后的值



### pluck

获取数据流对象中的属性值

<img src="Angular.assets/image-20220111132922588.png" alt="image-20220111132922588" style="zoom:67%;" /> 

```typescript
import { interval } from 'rxjs'
import { pluck, mapTo } from 'rxjs/operators'

interval(1000)
    .pipe(
    	mapTo({ name: '张三', a: { b: 'c' } }),
    	pluck("a", "b")
).subscribe(n => n.console.log(n))
```



### switchMap

切换可观察对象。

当外层的可观察对象重新发出数据流时，switchMap会抛弃上一次发出的数据流，而重新进行数据流的发送

点击一个按钮发送一个请求，点击事件是一个observable, 发送请求也是一个observable，默认情况下用户订阅的是点击事件，当点击事件被触发时，需要把observable进行切换到发送请求的observable。订阅者就可以拿到发送请求的结果

<img src="Angular.assets/image-20220111134845769.png" alt="image-20220111134845769" style="zoom:67%;" /> 

```typescript
import { fromEvent } from 'rxjs'
import {}

const button = document.getElementById('btn')

fromEvent(button, 'click')
.pipe(switchMap(event => interval(1000)))
.subscribe(console.log)
```



### take、takeWhile、takeUtil

take：获取数据流中前几个

<img src="Angular.assets/image-20220111142006545.png" alt="image-20220111142006545" style="zoom:67%;" /> 

```typescript
import { range } from 'rxjs'
import { take } from 'rxjs/operators'

range(1, 10).pipe(take(5)).subscribe(console.log)
```

takeWhile：根据条件从数据源前面开始获取

<img src="Angular.assets/image-20220111143854169.png" alt="image-20220111143854169" style="zoom:67%;" /> 

```typescript
import { range } from 'rxjs'
import { takeWhile } from 'rxjs/operators'

range(1, 10)
.pipe(takeWhile(n => n < 8))
.subscribe(console.log)
```

takeUntil：接受可观察对象，当可观察对象发出值时，终止主数据源

<img src="Angular.assets/image-20220111144614754.png" alt="image-20220111144614754" style="zoom:67%;" /> 

```typescript
import { fromEvent } from 'rxjs'
import { takeUntil } from 'rxjs/operators'

const button = document.getElementById("btn")

fromEvent(document, "mousemove").pipe(
	takeUntil(fromEvent(button, "click"))
).subscribe(console.log)
```



### throttleTime

节流，可观察对象高频次向外部发出数据流，通过throttleTime 限制在规定时间内每次只向订阅者传递一次数据流

<img src="Angular.assets/image-20220111144832420.png" alt="image-20220111144832420" style="zoom:67%;" /> 

```typescript
import { fromEvent } from 'rxjs'
import { throttleTime } from 'rxjs/operators'

fromEvent(document, 'click')
.pipe(throttleTime(2000))
.subscribe(x => console.log(x))
```



### debounceTime

防抖，触发高频事件，只响应最后一次

<img src="Angular.assets/image-20220111145744038.png" alt="image-20220111145744038" style="zoom:67%;" /> 

```typescript
import { fromEvent } from 'rxjs'
import { debounceTime } from 'rxjs/operators'

fromEvent(document, 'click')
.pipe(debounceTime(1000))
.subscribe(x => console.log(x))
```



## 应用

- ##### 元素拖拽

  ```html
  <style>
      #box {
          width: 200px;
          height: 200px;
          background: skyblue;
          position: absolute;
          left: 0;
          top: 0;
      }
  </style>
  <div id="box"></div>
  ```

  ```typescript
  // 原生JavaScript
  box.onmousedown = function(evnet) {
      let distinctX = event.clientX - event.target.offsetLeft
      let distinctY = event.clientY - event.traget.offsetTop
      document.onmousemove = function(event) {
          let positionX = event.clientX - distinctX
          let positionY = event.clientY - distinctY
          box.style.left = positionX + 'px'
          box.style.top = positionY + 'px'
      }
      box.onmouseup = function(){
          document.onmousemove = null;
      }
  }
  
  // Rxjs
  import { fromEvent } from 'rxjs'
  import { map, switchMap, takeUntil } from 'rxjs/operators'
  
  const box = document.getElementById("box")
  fromEvent(box, 'onmousedown').pipe(
      // map 转化数据流为距离对象形式
  	map(event => ({
          distinctX: event.clientX - event.target.offsetLeft,
          distinctY: event.clientY - event.target.offsetTop
      })),
      // 切换数据流 切换到mousemove事件 参数为上一步处理出来的距离对象数据流
      switchMap(({ distinctX, distinctY }) => {
          fromEvent(document, "mousemove").pipe(
              // 转换为坐标对象数据流
          	map(event => ({
                  positionX: event.clientX - distinceX,
                  positionY: event.clientY - distinceY
              })),
              // 停止移动 停止数据流
              takeUntil(fromEvent(box, "mouseup"))
          )
      })
  ).subscribe(({ positionX, positionY }) => {
      // 接收最后的数据流
      box.style.left = positionX + "px"
      box.style.top = positionY + "px";
  })
  ```

- ##### 搜索

  ```html
  <input id="search" type="text" placeholder="请输入搜索内容..." />
  ```

  ```typescript
  import { fromEvent, from } from 'rxjs'
  import { map, switchMap, debounceTime, distinctUntilChanged, pluck, catchError } from 'rxjs/operators'
  import axios from 'axios'
  
  fromEvent(search, "keyup")
  .pipe(
      // 防抖
  	debounceTime(1000),
      // 用户输入的值
      map(event => event.target.value),
      // 检测当前是否和上一次输入的内容相同 在map之后调用，因为之前得到的是事件对象
      distinctUntilChanged(),
      // 服务器搜索，切换数据流
      switchMap(keyword => 
          from(
          	axios.get(`https://jsonplaceholder.typicode.com/posts?q=${keyword}`)
      	).pipe(
          	// 取过来的数据只要data属性内容
      		pluck("data"),
          	catchError(error => throwError(`发生了错误：${error.message}`))
      	)
      )
  ).subscribe({
  	next: value => {
          console.log(value)
      },
      error: error => {
          console.log(error)
      }
  })
  ```

- ##### 串联请求

  ```html
  <button id="btn">获取用户信息</button>
  ```

  ```typescript
  import axios from 'axios'
  import { from, fromEvent } from 'rxjs'
  import { pluck, contactMap } from 'rxjs/operators'
  
  const button = document.getElementById('btn')
  
  fromEvent(button, 'click')
  .pipe(
      // 先去获取token
      // concatMap 合并可观察对象
  	concatMap(event => 
                from(axios.get("http://localhost:3005/token"))
                .pipe(pluck("data", "token"))),
      // 再根据token去获取用户信息
      concatMap(token => from(axios.get("http://localhost:3005/userInfo"))
               .pipe(pluck("data")))
  )
  .subscribe()
  ```




# 待整理

## 编译

```bash
ng build --prod

## vendor-chunk 依赖的第三方类库单独打包
ng build --prod --aot --build-optimizer --vendor-chunk
```

可以将编译命令写到package.json文件的script中,使用npm run build 脚本调用这个命令





## Ivy 渲染引擎

```bash
ng new xxx -enable-ivy
```

#### 已有项目改造

angular.json

```json
"angularCompilerOptions": {
    "enableIvy": true,
    "allowEmptyCodegenFiles": true
}
```

编译

```bash
npm run ivy
```

