# 安装

```bash
npm install highcharts-angular --save
npm install highcharts --save
```



# 配置

导入模块

```typescript
import { HighchartsChartModule } from 'highcharts-angular';
@NgModule({
imports: [
	...
	HighchartsChartModule
```

组件引入

```typescript
import * as Highcharts from 'highcharts';
export class AppComponent {
	Highcharts: typeof Highcharts = Highcharts;
	chartOptions: Highcharts.Options = {
		series: [{
		data: [1, 2, 3],
        // 如果从官网复制的代码，要再加上type属性
		type: 'line'
        ......
	}]
};
```

```html
<highcharts-chart
	[Highcharts]="Highcharts"
	[options]="chartOptions1"
	style="width: 100%; height: 400px; display: block;"
	#pieChart>
</highcharts-chart>
```

添加数据

```typescript
// ViewChild 中获取Dom 节点
@ViewChild("pieChart") pieChart: any;
```

```typescript
this.pieChart.chart.addSeries({
	type: "pie",  //注意在angular里面必须配置
	name: '人口数',
	colorByPoint: true,
	data: [
        { name: '深圳', y: 614111, sliced: true, selected: true },
        { name: '北京', y: 213111 },
        { name: '上海', y: 213111 }, 
        { name: '武汉', y: 613111 },
        { name: '广州', y:813111 },
        { name: '西安', y:213111 }
    ]
});
```

更新数据

```typescript
this.pieChart.chart.update({
   series:[{
        name: '人口数',
		colorByPoint: true,
       	data: [
        	{ name: '深圳', y: 614111, sliced: true, selected: true },
        	{ name: '北京', y: 213111 },
        	{ name: '上海', y: 213111 }, 
        	{ name: '武汉', y: 613111 },
        	{ name: '广州', y:813111 },
        	{ name: '西安', y:213111 }
		]
   }]
});
```

更新一列数据

```typescript
this.columnChart.chart.series[0].update({
	data: [1229.9, 71.5, 306.4, 429.2, 144.0, 176.0, 135.6, 248.5, 216.4, 194.1, 95.6, 54.4]
});
```

