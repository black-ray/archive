- #### RxJS是一组可以用来处理 非同步 或 事件 的JavaScript函数库



# 核心概念

## 可观察的对象 Observable 

代表一组**未来**即将产生的**事件资料**（被观察的对象）

例如click事件就是一个可以被观察的事件，点击会发出事件资料，称为**Stream**，这些资料是可以被观察的

## 观察者对象 Observer 

代表一个用来接收**观察结果**的对象（收到的就是**事件资料**）

Observer对象包含有三个回调函数属性：**next**，**error**，**complete**

## 订阅对象 Subscription 

代表正在执行Observable/Observer 的执行个体（可以用来**取消订阅**）

通过**Observer观察者对象**去观察**Observable可观察的对象**的资料，这整件事被称为**Subscription订阅** 

没有观察者去订阅，可观察对象Observable就不会触发

## 操作符 Operators 

从**Observable可观察对象**丢出来的资料，到被**订阅**拿到资料的过程，可以想象成一个水管。

**Operators操作符**就是水管中**过滤**这些**事件资料集合**的运算，最后让**Observer观察者**拿到结果

常见操作符：`map` `fliter` `concat` `flatMap` `switchMap`...

## 主体对象 Subject 

如同EventEmitter一样，主要用来**广播**收到的事件资料给多位Observer观察者。**一对多的广播**

**Subject主体对象本身**是**Observable可观察对象**，同时也是**Observer观察者**

当多个地方都需要Observable提供的资料时，如果同时订阅多次，会出现预想外的问题

## 排程控制器 Schedulers 

用来管理和调度多个不同事件之间的资料，以控制事件并发情况



# 运作方法

```javascript
rxjs.interval(500)				// 建立运算子（Creation）
.pipe(rxjs.operators.take(4))	// 过滤运算子（Filtering）
.subscribe(						// 返回订阅对象(Subscription)
    console.log					// 观察者（Observer）
)
```

- ##### 建立可观察的Observable对象

  ```javascript
  // 变量后带$通常代表是一个Observable
  var clicks$ = rxjs.fromEvent(document, 'click');
  ```

- ##### 建立观察者对象（Observer）

  ```javascript
  var observer = { next: (x) => console.log(x) };
  ```

- ##### 建立订阅对象（订阅Observable对象，并传入Observer观察者对象）

  ```javascript
  // 通过subscribe传入Observer
  var subs$ = clicks$.subscribe(observer);
  ```

- ##### 取消订阅Subscription对象

  ```javascript
  subs$.unsubscribe();
  ```

  

# 操作符过滤

- ##### 建立可观察的Observable对象

  ```javascript
  var clicks$ = rxjs.fromEvent(document, 'click');
  ```

- ##### 套用操作符

  ```javascript
  // 通过pipe传入Operator
  clicks$ = clicks$.pipe(
      filter(x => x.clientX < 100),
      take(4)
  );
  ```

- ##### 建立订阅对象（订阅Observable对象并自动建立观察者对象）

  ```javascript
  var sub$ = clicks$.subscribe((x) => console.log(x));
  ```

- ##### 取消订阅Subscription对象

  ```javascript
  subs$.unsubscribe();
  ```

  

# 主体对象的用法

- ##### 建立主体对象（Subject）（之后要靠这个主体对象进行广播）

  ```javascript
  var subject = new rxjs.Subject();
  ```

- ##### 建立可观察的Observable对象

  ```javascript
  var clicks$ = rxjs.formEvent(document, 'click');
  ```

- ##### 设定最多取两个事件资料就将Obserable对象设置为完成

  ```javascript
  clicks$ = clicks$.pipe(take(2));
  ```

- ##### 设定将clicks$全部交由subject主体对象进行广播

  ```javascript
  // 用主体对象Subject去做观察者Observer的动作
  clicks$.subcribe(subject);
  ```

- ##### 最后再由subject去建立Observer观察者对象

  ```javascript
  // 两个观察者订阅同一个subject
  // 事实上subject只会发出一个request去观察这个Observable
  var subs1$ = subject.subscribe((x) => console.log(x.clientX));
  var subs2$ = subject.subscribe((x) => console.log(x.clientY));
  ```

- ##### 取消订阅Subscription对象

  ```javascript
  sub1$.unsubscribe();
  sub2$.unsubscribe();
  ```



# 操作符

## 创建

负责创建一个Observable对象

- ##### 常用操作符

  from

  fromEvent

  fromEventPattern

  interval

  of

  range

  throwError

  timer

- ##### 其他操作符

  ajax

  bindCallback

  bindNodeCallback

  defer

  generate

  iif



## 组合建立

可将多个Observable对象组合成一个Observable对象

- ##### 常用操作符

  ~~combineLatest~~

  ~~concat~~

  forkJoin

  ~~merge~~

- ##### 其他操作符

  ~~race~~

  ~~zip~~

  ~~partition~~



## 转换

- ##### 常用操作符

  concatMap

  concatMapTo

  map

  mapTo

  mergeMap

  mergeMapTo

  switchMap

  switchMapTo

  pluck

- ##### 其他操作符

  buffer

  bufferCount

  bufferTime

  bufferToggle

  bufferWhen

  exhaust

  exhaustMap

  expand

  groupBy

  mergeScan

  pairwise

  scan

  window

  windowCount

  windowTime

  windowToggle

  windowWhen



## 过滤

负责将Observable传入的资料过滤筛选掉

- ##### 常用操作符

  debounce

  debounceTime

  distinct

  filter

  first / last

  skip / take

  throttle

  throttleTime

  - 延迟执行

    ```typescript
    var button = document.querySelector('button');
    fromEvent(button, 'click').pipe(
    	throttleTime(1000)
    ).subscribe(() => console.log(`Clicked`));
    ```

- ##### 其他操作符

  audit

  auditTime

  distinctUntilChanged

  distinctKey

  distinctUntilKeyChanged

  elementAt

  ignoreElements

  sample

  sampleTime

  single

  skipLast

  skipUntil

  skipWhile

  takeLast

  takeUntile

  takeWhile



## 组合

负责组合多个Observable。

**组合运算符**是把多个Observable用pipe的方式组合在一起

**组合建立操作符**是在建立Observable的时候就一次合并在一起

- ##### 常用操作符

  combineAll

  concatAll

  mergeAll

  startWith

- ##### 其他操作符

  exhaust

  withLatestFrom



## 多播

负责将Observable广播给多位观察者

- ##### 常用操作符

  publish

  publishReplay

  share

- ##### 其他操作符

  multicast

  publishBehavior

  publishLast



## 错误处理

负责处理Observable观察过程中出现的例外错误

- ##### 常用运算符

  catchError

  retry

  retryWhen 



## 工具函数

负责提供Observable执行过程的工具函数

- ##### 常用运算符

  tap

  delay

  materialize

  timeout

  timeoutWith

  toArray

- ##### 其他操作符

  delayWhen

  dematerialize

  observeOn

  subscribeOn

  timeInterval

  timestamp



## 条件

负责计算特定条件并返回布尔值的操作符

- ##### 常用运算符

  defaultIfEmpty

  every

  find

  findIndex

  isEmpty



## 数学与汇总操作符

负责将Observable传来的资料进行数学/汇总运算

- ##### 常用运算符

  count

  max

  min

- ##### 其他操作符

  reduce



# Promise和RxJS对比

- Promise创建之后动作无法撤回

  Observable动作可以通过unsbscribe() 方法中途撤回

  ```javascript
  let promise = new Promise(resolve => {
  	setTimeout(() => {
  		resolve('---promise timeout---');
  	}, 2000);
  });
  // 创建之后动作无法撤回
  promise.then(value => console.log(value));
  ```

  ```javascript
  let stream = new Observable(observer => {
  let timeout = setTimeout(() => {
  	clearTimeout(timeout);
  	observer.next('observable timeout');
  }, 2000);
  });
  let disposable = stream.subscribe(value => console.log(value));
  setTimeout(() => {
  	// 通过unsubscribe() 可以撤回subscribe 的动作
  	disposable.unsubscribe();
  }, 1000);
  ```

- Promise不能订阅后多次执行，对于Promise 来说，最终结果要么resole兑现，要么reject拒绝，而且都只能触发一次

  Observable可以不断地触发下一个值

  ```javascript
  let promise = new Promise(resolve => {
  	setInterval(() => {
  		resolve('---promise setInterval---');
  	}, 2000);
  });
  // 只能输出一次
  promise.then(value => console.log(value));
  ```

  ```javascript
  let stream = new Observable<number>(observer => {
  	let count = 0;
  	setInterval(() => {
  		observer.next(count++);
  	}, 1000);
  });
  // 可以不断输出
  stream.subscribe(value => console.log("Observable>" + value));
  ```

  



