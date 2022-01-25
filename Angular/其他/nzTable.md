# NzTable

### 使用

```typescript
import { NzTableModule } from 'ng-zorro-antd/table';
```



### 数据传入

将数据传入`[nzData]`，经过组件处理之后（包括分页、排序、筛选等），通过模板变量 获取当前展示表格部分的数据，使用 `*ngFor` 依据需求将数据渲染。

```html
<nz-table #basicTable [nzData]="dataSet">
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Address</th>
    </tr>
  </thead>
  <tbody>
      <!-- 模板变量 basicTable 获取处理后的数据 -->
      <tr *ngFor="let data of basicTable.data">
      <td>{{data.name}}</td>
      <td>{{data.age}}</td>
      <td>{{data.address}}</td>
    </tr>
  </tbody>
</nz-table>
```



### 选择框



