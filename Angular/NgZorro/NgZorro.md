# 安装

## 创建项目安装

```bash
ng new PROJECT_NAME
cd PROJECT_NAME
ng add ng-zorro-antd
```



## 单独安装

- ##### 安装组件

  ```bash
  npm install ng-zorro-antd --save
  ```

- ##### angular.json 中引入样式

  ```json
  {
  	"styles": [
  		"node_modules/ng-zorro-antd/ng-zorro-antd.min.css"
  	]
  }
  ```

- ##### style.css 中引入预构建样式文件

  ```css
  @import "~ng-zorro-antd/ng-zorro-antd.min.css";
  ```



## 模板中使用

- ##### 引入组件模块

  ```typescript
  import { NzButtonModule } from 'ng-zorro-antd/button';
  
  @NgModule({
  	declarations: [ AppComponent ],
  	imports: [ NzButtonModule ]
  })
  ```

  ```html
  <button nz-button nzType="primary">Primary</button>
  ```

  