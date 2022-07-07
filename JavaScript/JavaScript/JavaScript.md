#   JavaScript的组成

- ECMAScript JavaScript语法

  ECMAScript 规定了JS的编程语法和基础核心知识，是所有浏览器厂商共同遵守的一套JS语法工业标准

- **DOM 页面文档对象模型**

  是处理可扩展标记语言的标准编程接口。通过 DOM 提供的接口可以对页面上的各种元素进行操作（大小、位置、颜色等）

- **BOM 浏览器对象模型** 

  提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。通过BOM可以操作浏览器窗口，比如弹出框、控制浏览器跳转、获取分辨率等。



# 数据类型和运算符

## 值类型

简单类型又叫做**基本数据类型**或者**值类型**

在存储时变量中**存储的是值本身**，因此叫做值类型

number，boolean，string，undefined，null，symbol(ES6)

- 简单数据类型存放到栈里面，存放的是值

- 简单类型传参

  值类型变量作为参数传给函数的形参时，其实是把变量在栈空间里的值复制了一份给形参，

  方法内部对形参做任何修改，都不会影响到的外部变量

- 数字型特殊值

  - 最大值：Number.MAX_VALUE：1.7976931348623157e+308
  - 最小值：Number.MIN_VALUE：5e-32
  - 无穷大，大于任何数值 Infinity
  - 无穷小，小于任何数值 -Infinity
  - 非数值 NaN

- `isNaN()`来判断一个变量是否为非数字的类型，是数字返回 true， 不是数字返回 false

- undefined 和 number或boolean 相加，最后结果是NaN

- 浮点数最高精度是17位小数，在算术计算时其精度远远不如整数

  `var result = 0.1 + 0.2; // 结果不是 0.3，而是：0.30000000000000004
  console.log(0.07 * 100); // 结果不是 7， 而是：7.000000000000001`

  不要直接判断两个浮点数是否相等 
  
- 值类型null，返回的是一个空对象object

  ```javascript
  var timer = null;
  console.log(typeof timer);		// object
  ```

- 二进制、八进制、十六进制写法（ES6）

  ```javascript
  const num1 = 100;	// 十进制
  // b -> binary
  const num2 = 0b100;	// 二进制
  // o -> octonary
  const num3 = 0o100;	// 八进制
  // x -> hexadecimal
  const num4 = 0x100;	// 十六进制
  console.log(num1, num2, num3, num4);
  // 100 4 64 256
  ```

- 大数值连接符（ES12）

  数字过长时，可以使用_作为连接符

  ```javascript
  const num = 100_000_000;
  console.log(num);	// 100000000
  ```

- BigInt大整数类型

  ES11之前可以表示的最大整数是Number.MAX_SAFE_INTEGER，大于MAX_SAFE_INTEGER的数值，表示的可能是不正确的

  ES11中，引入了新的数据类型BigInt，用于表示大的整数，表示方法是在数值的后面加上n

  ```javascript
  const maxInt = Number.MAX_SAFE_INTEGER
  console.log(maxInt) 				// 9007199254740991
  console.log(maxInt + 1)				// 9007199254740992
  console.log(maxInt + 2)				// 9007199254740992
  
  const bigInt = 900719925474099100n
  console.log(bigInt + 10n)			// 900719925474099110n
  
  const num = 100
  console.log(bigInt + BigInt(num))	// 900719925474099200n
  
  // 大数字转小数字
  const smallNum = Number(bigInt)
  console.log(smallNum)				// 900719925474099100
  ```



## 引用类型

复杂类型又叫做引用类型

在存储时变量中存储的仅仅是地址（引用），因此叫做引用数据类型

通过 new 关键字创建的对象（系统对象、自定义对象），如 Object、Array、Date等

- 复杂数据类型栈里存放的是地址，真正的对象实例存放在堆空间中

- 复杂类型传参

  把引用类型变量传给形参时，其实是把变量在栈空间里保存的堆地址复制给了形参，

  形参和实参其实保存的是同一个堆地址，所以操作的是同一个对象



## Symbol数据类型

Symbol是ES6中新增的一个基本数据类型/值类型

对象的属性名都是字符串形式，那么很容易造成属性名的冲突，从而覆盖掉它内部的某个属性

Symbol可以用来生成一个独一无二的值

- Symbol值是通过Symbol函数来生成的，生成后可以作为属性名

  ```javascript
  const s1 = Symbol();
  const s2 = Symbol();
  console.log(s1 === s2);	// false
  ```

- 在ES6中，对象的属性名可以使用字符串，也可以使用Symbol值

  ```javascript
  const s1 = Symbol();
  const s2 = Symbol();
  const obj = {
    [s1]: "abc",
    [s2]: "cba"
  };
  // 新增属性
  obj[s3] = "nba";
  // Object.defineProperty方式
  const s4 = Symbol();
  Object.defineProperty(obj, s4, {
    enumerable: true,
    configurable: true,
    writable: true,
    value: "mba"
  });
  // 不能通过.语法获取
  console.log(obj[s1], obj[s2], obj[s3], obj[s4]);
  ```

- 可以在创建Symbol值的时候传入一个描述(description)

  ```javascript
  const s = Symbol("name");
  console.log(s.description);
  ```

- 使用Symbol作为key的属性名,在遍历/Object.keys等中是获取不到的

  ```java
  const s1 = Symbol();
  const s2 = Symbol();
  const obj = {
    [s1]: "abc",
    [s2]: "cba"
  };
  console.log(Object.keys(obj));					// []
  console.log(Object.getOwnPropertyNames(obj));	// []
  // 需要Object.getOwnPropertySymbols来获取所有Symbol的key
  console.log(Object.getOwnPropertySymbols(obj));	// [ Symbol(), Symbol() ]
  // 获取并遍历
  const sKeys = Object.getOwnPropertySymbols(obj)
  for (const sKey of sKeys) {
    console.log(obj[sKey]);
  }
  // 'abc'
  // 'cba'
  ```

- 使用Symbol.for(key)创建相同的值

  通过Symbol.keyFor方法来获取对应的key

  ```javascript
  const sa = Symbol.for("aaa");
  const sb = Symbol.for("aaa");
  console.log(sa === sb);			// true
  // 通过key获取值
  const key = Symbol.keyFor(sa);	// 'aaa'
  ```



## Set/Map

### Set

Set是ES6中新增的数据结构，可以用来保存数据，类似于数组，但是和数组的区别是**元素不能重复**

创建Set需要通过**Set构造函数**

```javascript
const set = new Set();
set.add(10);
set.add(20);
set.add(40);
set.add(333);
set.add(10);
console.log(set);	// { 10, 20, 40, 333 }
```

添加对象时需要特别注意，对象的内存地址是不同的

```javascript
const set = new Set();
set.add(10);
set.add({});
set.add({});
console.log(set);	// { 10，{}，{} }

const obj = {};
const set2 = new Set();
set.add(10);
set.add(obj);
set.add(obj);
console.log(set);	// { 10，{} }
```

给数组去重

```javascript
const arr = [33, 10, 26, 30, 33, 26];
const arrSet = new Set(arr);
console.log(arrSet);	// { 33, 10, 26, 30 }
// set转换成数组
const newArr = Array.from(arrSet);
// const newArr = [...arrSet];
console.log(newArr);	// [33, 10, 26, 30]
```

常见方法

- size：返回Set中元素的个数

  ```javascript
  const arr = [33, 10, 26, 30, 33, 26];
  const arrSet = new Set(arr);
  console.log(arrSet.size);	// 4
  ```

- add(value)：添加某个元素，返回Set对象本身

  ```javascript
  const arr = [33, 10, 26, 30, 33, 26];
  const arrSet = new Set(arr);
  arrSet.add(100);
  console.log(arrSet);	// { 33, 10, 26, 30, 100 }
  ```

- delete(value)：从set中删除和这个值相等的元素，返回boolean类型。不支持索引，只能传入元素

  ```javascript
  const arr = [33, 10, 26, 30];
  const arrSet = new Set(arr);
  arrSet.delete(33);
  console.log(arrSet);	// { 10, 26, 30 }
  ```

- has(value)：判断set中是否存在某个元素，返回boolean类型

  ```javascript
  const arr = [33, 10, 26, 30];
  const arrSet = new Set(arr);
  console.log(arrSet.has(33));	// true
  ```

- clear()：清空set中所有的元素，没有返回值

  ```javascript
  const arr = [33, 10, 26, 30];
  const arrSet = new Set(arr);
  arrSet.clear();
  console.log(arrSet);	// {}
  ```

- forEach(callback, [, thisArg])：通过forEach遍历set

  ```javascript
  const arr = [33, 10, 26, 30];
  arrSet.forEach(item => {
    console.log(item)
  })
  for (const item of arrSet) {
    console.log(item)
  }
  ```



### WeakSet

WeakSet是和Set类似的另外一个数据结构，也是内部元素不能重复的数据结构

WeakSet和Set的区别

1. WeakSet中只能存放对象类型，不能存放基本数据类型
2. WeakSet对元素的引用是弱引用，如果没有其他引用对某个对象进行引用，那么GC可以对该对象进行回收

```javascript
let obj = { 
  name: "jack"
}
const set = new Set();
// 建立的是强引用
set.add(obj);
```

```javascript
let obj = { 
  name: "jack"
}
const weakSet = new WeakSet();
// 建立的是弱引用
weakSet.add(obj);
```

<img src="JavaScript.assets/image-20220502002719098.png" alt="image-20220502002719098" style="zoom:67%;" /> 

常见方法

- add(value)：添加某个元素，返回WeakSet对象本身
- delete(value)：从WeakSet中删除和这个值相等的元素，返回boolean类型
- has(value)：判断WeakSet中是否存在某个元素，返回boolean类型

WeakSet不能遍历

- 因为WeakSet只是对对象的弱引用，如果我们遍历获取到其中的元素，那么有可能造成对象不能正常的销毁
- 所以存储到WeakSet中的对象是没办法获取的

WeakSet的应用场景

```javascript
// 如果使用Set，创建出来的对象会被强引用，即使p = null，Set中的对象p也不会销毁
const personSet = new WeakSet();
class Person {
    constructor() {
        // 构造器中每次创建出来的对象都添加到Set
        personSet.add(this);
    }
    running() {
        // 在每次调用方法的时候，去判断当前的调用是不是通过构造器创建出来的
        if (!personSet.has(this)) {
            throw new Error("不能通过非构造方法创建出来的对象调用running方法");
        }
        console.log('running', this);
    }
}
const p = new Person();
p.running();
p = null;

// 不允许通过其他的对象对方法调用
p.running.call({name: "jack"});		// Error: 不能通过非构造方法创建出来的对象调用running方法
```



### Map

ES6中新增的数据结构是Map，用于存储映射关系

对象存储映射关系只能用字符串（ES6新增了Symbol）作为属性名（key），某些情况下可能希望通过其他类型作为key，比如对象

```javascript
// 对象中是不能使用对象来作为key的
const obj1 = { name: "kobe" };
const info = {
  [obj1]: "aaa"
}
// 会自动将对象转成字符串来作为key
console.log(info);	// { "[object Object]": "aaa" }
```

Map允许对象类型来作为key，使用构造方法创建Map

```javascript
const obj1 = { name: "jack" };
const obj2 = { name: "kobe" };
const map = new Map();
// set方法添加key，value
map.set(obj1, "aaa");
map.set(obj2, "bbb");
map.set(1, "ccc");
console.log(map);		// { { name: 'jack' }: 'aaa', { name: 'kobe' }: 'bbb', 1: 'ccc' }

const map2 = new Map([[obj1, "aaa"], [obj2, "bbb"], [2, "ddd"]]);
console.log(map2);		// { { name: 'jack' }: 'aaa', { name: 'kobe' }: 'bbb', 2: 'ddd' }
```

常见方法

- size：返回Map中元素的个数

  ```javascript
  const map = new Map([[obj1, "aaa"], [obj2, "bbb"], [2, "ddd"]]);
  console.log(map.size);	// 3
  ```

- set(key, value)：在Map中添加key、value，并且返回整个Map对象

  ```javascript
  const map = new Map([[obj1, "aaa"], [obj2, "bbb"], [2, "ddd"]]);
  map2.set("name", "eee");
  console.log(map2);
  ```

- get(key)：根据key获取Map中的value

  ```javascript
  const map = new Map([['name', 'jack'], ['address', '北京']]);
  console.log(map.get("name"));	// jack
  ```

- has(key)：判断是否包括某一个key，返回Boolean类型

  ```javascript
  const map = new Map([['name', 'jack'], ['address', '北京']]);
  console.log(map.has("name"));	// true
  ```

- delete(key)：根据key删除一个键值对，返回Boolean类型

  ```javascript
  const map = new Map([['name', 'jack'], ['address', '北京']]);
  map.delete("name");
  console.log(map);
  ```

- clear()：清空所有的元素

  ```javascript
  const map = new Map([['name', 'jack'], ['address', '北京']]);
  map.clear();
  console.log(map);
  ```

- forEach(callback, [, thisArg])：通过forEach遍历Map

  ```javascript
  const map = new Map([['name', 'jack'], ['address', '北京']]);
  map.forEach((item, key) => {
    console.log(item, key);
  })
  // jack name
  // 北京 address
  for (const item of map) {
    console.log(item);
  }
  // ['name', 'jack']
  // ['address', '北京']
  for (const [key, value] of map) {
    console.log(key, value)
  }
  // name jack
  // address 北京
  ```



### WeakMap

WeakMap数据结构和Map类型相似，也是以键值对的形式存在的

WeakMap和Map的区别

1. WeakMap的**key只能使用对象**，不接受其他的类型作为key
2. WeakMap的key对对象的引用是**弱引用**，如果没有其他引用引用这个对象，那么GC可以回收该对象

```javascript
const obj = {name: "obj1"};
const map = new Map();
// 强引用：obj = null,由于强引用，obj对象的内存不会销毁
map.set(obj, "aaa");
const weakMap = new WeakMap();
// 弱引用：obj = null,由于弱引用，obj对象的内存会被销毁
weakMap.set(obj, "aaa");
```

常见方法

- set(key, value)：在Map中添加key、value，并且返回整个Map对象
- get(key)：根据key获取Map中的value
- has(key)：判断是否包括某一个key，返回Boolean类型
- delete(key)：根据key删除一个键值对，返回Boolean类型

WeakMap是不能遍历的，没有forEach方法，也不支持通过for of的方式进行遍历

WeakMap的应用场景

```javascript
// 应用场景(vue3响应式原理)
const obj1 = {
  name: "why",
  age: 18
}
// 与obj1.name绑定，obj1.name改变时执行
function obj1NameFn1() {
  console.log("obj1NameFn1被执行");
}
function obj1NameFn2() {
  console.log("obj1NameFn2被执行");
}
// 与obj1.age绑定，obj1.age改变时执行
function obj1AgeFn1() {
  console.log("obj1AgeFn1");
}
function obj1AgeFn2() {
  console.log("obj1AgeFn2");
}

const obj2 = {
  name: "kobe",
  height: 1.88,
  address: "广州市"
}
// 与obj2.name绑定，obj2.name改变时执行
function obj2NameFn1() {
  console.log("obj1NameFn1被执行");
}
function obj2NameFn2() {
  console.log("obj1NameFn2被执行");
}

// 1.创建WeakMap
const weakMap = new WeakMap();

// 2.收集依赖结构
// 2.1.对obj1收集的数据结构
const obj1Map = new Map();
obj1Map.set("name", [obj1NameFn1, obj1NameFn2]);
obj1Map.set("age", [obj1AgeFn1, obj1AgeFn2]);
weakMap.set(obj1, obj1Map);

// 2.2.对obj2收集的数据结构
const obj2Map = new Map();
obj2Map.set("name", [obj2NameFn1, obj2NameFn2]);
weakMap.set(obj2, obj2Map);

// 3.如果obj1.name发生了改变
// Proxy/Object.defineProperty
obj1.name = "james";
const targetMap = weakMap.get(obj1);
const fns = targetMap.get("name");
fns.forEach(item => item());

// 使用weakMap，假如obj1 = null，由于是弱引用，obj1的内存会被销毁，同时obj1对应的value的对象的内存也会被销毁
```



## 数据类型转换

### 转换为字符串

<img src="JavaScript.assets/image-20211130231947016.png" alt="image-20211130231947016" style="zoom: 67%;" /> 



### 转换为数字

<img src="JavaScript.assets/image-20211130232531550.png" alt="image-20211130232531550" style="zoom:67%;" /> 

- 隐式转换`var a = + '11'; // 11  `
- `parseInt('120px'); // 120`



### 转换为布尔值

- 一个感叹号

  代表空、否定的值会被转换为 false ，如 ''、0、NaN、null、undefined。其余值都会被转换为 true

- 两个感叹号

  `!!`常常用来做类型判断。

  - `undefined`和`null`为false
  - 任意数组，对象，函数都转化为true，即使是空数组，空对象。
  - 空字符串为false，非空字符串为true。
  - 数值`0`，不确定值`NaN`为false，其它为true，无穷大也是true。

  如果值为真:

  - 可以排除`undefined`和`null`

  - 数值：表示不是0，且有确定含义的值（包括无穷大）
  - 字符串：表示长度大于0的字符串
  - 数组，对象，函数：只能表示不是`undefined`或`null`,并不能判断是否有元素和内容。

  下面两个用法其实是完全等价的

  ```javascript
  let a; // null、undefined、''、0
  if (a !== null && typeof(a) !== "undefined" && a !== undefined && a !== '' && a !== 0) {
  }
  
  let a;
  if(!!a){
  }
  ```

- javascript中类型转换不具有传递性。字符串`”0″`和数值`0`可以相互转换，但它们转换为不同的布尔值，即0可转换为”0″，”0″可转换为true,但0却转换为false



### 检测数据类型

- `typeof `主要用于检测基本类型

  <img src="JavaScript.assets/image-20211130231820193.png" alt="image-20211130231820193" style="zoom:50%;" /> 

  ```javascript
  typeof {};				//=> object
  typeof [];				//=> object
  typeof function() {};	//=> function
  ```

- `instanceof` 主要用于检测引用类型

  ```javascript
  // 根据对象的原形链往上找,如果原形链上有右边函数.prototype,返回true;
  var obj = {}; obj instanceof Object; 			//=> true; 
  var arr = []; arr instanceof Array; 			//=> true;
  var fn = function() {}; fn instanceof Function; //=> true;
  ```

- `Object.prototype.toString.call(sth)` 原形链的检测有漏洞(原型是可以改变的)。所以会造成检测结果不准确，所以可以采用此种形式。

  ```javascript
  var toString = Object.prototype.toString;
  toString.call(undefined);					//=> [object Undefined]
  toString.call(1);							//=> [object, Number]
  toString.call(NaN);							//=> [object, Number]
  toString.call('a');							//=> [object, String]
  toString.call(true);						//=> [object, Boolean]
  toString.call({});							//=> [object, Object]
  toString.call(function() {});				//=> [object, Function]
  toString.call([]);							//=> [object, Array]
  toString.call(null);						//=> [object, Null]
  ```



## 拷贝

### 浅拷贝

只拷贝一层, 更深层次对象级别的只拷贝引用

```javascript
var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    }
};
var o = {};
for (var k in obj) {
    // k 是属性名   obj[k] 属性值
    o[k] = obj[k];
}
```

es6 新增浅拷贝方法

`Object.assign(target, ...sources) `



### 深拷贝

拷贝多层, 每一级别的数据都会拷贝

```javascript
var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    },
    color: ['pink', 'red']
};
var o = {};

function deepCopy(newobj, oldobj) {
    for (var k in oldobj) {
    	// 判断属性值属于那种数据类型
    	// 1. 获取属性值  oldobj[k]
    	var item = oldobj[k];
    	// 2. 判断这个值是否是数组
        // 数组也是对象，所以要放到对象判断前面
    	if (item instanceof Array) {
    	    newobj[k] = [];
    	    deepCopy(newobj[k], item)
    	} else if (item instanceof Object) {
    	    // 3. 判断这个值是否是对象
    	    newobj[k] = {};
    	    deepCopy(newobj[k], item)
    	} else {
    	    // 4. 属于简单数据类型
    	    newobj[k] = item;
    	}
	}
}

deepCopy(o, obj);
```



## 递增和递减运算符

递增（++）和递减（ -- ）既可以放在变量前面，也可以放在变量后面

- ##### 前置递增运算符

  `++num` 前置递增，就是自加1， `num = num + 1`

  先自加，后返回值

  `var num = 10;
  alert(++num + 10); // 21`

- ##### 后置递增运算符

  `num++` 后置递增，就是自加1， `num = num + 1`

  先返回原值，后自加

  `var num = 10;
  alert(10 + num++); // 20`



## 指数运算符

在ES7之前，计算数字的乘方需要通过Math.pow 方法来完成

在ES7中，增加了** 运算符，可以对数字来计算乘方

```javascript
// 计算3的3次方
const result1 = Math.pow(3, 3)
const result2 = 3 ** 3
console.log(result1, result2)
```



## 空值合并运算符

空值合并操作符（??）,当左侧的操作数为null或者undefined时，返回其右侧操作数，否则返回左侧操作数（ES11）

```javascript
// 用逻辑或来设置默认值有弊端
const foo = 0;
const bar = foo || 'defualt value';
console.log(bar);
// 'defualt value'

const foo = '';
const bar = foo || 'defualt value';
console.log(bar);
// 'defualt value'

// 使用空值合并运算符??
// 只判断是不是null和undefined
const foo = undefined;
const bar = foo ?? "defualt value";
console.log(bar);
// 'defualt value'
```



## 逻辑赋值运算符

逻辑或赋值运算 `||=` （ES12）

```javascript
let message = "hello world;
// message = message || "default value";
message ||= "default value";
```

逻辑与赋值运算 `&&=`（ES12）

```javascript
let info = { name: "aa" };
// info = info && info.name;
info &&= info.name;
```

逻辑空赋值运算 `??=`（ES12）

```javascript
let message = undefined;
// message = message ?? "default value";
message ??= "default value";
```



## 运算符优先级

<img src="JavaScript.assets/image-20211201000935844.png" alt="image-20211201000935844" style="zoom:67%;" /> 

- 一元运算符里面的[逻辑非]优先级很高
- [逻辑与]比[逻辑或]优先级高



# 语法

## let/const

ES6开始新增了两个关键字可以声明变量：let、const

**const关键字**保存的数据一旦被赋值，就**不能被修改**，但是如果赋值的是引用类型，那么可以通过引用找到对应的对象，修改对象内容

```javascript
const obj = {
  foo: "foo"
}
obj.foo = "aaa";
```

```javascript
const names = ["abc", "cba", "nba"];
// const 定义的常量不能做++操作
for (const i = 0; i < names.length; i++) {	// TypeError: Assignment to constant variable
  console.log(names[i])
}
for (const item of names) {
  console.log(item)
}
```

let、const**不允许重复声明变量**

```javascript
var bar = "abc";
var bar = "cba";
let foo = "abc"
// SyntaxError: Identifier 'foo' has already been declared
let foo = "cba"
```

let/const是**没有作用域提升**的

但是会在执行上下文创建阶段被创建出来，但是是不可以访问它们的，直到词法绑定被求值

```java
console.log(foo);
var foo = "foo";
console.log(bar);
// ReferenceError: Cannot access 'bar' before initialization
let bar = 'bar';
```

let和const声明的变量具有**块级作用域**，var定义的变量可以跨块作用域访问

```javascript
var arr = [];
for (var i = 0; i < 2; i++) {
    arr[i] = function () {
        console.log(i); 
    }
}
// 变量i是全局的，函数执行时输出的都是全局作用域下的i值
arr[0](); // 2
arr[1](); // 2
```

```javascript
let arr = [];
for (let i = 0; i < 2; i++) {
    arr[i] = function () {
        console.log(i); 
    }
}
// 每次循环都会产生一个块级作用域，每个块级作用域中的变量都是不同的
// 函数执行时输出的是自己上一级（循环产生的块级作用域）作用域下的i值
arr[0](); // 0
arr[1](); // 1
```

使用let、const声明的变量，在声明之前，变量都是不可以访问的，这种现象称之为**暂时性死区**

```javascript
var foo = "foo"
if (true) {
  console.log(foo);	// ReferenceError: Cannot access 'foo' before initialization
  let foo = "abc";
}
```



## 字面量增强

ES6中对对象字面量进行了增强，称之为Enhanced object literals（增强对象字面量）

### 属性的简写

```javascript
var name = 'jack';
var age = 18;
var obj = {
    // name: name,
    // age: age
    name,
    age
}
```

### 方法的简写

```javascript
var obj = {
    // foo: function() { }
    foo(){
        
    }
}
```

### 计算属性名

```javascript
var name = 'jack';
var obj = {
    [name + '123']: 'haha'
}
```



## 解构

ES6中新增了从数组或对象中方便获取数据的方法

### 数组解构

```javascript
var names = ['abc', 'cba', 'nba'];
// var item1 = names[0];
// var item2 = names[1];
// var item3 = names[2];
var [item1, item2, item3] = names;
```

默认从头开始获取

```javascript
var names = ['abc', 'cba', 'nba'];
var [itema, itemb] = names;
// itema: 'abc'
// itemb: 'cba'
```

解构后面的元素

```javascript
var names = ['abc', 'cba', 'nba'];
var [, , itemc] = names;
// itemc: 'nba'
```

解构出一个元素,后面的元素放到一个新数组中

```javascript
var names = ['abc', 'cba', 'nba'];
var [itemx, ...newNames] = names;
// itemx: 'abc'
// newNames: ['cba', 'nba']
```

 如果解构不成功，变量的值为undefined

```javascript
var names = ['abc', 'cba', 'nba'];
var [item1, item2, item3, item4] = names;
// item4: undefined
```

没有解构出来设置默认值

```javascript
var names = ['abc', 'cba', 'nba'];
var [itema, itemb, itemc, itemd = 'aaa'] = names;
// itemd: 'aaa'
```

### 对象解构

```javascript
var obj = {
  name: "jack",
  age: 18,
  height: 1.88
}
var { name, age, height } = obj;
// name: 'jack'
// age: 18
// height: 1.88
```

不会根据顺序赋值

```javascript
var obj = {
  name: "jack",
  age: 18,
  height: 1.88
}
var { height, age, name } = obj;
// height: 1.88
// age: 18
// name: 'jack'
```

只解构其中某个值

```javascript
var obj = {
  name: "jack",
  age: 18,
  height: 1.88
}
var { age } = obj;
// age: 18
```

自定义变量名

```javascript
var obj = {
  name: "jack",
  age: 18,
  height: 1.88
}
var { name: newName } = obj;
// newName: 'jack'
```

没有解构出来设置默认值

```javascript
var obj = {
  name: "jack",
  age: 18,
  height: 1.88
}
var { address: newAddress = "广州市" } = obj;
// newAddress: '广州市'
```

方法参数解构

```javascript
var obj = {
  name: "jack",
  age: 18,
  height: 1.88
}
function foo(info) {
    console.log(info.name, info.age);
}
foo(obj);
function bar({ name, age }) {
    console.log(name, age);
}
bar(obj);
// 'jack', 18
```



## 展开运算符

展开运算符可以将数组或者字符串拆分成以逗号分隔的参数序列（ES6）

还可以在构造字面量对象时, 将对象表达式按key-value的方式展开（ES9）

```javascript
const ary = ["a", "b", "c"];
// ...ary =>> "a", "b", "c"
console.log(...ary); // a b c

function foo(x, y, z) {
    console.log(x, y, z);
}
foo(...ary);
```

字符串拆分

```javascript
const name = 'tom';
function foo(x, y, z) {
    console.log(x, y, z);
}
foo(...name);
```

数组合并

```javascript
let ary1 = [1, 2, 3];
let ary2 = [4, 5, 6];
let ary3 = [...ary1, ...ary2];
```

```javascript
let ary1 = [1, 2, 3];
let ary2 = [4, 5, 6];
ary1.push(...ary2);
```

伪数组转换为真正的数组

```javascript
var oDivs = document.getElementsByTagName('div');
var ary = [...oDivs];
```

剩余参数和解构配合

```javascript
let students = ['wangwu', 'zhangsan', 'lisi'];
let [s1, ...s2] = students;
console.log(s1);  // 'wangwu'
console.log(s2);  // ['zhangsan', 'lisi']
```

展开字面量对象

```javascript
const info = { name: 'jack', age: 18 };
const obj = { ...info, address: '北京市' };
console.log(obj);	// { name: 'jack', age: 18, address: '北京市' }
```

展开运算符其实是一种**浅拷贝**

```javascript
const info = {
  name: "jack",
  friend: { name: "kobe" }
};
const obj = { ...info, name: "tom" };
console.log(obj);				// { name: 'tom', friend: { name: 'kobe' } }
obj.friend.name = "james";
console.log(info.friend.name);	// james
```



## 可选链

ES11中新增一个特性，主要作用是让代码在进行null和undefined判断时更加清晰和简洁

```javascript
const obj = {
    friend: {
        girlFriend: {
            name: 'lucy'
        }
    }
}
if (obj.friend && obj.friend.girlFriend) {
    console.log(obj.friend.girlFriend.name);
}
// 可选链形式
console.log(obj.friend?.girlFriend?.name);
```



## 严格模式

严格模式是一种具有限制性的JavaScript模式，是代码隐式的脱离了懒散模式

支持严格模式的浏览器在检测到代码中有严格模式时，会以更加严格的方式对代码进行检测和执行

严格模式的限制

1. 抛出错误来消除一些原有的静默(silent)错误
2. JS引擎在执行代码时可以进行更多的优化（不需要对一些特殊的语法进行处理）
3. 禁用了再ECMAScript为了版本中可能会定义的一些语法（保留字，关键字）

开启严格模式进行解析

1. js文件开启严格模式

   ```javascript
   'use strict';
   
   message = 'Hello World';
   console.log(message);
   ```

2. 某一函数开启严格模式

   ```javascript
   function foo() {
       'use strict';
       true.foo = 'abc';
   }
   ```

严格模式常见限制

1. 无法意外的创建全局变量

2. 严格模式会使引起静默失败（不报错也没有任何效果）的赋值操作抛出异常

3. 严格模式下试图删除不可删除的属性

4. 不允许函数参数有相同的名称

5. 不允许0的八进制语法

6. 不允许使用with

7. eval不再为上层引用变量

   ```javascript
   var jsString = 'var message = "Hello World"; console.log(message);'
   eval(jsString);
   
   // 不开启严格模式
   console.log(message);		// Hello World
   // 开启严格模式
   console.log(message);		// 报错 message is not define
   ```

8. this绑定不会默认转成对象

   ```javascript
   function foo() {
       console.log(this);
   }
   foo();
   // 不开启严格模式：window
   // 开启严格模式：undefined
   
   var obj = {
       name: 'jack',
       foo: foo
   };
   var bar = obj.foo;
   bar();
   // 不开启严格模式：window
   // 开启严格模式：undefined
   ```

   ```javascript
   setTimeout(function() {
       console.log(this)
   }, 1000)
   // 无论是不是严格模式 都是 window
   // 内部函数fn.apply(this) this就是window
   ```




# 函数

## 函数的定义

- 命名函数 `function `关键字 

- 匿名函数

- `new Function('参数1','参数2'..., '函数体')`

  函数也属于对象

- 箭头函数` () => {}` 

- 函数也是一种数据类型，可以作为参数和返回值



## 函数参数

### 不定参数arguments

- arguments 是当前函数的一个内置对象

  所有函数都内置了一个 arguments 对象，arguments 对象中存储了传递的所有实参

  ```javascript
  function foo(num1, num2, num3) {
      // 会去AO中寻找arguments
      console.log(arguments);			// Arguments(5) [10, 20, 30, 40, 50]
      console.log(num1, num2, num3);
  }
  foo(10, 20, 30, 40, 50);
  
  // AO对象
  // var ao = {
  //     num1: undefined,
  //     num2: undefined,
  //     num3: undefined,
  //     arguments: {}
  // }
  ```

- arguments展示形式是一个**伪数组**，可以进行遍历
  - 具有 length 属性
  - 按索引方式储存数据
  - 不具有数组的 push , pop ,forEach,map等方法
  
- arguments常见操作

  1. 获取参数的长度：arguments.length
  2. 根据索引值获取某一个参数：arguments[2]
  3. 获取当前arguments所在的函数：arguments.callee

- 箭头函数没有arguments，如果想要获取arguments只能去上层作用域找

  ```javascript
  function foo() {
      var bar = () => {
          console.log(arguments)
      };
      return bar;
  }
  var fn = foo(123);
  fn();	// {'0': 123}
  ```

- arguments转array类型

  `var newArray = Array.prototype.slice.call(arguments)`

  `var newArray = [].slice.call(arguments)`

  `var newArray = Array.from(arguments)` 

  `var newArray = [...arguments]`

  如果不使用call调用，获取的this是原型对象prototype，但是需要获取的是可遍历对象

  ```javascript
  var names = ['jack', 'tom', 'john', 'jerry'];
  names.slice(1, 3);
  
  // 模拟Array中的slice实现
  Array.prototype.slice  = function(start, end) {
      var arr = this;
      start = start || 0;
      end = end || arr.length;
      var newArray = [];
      for (var i = start; i < end; i++) {
          newArray.push(arr[i]);
      } 
      return newArray;
  }
  var newArray = Array.prototype.slice.call(['a', 'b', 'c']);
  ```




### 剩余参数

将一个不定数量的参数表示为一个数组（ES6新增）

如果最后一个参数是... 为前缀的，那么它会将剩余的参数放到该参数中，并且作为一个数组

剩余参数必须放到最后一个位置，否则会报错

```javascript
function sum (first, ...args) {
    console.log(first); // 10
    console.log(args); // [20, 30] 
}
sum(10, 20, 30)
```

剩余参数和arguments的区别

1. 剩余参数只包含那些没有对应形参的实参，而arguments 对象包含了传给函数的所有实参
2. arguments对象不是一个真正的数组，剩余参数是一个真正的数组，可以进行数组操作



### 默认参数

在ES6中，允许给函数参数默认值

```javascript
function foo(x = 20, y = 30) {
    console.log(x, y);
}
foo(50, 100);	// 50, 100
foo();			// 20, 30
```

默认值可以和解构一起使用

```javascript
// 写法一
function foo({ name, age } = { name: 'jack', age: 18 }) {
    console.log(name, age);
}
// 写法二
function foo({ name = 'jack', age = 18 } = {}) {
    console.log(name, age);
}
```

有默认值的形参最好放最后

```javascript
function foo(x, y, z = 30) {
    console.log(x, y, z);
}
foo(10, 20);
function bar(x = 30, y, z) {
    console.log(x, y, z);
}
foo(undefined, 10, 20);
```

默认值会改变函数的length的个数，默认值以及后面的参数都不计算在length之内

```javascript
function bar(x, y, z, m, n) {
    console.log(x, y, z, m, n);
}
console.log(bar.length);			// 5

function foo(x, y, z = 30, m, n) {
    console.log(x, y, z, m, n);
}
console.log(foo.length);			// 2
```



### 标签模板字符串

使用标签模板字符串，并且在调用函数的时候插入其他的变量（ES6新增）

1. 第一个元素是数组，是被模板字符串拆分的字符串组合
2. 后面的元素是每个模板字符串传入的内容

```javascript
function foo(...args) {
    console.log(args);
}
foo('Hello World');

const name = 'jack';
const age = 18;
// [ ['Hello ', ' World ', ''], 'jack', 18 ]
foo`Hello ${name} World ${age}`;
```



## 高阶函数

一个函数如果接受另外一个**函数作为参数**，或者该函数会返回另外一个**函数作为返回值**，那么这个函数就称为高阶函数

**函数作为参数**

```javascript
function calc(num1, num2, calcFn) {
    console.log(calcFn(num1, num2));
}
function add(num1, num2) {
    return num1 + num2;
}
function sub(num1, num2) {
    return num1 - num2;
}
function mul(num1, num2) {
    return num1 * num2;
}

var m = 20;
var n = 30;
calc(20, 30, mul);
```

**函数作为返回值**

```javascript
function makeAdder(count) {
    function add(num) {
        return count + num;
    }
    return add;
}
var add5 = makeAdder(5);
console.log(add5(6));
console.log(add5(10));
```



## 箭头函数

箭头函数**不绑定this、arguments**属性

箭头函数中的this，指向的是**函数定义位置的上下文this**

箭头函数是**没有显式原型**的，**不能作为构造函数**来使用，**不能使用new**来创建对象

```javascript
// 普通函数
function foo() {}
var f = new foo();			// f.__proto__ = foo.prototype
// 箭头函数
var bar = () => {}
console.log(bar.prototype);	// undedined
var b = new bar();			// TypeError: bar is not a constructor
```

箭头函数的简写

1. 参数只有一个，()可以省略

   ```javascript
   var nums = [10, 20, 30, 40];
   // 一个可以省略
   nums.foreach(item => {
      console.log(item); 
   });
   // 多个不可省略
   nums.foreach((item, index) => {
      console.log(item, index); 
   });
   ```

2. 函数执行体只有一行代码，{}可以省略

   并且会默认将这一行代码的执行结果作为返回值

   ```javascript
   var nums = [10, 20, 30, 40];
   nums.foreach(item => console.log(item));
   nums.filter(item => item % 2 === 0);
   ```

3. 如果只有一行代码，并且返回一个对象，在返回对象外加()

   ```javascript
   var bar = () => ({ name: 'jack', age: 18 });
   ```

   



## 立即执行函数

- `(function() {})()` 或 `(function(){}())`
- 会创建一个独立的作用域。 避免了命名冲突问题



## 纯函数

纯函数需要符合以下条件

1. 确定的输入，一定会产生确定的输出

2. 函数在执行过程中，不能产生副作用

   副作用表示在执行一个函数时，除了返回函数值之外，还对调用函数产生了附加影响，比如修改了全局变量，修改参数或者改变外部存储

纯函数的优势：只需要关心函数的参数和返回值就可以，单纯实现业务逻辑即可

```javascript
// 纯函数
function foo(num1, num2) {
    return num1 * 2 + num2 * num2;
}
// 不是纯函数
var name = 'abc'
function bar() {
    name = 'cba';
}

// 不是纯函数
function baz(info) {
    info.age = 100;
}
var obj = { name: 'jack', age: 18 }
baz(obj);
```

```javascript
var names = ['abc', 'cba', 'nba', 'dna'];

// slice方法只要给它传入一个start/end，对于同一个数组来说，它会返回确定的值
// slice方法本身是不会修改原来的数组
// slice本身就是纯函数
var newNames1 = names.slice(0, 3);
console,log(newNames1);			// ['abc', 'cba', 'nba']
console.log(names);				// ['abc', 'cba', 'nba', 'dna'];

// splice在执行时，有修改调用的数组对象本身，修改这个操作就是产生的副作用
// splice不是一个纯函数
var newNames2 = names.splice(2);
console.log(newNames2);			// ['nba', 'dna']
console.log(names);				// ['nba', 'dna']
```



## 函数柯里化

函数柯里化：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩余的参数

```javascript
function add(x, y, z) {
    return x + y + z;
}
var result1 = foo(10, 20, 30);

// 柯里化
function sum(x) {
    return function(y) {
        return function(z) {
			return x + y + z;
        }
    }
}
var result2 = sum(10)(20)(30);

// 简化柯里化
var sum2 = x => y => z => x + y + z;
var result2 = sum2(10)(20)(30);
```

柯里化优势：**单一职责**，希望一个函数处理的问题尽可能的单一，而不是将一大堆的处理过程交给一个函数来处理

可以将每次传入的参数在单一函数中进行处理，处理完后在下一个函数中再使用处理后的结果

```javascript
function add(x, y, z) {
    x = x + 2；
    y = y + 2;
    z = z * z;
    return x + y + z;
}
// 柯里化
function sum(x) {
    x = x + 2;
    return function(y) {
        y = y * 2;
        return function(z) {
            z = z * z;
            return x + y + z;
        }
    }
}
```

函数的柯里化可以使逻辑充分的复用

```javascript
function makeAdder(count) {
    count = count * count;
    return function(num) {
        return count + num;
    }
}
var adder5 = makeAdder(5);
adder5(10);
adder5(14);
```

```javascript
function log(data, type, message) {
    console.log(`[${data.getHours()}:${date.getMinutes}][${type}]:[${message}]`);
}
log(new Date(), 'DEBUG', '轮播图出现BUG');
log(new Date(), 'DEBUG', '查询菜单出现BUG');
log(new Date(), 'DEBUG', '查询数据出现BUG');

// 柯里化
var log => data => type => message => {
    console.log(`[${data.getHours()}:${date.getMinutes}][${type}]:[${message}]`);
}
// 打印相同的时间
var nowLog = log(new Date());
nowLog('DEBUG')('轮播图出现BUG');
nowLog('FUTURE')('新增了添加用户的功能');

// 打印相同时间的DEBUG
var nowLogAndDebug = log(new Date())('Debug');
nowLogAndDebug('轮播图出现BUG');
nowLogAndDebug('查询菜单出现BUG');
```

自动柯里化函数的实现

```javascript
function hyCurrying(fn) {
    function curried(..args) {
        // 判断当前已经接收的参数的个数，可以参数本身需要接受的参数是否已经一致了
        // 1. 当已经传入的参数 大于等于 需要的参数时，就执行函数
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        } else {
           // 没有参数达到个数时，需要返回一个新的函数，继续接收参数
            function curried2(...args2) {
                return curried.apply(this, args2.concat(args2));
            }
            return curried2;
        }
    }
    return curried;
}

function add(x, y, z) {
    return x + y + z;
}
var curryAdd = hyCurrying(add1);
console.log(curryAdd(10, 20, 30));
console.log(curryAdd(10, 20)(30));
console.log(curryAdd(10)(20)(30));
```



## 组合函数

如果需要对某一个数据进行函数调用，需要依次执行两个函数fn1和fn2，那么每次都需要进行两个函数的调用操作，就会显得重复

将这两个函数组合起来，自动依次调用的过程就是对函数的组合，称之为组合函数

```javascript
function double(num) {
    return num * 2;
}
function square(num) {
    return num ** 2;
}
var count = 10;
var result = square(double(count));
console.log(result);

// 组合函数
function composeFn(m, n) {
    return function (count) {
        return n(m(count));
    }
}
var newFn = composeFn(double, square);
console.log(newFn(10))
```

通用组合函数

```javascript
// 通用组合函数
function hyCompose(...fns) {
    var length = fns.length;
    for (var i = 0; i < length; i++) {
        if (typeof fns[i] !== 'function') {
            throw new TypeError('Expected arguments are functions');
        }
    }
    function compose(...args) {
        var index = 0;
        var result = length ? fns[index].apply(this, args) : args;
        while (++i < length) {
            result = fns[index].call(this, result);
        }
        return result;
    }
    return compose;
}

function double(num) {
    return num * 2;
}
function square(num) {
    return num ** 2;
}
var newFn = hyCompose(double, square);
console.log(newFn(10));
```



## eval函数

将传入的字符串当做javaScript代码来运行

```javascript
var jsString = 'var message = "Hello World"; console.log(message);'
eval(jsString);
```

不建议在开发中使用：可读性差，有攻击风险，必须经过jS解释器，不能被JS引擎优化





# 对象

## 创建对象

### 字面量创建对象

```javascript
var star = {
	name : 'pink',
	age : 18,
	sex : '男',
	sayHi : function(){
		alert('大家好啊~');
	}
};
```



### new Object创建对象

```javascript
var andy = new Obect();
andy.name = 'pink';
andy.age = 18;
andy.sex = '男';
andy.sayHi = function(){
	alert('大家好啊~');
}
```



### 构造函数创建对象

1. 构造函数约定首字母大写

2. 函数内的属性和方法前面需要添加 this ，表示当前对象的属性和方法

3. 构造函数中不需要 return 返回结果

4. 当我们创建对象的时候，必须用 new 来调用构造函数

new关键字执行的操作：

1. 在内存中创建一个新的空对象
2. 这个对象内部的[[prototype]]属性会被赋值为该构造函数的prototype属性
3. 让 this 指向这个新的对象
4. 执行构造函数里面的代码，给这个新对象添加属性和方法
5. 返回这个新对象（所以构造函数里面不需要return）

```javascript
function Person(name, age, sex) {
	this.name = name;
	this.age = age;
	this.sex = sex;
	this.sayHi = function() {
		alert('我的名字叫：' + this.name + '，年龄：' + this.age + '，性别：' + this.sex);
	}
}
var bigbai = new Person('大白', 100, '男');
var smallbai = new Person('小白', 21, '男');
console.log(bigbai.name);
console.log(smallbai.name);
```



## 对象属性操作

### 获取/修改/删除对象属性

```javascript
var obj = {
    name: 'jack';
    age: 18;
}
// 获取属性
console.log(obj.name);
// 给属性赋值
obj.name = 'kobe';
// 删除属性
delete obj.name;
```



### 遍历对象属性

```javascript
for (var k in obj) {
    console.log(k); // 这里的 k 是属性名
	console.log(obj[k]); // 这里的 obj[k] 是属性值
}
```

`Object.keys(obj)`返回一个由属性名组成的数组



### 属性操作控制

#### 属性描述符

对一个属性进行比较精准的操作控制，可以使用**属性描述符**

属性描述符可以精准的添加或修改对象的属性

属性描述符类型：

1. **数据属性描述符**  `value`、`writable`、`enumerable`、`configurable`

2. **存取属性描述符** `enumerable`、`configurable`、`get`、`set`
   1. 隐藏某个私有属性，不希望直接被外界使用和赋值
   2. 希望截取某个属性的访问和设置值的过程

数据属性描述符和存取属性描述符不可以混着写

- `value`: 设置属性的值 

  默认为undefined

- `writable`: 值是否可以重写(修改)

  直接在一个对象上定义某个属性时，默认值为true

  通过属性描述符定义某个属性时，默认值为false

- `enumerable`: 目标属性是否可以被枚举(通过for-in或者Object.key()返回该属性)

  直接在一个对象上定义某个属性时，默认值为true

  通过属性描述符定义某个属性时，默认值为false

- `configurable`: 目标属性是否可以被delete删除，是否可以重新定义属性描述符，或者是否可以修改为存储属性描述符

  直接在一个对象上定义某个属性时，默认值为true

  通过属性描述符定义某个属性时，默认值为false

- `get`: 获取属性时会执行的函数

  默认为undefined

- `set`: 设置属性时会执行的函数

  默认为undefined



#### 定义属性描述符

属性描述符需要使用**Object.defineProperty**来对属性进行添加或者修改，可以直接在对象上定义新属性或修改原有属性，并返回此对象

`Object.defineProperty(obj, prop, descriptor)`

1. 参数 `obj`：必需。目标对象 

2. 参数 `prop`：必需。需定义或修改的属性的名称或Symbol

3. 参数 `descriptor`：必需。需定义或修改的属性描述符。以对象形式 { } 书写

`Object.defineProperties(obj, props)`

1. 参数 `obj`：必需。目标对象 
2. 参数 `props`：必需。需定义或修改的属性的名称和属性描述符的对象

- ##### 定义数据属性描述符

  ```javascript
  // 数据属性描述符
  var obj = {
      id: 1,
      pname: '小米',
      price: 1999
  };
  // 新添加的属性如果是不可枚举的，打印对象时是看不到的
  Object.defineProperty(obj, 'size', {
      value: 12,
      // enumerable: true
  });
  console.log(obj);		// { id: 1, pname: '小米', price: 1999 }
  console.log(obj.size);	// 12
  
  // 以前的对象添加和修改属性的方式
  Object.defineProperty(obj, 'num', {
      value: 1000,
      enumerable: true
  });
  
  Object.defineProperty(obj, 'id', {
      // 如果值为false 不允许修改这个属性值 默认值也是false
      writable: false,
  });
  
  // 数据属性描述符
  Object.defineProperty(obj, 'address', {
      value: '中国山东蓝翔技校xx单元',
      // 如果只为false 不允许修改这个属性值 默认值也是false
      writable: false,
      // enumerable 如果值为false 则不允许遍历, 默认的值是 false
      enumerable: false,
      // configurable 如果为false 则不允许删除这个属性 不允许在修改第三个参数里面的特性 默认为false
      configurable: false
  });
  ```

- ##### 定义存储属性描述符

  ```javascript
  // 存取属性描述符
  var obj = {
      id: 1,
      pname: '小米',
      _address: '北京市'
  };
  
  Object.defineProperty(obj, 'address', {
      enumerable: true,
      configurable: true,
      get: function() {
          console.log('获取了address的值');
          return this._address;
      },
      set: function(value) {
          console.log('设置了address的值');
          this._address = value;
      }
  });
  console.log(obj.address);
  obj.address = '上海市';
  ```

- ##### 定义多个属性描述符

  ```javascript
  var obj = {
      _age: 18
  };
  Object.definedProperties(obj, {
      name: {
          configurable: true,
          enumerable: true,
          writable: true,
          value: 'jack'
      },
      age: {
  		configurable: true,
          enumerable: true,
          get: function() {
              return this._age;
          },
          set: function(value) {
              this._age = value;
          }
      }
  })
  ```



#### 获取属性描述符

获取某个属性的属性描述符： `Object.getOwnPropertyDescriper(obj, prop)`

获取对象所有属性描述符： `Object.getOwnPropertyDescripers(obj)`

```javascript
var obj = { };
Object.defineProperty(obj, 'name', {
    value: 'jack',
    writable: true,
    enumerable: true,
    configurable: true
});
console.log(Object.getOwnPropertyDescriper(obj, 'name'));
// { value: 'jack', writable: true, enumerable: true, configurable: true }
console.log(Object.getOwnPropertyDescripers(obj));
```



### 对象限制方法

- 禁止对象继续添加新的属性：`Object.preventExtensions(obj)`

  ```javascript
  var obj = {
      name: 'jack',
      age: 18
  }
  Object.preventExtensions(obj);
  
  obj.height = 1.8;
  obj.address = '北京';
  console.log(obj);	// { name: 'jack', age: 18 }
  ```

- 禁止对象配置/删除属性：`Object.seal(obj)`

  ```java
  var obj = {
      name: 'jack',
      age: 18
  }
  Object.seal(obj);
  delete obj.name;
  console.log(obj.name);		// jack
  ```

- 使属性不可修改（writable: false）：`Object.freeze(obj)`

  ```javascript
  var obj = {
      name: 'jack',
      age: 18
  }
  Object.freeze(obj);
  obj.name = 'kobe';
  console.log(obj.name);		// jack
  ```

  




## 构造函数

构造函数主要用来初始化对象，为对象成员变量赋初始值。总与 new 一起使用

但是存在浪费内存的问题

- ##### 静态成员

  在构造函数本身上添加的成员称为静态成员，只能由构造函数本身来访问

  ```javascript
  function Star(uname, age) {
      .....
  }
  // 静态成员
  Star.sex = '男';
  ```

- ##### 实例成员

  在构造函数内部创建的对象成员称为实例成员，只能由实例化的对象来访问

  构造函数内部通过this添加的成员



## 原型对象

### 函数原型/显示原型

**所有的函数(构造函数)都有一个prototype对象**，称之为**显示原型**，这个对象的所有属性和方法，都会被函数(构造函数)所拥有

可以为prototype对象添加新的属性

- 直接添加属性

  ```javascript
  function foo() {}
  foo.prototype.name = 'jack';
  foo.prototype.age = 18;
  foo.proto.eating = function() {};
  var f = new foo();
  console.log(f.name, f.age);	// 'jack', 18
  ```

- 重写整个prototype对象

  ```javascript
  function foo() {}
  foo.prototype = {
      name: 'jack',
      age: 18,
      height: 1.8
  }
  // 一般通过Object.defineProperty方式添加constructor
  Object.defineProperty(foo.prototype, 'constructor', {
      enumerable: false,
  	configurable: true,
      writable: true,
      value: foo
  })
  var f = new foo();
  console.log(f.name, f.age, f1.height);	// 'jack', 18, 1.8
  ```

函数(构造函数)通过**原型分配的函数是所有对象所共享**的

- 可以把那些不变的公共方法直接定义在 prototype 对象上，公共属性定义到构造函数里面

- 原型对象函数里面的this指向的是 这个方法的调用者, 也就是实例对象

  ```javascript
  function Person(name, age, height, address) {
      this.name = name;
      this.age = age;
      this.height = height;
      this.address = address;
  }
  Person.prototype.eating = function() {
      console.log(this.name + 'is eating');
  }
  Person.prototype.running = function() {
      console.log(this.name + 'is running');
  }
  var p1 = new Person('jack', 18, 1.8, '北京');
  var p2 = new Person('tom', 18, 1.8, '上海');
  
  p1.eating();
  p2.eating();
  ```

- 可以通过原型对象，对原来的内置对象进行扩展自定义的方法。

  ```javascript
  // 数组增加自定义求和的功能
  Array.prototype.sum = function() {
      var sum = 0;
      for (var i = 0; i < this.length; i++) {
          sum += this[i];
      }
      return sum;
  };
  var arr = [1, 2, 3];
  console.log(arr.sum());
  ```

  数组和字符串内置对象不能给原型对象覆盖操作 `Array.prototype = {}` 

  只能是 `Array.prototype.xxx = function(){}` 的方式



###  对象原型/隐式原型

**每个对象（函数也是对象）都有一个特殊的内置属性[[prototype]] ，称为隐式原型**

**[[prototype]]属性是无法查看的**，但是浏览器给对象提供了一个属性**\_\_proto\_\_**

ES5也提供了获取对象原型的方法`Object.getPrototypeOf(obj)`

它是一个**非标准属性**，因此实际开发中，不可以使用这个属性

- ##### 隐式原型的作用：\_\_proto\_\_对象原型的意义就在于为对象的查找机制提供一个方向

  当从一个对象中获取某个属性时，它会触发[[get]]操作

  1. 从当前对象中去查找对应的属性，如果找到就直接使用
  2. 如果没有找到，就会沿着它的原型（原型链）去查找，也就是去 \_\_proto\_\_ 中去查找

  ```javascript
  var obj = { name: 'jack' };
  console.log(obj.name);	// undefined
  
  // 找不到去原型链中查找
  obj.__proto__.age = 18;
  console.log(obj.name);	// 18
  ```

- ##### 字面量直接创建的对象也有内置属性[[prototype]] 

- ##### 函数既有显示原型也有隐式原型

  ```javascript
  // 显示原型:
  // Foo是一个函数，在创建函数时，显示原型赋值Foo.prototype = { constructor: Foo }
  // 隐式原型:
  // Foo也是一个对象，function Foo() {} 其实就是 var Foo = new Function()
  // new Function()时，隐式原型赋值Foo.__proto__ = Function.prototype
  // Function.prototype = { constructor: Function }
  function Foo() { }
  console.log(Foo.prototype === Foo.__proto__);	// false
  console.log(Foo.prototype.constructor);			// [Function: Foo]
  console.log(Foo.__proto__.constructor);			// [Function: Function]
  ```

  <img src="JavaScript.assets/image-20220430190521371.png" alt="image-20220430190521371" style="zoom:67%;" /> 

- ##### 对象可以使用函数原型对象prototype的属性和方法，就是因为对象有\_\_proto\_\_ 原型的存在

  **new关键字创建对象**的时候，会在内存中创建一个新的对象，这个**对象内部的\_\_proto\_\_属性会被赋值为该构造函数的prototype属性**（**显示原型赋值给隐式原型**）

  也就是，函数对象的\_\_proto\_\_指向函数原型对象prototype

  ```javascript
  function foo() { }
  var fn = new foo();
  console.log(fn.__proto__ === foo.prototype);	// true
  ```

   <img src="JavaScript.assets/image-20220429222454049.png" alt="image-20220429222454049" style="zoom:35%;" /> 

- 对象原型对象里面的 \_\_proto\_\_ 原型指向的是 Object.prototype，Object.prototype原型对象里面的 \_\_proto\_\_ 原型  指向为 null



### 构造函数 constructor

对象原型\_\_proto\_\_和函数原型prototype里面都有一个 constructor 属性 

constructor 称为构造函数，因为它指回构造函数本身

- constructor 主要用于记录该对象引用于哪个构造函数，可以让原型对象重新指向原来的构造函数

- 很多情况下,需要手动的利用constructor 这个属性指回 原来的构造函数

  ```javascript
  function Star(uname, age) {
      this.uname = uname;
      this.age = age;
  }
  Star.prototype = {
  	// 如果修改了原来的原型对象,给原型对象赋值的是一个对象,则必须手动的利用constructor指回原来的构造函数
      constructor: Star,
      sing: function() {
          console.log('我会唱歌');
      },
      movie: function() {
          console.log('我会演电影');
      }
  }
  ```

- ##### 构造函数、实例、原型对象三者之间的关系

  <img src="JavaScript.assets/image-20211217143313681.png" alt="image-20211217143313681" style="zoom:67%;" /> 



### 原型链

对象的查找机制：

\_\_proto\_\_对象原型的意义就在于为对象成员查找机制提供一个方向

1. 当访问一个对象的属性（包括方法）时，首先查找这个对象自身有没有该属性
2. 如果没有就查找它的原型（也就是 \_\_proto\_\_，指向的 prototype 原型对象）
3. 如果还没有就查找原型对象的原型（Object的原型对象）
4. 依此类推一直找到 Object 为止（null）

<img src="JavaScript.assets/image-20211217144344144.png" alt="image-20211217144344144" style="zoom: 67%;" /> 

**原型链最顶层的原型对象就是Object的原型对象**

- 从Object直接创建出来的对象的原型都是[Object: null prototype] {}
- 该原型不再继续有原型属性了，也就是顶层原型，该对象上有很多默认的属性和方法



### 原型检测方法

#### Object.create()

创建一个新对象，**使用现有的对象来提供新创建的对象的\_\_proto\_\_**

`Object.create(proto，[propertiesObject])`

- 参数1：原型对象
- 参数2：可选。为新创建的对象添加指定的属性值和属性描述符

```javascript
var obj = {
    name: 'jack',
    age: 18
};
var info = Object.create(obj, {
    address: {
        value: '北京',
        enumerable: true
    }
});
console.log(info);				// { address: '北京' }
console.log(info.__proto__);	// { name: 'jack', age: 18 }
```



#### hasOwnProperty()

判断对象自身属性中是否具有指定的属性（不是原型上的属性）

`obj.hasOwnProperty(prop)`

```javascript
var obj = {
    name: 'jack',
    age: 18
};
var info = Object.create(obj, {
    address: {
        value: '北京',
        enumerable: true
    }
});
// 当前对象存在才返回true
console.log(info.hasOwnProperty('address'));	// true
console.log(info.hasOwnProperty('name'));		// false
```



#### in 操作符

判断某个属性是否在某个对象或者对象原型上

```javascript
var obj = {
    name: 'jack',
    age: 18
};
var info = Object.create(obj, {
    address: {
        value: '北京',
        enumerable: true
    }
});
// in操作符：不管在当前对象还是原型中存在，都返回true
console.log('address' in info);					// true
console.log('name' in info);					// true
```



#### instanceof

用于**检测构造函数的prototype**，是否出现在**某个实例对象的原型链上**

```javascript
function Person() {}
function Student() {}
// Student 继承 Person
inheritPrototype(Student, Person);
var stu = new Student();

// 判断Student.prototype是否出现在stu的原型链中，如果找到就是true
console.log(stu instanceof Student);	// true
console.log(stu instanceof Person);		// true
console.log(stu instanceof Object);		// true

function inheritPrototype(SubType, SuperType) {
    SubType.prototype = Object.create(SuperType.prototype);
    Object.defineProperty(SubType.prototype, 'constructor', {
        enumerable: false,
        configurable: true,
        writable: true,
        value: SubType
    });
}
```



#### isPrototypeOf

用于**检测某个对象**，是否出现在**某个实例对象的原型链**上

```javascript
function Person() {}
var p = new Person();
console.log(p instanceof Person);				// true
console.log(Person.prototype.isPrototypeOf(p));	// true
```

```javascript
var obj = {
    name: 'jack',
    age: 18
};
var info = Object.create(obj);
// instanceof 只能检测构造函数
console.log(obj.isPrototypeOf(info))
```



## 继承

通过构造函数+原型对象模拟实现继承，被称为组合继承

### 构造函数继承父类属性

通过 call() 把父类型的 this 指向子类型的 this ，这样就可以实现子类型继承父类型的属性

```javascript
// 父类
function Person(name, age, sex) {
  // this 指向父构造函数的对象实例
  this.name = name;
  this.age = age;
  this.sex = sex;
}
// 子类
function Student(name, age, sex, score) {
  // this 指向子构造函数的对象实例
  Person.call(this, name, age, sex);  // 此时父类的 this 指向子类的 this，同时调用这个函数
  this.score = score;
}
var s1 = new Student('zs', 18, '男', 100);
console.dir(s1); 
```



### 原型对象继承父类方法

一般对象的方法都在构造函数的原型对象中设置，通过构造函数无法继承父类方法

#### 组合继承

使用组合继承

1. 将子类所共享的方法提取出来，让**子类的 prototype 原型对象 = new 父类()** 

   子类原型对象等于是实例化父类，因为父类实例化之后另外开辟空间，就不会影响原来父类原型对象

2. 将子类的 constructor 重新指向子类的构造函数

组合继承存在的问题

1. 无论在什么情况下，**都会调用两次父类构造函数**

   一次在创建子类原型的时候，另一次在创建子类实例的时候

2. 所有的子类实例上事实上会**拥有两份父类的属性**

   一份在当前的实例里面，另一份在子类对应的原型对象中（son. \_\_proto\_\_里面）

```javascript
// 父构造函数
function Father(uname, age) {
    // this 指向父构造函数的对象实例
    this.uname = uname;
    this.age = age;
}
Father.prototype.money = function() {
	console.log(100000);
};
// 子构造函数
function Son(uname, age, score) {
	// this 指向子构造函数的对象实例
    Father.call(this, uname, age);
    this.score = score;
}
// Son.prototype = Father.prototype;  这样直接赋值会有问题,如果修改了子原型对象,父原型对象也会跟着一起变化
Son.prototype = new Father();
// 利用constructor 指回原来的构造函数
Son.prototype.constructor = Son;
// 子构造函数专门的方法
Son.prototype.exam = function() {
    console.log('考试');
}
var son = new Son('刘', 18, 100);
```



#### 寄生组合继承

创建一个封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再将这个对象返回

```javascript
function Person(name, age, friends) {
    this.name = name;
    this.age = age;
    this.friends = friends;
}
Person.prototype.running = function() {
    console.log('runnning');
}
Person.prototype.eating = function() {
    console.log('eating');
}

function Student(name, age, friends, sno, scroe) {
    Person.call(this, name, age, friends);
    this.sno = sno;
    this.score = score;
}
// 使用工具函数，继承父类原型
inheritPrototype(Student, Person);
Student.prototype.studying = function() {
    console.log('studying');
}
var stu = new Student('jack', 18, ['kobe'], 111, 100);

// 工具函数：原型式继承
function inheritPrototype(SubType, SuperType) {
    // 最新ECMA提供的方法，使用现有的对象来提供新创建的对象的__proto__
    SubType.prototype = Object.create(SuperType.prototype);
    // 旧原型式继承方法
    // SubType.prototype = createObject1(SuperType);
    // SubType.prototype = createObject2(SuperType);
    Object.defineProperty(SubType.prototype, 'constructor', {
        enumerable: false,
        configurable: true,
        writable: true,
        value: SubType
    });
}
// 旧原型式继承
function createObject1(obj) {
    function Func() {};
    Func.prototype = obj;
    return new Func();
}
function createObject2(obj) {
    var newObj = {};
    Object.setPrototypeOf(newObj, obj);
    return newObjl
}
```

<img src="JavaScript.assets/image-20220430173902076.png" alt="image-20220430173902076" style="zoom:67%;" /> 



## 类

### 创建类

在 ES6 中新增加了类的概念，可以使用 class 关键字声明一个类，之后以这个类来实例化对象

类其实就是语法糖

- class本质还是function

- **类的所有方法都定义在类的prototype属性上**，里面有constructor指向类本身

  类可以通过原型对象添加方法

- 类创建的实例,里面也有\_\_proto\_\_指向类的prototype原型对象

类必须使用 new 实例化对象

类声明

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    eating() {
        console.log(this.name + ' eating');
    }
    running() {
        console.log(this.name + ' running');
    }
}
```

类表达式

```javascript
var Animal = class {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    eating() {
        console.log(this.name + ' eating');
    }
    running() {
        console.log(this.name + ' running');
    }
}
```

类的访问器定义

```javascript
class Person {
    constructor(address) {
        this._address = '北京';
    }
    get address() {
        return this._address;
    }
    set address(newAddress) {
        this._address = newAddress;
    }
}
```

类的静态方法

```JavaScript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    // 实例方法：通过创建出来的对象进行访问
    eating() {
        console.log(this.name + ' eating');
    }
    // 静态方法：直接通过类调用
    static randomPerson() {
        var names = ['jack', 'tom', 'jerry', 'lucy'];
        var nameIndex = Math.floor(Math.random() * names.length);
        var name = names[nameIndex];
        var age = Math.floor(Math.random() * 100);
        return new Person(name, age);
    }
}
var p = Person.randomPerson();
```



### 构造函数

constructor() 方法是类的构造函数，用于传递参数，返回实例对象

如果没有显示定义, 类内部会自动给我们创建一个constructor()，一个类只能有一个构造函数

通过 new 命令生成对象实例时，自动调用该方法

构造函数执行过程

1. 在内存中创建一个对象
2. 将类的原型prototype赋值给创建出来的对象的\_\_proto\_\_
3. 构造函数内部的this，指向创建出来的新对象
4. 执行函数体中的代码
5. 自动返回创建出来的对象

```javascript
class Person {
  constructor(name,age) {   // constructor 构造方法或者构造函数
      this.name = name;
      this.age = age;
    }
}
var p = new Person('name', 18); 
```

- 不存在变量提升，必须先定义类，才能实例化

- 类里面的公有属性和方法一定要加this使用

  constructor里面的this执行的是创建的实例对象，方法里面的this还是指向方法的调用者

  如果方法想用创建的实例对象，可以定义全局变量，把constructor里面的this赋值给这个全局变量



### 继承

子类可以继承父类的一些属性和方法。

```javascript
// 父类
class Father {
	constructor(surname) {
        this.surname= surname;
	}
	say() {
		console.log('你的姓是' + this.surname);
	}
}
// 子类继承父类
class Son extends Father {
}
var s = new Son('刘');
s.say();
```

**super 关键字**用于访问和调用对象父类上的函数。可以**调用父类的构造函数**，也可以**调用父类的普通函数**

子类在构造函数中使用super, 必须放到 this 前面 (必须**先调用父类的构造方法,再使用子类构造方法**)

```javascript
class Father {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    sum() {
        console.log(this.x + this.y);
    }
    multi() {
        console.log(this.x * this.y);
    }
    print() {
        console.log(this.x);
        console.log(this.y);
    }
    static staticMethod() {
        console.log('FatherStaticMedthod');
    }
}
// 子类继承父类加法方法 同时 扩展减法方法
class Son extends Father {
    constructor(x, y, z) {
        // 利用super 调用父类的构造函数
        // super 必须在子类this之前调用
        super(x, y);
        this.z = z;
    }
    subtract() {
        console.log(this.x - this.y);
    }
    // 子类对父类的方法的重写
    multi() {
        console.log(this.x * this.y * this.z);
    }
    // 子类对父类的方法的重写
    print() {
        // 调用父类方法
        super.print();
        console.log(this.z);
    }
    // 重写静态方法
    static staticMethod() {
        // 调用父类静态方法
        super.staticMethod();
        console.log('SonStaticMedthod');
    }
}
var son = new Son(5, 3);
son.subtract();
son.sum();
```

继承中的属性或者方法查找原则: 就近原则

- 输出一个方法,先看子类有没有这个方法,如果有就先执行子类的
- 如果子类里面没有,就去查找父类有没有这个方法,如果有,就执行父类的这个方法(就近原则)



### 继承内置类

```javascript
class MyArray extends Array {
    firstItem() {
        return this[0];
    }
    lastItem() {
        return this[this.lenght - 1];
    }
}
var arr = new MyArray(1, 2, 3);
console.log(arr.firstItem());
console.log(arr.lastItem());
```



### 类的混入

javascript的类只支持单继承，当需要在一个类中添加更多相似的功能是，可以使用混入mixin

```javascript
class Person { }
// js中只可以单继承
class Student extends Person { }

function mixinRunner(BaseClass) {
    return class extents BaseClass {
        running() {
            console.log('running');
        }
    };
}
function mixinEater(BaseClass) {
    return class extents BaseClass {
        eating() {
            console.log('eating');
        }
    };
}

var NewStudent = mixinEater(mixinRunner(Student));
var ns = new NewStudent();
ns.running();
ns.eating();
```





# 内置对象

## 数学对象Math

Math 对象不是构造函数，具有数学常数和函数的属性和方法。直接使用里面的属性和方法即可

- ##### 圆周率 `Math.PI `

- ##### 向下取整 `Math.floor() `

  往小了取值

- ##### 向上取整 `Math.ceil() ` 

  往大了取值

- ##### 四舍五入 `Math.round()`

   注意 -3.5 结果是 -3 。

  .5特殊，往大了取值

- ##### 绝对值`Math.abs()`

- ##### 求最大和最小值`Math.max()` `Math.min()`

- ##### 随机数`random()`

  取值范围0 <= x < 1

  两个数之间的随机整数，并且包含这两个整数
  
  `Math.floor(Math.random() * (max - min + 1)) + min;`



## 日期对象Date

Date是一个构造函数，需要实例化后才能使用

- Date()不写参数，就返回当前时间

- Date()里面写参数，就返回括号里面输入的时间

- Date 对象是基于1970年1月1日（世界标准时间）起的毫秒数

  获取毫秒数`var now = + new Date();` `Date.now();`



## 数组对象Array

- ##### 检测是否为数组

  `Array.isArray();`(ie9以上版本) 或`xx instanceof Array`

- ##### 末尾添加一个或多个元素 `push(参数1..)`

  修改原数组，返回新的长度

- ##### 删除最后一个元素 `pop()`

  修改原数组，返回删除的元素的值

- ##### 开头添加一个或多个元素 `unshift(参数1..)` 

  修改原数组，返回新的长度

- ##### 删除第一个元素 `shift()` 

  修改原数组，返回第一个元素的值

- ##### 颠倒元素的顺序 `reverse()`

  修改原数组，返回新数组

- ##### 元素进行排序 `sort()` 

  修改原数组，返回新数组

- ##### 查找元素的第一个索引 `indexOf(数组元素)` 

  如果存在返回索引号，不存在返回-1 只返回第一个满足条件的索引号

- ##### 查找元素的最后一个索引 `lastIndexOf(数组元素)` 

  如果存在返回索引号，不存在返回-1

- ##### 数组转换字符串，逗号分隔 `toString()` 

  返回字符串

- ##### 把数组中的所有元素转换为一个字符串 `join('分隔符')` 

  返回字符串

- ##### 连接两个或多个数组 `concat()` 

  不影响原数组，返回新数组

- ##### 数组截取 `slice(begin,end)`

  返回被截取项目的新数组

- ##### 数组删除 `splice(第几个开始，要删除个数)` 

  修改原数组，返回被删除项目的新数组

- ##### 遍历数组`forEach()`

  `array.forEach(function(currentValue, index, arr))`

  `currentValue`：数组当前项的值

  `index`：数组当前项的索引

  `arr`：数组对象本身

- ##### 筛选数组 `filter()`

  `array.filter(function(currentValue, index, arr))`

  返回一个新的数组，把所有满足条件的元素返回回来

- ##### 数组元素是否满足指定条件`some()`

  `array.some(function(currentValue, index, arr))`

  返回值是布尔值

  如果查找到第一个满足条件的元素就终止循环

- ##### 数组元素是否全部满足指定条件`every()`

  `array.every(function(currentValue, index, arr))`

  返回值是布尔值

  如果检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测

- ##### 返回新数组 `map() `

  `array.map(function(currentValue, index, arr))`

  返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值

- ##### 伪数组转换为真正数组`from()`

  `Array.from(arrayLike[, mapFn[, thisArg]])`

  对一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例

  ```javascript
  let arrayLike = {
      '0': 'a',
      '1': 'b',
      '2': 'c',
      length: 3
  }; 
  let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
  ```

  第二个参数作用类似于数组的`map`方法

  `let newAry = Array.from(aryLike, item => item *2)`

- ##### 返回第一个满足指定条件的数组成员`find()`

  `array.find(function(currentValue, index, arr))`

  没有找到返回undefined

- ##### 回第一个满足指定条件的数组成员位置`findIndex()`

  `array.findIndex(function(currentValue, index, arr))`

  没有找到返回-1

- ##### 是否包含一个指定的值`includes()`

  `arr.includes(searchElement)`

  `arr.includes(searchElement, fromIndex)`

  返回值是布尔值
  
  ```javascript
  const names = ["abc", "cba", "nba", "mba", NaN];
  if (names.includes("cba", 2)) {
    console.log("包含abc元素")
  }
  // indexOf是不能检测是否含有NaN
  if (names.indexOf(NaN) !== -1) {
    console.log("包含NaN")
  }
  // includes可以检测NaN
  if (names.includes(NaN)) {
    console.log("包含NaN")
  }
  ```

- ##### flat() 数组降维

  按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回（ES10）

  ```javascript
  const nums = [10, 20, [2, 9], [[30, 40], [10, 45]], 78, [55, 88]]
  // 数组降一维
  const newNums1 = nums.flat()
  console.log(newNums1)			//  [10, 20, 2, 9, [30, 40], [10, 45], 78, 55, 88]
  // 数组降二维
  const newNums2 = nums.flat(2)
  console.log(newNums2)			// [10, 20, 2, 9, 30, 40, 10, 45, 78, 55, 88]
  ```

- ##### flatMap() 首先使用映射函数映射每个元素，然后将结果压缩成一个新数组

  flatMap是先进行map操作，再做flat的操作（ES10）

  flatMap中的flat相当于深度为1（flat(1)）

  ```javascript
  const messages = ["Hello World", "hello lyh", "my name is coderwhy"]
  const words = messages.flatMap(item => {
    // 对返回结果再进行一次降维 [['Hello', 'World'], ['hello', 'lyh'], ['my', 'name', 'is', 'coderwhy']]
    return item.split(" ")
  })
  console.log(words)	//  ['Hello', 'World', 'hello', 'lyh', 'my', 'name', 'is', 'coderwhy']
  ```



## 字符串对象

- ##### 基本包装类型

  基本包装类型就是把简单数据类型包装成为复杂数据类型，这样基本数据类型就有了属性和方法

  提供了三个特殊的引用类型：String、Number和 Boolean

  ```javascript
  var str = 'andy';
  console.log(str.length);
  
  // 包装过程
  // 1. 生成临时变量，把简单类型包装为复杂数据类型
  var temp = new String('andy');
  // 2. 赋值给我们声明的字符变量
  str = temp;
  // 3. 销毁临时变量
  temp = null;
  ```

- ##### 字符串的不可变

  里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内存空间

- ##### 指定内容的索引`indexOf('要查找的字符',开始的位置)`

  如果存在返回索引号，不存在返回-1

- ##### 指定内容的最后索引`lastIndexOf('要查找的字符')` 

  如果存在返回索引号，不存在返回-1

- ##### 返回指定位置的字符`charAt(index)` 

  `str[index]` H5新增

- ##### 获取指定位置处字符的ASCII码`charCodeAt(index)`

- ##### 连接两个或多个字符串`concat()`

- ##### 截取字符串

  `substr('截取的起始位置','截取几个字符')`

  `slice(start, end)` 从start位置开始，截取到end位置，end取不到 （start和end都是索引）

  `substring(start, end)`从start位置开始，截取到end位置，end取不到 (start和end都是索引）不接受负值

- ##### 替换字符`replace(被替换的字符串/正则表达式 ， 要替换为的字符串) `

  只会替换第一个字符（正则表达式可以通过修饰符来修改匹配机制）

  返回值是一个替换完毕的新字符串

- ##### 切分字符串`split('分隔符')`

- ##### 删除两端空白字符 `trim()`

  不影响原字符串本身，返回新字符串	
  
- ##### 单独去除前面或者后面空白字符 trimStart / trimEnd（ES10）

  ```javascript
  const message = "    Hello World    ";
  console.log(message.trim());			// Hello World
  console.log(message.trimStart());		// Hello World    
  console.log(message.trimEnd());			//     Hello World
  ```
  
- ##### 模板字符串

  使用字符串模板来嵌入JS的变量或者表达式来进行拼接（ES6新增）

  ```javascript
  let name = '张三';
  let age = 18;
  function nextAge() {
      return age + 1;
  }
  
  let sayHello = `hello,my name is ${name}`;
  let ageDouble = `double age is ${age * 2}`;
  let ageNext = `next year age is ${nextAge()}`;
  ```

  - 模板字符串中可以换行

- ##### 是否以指定的子字符串开始/结束 `startsWith()`/`endsWith()`

  `startsWith()`：表示参数字符串是否在原字符串的头部，返回布尔值

  `endsWith()`：表示参数字符串是否在原字符串的尾部，返回布尔值

  大小写敏感

- ##### 重复字符串 `repeat()`

  `string.repeat(count)`

  返回新字符串
  
- ##### 字符串前后填充 padStart()/padEnd()

  `padStart(targetLength [, padString])`

  `padEnd(targetLength [, padString])`

  1. targetLength  字符串需要填充到的目标长度
  2. padString 填充字符串 默认" "

  ```javascript
  const message = "Hello World"
  const newMessage = message.padStart(15, "*").padEnd(20, "-")
  console.log(newMessage)				// ****Hello World-----
  ```

  ```javascript
  const cardNumber = "321324234242342342341312"
  const lastFourCard = cardNumber.slice(-4)
  const finalCard = lastFourCard.padStart(cardNumber.length, "*")
  console.log(finalCard)				// ********************1312
  ```

 

## 正则对象RegExp

- ##### 创建正则表达式

  `var 变量名 = new RegExp(/表达式/); `

  或通过字面量创建 `var 变量名 = /表达式/;`

- ##### 测试正则表达式 `test()`

  `regexObj.test(str) `

  - `regexObj `正则表达式
  - `str` 测试文本



## 唯一值集合Set

`Set `是唯一值的集合。不能提供索引。

- ##### 创建`Set`数据结构

  `const s = new Set();`

  可以接受一个可遍历数据作为参数，用来初始化

  `const set = new Set([1, 2, 3, 4, 4]);`

- ##### 元素个数 `set.size`

- ##### 添加元素 `set.add(value)`

  如果已有重复，则不产生效果

  返回集合本身

- ##### 删除元素 `set.delete(value)`

  如果并不存在，则不产生效果

  返回集合本身

- ##### 是否含有指定元素 `set.has(value)`

  返回布尔值

- ##### 清空集合`set.clear()`

  没有返回值

- ##### 遍历 `forEach`

  `set.forEach(callback)`

- ##### 数组去重

  ```javascript
  function do(array) {
    return Array.from(new Set(array));
  }
  do([1, 1, 2, 3]) // [1, 2, 3]
  ```

  ```javascript
  let arr = [3, 5, 2, 2, 5, 5];
  let unique = [...new Set(arr)]; // [3, 5, 2]
  ```



## Object对象

- ##### 获取所有的value值 Object.values（ES8）

  ```javascript
  const obj = {
    name: "jack",
    age: 18
  }
  console.log(Object.keys(obj));		// ['name', 'age']
  console.log(Object.values(obj));	// ['jack', 18]
  ```

  ```javascript
  // 传入一个数组，会返回这个数组
  console.log(Object.values(["abc", "cba", "nba"]));	// ['abc', 'cba', 'nba']
  // 传入一个字符串，会返回字符串拆分后的数组
  console.log(Object.values("abc"));					// ['a', 'b', 'c']
  ```

- ##### Object.entries 获取到一个数组，数组中会存放可枚举属性的键值对数组（ES8）

  ```javascript
  const obj = {
    name: "jack",
    age: 18
  }
  console.log(Object.entries(obj));	// [['name': 'jack'], ['age': 18]]
  const objEntries = Object.entries(obj);
  objEntries.forEach(item => {
    console.log(item[0], item[1]);
  })
  // name jack
  // age 18
  ```

  ```javascript
  // 传入一个数组，会返回数组下标和值组成的数组
  console.log(Object.entries(["abc", "cba", "nba"]));	// [['0': 'abc'], ['1': 'cba'], ['2': 'nba']]
  // 传入一个字符串，会返回字符串拆分后的下标和值组成的数组
  console.log(Object.entries("abc"));					// [['0': 'a'], ['1': 'b'], ['2': 'c']]
  ```

- ##### Object.formEntries 将键值对数组转换成对象

  ```javascript
  const obj = {
    name: "why",
    age: 18,
    height: 1.88
  }
  const entries = Object.entries(obj)
  console.log(entries)						//  [['name', 'why'], ['age', 18], ['height', 1.88]]
  const newObj = Object.fromEntries(entries)
  console.log(newObj)							// { "name": "why", "age": 18, "height": 1.88 }
  ```

  ```javascript
  // 将查询字符串转换为对象
  const queryString = 'name=why&age=18&height=1.88'
  const queryParams = new URLSearchParams(queryString)
  for (const param of queryParams) {
    console.log(param)
  }
  // ['name', 'why']
  // ['age', '18']
  // ['height', '1.88']
  const paramObj = Object.fromEntries(queryParams)
  console.log(paramObj)			// {name: 'why', age: '18', height: '1.88'}
  ```



## JSON对象

- ##### JSON基本语法

  JSON的顶层支持三种类型的值

  1. 简单值：数字（Number）、字符串（String，不支持单引号）、布尔类型（Boolean）、null类型

  2. 对象值：由key、value组成，key是字符串类型，并且必须添加双引号，值可以是简单值、对象值、数组值

     ```json
     {
       "name": "why",
       "age": 18,
       "friend": {
         "name": "kobe"
       },
       "hobbies": ["篮球", "足球"]
     }
     ```

  3. 数组值：数组的值可以是简单值、对象值、数组值

     ```json
     [
       "abc",
       123,
       {
         "name": "kobe"
       }
     ]
     ```

- ##### JSON.stringify() 将JavaScript类型转成对应的JSON字符串

  `JSON.stringify(value[, replacer [, space]])`

  ```javascript
  const obj = {
    name: "why",
    age: 18,
    friends: {
      name: "kobe"
    },
    hobbies: ["篮球", "足球"]
  }
  const jsonString1 = JSON.stringify(obj)
  console.log(jsonString1);
  // {"name":"why","age":18,"friends":{"name":"kobe"},"hobbies":["篮球","足球"]}
  ```

  指定的replacer是数组，则可选择性地仅包含数组指定的属性

  ```javascript
  const obj = {
    name: "why",
    age: 18,
    friends: {
      name: "kobe"
    },
    hobbies: ["篮球", "足球"]
  }
  const jsonString2 = JSON.stringify(obj, ["name", "friends"])
  console.log(jsonString2)
  // {"name":"why","friends":{"name":"kobe"}}
  ```

  指定的replacer是回调函数

  ```javascript
  const obj = {
    name: "why",
    age: 18,
    friends: {
      name: "kobe"
    },
    hobbies: ["篮球", "足球"]
  }
  const jsonString3 = JSON.stringify(obj, (key, value) => {
    if (key === "age") {
      return value + 1
    }
    return value
  })
  console.log(jsonString3)
  // {"name":"why","age":19,"friends":{"name":"kobe"},"hobbies":["篮球","足球"]}
  ```

  指定缩进用的空白字符串，如果参数是个数字，代表有多少的空格，为字符串，将字符串作为空格

  ```javascript
  const obj = {
    name: "why",
    age: 18,
    friends: {
      name: "kobe"
    },
    hobbies: ["篮球", "足球"]
  }
  const jsonString4 = JSON.stringify(obj, null, 2)
  console.log(jsonString4)
  // {
  //   "name": "why",
  //   "age": 18,
  //   "friends": {
  //     "name": "kobe"
  //   },
  //   "hobbies": [
  //     "篮球",
  //     "足球"
  //   ]
  // }
  ```

  如果对象本身包含toJSON方法，那么会直接使用toJSON方法的结果

  ```javascript
  const obj = {
    name: "why",
    age: 18,
    friends: {
      name: "kobe"
    },
    hobbies: ["篮球", "足球"],
    toJSON: function() {
      return "123456"
    }
  }
  const jsonString1 = JSON.stringify(obj)
  console.log(jsonString1)
  // "123456"
  ```

- ##### JSON.parse() 解析JSON字符串，构造由字符串描述的JavaScript值或对象

  `JSON.parse(text[, reviver])`

  ```javascript
  const JSONString = '{"name":"why","age":19,"friends":{"name":"kobe"},"hobbies":["篮球","足球"]}'
  const info = JSON.parse(JSONString)
  console.log(info)
  // {name: 'why', age: 19, friends: {…}, hobbies: Array(2)}
  ```

  指定reviver函数，在返回之前对所得到的对象执行变换

  ```javascript
  const JSONString = '{"name":"why","time":"2000-01-01"}'
  const info = JSON.parse(JSONString, (key, value) => {
      if(key === 'time') {
          return new Date(value)
      }
      return value
  })
  console.log(info)
  // {name: 'why', time: Sat Jan 01 2000 09:00:00}
  ```

- ##### 使用JSON序列化深拷贝

  但是这种方法它对函数是无能为力的，stringify并不会对函数进行处理

  ```javascript
  const obj = {
    name: "why",
    age: 18,
    friends: {
      name: "kobe"
    },
    hobbies: ["篮球", "足球"],
    foo: function() {
      console.log("foo function")
    }
  }
  const jsonString = JSON.stringify(obj)
  const info3 = JSON.parse(jsonString)
  obj.friends.name = "curry"
  console.log(info3)
  // {age: 18, friends: {name: 'kobe'}, hobbies: ['篮球', '足球'], name: "why"}
  ```



# 异常处理

## throw关键字

throw语句用于抛出一个用户自定义的异常，当遇到throw语句时，当前的函数执行会被停止（**throw后面的语句不会执行**）

throw关键字可以接基本数据类型（比如number、string、Boolean），或者对象类型

```javascript
function sum(num1, num2) {
  if (typeof num1 !== "number" || typeof num2 !== "number") {
    throw "parameters is error type~";
  }
  return num1 + num2;
}
sum({ name: "why" }, true);
console.log("后续的代码~");
// Uncaught parameters is error type~
```

JavaScript已经给提供了一个**Error类**，可以直接创建这个类的对象

Error包含三个属性

1. messsage：创建Error对象时传入的message
2. name：Error的名称，通常和类的名称一致
3. stack：整个Error的错误信息，包括函数的调用栈，当我们直接打印Error对象时，打印的就是stack

```javascript
function foo(type) {
  console.log("foo函数开始执行");
  if (type === 0) {
    const err = new Error("type不能为0");
    console.log(err.message);
    console.log(err.name);
    console.log(err.stack);
    throw err;
    // 强调: 如果函数中已经抛出了异常, 那么后续的代码都不会继续执行了
    console.log("foo函数后续的代码");
  }
  console.log("foo函数结束执行");
}
foo(0);
// foo函数开始执行
// type不能为0
// Error
// Error: type不能为0
//     at foo (<anonymous>:4:17)
//     at <anonymous>:14:1
```

Error有自己的子类

- RangeError：下标值越界时使用的错误类型
- SyntaxError：解析语法错误时使用的错误类型
- TypeError：出现类型错误时，使用的错误类型

```javascript
function foo(type) {
  if (type === 0) {
    throw new TypeError("当前type类型是错误的");
  }
  console.log("foo函数结束执行");
}
foo(0);
// Uncaught TypeError: 当前type类型是错误的
```

在调用一个函数时，这个函数抛出了异常，但是没有对这个异常进行处理，那么这个**异常会继续传递到上一个函数调用中**

如果到了**最顶层（全局）的代码中依然没有对这个异常的处理代码**，这个时候就会报错并且**终止程序的运行**

```javascript
function foo() {
    // 抛出异常
    throw new Error('error message');
}
function bar() {
    foo();	// 异常会被继续传递到bar函数
    console.log('bar后续代码');
}
function test() {
    bar();	// 异常会被继续传递到test函数
    console.log('test后续代码');
}
test();		// 异常会被继续传递到全局代码
console.log('后续代码');
// Uncaught Error: error message
```



## 异常捕获

当出现异常时，不希望程序直接退出，而是可以正确的处理异常，就可以使用try catch

```javascript
function foo() {
    throw new Error('error message');
}
function bar() {
    try {
        foo();
		console.log('bar后续代码');
    } catch(err) {
        console.log("err:", err.message)
    }
}
function test() {
   // try {
        bar();
		console.log('test后续代码');
   // } catch(err) {
   //     console.log("err:", err.message)
   // }
}

// try {
	test();
	console.log('后续代码');
// } catch(err) {
//     console.log("err:", err.message)
// }

// err: error message
// test后续代码
// 后续代码
```

在ES10中，catch后面绑定的error可以省略

finally表示最终一定会被执行的代码结构，如果try和finally中都有返回值，那么会使用finally当中的返回值



# 代理和反射

## 代理类Proxy

Proxy类用于帮助我们创建一个代理（ES6）

- 如果希望**监听一个对象的相关操作**，那么可以**先创建一个代理对象**（Proxy对象）
- 之后对该对象的所有操作，都**通过代理对象来完成**，代理对象可以监听想要对原对象进行哪些操作

**Object.defineProperty监听的缺点**

1. Object.defineProperty设计的初衷不是为了监听对象中的属性

   定义属性的时候，初衷是定义普通的数据属性，但是为了监听强行把属性变成访问/存储属性

2. 新增属性、删除属性等监听无能为力

3. 复杂对象需要递归监听

```javascript
const obj = {
  name: "why",
  age: 18
}
Object.keys(obj).forEach(key => {
  let value = obj[key];
  Object.defineProperty(obj, key, {
    get: function() {
      console.log(`监听到obj对象的${key}属性被访问了`);
      return value;
    },
    set: function(newValue) {
      console.log(`监听到obj对象的${key}属性被设置值`);
      value = newValue;
    }
  })
})
obj.name = "kobe";
obj.age = 30;
```

**new Proxy**创建对象，传入需要侦听的对象和一个捕获器对象

`const p = new Proxy(target, handler)`

之后的操作都是直接对Proxy的操作，而不是原有的对象，在捕获器对象里面进行侦听

```javascript
const obj = {
  name: "why",
  age: 18
}
// 创建代理对象
const objProxy = new Proxy(obj, {});
console.log(objProxy.name);			// why
console.log(objProxy.age);			// 18

// 修改代理对象，原对象也会自动被修改
objProxy.name = "kobe";
objProxy.age = 30;
console.log(obj.name);				// kobe
console.log(obj.age);				// 30
```

**使用Proxy监听对象**

```javascript
const obj = {
  name: "why",
  age: 18
}
const objProxy = new Proxy(obj, {
    // 获取值时的捕获器
	get: function(target, key) {
	  console.log(`监听到对象的${key}属性被访问了`, target);
	  return Reflect.get(target, key);
	},
    // 设置值时的捕获器
	set: function(target, key, newValue) {
	  console.log(`监听到对象的${key}属性被设置值`, target);
	  Reflect.set(target, key, newValue);
	}
});

console.log(objProxy.name);		// 监听到对象的name属性被访问了
console.log(objProxy.age);		// 监听到对象的age属性被访问了
objProxy.name = "kobe";			// 监听到对象的name属性被设置值
objProxy.age = 30;				// 监听到对象的age属性被设置值
```



## Proxy捕获器

- ##### handler.get() 

  属性读取操作的捕捉器

  `get: function(target, property, receiver) {}`

  - target：目标对象
  - property：被获取的属性名
  - receiver：调用的代理对象

  ```javascript
  const obj = {
  	_name: "why",
  	get name() {
          // 访问name属性的时候，正常this的指向是obj
          // 不过不修改对应的this指向到代理对象，那么代理对象就永远无法捕获到_name属性
          // 所以需要使用receiver来指定
  		return this._name;
  	},
  	set name(newValue) {
  		this._name = newValue;
  	}
  }
  const objProxy = new Proxy(obj, {
  	get: function(target, key, receiver) {
  		// receiver是创建出来的代理对象objProxy
  		console.log("get方法被访问", key, receiver);
  		console.log(receiver === objProxy);				// true
          // Reflect.get中的receiver会改变调用对象的this
  		return Reflect.get(target, key, receiver);
  	}
  })
  objProxy.name = "kobe";		// 拦截器会被访问两次，分别是name和_name
  // get方法被访问 name { _name: 'why', name: {Getter/Setter} }
  // true
  // get方法被访问 _name { _name: 'why', name: {Getter/Setter} }
  // true
  ```

- ##### handler.set() 

  属性设置操作的捕捉器

  `set: function(target, property, value, receiver) {}`

  - target：目标对象
  - property：被获取的属性名
  - value：新属性值
  - receiver：调用的代理对象

  ```javascript
  const obj = {
  	_name: "why",
  	get name() {
          // 访问name属性的时候，正常this的指向是obj
          // 不过不修改对应的this指向到代理对象，那么代理对象就永远无法捕获到_name属性
          // 所以需要使用receiver来指定
  		return this._name;
  	},
  	set name(newValue) {
  		this._name = newValue;
  	}
  }
  const objProxy = new Proxy(obj, {
  	set: function(target, key, newValue, receiver) {
          // receiver是创建出来的代理对象objProxy
  		console.log("set方法被访问", key);
          // Reflect.get中的receiver会改变调用对象的this
  		Reflect.set(target, key, newValue, receiver);
  	}
  });
  console.log(objProxy.name);		// 拦截器会被访问两次，分别是name和_name
  // set方法被访问 name
  // set方法被访问 _name
  ```

- ##### handler.has() 

  in操作符的捕捉器

  `has: function(target, prop) {}`

  - target：目标对象
  - prop：需检查的属性名

  ```javascript
  const obj = { name: "jack", age: 18 };
  const objProxy = new Proxy(obj, {
  	// 监听in的捕获器
  	has: function(target, key) {
  	  console.log(`监听到对象的${key}属性in操作`, target);
  	  return Reflect.has(target, key);
  	}
  });
  console.log("name" in objProxy);
  // 监听到对象的name属性in操作 { name: "jack", age: 18 }
  // true
  ```

- ##### handler.deleteProperty() 

  delete操作符的捕捉器

  `deleteProperty: function(target, property) {}`

  - target：目标对象
  - property：待删除的属性名

  ```javascript
  const obj = { name: "jack", age: 18 };
  const objProxy = new Proxy(obj, {
  	// 监听delete的捕获器
  	deleteProperty: function(target, key) {
  	  console.log(`监听到对象的${key}属性delete操作`, target);
  	  Reflect.deleteProperty(target, key);
  	}
  });
  delete objProxy.name;
  // 监听到对象的name属性delete操作 { name: "jack", age: 18 }
  ```

- ##### handler.apply() 

  函数调用操作的捕捉器

  `apply: function(target, thisArg, argumentsList) {}`

  - target：目标对象（函数）
  - thisArg：被调用时的上下文对象
  - argumentsList：被调用时的参数数组

  ```javascript
  function foo() {}
  const fooProxy = new Proxy(foo, {
  	apply: function(target, thisArg, argArray) {
  		console.log("对foo函数进行了apply调用");
          return Reflect.apply(target, thisArg, argArray);
  	}
  });
  fooProxy.apply({}, ["abc", "cba"]);
  // 对foo函数进行了apply调用
  ```

- ##### handler.construct() 

  new 操作符的捕捉器

  `construct: function(target, argumentsList, newTarget) {}`

  - target：目标对象（函数）
  - argumentsList：constructor的参数数组
  - newTarget：最初被调用的构造函数

  ```javascript
  function foo() {}
  const fooProxy = new Proxy(foo, {
  	construct: function(target, argArray, newTarget) {
  		console.log("对foo函数进行了new调用");
          return Reflect.construct(target, argArray);
  	}
  });
  new fooProxy("abc", "cba");
  // 对foo函数进行了new调用
  ```

- ##### handler.getPrototypeOf() 

  Object.getPrototypeOf （返回对象的原型）方法的捕捉器

  `getPrototypeOf(target) {}`

  - target：目标对象

- ##### handler.setPrototypeOf()

  Object.setPrototypeOf （设置对象的原型）方法的捕捉器

  `setPrototypeOf: function(target, prototype) {}`

  - target：目标对象
  - prototype：对象新原型或为null

- ##### handler.isExtensible()

  Object.isExtensible（判断对象是否是可扩展）方法的捕捉器

  `isExtensible: function(target) {}`

  - target：目标对象

- ##### handler.preventExtensions()

  Object.preventExtensions （让对象变的不可扩展）方法的捕捉器

  `preventExtensions: function(target) {}`

  - target：目标对象

- ##### handler.getOwnPropertyDescriptor()

  Object.getOwnPropertyDescriptor （指定对象上属性的属性描述符）方法的捕捉器

  `getOwnPropertyDescriptor: function(target, prop) {}`

  - target：目标对象
  - prop：属性名称的描述

- ##### handler.defineProperty()

  Object.defineProperty （对象上定义新属性或修改现有属性）方法的捕捉器

  `defineProperty: function(target, property, descriptor) {}`

  - target：目标对象
  - property：属性名
  - descriptor：待定义或修改的属性的描述符

- ##### handler.ownKeys()

  Object.getOwnPropertyNames （返回对象自身属性的属性名）方法

  和Object.getOwnPropertySymbols （返回对象自身所有Symbol属性的数组）方法的捕捉器

  `ownKeys: function(target) {}`

  - target：目标对象



## 反射对象Reflect

Reflect反射是ES6新增的一个API，它是一个对象，主要提供了很多操作JavaScript对象的方法，有点像**Object中操作对象的方法**

让这些Object操作都集中到了Reflect对象上

1. Object作为一个构造函数，对对象本身的操作实际上放到它身上并不合适
2. 类似于in、delete操作符让JS看起来有一些奇怪

[Object和Reflect对象之间的API关系](https://developer.mozilla.org/zh-
CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/Comparing_Reflect_and_Object_methods)



## Reflect常见方法

Reflect方法**和Proxy是一一对应的**

- ##### Reflect.get(target, propertyKey[, receiver])

  获取对象身上某个属性的值，类似于target[name]

- ##### Reflect.set(target, propertyKey, value[, receiver])

  将值分配给属性的函数。返回一个Boolean，如果更新成功，则返回true

- ##### Reflect.has(target, propertyKey)

  判断一个对象是否存在某个属性，和in 运算符的功能完全相同

- ##### Reflect.deleteProperty(target, propertyKey)

  作为函数的delete操作符，相当于执行delete target[name]

- ##### Reflect.apply(target, thisArgument, argumentsList)

  对一个函数进行调用操作，同时可以传入一个数组作为调用参数，和Function.prototype.apply() 功能类似

- ##### Reflect.construct(target, argumentsList[, newTarget])

  对构造函数进行new 操作，相当于执行new target(...args)

  ```javascript
  // 执行Student函数中的内容, 创建Teacher对象
  function Student(name, age) {
  	this.name = name;
  	this.age = age;
  }
  function Teacher() {
  }
  const teacher = Reflect.construct(Student, ["why", 18], Teacher);
  console.log(teacher);									// Teacher { name: 'why', age: 18 }
  console.log(teacher.__proto__ === Teacher.prototype);	// true
  ```

- ##### Reflect.getPrototypeOf(target)

  类似于Object.getPrototypeOf()

- ##### Reflect.setPrototypeOf(target, prototype)

  设置对象原型的函数，返回一个Boolean， 如果更新成功，则返回true

- ##### Reflect.isExtensible(target)

  类似于Object.isExtensible()

- ##### Reflect.preventExtensions(target)

  类似于Object.preventExtensions()，返回一个Boolean

- ##### Reflect.getOwnPropertyDescriptor(target, propertyKey)

  类似于Object.getOwnPropertyDescriptor()，如果对象中存在该属性，则返回对应的属性描述符, 否则返回undefined

- ##### Reflect.defineProperty(target, propertyKey, attributes)

  和Object.defineProperty() 类似。如果设置成功就会返回true

- ##### Reflect.ownKeys(target)

  返回一个包含所有自身属性（不包含继承属性）的数组。(类似于Object.keys(), 但不会受enumerable影响)



## 响应式

可以自动响应数据变量的代码机制，就称之为是响应式

### Proxy响应式函数

通过new Proxy的方式（vue3采用的方式）

```javascript
// 保存当前需要收集的响应式函数
let activeReactiveFn = null;
// 管理响应式的数据依赖 
// 不同的对象，不同的属性，对应不同的响应式函数
// { obj1: { key1: Depend, key2: Depend }, obj2: { key: Depend } }
const targetMap = new WeakMap();

// 管理某一个对象的某一个属性的所有响应式函数
class Depend {
	constructor() {
        // 响应式函数的数组
		this.reactiveFns = new Set();
	}
	depend() {
        // 给depend对象中添加响应函数
		if (activeReactiveFn) {
			this.reactiveFns.add(activeReactiveFn);
		}
	}
	notify() {
        // 属性变更时，执行响应式函数
		this.reactiveFns.forEach(fn => {
			fn();
		})
	}
}

// 封装一个响应式的函数
function watchFn(fn) {
	activeReactiveFn = fn;
    // fn中对属性的操作会触发监听操作
	fn();
	activeReactiveFn = null;
}

// 封装获取depend的函数
function getDepend(target, key) {
	// 根据target对象获取map
	let map = targetMap.get(target);
	if (!map) {
		map = new Map();
		targetMap.set(target, map);
	}
	// 根据key获取depend对象
	let depend = map.get(key);
	if (!depend) {
		depend = new Depend();
		map.set(key, depend);
	}
	return depend;
}

// 将对象变成响应式对象，监听对象的属性变量
function reactive(obj) {
	return new Proxy(obj, {
		get: function(target, key, receiver) {
			// 根据target.key获取对应的depend
			const depend = getDepend(target, key);
			// 给depend对象中添加响应函数
			depend.depend();
			return Reflect.get(target, key, receiver);
		},
		set: function(target, key, newValue, receiver) {
			Reflect.set(target, key, newValue, receiver);
			const depend = getDepend(target, key);
            // 执行属性变更通知
			depend.notify();
		}
	});
}

// 将对象变成响应式对象
const objProxy = reactive({
	name: "why",			// 要对应一个depend对象
	age: 18					// 要对应一个depend对象
});
const infoProxy = reactive({
	address: "广州市",		  // 要对应一个depend对象
	height: 1.88			// 要对应一个depend对象
});

// 注册address的响应式函数
watchFn(() => {
  console.log(infoProxy.address);
});

infoProxy.address = "北京市";
// 执行结果：
// 广州市				->> 注册响应式函数时的执行结果
// 北京市				->> 修改属性值时监听的执行结果
```



### defineProperty响应式函数

通过Object.defineProperty的方式（vue2采用的方式）

```javascript
// 保存当前需要收集的响应式函数
let activeReactiveFn = null;
// 管理响应式的数据依赖 
// 不同的对象，不同的属性，对应不同的响应式函数
// { obj1: { key1: Depend, key2: Depend }, obj2: { key: Depend } }
const targetMap = new WeakMap();

// 管理某一个对象的某一个属性的所有响应式函数
class Depend {
	constructor() {
        // 响应式函数的数组
		this.reactiveFns = new Set();
	}
	depend() {
        // 给depend对象中添加响应函数
		if (activeReactiveFn) {
			this.reactiveFns.add(activeReactiveFn);
		}
	}
	notify() {
        // 属性变更时，执行响应式函数
		this.reactiveFns.forEach(fn => {
			fn();
		})
	}
}

// 封装一个响应式的函数
function watchFn(fn) {
	activeReactiveFn = fn;
    // fn中对属性的操作会触发监听操作
	fn();
	activeReactiveFn = null;
}

// 封装获取depend的函数
function getDepend(target, key) {
	// 根据target对象获取map
	let map = targetMap.get(target);
	if (!map) {
		map = new Map();
		targetMap.set(target, map);
	}
	// 根据key获取depend对象
	let depend = map.get(key);
	if (!depend) {
		depend = new Depend();
		map.set(key, depend);
	}
	return depend;
}

// 将对象变成响应式对象，监听对象的属性变量
function reactive(obj) {
    // ES6之前, 使用Object.defineProperty
	Object.keys(obj).forEach(key => {
		let value = obj[key];
		Object.defineProperty(obj, key, {
			get: function() {
				const depend = getDepend(obj, key);
				depend.depend();
				return value;
			},
			set: function(newValue) {
				value = newValue;
				const depend = getDepend(obj, key);
				depend.notify();
			}
		})
	});
	return obj;
}

// 将对象变成响应式对象
const objProxy = reactive({
	name: "why",			// 要对应一个depend对象
	age: 18					// 要对应一个depend对象
});
const infoProxy = reactive({
	address: "广州市",		  // 要对应一个depend对象
	height: 1.88			// 要对应一个depend对象
});

// 注册address的响应式函数
watchFn(() => {
  console.log(infoProxy.address);
});

infoProxy.address = "北京市";
// 执行结果：
// 广州市				->> 注册响应式函数时的执行结果
// 北京市				->> 修改属性值时监听的执行结果
```



# 迭代器和生成器

## 迭代器

迭代器是帮助对某个数据结构进行遍历的对象

迭代器对象需要符合迭代器协议，在js中这个标准就是一个特定的**next方法**

next方法的要求：一个无参数或者一个参数的函数，返回一个应当拥有以下两个属性的对象

1. **done（boolean）** 如果迭代器可以产生序列中的下一个值，则为false；如果迭代器已将序列迭代完毕，则为true
2. **value** 迭代器返回的任何JavaScript值，done为true时可省略或默认返回值

```javascript
const names = ["abc", "cba", "nba"];
function createArrayIterator(arr) {
	let index = 0;
	return {
		next: function() {
			if (index < arr.length) {
				return { done: false, value: arr[index++] };
			} else {
				return { done: true, value: undefined };
			}
		}
	}
}
const namesIterator = createArrayIterator(names);
console.log(namesIterator.next());		// {done: false, value: 'abc'}
console.log(namesIterator.next());		// {done: false, value: 'cba'}
console.log(namesIterator.next());		// {done: false, value: 'nba'}
console.log(namesIterator.next());		// {done: true, value: undefined}
```



## 可迭代对象

当一个对象实现了可迭代协议时，它就是一个可迭代对象

这个对象必须实现@@iterator方法，在代码中使用**Symbol.iterator**访问该属性

当一个对象变成一个可迭代对象时，进行某些操作，例如for-of等操作时，其实就会调用它的@@iterator方法

1. 语法：for ...of、展开语法、yield*、解构赋值
2. 创建对象：new Map([Iterable])、new WeakMap([iterable])、new Set([iterable])、new WeakSet([iterable])
3. 方法调用：Promise.all(iterable)、Promise.race(iterable)、Array.from(iterable)

```javascript
// iterableObj对象就是一个可迭代对象
const iterableObj = {
	names: ["abc", "cba", "nba"],
	[Symbol.iterator]: function() {
		let index = 0
		return {
			next: () => {
				if (index < this.names.length) {
					return { done: false, value: this.names[index++] };
				} else {
					return { done: true, value: undefined };
				}
			}
		}
	}
}
const iterator = iterableObj[Symbol.iterator]();
console.log(iterator.next());						// {done: false, value: 'abc'}
console.log(iterator.next());						// {done: false, value: 'cba'}
console.log(iterator.next());						// {done: false, value: 'nba'}
console.log(iterator.next());						// {done: true, value: undefined}

// 第二次创建，会创建新的迭代器
const iterator2 = iterableObj[Symbol.iterator]();
console.log(iterator2.next());						// {done: false, value: 'abc'}

// for...of可以遍历可迭代对象
for (const item of iterableObj) {
	console.log(item); 
}
// abc
// cba
// nba
```

在设计类的时候就可以添加上@@iterator 方法，类创建出来的对象默认是可迭代的

如果需要**迭代中断**（break、continue、return、throw等中断循环），可以添加**return方法**

```javascript
class Classroom {
	constructor(address, name, students) {
		this.address = address;
		this.name = name;
		this.students = students;
	}
	entry(newStudent) {
		this.students.push(newStudent);
	}
	[Symbol.iterator]() {
		let index = 0;
		return {
			next: () => {
				if (index < this.students.length) {
					return { done: false, value: this.students[index++] };
				} else {
					return { done: true, value: undefined };
				}
			},
			return: () => {
				console.log("迭代器提前终止了~");
				return { done: true, value: undefined };
			}
		}
	}
}

const classroom = new Classroom("3幢5楼205", "计算机教室", ["james", "kobe", "curry", "why"]);
classroom.entry("lilei");
for (const stu of classroom) {
  console.log(stu);
  if (stu === "why") break;
}
// james
// kobe
// curry
// why
// 迭代器提前终止了~
```

平时创建的很多原生对象已经实现了可迭代协议，会生成一个迭代器对象：

String、Array、Map、Set、arguments对象、NodeList集合...

```javascript
const names = ["abc", "cba", "nba"];
const iterator1 = names[Symbol.iterator]();
console.log(iterator1.next());	// {value: 'abc', done: false}
```



## 生成器

### 生成器函数

生成器是ES6中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执行等

生成器函数也是一个函数，但是和普通的函数有一些区别：

1. 生成器函数需要在function的后面加**符号***

2. 生成器函数可以通过**yield关键字**来控制函数的执行流程，当遇到yield时候值**暂停函数执行**

   可以通过yield来返回结果，不指定默认返回undefined

   生成器函数中遇到return时候生成器就停止执行

3. 生成器函数的**返回值是一个Generator**（生成器对象），生成器是一种**特殊的迭代器**

   调用next来执行生成器对象

```javascript
function* foo() {
  console.log("函数开始执行~");

  const value1 = 100;
  console.log("第一段代码:", value1);
  yield value1;

  const value2 = 200;
  console.log("第二段代码:", value2);
  yield value2;

  const value3 = 300;
  console.log("第三段代码:", value3);
  yield value3;

  console.log("函数执行结束~");
  return "123";
}
const generator = foo();
console.log("返回值1:", generator.next());
console.log("返回值2:", generator.next());
console.log("返回值3:", generator.next());
console.log("返回值3:", generator.next());
// 函数开始执行~
// 第一段代码: 100
// 返回值1: {value: 100, done: false}
// 第二段代码: 200
// 返回值2: {value: 200, done: false}
// 第三段代码: 300
// 返回值3: {value: 300, done: false}
// 函数执行结束~
// 返回值3: {value: '123', done: true}
```

生成器是一种特殊的迭代器，可以使用生成器来替代迭代器

还可以使用**yield***来生产一个可迭代对象，相当于是一种yield的语法糖，会依次迭代这个可迭代对象，每次迭代其中的一个值

```javascript
const names = ["abc", "cba", "nba"];
// 迭代器
function createArrayIterator(arr) {
	let index = 0;
	return {
		next: function() {
			if (index < arr.length) {
				return { done: false, value: arr[index++] };
			} else {
				return { done: true, value: undefined };
			}
		}
	}
}
const namesIterator = createArrayIterator(names);
console.log(namesIterator.next());		// {done: false, value: 'abc'}
console.log(namesIterator.next());		// {done: false, value: 'cba'}
console.log(namesIterator.next());		// {done: false, value: 'nba'}
console.log(namesIterator.next());		// {done: true, value: undefined}

// 转换为生成器
// 第一种写法
function* createArrayGenerator1(arr) {
    let index = 0;
    yield arr[index++];
    yield arr[index++];
    yield arr[index++];
}
const namesGenerator1 = createArrayGenerator1(names);
console.log(namesGenerator1.next());		// {value: 'abc', done: false}
console.log(namesGenerator1.next());		// {value: 'cba', done: false}
console.log(namesGenerator1.next());		// {value: 'nba', done: false}
console.log(namesGenerator1.next());		// {value: undefined, done: true}

// 第二种写法
function* createArrayGenerator2(arr) {
	for (const item of arr) {
        yield item
	}
}
const namesGenerator2 = createArrayGenerator2(names);
console.log(namesGenerator2.next());		// {value: 'abc', done: false}
console.log(namesGenerator2.next());		// {value: 'cba', done: false}
console.log(namesGenerator2.next());		// {value: 'nba', done: false}
console.log(namesGenerator2.next());		// {value: undefined, done: true}

// 第三种写法
function* createArrayGenerator3(arr) {
	yield* arr
}
const namesGenerator3 = createArrayGenerator3(names);
console.log(namesGenerator3.next());		// {value: 'abc', done: false}
console.log(namesGenerator3.next());		// {value: 'cba', done: false}
console.log(namesGenerator3.next());		// {value: 'nba', done: false}
console.log(namesGenerator3.next());		// {value: undefined, done: true}
```

```javascript
// 可迭代类使用生成器
class Classroom {
	constructor(address, name, students) {
		this.address = address;
		this.name = name;
		this.students = students;
	}
	entry(newStudent) {
		this.students.push(newStudent);
	}
	*[Symbol.iterator]() {
        yield* this.students;
	}
	// [Symbol.iterator] = function*() {
	// 	yield* this.students;
	// }
}
const classroom = new Classroom("3幢5楼205", "计算机教室", ["james", "kobe", "curry", "why"]);
for (const item of classroom) {
	console.log(item);
}
// james
// kobe
// curry
// why
```

生成器案例

```javascript
// 创建一个函数, 这个函数可以迭代一个范围内的数字
function* createRangeIterator(start, end) {
  let index = start
  while (index < end) {
    yield index++
  }
}
const rangeIterator = createRangeIterator(10, 20)
console.log(rangeIterator.next())		// {value: 10, done: false}
console.log(rangeIterator.next())		// {value: 11, done: false}
console.log(rangeIterator.next())		// {value: 12, done: false}
console.log(rangeIterator.next())		// {value: 13, done: false}
console.log(rangeIterator.next())		// {value: 14, done: false}
```



### next方法

生成器对象调用**next函数**的时候，可以给传递参数，这个**参数会作为上一个yield语句的返回值**

```javascript
function* foo(initial) {
    console.log('函数执行函数~');
    const value1 = yield initial + 'aaa';
    const value2 = yield value1 + 'bbb';
    const value3 = yield value2 + 'ccc';
}
const generator = foo('why');
const result1 = generator.next();
console.log('result1:', result1);
// next传入的值会传到value1
const result2 = generator.next(result1.value + '1');
console.log('result2:', result2);
// next传入的值会传到value2
const result3 = generator.next(result2.value + '2');
console.log('result3:', result3);
// result1: {value: 'whyaaa', done: false}
// result2: {value: 'whyaaa1bbb', done: false}
// result3: {value: 'whyaaa1bbb2ccc', done: false}
```



### return方法

生成器对象调用**return函数**，会让生成器提前结束，可以传递参数，返回给定的值

```javascript
function* foo(num) {
  console.log("函数开始执行~");

  const value1 = 100 * num;
  console.log("第一段代码:", value1);
  const m = yield value1;

  const value2 = 200 * n;
  console.log("第二段代码:", value2);
  const n = yield value2;

  const value3 = 300 * count;
  console.log("第三段代码:", value3);
  yield value3;

  console.log("函数执行结束~");
  return "123";
}
const generator = foo(10);
console.log("返回值1:", generator.next());
// 提前终止生成器函数代码继续执行
// 相当于在第一段代码后面加上return
console.log("返回值2:", generator.return(15));
console.log("返回值3:", generator.next());
// 函数开始执行~
// 第一段代码: 1000
// 返回值1: {value: 1000, done: false}
// 返回值2: {value: 15, done: true}
// 返回值3: {value: undefined, done: true}
```



### throw方法

生成器对象调用**throw函数**，可以给生成器函数内部抛出异常，可以传入异常信息

抛出异常后可以在生成器函数中捕获异常，捕获异常后可以使用next继续函数的执行

```javascript
// 没有捕获异常
function* foo() {
  console.log("代码开始执行~")
  const value1 = 100
  yield value1

  console.log("第二段代码继续执行")
  const value2 = 200
  yield value2

  console.log("第三段代码继续执行")
  const value3 = 300
  yield value3
    
  console.log("代码执行结束~")
  return "123"
}
const generator = foo()
console.log(generator.next())
generator.throw("error message")
console.log(generator.next())
// 代码开始执行~
// {value: 100, done: false}
// Uncaught error message
```

```javascript
// 生成器函数中捕获异常
function* foo() {
  console.log("代码开始执行~")
  const value1 = 100
  try {
    yield value1
  } catch (error) {
    console.log("捕获到异常情况:", error)
  }

  console.log("第二段代码继续执行")
  const value2 = 200
  yield value2

  console.log("第三段代码继续执行")
  const value3 = 300
  yield value3

  console.log("代码执行结束~")
  return "123"
}
const generator = foo()
console.log(generator.next())
generator.throw("error message")
console.log(generator.next())
console.log(generator.next())
// 代码开始执行~
// {value: 100, done: false}
// 捕获到异常情况: error message
// 第二段代码继续执行
// 第三段代码继续执行
// {value: 300, done: false}
// 代码执行结束~
// {value: '123', done: true}
```




# 预解析和作用域

## 预解析

**js引擎运行js分为两步：1. 预解析 2. 代码执行**

1. js引擎会把js里面所有的var 还有 function 提升到当前作用域的最前面。
2. 之后按照书写的顺序代码从上往下执行

### 变量提升

把所有的**变量声明提升到当前的作用域最前面**

只提升变量声明，**不提升赋值操作**

```javascript
console.log(num); // 输出undefined
var num = 10;

fun();			  // 报错
var fun = function() {
    console.log(22);
}
```

**块极变量`let`和`const`声明的变量只能在作用域内访问，不存在变量提升**

**暂时性死区**：在当前同级作用域内在达到声明处之前都是无法访问的

- let、const关键字声明的变量会产生块级作用域，会先在作用域中被创建出来，但是如果此时还未完成语法绑定，如果访问就会抛出错误

  ```javascript
  console.log(a);
  let a = 1;
  ```

  ```javascript
  var tmp = 123;
  if (true) { 
  	tmp = 'abc';
  	let tmp; 
  }
  ```



### 函数提升

把所有的函数声明提升到当前作用域的最前面

只声明函数，不调用函数



## 作用域

### 全局作用域和全局变量

**全局作用域**：整个script标签或者整个js文件

**全局变量**：在全局作用域下的变量

- 在全局作用域下声明的变量是全局变量

  在函数内部，**没有声明而直接赋值的变量**也是全局变量局部变量

  ```javascript
  function fun() {
      var num1 = 10; // 局部
      num2 = 20;	   // 全局
  }
  ```

  ```javascript
  function foo() {
      var a = b = 10;
      // 转化为 var a = 10; b = 10;
  }
  foo();
  console.log(a);	// 报错
  console.log(b);	// 10
  ```

- 浏览器关闭才会销毁。占内存



### 局部作用域和局部变量

**局部作用域/函数作用域**：只在函数内部起效果

**局部变量：**在局部作用域下的变量

- 在函数内部的声明变量就是局部变量
- 代码块执行才会初始化，代码结束就会销毁，节省内存

函数的**参数作用域**：当函数参数有默认值的情况下，会形成一个新的作用域,，这个作用域用于保存参数的值

```javascript
var x = 0
function foo(x, y = function() { x = 3; console.log(x) }) {
  console.log(x)
  var x = 2
  console.log(x)
  y()
  console.log(x)
}

foo(1)
console.log(x)

// undefined
// 3
// 2
// 0
```



### 块级作用域和块级变量

**块级作用域：**ES6中新增，在{ } 内，通过let、const、function、class声明的标识符是具备块级作用域的限制

有的浏览器会对函数的声明进行特殊的处理（兼容以前的代码），允许像var那样进行提升

```javascript
{
    var name = 'jack';
    let foo = 'foo';
    function bar() {
        console.log('bar');
    }
    class Person {}
}
console.log(name);		// 'jack'
console.log(foo);		// ReferenceError: foo is not defined
bar();					// 可以访问
var p = new Person();	// ReferenceError: Person is not defined
```

**if-switch-for都是块级作用域**

```javascript
if (true) {
  var foo = "foo";
  let bar = "bar";
}
console.log(foo);	// foo
console.log(bar);	// ReferenceError: bar is not defined
```

```javascript
switch (color) {
  case "red":
    var foo = "foo";
    let bar = "bar";
}
console.log(foo);	// foo
console.log(bar);	// ReferenceError: bar is not defined
```

```javascript
for (var i = 0; i < 10; i++) {
    console.log("Hello World" + i);
}
console.log(i);		// 10

for (let j = 0; j < 10; j++) {
    console.log("Hello World" + j);
}
console.log(j);		// ReferenceError: j is not defined
```

获取多个按钮监听点击

```javascript
const btns = document.getElementsByTagName('button');
// 上层作用域的i已经是最后的结果了
// for (var i = 0; i < btns.length; i++) {
// 	btns[i].onclick = function() {
//         console.log("第" + i + "个按钮被点击")
//     }
// }

for (var i = 0; i < btns.length; i++) {
  (function(n) {
    btns[i].onclick = function() {
        console.log("第" + n + "个按钮被点击")
    }
  })(i)
}

for (let j = 0; j < btns.length; j++) {
  btns[j].onclick = function() {
    console.log("第" + j + "个按钮被点击")
  }
}
```



### 作用域链

内部函数查找外部函数的变量，采取**链式查找**的方式来决定取哪个值，就称为作用域链

就近原则，从目标出发，**一层一层往外查找**



### with语句

with语句扩展一个语句的作用域链，可以形成自己的作用域

不建议使用，在严格模式下不能使用

```javascript
var obj = { name: 'jack', age: 18, message: 'hello world' };
function foo() {
    function bar() {
        with(obj) {
            // 变量在查找时，不会直接到函数的AO去查找，会先去with的函数对象中去查找，如果没有找到再去上一层找
            console.log(message);
        }
    }
    bar();
}
foo();
```



## JS引擎解析运行

### 全局对象

引擎会在执行代码之前，会在堆内存中创建**全局对象**（**GlobalObject** -> GO）

- 该对象所有的作用域（scope）都可以访问
- 里面会包含Date、Array、String、Number、setTimeout、setInterval等
- 包含一个**window**属性，指向自己

```javascript
var name = 'why';
var num1 = 20;
var num2 = 30;
var result = num1 + num2;
function foo {
    console.log('foo');
}

// 解析后的 GlobalObject -> go 对象
var globalObject = {
    String: '类',
    Date: '类',
    setTimeout: '函数',
    window: globalObject,
    name: undefined,
    num1: undefined,
    num2: undefined,
    result: undefined,
    foo: 0xa00
}
```



### 执行上下文栈

引擎内部会有一个执行上下文栈（Execution Context Stack -> ECStack 函数调用栈），用于执行代码的调用栈

代码如果要执行，需要放入执行上下文栈中去运行（**入栈**）

<img src="JavaScript.assets/image-20220408170455287.png" alt="image-20220408170455287" style="zoom: 45%;" /> 



### 全局执行上下文

全局代码为了执行，会创建全局执行上下文（Global Execution Context），会被放到ECStack中执行

全局执行上下文（GEC）被放入到执行上下文栈（ECS）中，里面包含两个部分

1. 代码在执行前，在解析器（parser）转成抽象语法树（AST）的过程中，会将**全局定义的变量、函数等加入到GlobalObject中**，但是并**不会赋值**。这个过程也称为**变量的作用域提升**（hoisting）

   **编译函数**时，会在内存中开辟一块空间去存储函数，其中包括**父级作用域**（parent scope）和**函数的执行体**（代码块）

   在全局对象（GO）中的函数定义**保存函数存储空间的内存地址**

2. 在代码执行中，按照顺序**从上往下执行代码**，对变量赋值，或者执行其他的函数



### 函数执行上下文

函数执行时，会根据函数体创建一个函数执行上下文（Functional Execution Content -> FEC）,并且加入到执行上下文栈（EC Stack）中

一旦函数执行结束，函数执行上下文会移出栈，并销毁掉

函数执行上下文（FEC）中包含三部分内容

1. 在解析函数成为抽象语法树（AST）结构时，会创建一个**函数活跃对象**（Activation Object -> **AO**）

   AO中包含形参、arguments、函数定义和指向函数对象、定义的变量

   AO对象在函数执行结束后，如果没有引用指向它，就会被销毁掉

2. **作用域链**：由变量对象（Variable Object -> VO）（在函数中就是AO对象）和父级VO组成，查找时会一层层查找

   - 当**查找一个变量**时，查找路径是**沿着作用域链一层一层往上查找**

     <img src="JavaScript.assets/image-20220409231920467.png" alt="image-20220409231920467" style="zoom:67%;" /> 

   - 在函数被编译的时候，父级作用域已经确定了

     <img src="JavaScript.assets/image-20220410005608207.png" alt="image-20220410005608207" style="zoom:67%;" /> 

    

3. **this绑定的值**：**动态绑定**，不是在编译时而是在执行时绑定

<img src="JavaScript.assets/image-20220408174641490.png" alt="image-20220408174641490" style="zoom:50%;" />  



### 变量对象 

**早期ECMA版本规范**：每一个执行上下文会被关联到一个**变量对象**（Variable Object -> VO），在源代码中的变量和函数声明会被作为属性添加到VO中。对于函数来说，参数也会被添加到VO中

**最新ECMA版本规范**：每一个执行上下文会关联到一个**变量环境**（Variable  Environment）中，在执行代码中变量和函数的声明会作为环境记录（Environment Record）添加带变量环境中。对于函数来说，参数也会被作为环境记录添加到变量环境中。环境记录不一定是对象形式。



## 内存管理

### 栈内存和堆内存

javaScript会在**定义变量时分配内存**

**基本数据类型**内存的分配会在执行时，直接在**栈空间**进行分配

**复杂数据类型**内存的分配会在**堆内存**中开辟一块空间，并且将这块空间的**指针**返回值变量引用

<img src="JavaScript.assets/image-20220425121954921.png" alt="image-20220425121954921" style="zoom:70%;" /> 



### 垃圾回收

内存的大小是有限的，所有当内存不再需要的时候，需要对其进行释放，以便腾出更多内存空间

#### GC算法：引用计数

当一个对象有一个**引用指向**它时，那么这个对象的引用就**+1**，当一个对象的**引用为0**时，这个对象就可以被**销毁**

<img src="JavaScript.assets/image-20220425130318486.png" alt="image-20220425130318486" style="zoom:60%;" /> 

这个算法有很大的弊端，就是会产生**循环引用**

<img src="JavaScript.assets/image-20220425123619764.png" alt="image-20220425123619764" style="zoom:50%;" /> 

如果忘记清除引用`obj.info = null`的时候，内存永远也不会释放，会造成内存泄漏



#### GC算法：标记清除

设置一个**根对象**（root object），垃圾回收器会定期从这个根开始，找所有**从根开始有引用到的对象**，对于哪些没有引用到的对象，就认为是不可用的对象

<img src="JavaScript.assets/image-20220425132940343.png" alt="image-20220425132940343" style="zoom:50%;" /> 

这个算法可以很好解决循环引用的问题

JS引擎比较广泛的采用标记清除算法，V8引擎为了更好的优化，也在算法的实现细节上结合一些其他的算法



### 函数执行内存变化

<img src="JavaScript.assets/函数执行内存.png" alt="函数执行内存" style="zoom:80%;" /> 



### 清理回调

FinalizationRegistry对象可以在对象被垃圾回收时请求一个回调（ES12）

当一个注册的对象被销毁时，会在某个时间点上执行清理回调（GC是不定时的检测）

```javascript
const finalRegistry = new FinalizationRegistry((value) => {
  console.log("注册在finalRegistry的对象被销毁了", value)
})
let obj = { name: "why" };
let info = { age: 18 };

// 注册需要回调的对象，注册的时候可以起别名
// finalRegistry.register(obj);
finalRegistry.register(obj, "obj");
finalRegistry.register(info, "value");

obj = null;
info = null;
// 注册在finalRegistry的对象被销毁了 obj
// 注册在finalRegistry的对象被销毁了 value
```



### 弱引用

默认将一个对象赋值给另外一个引用，那么这个引用是一个强引用

如果希望是一个弱引用的话，可以使用WeakRef（ES12）

WeakRef.prototype.deref()获取弱引用的原对象，如果原对象已经销毁, 那么获取到的是undefined

```javascript
// 强引用
const finalRegistry = new FinalizationRegistry((value) => {
  console.log("注册在finalRegistry的对象被销毁了", value)
})
let obj = { name: "why" };
// 强引用
let info = obj;
finalRegistry.register(obj, "obj");
// 即使obj = null，因为info的引用，obj的内存也不会消除
obj = null;
```

```javascript
// 弱引用
const finalRegistry = new FinalizationRegistry((value) => {
  console.log("注册在finalRegistry的对象被销毁了", value)
})
let obj = { name: "why" };
// 弱引用
let info = new WeakRef(obj);
finalRegistry.register(obj, "obj");
// info对obj的内存是弱引用，内存会被GC销毁
obj = null;
// 注册在finalRegistry的对象被销毁了 obj

setTimeout(() => {
    // 获取原对象
    console.log(info.deref()?.name);
}, 10000);
// undefined
```



## 闭包

闭包（closure）指**有权访问另一个函数作用域中变量的函数**。

也就是说，闭包可以让：**在一个作用域（内层函数）可以访问到另外一个函数内部（其外层函数）的局部变量（作用域）**

闭包是两部分组成的：**函数+可以访问的自由变量**

闭包的作用是延伸变量的作用范围

```javascript
function fn() {
	var num = 10;
    return function {
        console.log(num); // 10
    }
}
var f = fn();
// 访问了另外一个函数 fn 里面的局部变量 num
f();
```

- 利用闭包循环注册点击事件

  ```javascript
  var lis = document.querySelector('.nav').querySelectorAll('li');
  // 利用闭包的方式得到当前小li 的索引号
  for (var i = 0; i < lis.length; i++) {
      // 利用for循环创建了4个立即执行函数
      // 立即执行函数也成为小闭包因为立即执行函数里面的任何一个函数都可以使用它的i这变量
      (function(i) {
      	lis[i].onclick = function() {
          	console.log(i);
      	}
  	})(i);
  }
  ```

- 定时器中的闭包

  ```javascript
  // 3秒钟之后,打印所有li元素的内容
  var lis = document.querySelector('.nav').querySelectorAll('li');
  for (var i = 0; i < lis.length; i++) {
      (function(i) {
          setTimeout(function() {
              console.log(lis[i].innerHTML);
          }, 3000)
      })(i);
  }
  ```

- 闭包思考题

  ```javascript
  var name = "The Window";
  var object = {
      name: "My Object",
      getNameFunc: function() {
          return function() {
  			return this.name;
  		};
      }
  }
  console.log(object.getNameFunc()());
  
  // var f = object.getNameFunc(); 类似于
  var f = function() {
      // this指向是window
      return this.name;
  }
  f();
  ```

**每当创建一个函数，闭包就会在函数创建的同时被创建出来**

```javascript
// 创建函数，形成闭包
var name = 'demo';		// 可以访问的外层作用域
function demo() {		// 函数
    console.log(name)
}
```

函数执行过程解释闭包原理以及**闭包的内存泄漏**

![ ](JavaScript.assets/image-20220427010026705.png)

AO对象不会被销毁时，不被使用的属性会被释放

 <img src="JavaScript.assets/image-20220427123058788.png" alt="image-20220427123058788" style="zoom:67%;" />



## this指向

### this指向的绑定

this的指向的**跟函数所处的位置没有关系**，**跟函数被调用的方式有关系**

this是在**运行时被绑定**的

<img src="JavaScript.assets/image-20220427142800584.png" alt="image-20220427142800584" style="zoom:67%;" /> <img src="JavaScript.assets/image-20220427142828466.png" alt="image-20220427142828466" style="zoom:75%;" />

如果没有this，很多问题也是有解决方案的，但是编写代码会非常不方便

```javascript
// this 的写法
var obj1 = {
    name: 'jack',
    eating: function() {
        console.log(this.name + 'is eating');
    }
    running: function() {
        console.log(this.name + 'is running');
    }
    studying: function() {
        console.log(this.name + 'is studying');
    }
}
var obj2 = {
    name: 'john',
    eating: function() {
        console.log(this.name + 'is eating');
    }
    running: function() {
        console.log(this.name + 'is running');
    }
    studying: function() {
        console.log(this.name + 'is studying');
    }
}
// 不使用this
var obj1 = {
    name: 'jack',
    eating: function() {
        console.log(obj1.name + 'is eating');
    }
    running: function() {
        console.log(obj1.name + 'is running');
    }
    studying: function() {
        console.log(obj1.name + 'is studying');
    }
}
var obj2 = {
    name: 'john',
    eating: function() {
        console.log(obj2.name + 'is eating');
    }
    running: function() {
        console.log(obj2.name + 'is running');
    }
    studying: function() {
        console.log(obj2.name + 'is studying');
    }
}
```



### 全局作用域的指向

- **浏览器**：**window**（globalObject）

  <img src="JavaScript.assets/image-20220427133226680.png" alt="image-20220427133226680" style="zoom: 67%;" /> 

- **Node**： **{}** 空对象 

  （Node的执行机制决定的：把js文件当成模块module -> 加载 -> 编译 -> 所有代码放到一个函数中 -> 执行这个函数.apply({})）

开发中很少全局作用域下使用this，通常都是在函数中使用

1. 所有的函数在调用时，都会创建一个执行上下文

2. 这个上下文对象中记录着函数的调用栈、AO对象等

3. this也是其中的一条记录



### 内置函数的指向

#### setTimeout

```javascript
setTimeout(function() {
    console.log(this);
}, 1000);
// 结果 window
```

```javascript
// setTimeout内部直接独立调用传入的方法，所以默认绑定window
function hySetTimeout(fn, duration) {
    fn();
}
hySetTimeout(function() {
    console.log(this);	// window
}, 3000)
```



#### 监听事件

```javascript
const boxDiv = document.querySelector('.box')
boxDiv.onclick = function() {
    console.log(this);
}
// 结果 .box的BOM对象
// 浏览器内部使用boxDiv.click()调用该事件，隐式绑定到该BOM对象上
```

```javascript
const boxDiv = document.querySelector('.box')
boxDiv.addEventLinstener('click', function() {
    console.log(this);
})
// 结果 .box的BOM对象
// 浏览器内部使用fn.call(boxDiv)调用该事件，显式绑定到该BOM对象上
```



#### 数组方法

```javascript
var names = ['abc', 'cba', 'nba'];
names.forEach(function(item) {
    console.log(this);
})
// 结果 循环打印window
```

数组的.forEach/map/filter/find.. 方法的第二个参数可以绑定this的指向

`forEach(callbackfn: (value: string, index: number, array: string[]) => void, thisArg?: any): void`

```javascript
var obj = { name: 'jack' };
var names = ['abc', 'cba', 'nba'];
names.forEach(function(item) {
    console.log(this);
}, obj);
// 结果 循环打印obj对象
```



### 默认绑定

**独立函数调用**的情况下使用默认绑定

独立函数调用可以理解成函数没有被绑定到某个对象上进行调用

```javascript
// 案例1
function foo() {
    console.log(this);
}
foo();
// 结果：window

// 案例2
function test1() {
    console.log(this);
    test2();
}
function test2() {
    console.log(this);
    test3();
}
function test3() {
    console.log(this);
}
test1();
// 结果：window window window

// 案例3
function foo(func) {
    func();
}
var obj = {
    name: 'why',
    bar: function() {
        console.log(this);
    }
}
foo(obj.bar);
```



### 隐式绑定

函数**通过某个对象进行调用**，是通过某个对象发起的函数调用

object.fn()此时object对象会被js引擎绑定到fn函数中的this里

隐式绑定有前提条件

1. 必须在调用的对象内部有一个对函数的引用（比如一个属性）
2. 如果没有这样的引用，在进行调用时，会报找不到该函数的错误
3. 正是通过这个引用，间接的将this绑定到了这个对象上

```javascript
// 案例1
function foo() {
    console.log(this);
}
var obj = {
    name: 'obj',
    foo: foo
}
obj.foo();
// 结果：obj

// 案例2
function foo() {
    console.log(this);
}
var obj1 = {
    name: 'obj1',
    foo: foo
}
var obj2 = {
    name: 'obj2',
    obj1: obj1
}
obj2.obj1.foo();
// 结果：obj1

// 案例3
function foo() {
    console.log(this);
}
var obj = {
    name: 'obj',
    foo: foo
}
var bar = obj.foo;
bar();
// 结果：window
// 此时执行bar已经获取不到obj的name
```



### 显式绑定

如果不希望在对象内部包含这个函数的引用，同时又希望在这个对象上进行强制调用

#### call

调用这个函数, 并且修改函数运行时的 `this `指向  

`fun.call(thisArg, arg1, arg2, ...)`

- `thisArg `：当前调用函数 `this `的指向对象
- `arg1`，`arg2`：传递的其他参数

```javascript
function fn(x, y) {
	console.log(this);
	console.log(x + y);	
}
var o = {
	name: 'andy'
};
// call() 可以改变这个函数的this指向 此时这个函数的this 就指向了o这个对象
fn.call(o, 1, 2);
```

#### apply

调用这个函数, 并且修改函数运行时的 `this `指向 

`fun.apply(thisArg, [argsArray])`

- `thisArg `：当前调用函数 `this `的指向对象
- `argsArray`：传递的值参数必须是数组

利用 `apply() `借助于数学内置对象求数组最大值

```javascript
var arr = [1, 66, 3, 99, 4];
var max = Math.max.apply(Math, arr);
```

#### bind

不会调用函数。但是能改变函数内部this 指向 

`fun.bind(thisArg, arg1, arg2, ...) `

- `thisArg `：在 fun 函数运行时指定的 this 值
- `arg1`，`arg2`：传递的其他参数
- 返回的是原函数改变this之后产生的新函数

如果有的函数不需要立即调用,但是又想改变这个函数内部的`this`指向此时用`bind()`

```javascript
// 点击按钮,禁用按钮3秒钟
var btns = document.querySelectorAll('button');
for (var i = 0; i < btns.length; i++) {
	btns[i].onclick = function() {
    	this.disabled = true;
        setTimeout(function() {
            this.disabled = false;
        }.bind(this), 2000);
    }
}
```



### new 绑定

函数可以当做一个类的构造函数来使用，也就是使用new关键字

使用new关键字来调用函数会执行如下操作

1. 创建一个全新的对象
2. 这个新对象会被执行prototype连接
3. 这个新对象会绑定到函数调用的this上（this 绑定在这个步骤完成）
4. 如果函数没有返回其他对象，表达式会返回这个新对象

```javascript
function Person(name, age) {
    this.name = 'name';
    this.age = 'age';
}
var p1 = new Person('why', 18);
```



### 绑定优先级 

**new绑定 > 显示绑定 > 隐式绑定 > 默认绑定**

- ##### 默认绑定优先级最低

  默认绑定优先级最低，存在其他规则时，就会通过其他规则的方式来绑定this

- ##### 显示绑定 > 隐式绑定

  ```javascript
  var obj = {
      name: 'obj',
      foo: function() {
          console.log(this);
      }
  }
  obj.foo();						// obj
  obj.foo.call('abc');			// abc
  obj.foo.apply('abc');			// abc
  var bar = obj.foo.bind('cba');
  bar();							// cba
  ```

  ```javascript
  function foo() {
      console.log(this);
  }
  var obj = {
      name: 'obj',
      foo: foo.bind('aaa')
  }
  obj.foo();
  // aaa
  ```

- ##### new绑定 > 隐式绑定

  ```javascript
  var obj = {
      name: 'obj',
      foo: function() {
          console.log(this);
      }
  }
  var f = new obj.foo();
  // foo {}
  ```

- ##### new绑定 > 显式绑定

  new关键字不能和apply / call 一起来使用

  new关键字只能和bind一起使用

  ```javascript
  function foo() {
      console.log(this);
  }
  var bar = foo.bind('aaa');
  var obj = new bar();
  // foo {}
  ```



### 特殊绑定

- ##### 忽略显示绑定

  apply / call / bind ： 当传入 null / undefined 时，自动将this绑定成全局对象

  ```javascript
  function foo() {
      console.log(this);
  }
  foo.apply('abc');		// abc
  foo.apply({});			// {}
  
  foo.apply(null);		// window
  foo.apply(undefined);	// window
  var bar = foo.bind(null);
  bar();					// window
  ```

- ##### 间接函数引用

  创建一个函数的间接引用，使用默认绑定规则

  ```javascript
  var obj1 = {
      name: 'obj1',
      foo: function() {
          console.log(this);
      }
  }
  var obj2 = {
      name: 'obj2'
  }
  obj2.bar = obj1.foo
  obj2.bar();
  // 结果 obj2
  
  // 加上小括号相当于将这个看成一个整体
  // 会当成独立的函数调用
  (obj2.bar = obj1.foo)();
  // 结果 window
  ```

  

### 箭头函数的this

箭头函数不使用this的四种标准规则（默认、隐式、显示、new），而是根据**外层作用域来决定this**（也就是**不绑定this**）

不断的从上层作用域找对应的this

```javascript
var foo = () => {
    console.log(this);
}
foo();					// window
var obj = { foo: foo };
obj.foo();				// window
foo.call('abc');		// window
```

```javascript
// 不使用箭头函数的解决方案
var obj = {
    data: [],
    // 发送请求，将结果添加到data属性中
    getData: function() {
        var _this = this;
        setTimeout(function) {
            var result = ['abc', 'cba', 'nba'];
            _this.data = result;
        }, 2000);
    }
}

// 使用箭头函数的解决方案
var obj = {
    data: [],
    // 发送请求，将结果添加到data属性中
    getData: function() {
        setTimeout(() => {
            var result = ['abc', 'cba', 'nba'];
            // 从上层作用域寻找属性
            this.data = result;
        }, 2000);
    }
}

obj.getData();
```

```javascript
const obj = { name: '张三'} 
function fn () {
    console.log(this);
    return () => {
        console.log(this)
    } 
} 
const resFn = fn.call(obj);
resFn();
// { name: '张三'}
```

```javascript
var age = 100;
var obj = {
	age: 20, // 定义对象不会产生作用域
	say: () => {
		alert(this.age)
	}
}
obj.say(); // 100
```



### globalThis

获取JavaScript环境的全局对象，在浏览器中可以通过this、window来获取，在Node中需要通过global来获取

在ES11中对获取全局对象进行了统一的规范：globalThis

```javascript
// 在浏览器下
console.log(window);
console.log(this);
// 在node下
console.log(global);
// ES11
console.log(globalThis);
```



### 面试题练习

```javascript
var name = "window";
var person = {
  name: "person",
  sayName: function () {
    console.log(this.name);
  }
};
function sayName() {
  var sss = person.sayName;
  sss(); 					// window 独立函数调用
  person.sayName(); 		// person 隐式调用
  (person.sayName)(); 		// person 隐式调用
  (b = person.sayName)(); 	// window 有赋值表达式，间接函数引用，独立函数调用
}
sayName();
```

```javascript
var name = 'window'
var person1 = {
  name: 'person1',
  foo1: function () {
    console.log(this.name)
  },
  foo2: () => console.log(this.name),
  foo3: function () {
    return function () {
      console.log(this.name)
    }
  },
  foo4: function () {
    return () => {
      console.log(this.name)
    }
  }
}

var person2 = { name: 'person2' }

person1.foo1(); 				// person1 隐式绑定
person1.foo1.call(person2); 	// person2 显式绑定

person1.foo2();					// window 箭头函数，找上层作用域
person1.foo2.call(person2);		// window 箭头函数，找上层作用域，4种绑定都无效

person1.foo3()();				// window 独立函数调用，第二遍调用的时候没有绑定
person1.foo3.call(person2)();	// window 独立函数调用，第二遍调用的时候没有绑定
person1.foo3().call(person2);	// person2 显式绑定，最终的调用是显式绑定

person1.foo4()();				// person1 箭头函数，上层作用域是foo4，foo4作用域中的this是person1
person1.foo4.call(person2)();	// person2 箭头函数，上层作用域被显式绑定了person2
person1.foo4().call(person2);	// person1 箭头函数，上层foo4作用域中的this是person1，箭头函数显式绑定无效
```

```javascript
var name = 'window'
function Person (name) {
  this.name = name
  this.foo1 = function () {
    console.log(this.name)
  },
  this.foo2 = () => console.log(this.name),
  this.foo3 = function () {
    return function () {
      console.log(this.name)
    }
  },
  this.foo4 = function () {
    return () => {
      console.log(this.name)
    }
  }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

person1.foo1() 					// person1 隐式绑定
person1.foo1.call(person2)		// person2 显式绑定

person1.foo2()					// person1 箭头函数，上层作用域是Person，也就是person1
person1.foo2.call(person2)		// person1 箭头函数，绑定无效，上层作用域是Person，也就是person1

person1.foo3()()				// window 独立函数调用
person1.foo3.call(person2)()	// window 最终的调用是独立函数调用
person1.foo3().call(person2)	// person2 显式绑定

person1.foo4()()				// person1 箭头函数，上层作用域是Person，也就是person1
person1.foo4.call(person2)()	// person2 上层作用域的foo4绑定到person2
person1.foo4().call(person2)	// person1 箭头函数，绑定无效，上层作用域是Person，也就是person1
```

```javascript
var name = 'window'
function Person (name) {
  this.name = name
  this.obj = {
    name: 'obj',
    foo1: function () {
      return function () {
        console.log(this.name)
      }
    },
    foo2: function () {
      return () => {
        console.log(this.name)
      }
    }
  }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

person1.obj.foo1()()				// window 独立函数调用
person1.obj.foo1.call(person2)()	// window 最终的调用是独立函数调用
person1.obj.foo1().call(person2)	// person2 显式绑定

person1.obj.foo2()()				// obj 箭头函数，上层作用域是foo2，foo2是被obj调用的，this是obj
person1.obj.foo2.call(person2)()	// person2 箭头函数，上层作用域foo2被绑定到person2
person1.obj.foo2().call(person2)	// obj 箭头函数，显式绑定无效，上层作用域是foo2，foo2是被obj调用的，this是obj
```



# 同步和异步

## 浏览器的事件循环

JavaScript是**单线程的**，JavaScript的线程有自己的容器进程：浏览器或者Node

多数浏览器是多进程的，每个进程中又有很多的线程，包括执行JavaScript代码的线程

**浏览器可以用不同的线程**来完成JavaScript耗时的操作

**事件循环**

<img src="JavaScript.assets/image-20220505151325078.png" alt="image-20220505151325078" style="zoom:67%;" /> 



## 同步任务和异步任务

- ##### 同步任务

  同步任务都在**主线程上执行**，形成一个**执行栈**

- ##### 异步任务

  异步是通过回调函数实现的

  异步任务有以下三种类型:

  1. 普通**事件**，如 click、resize 等
  2. **资源加载**，如 load、error 等
  3. **定时器**，包括 setInterval、setTimeout 等
  4. **Ajax**等

  **异步任务相关回调函数添加到任务队列中**（任务队列也称为消息队列、事件队列）

  <img src="JavaScript.assets/image-20211207000617533.png" alt="image-20211207000617533" style="zoom: 67%;" /> 




## 宏任务和微任务

在事件循环中，并非只维护着一个队列，事实上是有两个队列

- ##### 宏任务队列

  **setTimeout**，**setInterval**，requestAnimationFrame，I/O，**ajax**，**DOM**监听，UI Rendering等

- ##### 微任务队列

  process，nextTick，**Promise的then回调**，Object，observe，MutationObserver，**queueMicrotask**()等

**执行优先级**

1. 同步的代码
2. 微任务的异步代码
3. 宏任务的异步代码

**异步中先执行微任务再执行宏任务。**

**如果宏任务中有微任务，会执行完当前宏任务中的所有微任务以后，再执行下一个宏任务**

（执行宏任务之前，需要保证微任务队列已经被清空）

  ```javascript
console.log(1);
setTimeout(function(){					// 宏任务 进入事件队列
    console.log(2);
    new Promise(function(resovle){
        console.log(3);
        resolve();
    }).then(function(){					// 在宏任务中有微任务，先执行完所有微任务，再去执行下一个宏任务
        console.log(4);
    })
}, 0)
new Promise(function(resolve){			// 微任务 进入事件队列
    console.log(5);
    resolve();
}).then(function(){
    console.log(6);
})
setTimeout(function(){					// 宏任务 进入事件队列
    console.log(7);
    new Promise(function(resolve){
        console.log(8);
        resolve();
    }).then(function(){
        console.log(9);
    })
    console.log(10);
}, 0)
console.log(11);
// 执行顺序
// 1 5 11 6 2 3 4 7 8 10 9
  ```



##  执行机制

1. **先执行执行栈中的同步任务**

2. **异步任务**（回调函数）放入**任务队列中**

3. 一旦执行栈中的所有**同步任务执行完毕**，系统就会**按次序读取任务队列中的异步任务**

   被读取的异步任务结束等待状态，进入执行栈，开始执行

   <img src="JavaScript.assets/image-20211207001156393.png" alt="image-20211207001156393" style="zoom:67%;" /> 

   **异步函数会先提交给异步进程进行处理，异步进程处理决定是否放入到任务队列中**

   例如`onclick`的异步任务，**只有在点击了，才会进入任务队列**。**定时器时间到了，回调函数才会放入任务队列**中去

   由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为**事件循环**（ event loop）



## 回调函数

**正常方式是不能获取异步函数内的返回值**，只能返回undefined

```javascript
getCallbackData(){
    setTimeOut(()=>{
		var username = "张三";
		return username; 
    }, 1000);
}
```

使用回调函数获取异步函数返回值

```javascript
getCallbackData(callbackFunc){
    setTimeOut(()=>{
		var username = "张三";
		callbackFunc(username); 
    }, 1000);
}

let callbackData = getCallbackData((data)=>{
    console.log(data);
});
```

但是回调函数会引发**地狱回调**的问题

```javascript
// 两个异步函数无法确定哪个谁先执行完
&.ajax({
    url:"php/ok.php",
    sucess: function(res) {
        console.log('结果111', res);
    }
})
&.ajax({
    url:"php/ok.php",
    sucess: function(res) {
        console.log('结果22', res);
    }
})

// 利用回调函数嵌套规定执行顺序，但是如果一直嵌套会造成地狱回调问题
&.ajax({
    url:"php/ok.php",
    sucess: function(res) {
        console.log('结果111', res);
		&.ajax({
    		url:"php/ok.php",
    		sucess: function(res) {
        		console.log('结果22', res);
                &.ajax({
		    		url:"php/ok.php",
		    		sucess: function(res) {
	        		console.log('结果33', res);
			    	}
				})
	    	}
		})
    } 
})
```



## Promise

### 理解Promise

 Promise对象用于表示一个异步操作的最终完成（或失败），以及其结果值

在通过new创建Promise对象时，需要传入一个回调函数，称为executor

- 这个回调函数会被立即执行，并且给传入另外两个回调函数resolve、reject

- 当调用resolve回调函数时，会执行Promise对象的then方法中传入的成功回调函数
- 当调用reject回调函数时，会执行Promise对象的then方法中传入的失败回调函数，或者catch方法传入的回调函数

```javascript
let p = new promise(function(resolve, reject){
    // 处理异步操作
    setTimeout(function(){
        // 生成1-10的随机数
        var num = Math.ceil(Math.random() * 10);
        if(num<=5){
            // resolve 成功的函数
            resolve(num);
        }
        else{
            // reject 失败的函数
            reject('数字太大了');
        }
    }, 2000);
})
// p.then(成功的函数resolve, 失败的函数reject(可以省略))
p.then(function(data){
    // 成功函数
    console.log('resolved');
    console.log(data);
}, function(reason, data){
    // 失败函数
    console.log('rejected');
    console.log(reason);
})
```



### Promise三个状态

1. 待定（pending）: 初始状态，既没有被兑现，也没有被拒绝

   当执行executor中的代码时，处于该状态

2. 已兑现（fulfilled）: 意味着操作成功完成

   执行了resolve时，处于该状态

3. 已拒绝（rejected）: 意味着操作失败

   执行了reject时，处于该状态

Promise**状态一旦确定下来，就是不可更改的**(锁定)

1. 在调用resolve的时候，如果**resolve传入的值不是一个Promise**，那么会将该Promise的状态变成**已兑现**（fulfilled）

2. 如果**resolve传入的值是一个Promise**，那么当前的Promise的状态会由**传入的Promise来决定**（状态进行移交）

   ```javascript
   new Promise((resolve, reject) => {
      resolve(new Promise((resolve, reject) => {
          setTimeout(() => {
             resolve('第二个Promise的resolve'); 
          }, 3000);
      })) 
   }).then(res => {
       console.log('res:', res);
   }).catch(err => {
      console.log('err:', err); 
   });
   // res: 第二个Promise的resolve
   ```

3. 如果resolve中传入的是一个对象，并且这个**传入对象有实现then方法**，那么会**执行该then方法**，并且根据then方法的结果来决定Promise的状态

   ```javascript
   new Promise((resolve, reject) => {
      resolve({
          then: function(resolve, reject) {
              resolve('thenable value');
          }
      }) 
   }).then(res => {
       console.log(res);
   });
   // thenable value
   ```

4. 之后去调用reject时，已经不会有任何的响应了（并不是这行代码不会执行，而是无法改变Promise状态）

   ```javascript
   const promise = new Promise((resolve, reject) => {
       console.log('---');	 // pending状态（待定状态）
       resolve('哈哈');		// fulfilled状态（已敲定状态）
       reject('错误信息');	   // rejected状态（已拒绝状态）
   });
   promise.then(res => {
       console.log(res);
   }).catch(err => {
       console.log(err);
   });
   // ---
   // 哈哈
   ```



### Promise对象方法

#### then方法

then方法是Promise对象上的一个方法，其实是放在Promise的原型上的Promise.prototype.then

then方法接受两个参数

1. **fulfilled的回调函数**：当状态变成fulfilled时会回调的函数

2. **reject的回调函数**：当状态变成reject时会回调的函数

   ```javascript
   const promise = new Promise((resolve, reject) => {
   	reject("rejected status");
   })
   promise.then(undefined, err => {
   	console.log("err:", err);
   });
   // err: rejected status
   ```

   当抛出异常时, 也会调用reject回调函数的

   ```javascript
   const promise = new Promise((resolve, reject) => {
   	throw new Error("rejected status")
   })
   promise.then(undefined, err => {
   	console.log("err:", err);
   });
   // err: rejected status
   ```

**同一个Promise可以被多次调用then方法**，当resolve方法被回调时, 所有的then方法传入的回调函数都会被调用

```javascript
const promise = new Promise((resolve, reject) => {
  resolve("hahaha");
});
promise.then(res => {
  console.log("res1:", res);
})
promise.then(res => {
  console.log("res2:", res);
})
promise.then(res => {
  console.log("res3:", res);
})
// res1: hahaha
// res2: hahaha
// res3: hahaha
```

**then方法本身是有返回值的，它的返回值是一个Promise**

1. 如果then方法中的回调函数返回的是一个普通值，那么这个普通值会被作为一个新的Promise的resolve值

   ```javascript
   const promise = new Promise((resolve, reject) => {
     resolve("hahaha");
   });
   promise.then(res => {
       return 'aaa';
       // 相当于
       // return new Promise(resolve => {
       //    resolve('aaa'); 
       // });
   }).then(res => {
       console.log('res:', res);
   });
   // res: aaa
   ```

   如果回调函数没有返回值，会返回一个resolve(undefined)的新Promise

   ```javascript
   const promise = new Promise((resolve, reject) => {
     resolve("hahaha");
   });
   promise.then(res => {
       // 相当于
       // return new Promise(resolve => {
       //    resolve(undefined); 
       // });
   }).then(res => {
       console.log('res:', res);
   });
   // res: undefined
   ```

2. 如果返回的是一个Promise，返回的Promise的状态将取代调用的Promise的状态

   ```javascript
   const promise = new Promise((resolve, reject) => {
     resolve("hahaha");
   });
   promise.then(res => {
       return new Promise(resolve => {
          setTimeout(() => {
              resolve(111);
          }, 3000) 
       });
   }).then(res => {
       console.log('res:', res);
   });
   // res: 111
   ```

3. 如果返回的是一个对象, 并且该对象实现了then方法

   ```javascript
   const promise = new Promise((resolve, reject) => {
     resolve("hahaha");
   });
   promise.then(res => {
       return {
           then: function(resolve, reject) {
               resolve(222);
           }
       }
       // 相当于
       // return new Promise(resolve => resolve(obj.then));
   }).then(res => {
       console.log('res:', res);
   });
   // res: 222
   ```

使用链式编程，解决地狱回调

```javascript
let p = new promise(function(resolve, reject){
	&.ajax({
	    url:"php/ok.php",
	    sucess: function(res) {
	        console.log('结果111', res);
	    }
	})
})
p.then(function(res){
    console.log('成功的结果',res);
    let p2 = new promise(function(resolve, reject){
		&.ajax({
		    url:"php/ok.php",
		    sucess: function(res) {
		        console.log('结果222', res);
		    }
		})
	})
}).then(function(res){
    console.log('成功的结果2',res);
})
```



#### catch方法

catch方法是Promise对象上的一个方法：它也是放在Promise的原型上的Promise.prototype.catch

可以替代then的reject的回调函数写法，调用传入对应的reject回调

1. 如果executor中执行了reject函数或者抛出异常，catch执行的promise就是最初调用的promise

2. 如果executor中没有执行reject函数或者抛出异常，

   catch之前的then执行reject函数或者抛出异常，catch执行的promise就是then返回的promise

```javascript
const promise = new Promise((resolve, reject) => {
	reject("rejected status")
	// throw new Error("rejected status")
});
promise.then(res => {
    
}).catch(err => {
    // catch里执行的promise不是then执行之后返回的新promise，而是最初的promise
    console.log("err:", err);
});
// err: rejected status
```

```javascript
const promise = new Promise((resolve, reject) => {
	resolve();
});
promise.then(res => {
    return new Promise((resolve, reject) => {
        reject('then rejected status');
    });
    // throw new Error('then rejected status');
}).catch(err => {
    // catch里执行的promise是then执行之后返回的新promise
    console.log("err:", err);
});
// err: then rejected status
```

```javascript
const promise = new Promise((resolve, reject) => {
	reject("rejected status");
    // throw new Error("rejected status");
});
promise.then(res => {
    return new Promise((resolve, reject) => {
        reject('then rejected status');
    });
    // throw new Error('then rejected status');
}).catch(err => {
    // catch里执行的promise不是then执行之后返回的新promise，而是最初的promise
    console.log("err:", err);
});
// err: rejected status
```

一个Promise的catch方法是可以被多次调用的，当Promise的状态变成reject的时候，这些回调函数都会被执行

```javascript
const promise = new Promise((resolve, reject) => {
	reject(111);
});
promise.catch(err => {
   console.log('err1:', err); 
});
promise.catch(err => {
    console.log('err2:', err);
});
// err1: 111
// err2: 111
```

拒绝捕获的问题

```javascript
const promise = new Promise((resolve, reject) => {
	reject(111);
});
// 两次then和catch相当于两次独立的调用
// 第一次的then没有捕获reject，会报异常
promise.then(res => {
    console.log('res:', res);
});
// Uncaught (in promise)
promise.catch(err => {
    console.log('err:', err);
});
// err: 111
```

**catch的返回值也是一个Promise**

1. 返回新Promise的resolve值

   ```javascript
   const promise = new Promise((resolve, reject) => {
   	reject(111);
   });
   promise.catch(err => {
       console.log('err1:', err);
       // 相当于
       // return new Promise(resolve => resolve())
   }).catch(err => {
       console.log('err2;', err);
   }).then(res => {
      console.log('res:', res); 
   });
   // err1: 111
   // res: undefined
   ```

2. 返回带异常的新Promise的

   ```javascript
   const promise = new Promise((resolve, reject) => {
   	reject(111);
   });
   promise.catch(err => {
       console.log('err1:', err);
       throw new Error('error message');
   }).catch(err => {
       console.log('err2;', err);
   }).then(res => {
      console.log('res:', res); 
   });
   // err1: 111
   // err2: error message
   // res: undefined
   ```



#### finally方法

finally是在ES9中新增的一个特性，无论Promise对象无论变成fulfilled还是reject状态，最终都会被执行的代码。

finally方法不接收参数，无论前面是fulfilled状态，还是reject状态，都会执行

```javascript
const promise = new Promise((resolve, reject) => {
	resolve("resolve message");
	// reject("reject message");
})

promise.then(res => {
	console.log("res:", res);
}).catch(err => {
	console.log("err:", err);
}).finally(() => {
	console.log("finally code execute");
})
// res: resolve message
// finally code execute
```



### Promise类方法

#### resolve方法

将一个现成的内容转成Promise，并且执行resolve操作

Promise.resolve的用法相当于new Promise

```javascript
// const promise2 = new Promise((resolve, reject) => {
// 	resolve({ name: "why" });
// })
// 相当于
const promise = Promise.resolve({ name: "why" });
promise.then(res => {
	console.log("res:", res);
});
// res: {name: 'why'}
```

Promise.resolve可以传入Promise或者thenable对象

```javascript
const promise = Promise.resolve(new Promise(resolve => resolve(11)));
promise.then(res => {
	console.log("res:", res);
});
// res: 11
```



#### reject方法

将一个现成的内容转成Promise，将Promise对象的状态设置为reject状态

Promise.reject相当于new Promise，只是会调用reject

```javascript
// const promise2 = new Promsie((resolve, reject) => {
// 	reject("rejected message");
// })
// 相当于
const promise = Promise.reject("rejected message");
promise.then(res => {
	console.log("res:", res);
}).catch(err => {
	console.log("err:", err);
})
// err: rejected message
```

Promise.reject方法传入Promise或者thenable对象也不会调用，只会打印出来

```javascript
const promise = Promise.reject({
    then: function(resolve, reject) {
        resolve('111');
    }
});
promise.then(res => {
	console.log("res:", res);
}).catch(err => {
	console.log("err:", err);
})
// err: {then: ƒ}
```

```javascript
const promise = Promise.reject(new Promise(() => {}));
promise.then(res => {
	console.log("res:", res);
}).catch(err => {
	console.log("err:", err);
})
// Promise {<fulfilled>: undefined}
```



#### all方法

将多个Promise包裹在一起形成一个新的Promise

当所有的Promise状态都变成fulfilled状态时，新的Promise状态为fulfilled，并且再将所有Promise的返回值组成一个数组

```javascript
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(11);
	}, 1000);
})
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(22);
	}, 2000);
})
const p3 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(33);
	}, 3000);
})
// 所有的Promise都变成fulfilled时, 再拿到结果
Promise.all([p1, p2, p3, "aa"]).then(res => {
	console.log(res);
}).catch(err => {
	console.log("err:", err);
})
// [11, 22, 33, 'aa']
```

当有一个Promise状态为reject时，新的Promise状态会立即变成对应的reject状态，并且会将第一个reject的返回值作为参数

对于resolved的，以及依然处于pending状态的Promise，是获取不到对应的结果的

```javascript
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(11);
	}, 1000);
})
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		reject(22);
	}, 2000);
})
const p3 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(33);
	}, 3000);
})
// 所有的Promise都变成fulfilled时, 再拿到结果
Promise.all([p1, p2, p3, "aa"]).then(res => {
	console.log(res);
}).catch(err => {
	console.log("err:", err);
})
// err: 22
```



#### allSettled方法

会在所有的Promise都有结果（settled），无论是fulfilled，还是reject时，才会有最终的状态，并且这个Promise的结果一定是fulfilled（ES11）

```javascript
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(11);
	}, 1000);
})
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		reject(22);
	}, 2000);
})
const p3 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(33);
	}, 3000);
})
Promise.allSettled([p1, p2, p3]).then(res => {
	console.log(res);
}).catch(err => {
	console.log("err:", err);
})
// [
// 	{status: 'fulfilled', value: 11},
// 	{status: 'rejected', reason: 22},
// 	{status: 'fulfilled', value: 33}
// ]
```

 

#### race方法

只要有一个Promise变成fulfilled状态, 那么就结束

```javascript
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(11);
	}, 1000);
})
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(22);
	}, 2000);
})
const p3 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(33);
	}, 3000);
})
Promise.race([p1, p2, p3]).then(res => {
	console.log("res:", res);
}).catch(err => {
	console.log("err:", err);
})
// res: 11
```

```javascript
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		reject(11);
	}, 1000);
})
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(22);
	}, 2000);
})
const p3 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(33);
	}, 3000);
})
Promise.race([p1, p2, p3]).then(res => {
	console.log("res:", res);
}).catch(err => {
	console.log("err:", err);
})
// err: 11
```



#### any方法

any方法会等到一个fulfilled状态，才会决定新Promise的状态（ES12）

```javascript
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		reject(11);
	}, 1000);
})
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(22);
	}, 2000);
})
const p3 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(33);
	}, 3000);
})
Promise.any([p1, p2, p3]).then(res => {
	console.log("res:", res);
}).catch(err => {
	console.log("err:", err);
})
// res: 22
```

如果所有的Promise都是reject的，那么也会等到所有的Promise都变成rejected状态

如果所有的Promise都是reject的，那么会报一个AggregateError的错误

```javascript
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		reject(11);
	}, 1000);
})
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		reject(22);
	}, 2000);
})
const p3 = new Promise((resolve, reject) => {
	setTimeout(() => {
		reject(33);
	}, 3000);
})
Promise.any([p1, p2, p3]).then(res => {
	console.log("res:", res);
}).catch(err => {
    console.log("err:", err);
	console.log("err:", err.errors);
})
// err: AggregateError: All promises were rejected
// err: [11, 22, 33]
```



### 执行顺序

**new Promise会立即执行，then的需要异步的**

```javascript
console.log(1);
let p = new Promise(function(resolve, reject){
    console.log(2)
    resovle();
})
p.then(function(){
    console.log(3);
})
console.log(4);
// 执行结果
// 1 2 4 3
```

先微后宏

在异步处理中then的回调比setTimeout先执行

```javascript
const first = () => (new Promise(resolve, reject) => {
    console.log(3);
    let p = new Promise((resolve, reject) => {
        console.log(7);
        setTimeout(() => {
            console.log(5);
            resolve(6);
        }, 0);
        resolve(1);
    });
    resolve(2);
    p.then((arg) => {
       console.log(arg); 
    });
}));
first().then((arg) => {
   console.log(arg); 
});
console.log(4);
// 执行结果
// 3 7 4 1 2 5
// 一个promise的resolve只能调用一次
// 6 不执行 
```

理解then的第二回调函数和catch

```javascript
asyncThing1()
    .then(function(){ 
    	return asyncThing2();
	})
	.then(function(){
    	return asyncThing3();
	})
	.catch(function(){
    	return asyncRecovery1();
	})
	.then(
		function(){
            return asyncThing4();
        },
    	function(err){
            return asyncRecovery2();
        }
	)
	.catch(function(err){
    	console.log("Dont't worry about it");
	})
	.then(function(){
    	console.log("All done!");
	})
```

<img src="JavaScript.assets/image-20220331230222286.png" alt="image-20220331230222286" style="zoom: 50%;" /> 



## async/await

### async

async关键字用于**声明一个异步函数**

async异步函数写法

```javascript
async function foo1() {}
const foo2 = async function() {}
const foo3 = async () => {}
class Person {
    async foo() {}
}
```

async函数就是**generator函数加yield的语法糖**

```javascript
// 需求
// 1> url: why -> res: why					向服务器发送网络请求获取数据，一共需要发送三次请求
// 2> url: res + "aaa" -> res: whyaaa		第二次的请求url依赖于第一次的结果
// 3> url: res + "bbb" => res: whyaaabbb	第三次的请求url依赖于第二次的结果，依次类推
function requestData(url) {
  // 异步请求的代码会被放入到executor中
  return new Promise((resolve, reject) => {
    // 模拟网络请求
    setTimeout(() => {
      // 拿到请求的结果
      resolve(url);
    }, 2000);
  })
}
// 使用Promise + generator实现
function* getData() {
  const res1 = yield requestData("why");
  const res2 = yield requestData(res1 + "aaa");
  const res3 = yield requestData(res2 + "bbb");
  const res4 = yield requestData(res3 + "ccc");
  console.log(res4);
}
// 封装工具函数execGenerator自动执行生成器函数
function execGenerator(genFn) {
  const generator = genFn();
  function exec(res) {
    const result = generator.next(res);
    if (result.done) {
      return result.value;
    }
    result.value.then(res => {
      exec(res);
    });
  }
  exec();
}
execGenerator(getData);		// whyaaabbbccc

// 使用async/await实现
async function getData2() {
  const res1 = await requestData("why")
  const res2 = await requestData(res1 + "aaa")
  const res3 = await requestData(res2 + "bbb")
  const res4 = await requestData(res3 + "ccc")
  console.log(res4)
}
getData2();					// whyaaabbbccc
```

异步函数执行流程：异步函数的内部代码执行过程和普通的函数是一致的，默认情况下也是会被同步执行

```javascript
async function foo() {
  console.log("foo function start~");
  console.log("内部的代码执行");
  console.log("foo function end~");
}
console.log("script start");
foo();
console.log("script end");
// script start
// foo function start~
// 内部的代码执行
// foo function end~
// script end
```

异步函数也**可以有返回值**

1. 异步函数的返回值会被包裹到Promise.resolve中

   ```javascript
   async function foo() {
     console.log("foo function start~");
     console.log("内部的代码执行");
     console.log("foo function end~");
   }
   const promise = foo();
   promise.then(res => {
      console.log();
   });
   ```

2. 异步函数的返回值是一个对象并且实现了thenable

   ```javascript
   async function foo() {
     console.log("foo function start~");
     return {
       then: function(resolve, reject) {
         resolve("内部的代码执行");
       }
     }
   }
   const promise = foo();
   promise.then(res => {
      console.log("promise then function exec:", res);
   });
   // foo function start~
   // promise then function exec: 内部的代码执行
   ```

3. 异步函数的返回值是Promise

   ```javascript
   async function foo() {
     console.log("foo function start~");
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         resolve("内部的代码执行");
       }, 2000)
     })
   }
   const promise = foo();
   promise.then(res => {
      console.log("promise exec:", res);
   });
   // foo function start~
   // promise exec: 内部的代码执行
   ```

在**async中抛出异常**，并不会像普通函数一样报错，而是会作为Promise的reject来传递

```javascript
async function foo() {
  console.log("foo function start~");
  console.log("中间代码~");
  // 异步函数中的异常, 会被作为异步函数返回的Promise的reject值的
  throw new Error("error message");
  console.log("foo function end~");
}
foo().catch(err => {
  console.log("err:", err);
})
console.log("后续还有代码~~~~~");
// foo function start~
// 中间代码~
// 后续还有代码~~~~~
// err: Error: error message
```



### await

async函数另外一个特殊之处就是可以在它内部使用**await关键字**

1. await**后面会跟上一个表达式**，这个**表达式会返回一个Promise**

   await会**等到Promise的状态变成fulfilled状态，之后继续执行异步函数**

   await语句**之后的代码相当于在返回的Promise的then中执行的**

   ```javascript
   function requestData() {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         resolve(222);
       }, 2000);
     })
   }
   async function foo() {
     const res1 = await requestData();
     // 下面的语句相当于在promise的then中执行
     console.log("res1后面的代码", res1);
     console.log("后面的代码");
     const res2 = await requestData();
     console.log("res2后面的代码", res2);
   }
   foo();
   // res1后面的代码 222
   // 后面的代码
   // res2后面的代码 222
   ```

2. await后面是一个**Promise**

   ```javascript
   async function foo() {
     const res1 = await new Promise((resolve) => {
       resolve("why");
     })
     console.log("res1", res1);
   }
   foo();
   // res1 why
   ```

3. 返回的Promise是**reject的状态**，代码**不会继续往下执行**，会将这个**reject结果直接作为函数的Promise的reject值**

   ```javascript
   function requestData() {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         reject(1111);
       }, 2000);
     })
   }
   async function foo() {
     const res1 = await requestData();
     console.log("res1后面的代码", res1);
     console.log("后面的代码");
   }
   foo().catch(err => {
     console.log("err:", err)
   })
   // err: 1111
   ```

4. await后面是一个**普通的值**，那么会**直接返回这个值**

   ```javascript
   async function foo() {
     const res1 = await 123;
     console.log("res1", res1);
   }
   foo();
   // res1 123
   ```

5. await后面是一个**thenable的对象**，会调用对象的then方法

   ```javascript
   async function foo() {
     const res1 = await {
       then: function(resolve, reject) {
         resolve("abc");
       }
     }
     console.log("res1", res1);
   }
   foo();
   // res1 abc
   ```

   





使用async和await省略then，代码清晰易读

```javascript
document.getElementById('btn').onclick = async () => {
    // 使用await省略then
    let res = await axios();
    console.log('结果', res);
}
```



## RxJs

```javascript
getRxjsData(){
    return new Observable((observer) => {
    	setTimeOut(()=>{
			var username = "张三";
			observer.next(username); 
            // observer.error();
    	}, 1000);
    });
}

let rxjsData = getRxjsData();
rxjsData.subscribe((data) => {
   console.log(data); 
});
```



# 模块

模块化开发的目的是将程序划分成一个个小的结构，这个结构中编写属于自己的逻辑代码，有**自己的作用域**，不会影响到其他的结构，这个结构可以将自己希望暴露的**变量、函数、对象等导出**给其结构使用，也可以通过某种方式，**导入另外结构**中的变量、函数、对象等

## 早期模块化

早期没有模块化带来了很多的问题：比如**命名冲突**的问题

早期解决模块化：**立即函数调用表达式**（IIFE），但是仍有问题

1. 必须记得每一个模块中返回对象的命名，才能在其他模块使用过程中正确的使用
2. 代码写起来混乱不堪，每个文件中的代码都需要包裹在一个匿名函数中来编写
3. 没有合适的规范情况下，会出现模块名称相同的情况

```javascript
// index.js
var moduleA = (function() {
  var name = "why";
  var age = 18;
  var isFlag = true;
  return {
    name: name,
    isFlag: isFlag
  };
})();
```

```javascript
// name.js
(function() {
  if (moduleA.isFlag) {
    console.log("我的名字是" + moduleA.name);
  }
})();
```



## CommonJS

Node中对CommonJS进行了支持和实现，让我们在开发node的过程中可以方便的进行模块化开发

1. 在Node中每一个js文件都是一个单独的模块

2. 模块中包括CommonJS规范的核心变量：exports、module.exports、require，可以使用这些变量来方便的进行模块化开发

   **exports和module.exports**可以负责对模块中的内容进行**导出**

   **require函数**可以帮助我们**导入**其他模块（自定义模块、系统模块、第三方库模块）中的内容

```javascript
// index.js
const { name, age, sum } = require("./name.js");
console.log(name);
console.log(age);
console.log(sum(20, 30));
```

```javascript
// name.js
const name = "why";
const age = 18;
function sum(num1, num2) {
  return num1 + num2;
}
module.exports = {
  name,
  age,
  sum
}
```

**exports和module.exports导出**的对象和**require导入**的这个对象是**同一个引用**

**module.exports属性是exports对象的一个引用**

```javascript
// name.js
const name = "why";
const age = 18;
function sum(num1, num2) {
  return num1 + num2;
}
// 源码
// module.exports = {}
// exports = module.exports

// 第一种导出方式
module.exports = { name, age, sum }

// 第二种导出方式
// exports.name = name
// exports.age = age
// exports.sum = sum
```

**require的查找规则** 格式：require(X)

1. X是一个Node核心模块，比如path、http，直接返回核心模块，并且停止查找

2. X是以./ 或../ 或/（根目录）开头的，将X当做一个文件在对应的目录下查找

   - 如果有后缀名，按照后缀名的格式查找对应的文件

     如果没有后缀名，按照 X > X.js > X.json > X.node 顺序查找

   - 如果没有找到对应的文件，将X作为一个目录，查找目录下的index文件

     按照 X/index.js > X/index.json > X/index.node 顺序查找

     ```javascript
     const abc = require("./abc")
     // ./abc/index.js
     ```

   - 如果没有找到，报错：not found

3. 直接是一个X（没有路径），并且X不是一个核心模块，会到每一层路径下面的node_modules中查找

   如果没找到，报错：not found

   ```javascript
   const axios = require("axios")
   ```

   <img src="JavaScript.assets/image-20220505215750850.png" alt="image-20220505215750850" style="zoom:50%;" /> 

**模块的加载过程**

1. 模块在被第一次引入时，模块中的js代码会被运行一次

2. 模块被多次引入时，会缓存，最终只加载（运行）一次（每个模块对象module都有一个属性：loaded，为false还没有加载，为true表示已经加载）

3. 如果有循环引入，加载顺序是按照深度优先搜索算法

   <img src="JavaScript.assets/image-20220505224844313.png" alt="image-20220505224844313" style="zoom: 67%;" /> 

加载顺序：main -> aaa -> ccc -> ddd -> eee -> bbb

CommonJS**加载模块是同步的**，意味着只有等到对应的模块加载完毕，当前模块中的内容才能被运行

在浏览器中通常不使用CommonJS规范，因为浏览器加载js文件需要先从服务器将文件下载下来，之后再加载运行



## ES Module

ES Module模块采用**export**和**import**关键字来实现模块化，采用编译期的静态分析，并且也加入了动态引用的方式

export负责将模块内的内容导出，import负责从其他模块导入内容

采用ES Module将自动采用**严格模式**（use strict）

```javascript
// main.js
import { name, age } from "./foo.js"
console.log(name)
console.log(age)
```

```javascript
// foo.js
export const name = "why"
export const age = 18
```

引入js的时候需要将文件**指定为模块`type="module"`**

```html
<script src="./main.js" type="module"></script>
```

如果指定为模块，再通过本地加载Html 文件(比如一个file:// 路径的文件)，将会报CORS 错误，因为Javascript 模块安全性需要

需要通过服务器来加载测试（可以通过VSCode的Live Server插件）

<img src="JavaScript.assets/image-20220505232654779.png" alt="image-20220505232654779" style="zoom:67%;" /> 

**export关键字将一个模块中的变量、函数、类等导出**

1. 在语句声明的前面直接加上export关键字

   ```javascript
   // 导出
   export const name = "why"
   export const age = 18
   export function foo() {
     console.log("foo function")
   }
   export class Person { }
   // 导入
   import { name, age, foo, Person } from "./foo.js"
   ```

2. 将所有需要导出的标识符，放到export后面的{}中（{}不是表示一个对象）

   ```javascript
   // 导出
   const name = "why"
   const age = 18
   function foo() {
     console.log("foo function")
   }
   export {
     name,
     age,
     foo
   }
   // 导入
   import { name, age, foo } from "./foo.js"
   ```

3. 导出时可以给标识符起一个别名

   ```javascript
   // 导出
   const name = "why"
   const age = 18
   function foo() {
     console.log("foo function")
   }
   export {
     name as fName,
     age as fAge,
     foo as fFoo
   }
   // 导入
   import { fName, fAge, fFoo } from "./foo.js"
   ```

4. 最重要的导出可以使用**默认导出**，其他的导出方式叫做命名导出

   在一个模块中，只能有一个默认导出

   ```javascript
   // foo.js
   const name = "why"
   const age = 18
   const foo = "foo value"
   export {
     name,
     age,
     // 默认导出方式一：as default
     // foo as default
   }
   // 默认导出方式二
   export default foo
   ```

   ```javascript
   // main.js
   // 导入默认的导出
   import why from './foo.js'
   ```

**import关键字负责从另外一个模块中导入内容**

1. import {标识符列表} from '模块'；（{}不是一个对象）

   ```javascript
   import { name, age, foo } from "./foo.js"
   ```

2. 导入时给标识符起别名

   ```javascript
   import { name as fName, age as fAge, foo as fFoo } from './foo.js'
   ```

3. 通过* 将模块功能放到一个模块功能对象上

   ```javascript
   import * as foo from './foo.js'
   console.log(foo.name)
   console.log(foo.age)
   foo.foo()
   ```

4. 导入默认的导出，在导入时不需要使用{}，可以自定义名字

   ```javascript
   // 默认导出
   const foo = "foo value"
   export default foo
   ```

   ```javascript
   // 导入
   import myFoo from './foo.js'
   ```

**export和import可以结合使用**

在开发和封装一个功能库时，通常希望将暴露的所有接口放到一个文件中（index.js），这样方便指定统一的接口规范

```javascript
// ./utils/index.js
// 导出方式一
// import { add, sub } from './math.js'
// import { timeFormat, priceFormat } from './format.js'
// export { add, sub, timeFormat, priceFormat }

// 导出方式二
// export { add, sub } from './math.js'
// export { timeFormat, priceFormat } from './format.js'

// 导出方式三
export * from './math.js'
export * from './format.js'
```

```javascript
// main.js
import { add, sub, priceFormat, timeFormat } from './utils'
```

**import关键字导入也是同步**代码，在导入完成之前，后续的代码都是不会运行的

```javascript
import { name, age, foo } from './foo.js'
console.log("foo.js解析完成之后，后续的代码才会运行")
console.log(name)
```

使用**import() 函数异步导入**，import函数返回的结果是一个Promise，加载成功后，内部会调用resolve

```javascript
// 异步导入，返回的是所有的导出
import("./foo.js").then(res => {
  console.log("res:", res)
})
console.log("后续的代码运行")
// 后续的代码运行
// res: Module { age: {...}, foo: {...}, name: {...} }
```

可以使用import() 函数来动态加载某一个模块

```javascript
let flag = true;
if (flag) {
    import('./modules/a.js').then(a => {
        a.aa();
    })
} else {
    import('./modules/b.js').then(b => {
        b.bb();
    })
}
```

import.meta是一个给模块暴露特定上下文的元数据属性的对象（ES11）: { url: "当前模块所在的路径" }

```javascript
console.log(import.meta)
// { url: 'http://127.0.0.1:5500/XXX/main.js' }
```

ES Module的解析过程

<img src="JavaScript.assets/image-20220506123840545.png" alt="image-20220506123840545" style="zoom:67%;" /> 

1. 构建（Construction）

   根据地址下载js文件，将其解析成模块记录（Module Record）

   <img src="JavaScript.assets/image-20220506123911906.png" alt="image-20220506123911906" style="zoom:50%;" /> 

   针对重复导入的模块，有专门的Map进行管理

   <img src="JavaScript.assets/image-20220506124027372.png" alt="image-20220506124027372" style="zoom:50%;" /> 

2. 实例化（Instantiation）

   对模块记录进行实例化，并且分配内存空间，解析模块的导入和导出语句，把模块指向对应的内存地址

3. 运行（Evaluation）

   运行代码，计算值，并且将值填充到内存地址中

   <img src="JavaScript.assets/image-20220506124111644.png" alt="image-20220506124111644" style="zoom:70%;" /> 实例化阶段 & 求值阶段

CommonJs和ES Module互相引用

1. 在浏览器下不行互相引用

2. 在特定的Node版本可以支持互相引用

3. webpack环境，两者可以任意互相引用

   ```javascript
   // es module导出
   const name = "bar"
   const age = 100
   export {
     name,
     age
   }
   ```

   ```javascript
   // commonjs导入
   const bar = require("./bar")
   console.log(bar.name, bar.age)
   ```

   ```javascript
   // commonjs导出
   const name = "foo"
   const age = 18
   module.exports = {
     name,
     age
   }
   ```

   ```javascript
   // es module导入
   import { name, age } from './foo'
   console.log(name, age)
   ```



## npm包管理

Node Package Manager，也就是Node包管理器，不仅仅是在Node中，在前端项目中也在使用它来管理依赖的包

npm属于node的一个管理工具，所以需要先安装Node

npm管理的包存放在registry上面，安装一个包也是从registry上面下载

### 配置文件

#### package.json

1. 每一个项目都会有一个对应的配置文件
2. 配置文件会记录着项目的名称、版本号、项目描述等
3. 记录着项目所依赖的其他库的信息和依赖库的版本号

创建配置文件

1. `npm init -y` 所有信息使用默认的

   `npm init` 创建时填写信息

2. 通过脚手架创建项目

package.json的常见属性

- name：项目的名称 必须

- version：当前项目的版本号 必须

- description：项目的基本描述

- author：作者相关信息（发布用）

- license：开源协议（发布用）

- private：记录当前的项目是否是私有，true时，npm不能发布这个项目

- main：设置程序的入口

  这个入口和webpack打包的入口并不冲突，是在发布模块的时候用到的

  例如`const axios = require('axios');`，就是通过找到对应的main属性查找文件的

  <img src="JavaScript.assets/image-20220506162008600.png" alt="image-20220506162008600" style="zoom:40%;" /> 

- scripts：用于配置一些脚本命令，以键值对的形式存在

  配置后可以通过npm run 命令的key来执行这个命令

  （对于常用的start、test、stop、restart可以省略掉run直接通过npm start等方式运行）

- **dependencies**：**无论开发环境还是生成环境都需要依赖的包**

  通常是项目实际开发用到的一些库模块，例如vue、vuex、vue-router、react、react-dom、axios等等

- **devDependencies**：只在**开发环境需要依赖的包**

  比如webpack、babel等

  通过**npm install webpack --save-dev**，安装到devDependencies属性中

- peerDependencies：项目依赖关系是对等依赖

  也就是依赖的一个包，它必须是以另外一个宿主包为前提的

  比如element-plus是依赖于vue3的，ant design是依赖于react、react-dom

- engines：指定Node和NPM的版本号

  在安装的过程中，会先检查对应的引擎版本，如果不符合就会报错

  也可以指定所在的操作系统"os" : [ "darwin", "linux" ]

- browserslist：配置打包后的JavaScript浏览器的兼容情况，为webpack等打包工具服务的一个属性

  （否则需要手动的添加polyfills来让支持某些语法）

#### package-lock.json

**package-lock.json中记载了依赖包的真实版本**

如果项目中既有package也有lock，而且lock的版本符合package的版本要求时，就会按照lock的版本号下载

需要重新下载最新版本就要删除掉package-lock.json文件再下载

package-lock.json的常见属性

- name：项目的名称
- version：项目的版本
- lockfileVersion：lock文件的版本
- requires：是否使用requires来跟踪模块的依赖关系
- dependencies：项目的依赖
  - version：表示实际安装的axios的版本
  - resolved：记录下载的地址，registry仓库中的位置
  - requires：记录当前模块的依赖
  - integrity：用来从缓存中获取索引，再通过索引去获取压缩包文件



### 依赖版本管理

npm的包的版本规范是X.Y.Z

- X主版本号（major）：当做了不兼容的API 修改（可能不兼容之前的版本）
- Y次版本号（minor）：当做了向下兼容的功能性新增（新功能增加，但是兼容之前的版本）
- Z修订号（patch）：当做了向下兼容的问题修正（没有新功能，修复了之前版本的bug）

**^和~的区别**

- ^x.y.z：表示x是保持不变的，y和z永远安装最新的版本
- ~x.y.z：表示x和y保持不变的，z永远安装最新的版本



### npm命令

**npm install** 

- ##### 全局安装

  ```bash
  npm install webpack -g
  ```

  全局安装是直接将某个包安装到全局，通常全局安装的包都是一些工具包：yarn、webpack等

  类似于axios、express、koa等库文件，全局安装了之后并不等于能让所有的项目中都可以使用axios等库

- ##### 项目（局部）安装

  项目安装会在当前目录下生产一个node_modules 文件夹

  - 安装开发和生产依赖

    ```bash
    npm install axios
    npm i axios	
    ```

  - 生产依赖

    ```bash
    npm install webpack --save-dev
    npm install webpack -D
    npm i webpack –D
    ```

  - 安装package.json中的依赖包

    ```bash
    npm install
    ```

**npm install 原理**

<img src="JavaScript.assets/image-20220506185127922.png" alt="image-20220506185127922" style="zoom:70%;" /> 

npm install会检测是否有package-lock.json文件

1. 没有lock文件

   1. 分析依赖关系，这是因为可能包会依赖其他的包，并且多个包之间会产生相同依赖的情况
   2. 从registry仓库中下载压缩包（如果设置了镜像，那么会从镜像服务器下载）
   3. 获取到压缩包后会对压缩包进行缓存
   4. 将压缩包解压到项目的node_modules文件夹中

2. 有lock文件

   1. 检测lock中包的版本是否和package.json中一致（按照semver版本规范检测）

      不一致，那么会重新构建依赖关系，直接走顶层流程

   2. 一致的情况下，会去优先查找缓存

      没有找到，会从registry仓库下载，直接走顶层流程

   3. 查找到，会获取缓存中的压缩文件，并且将压缩文件解压到node_modules文件夹中

**卸载某个依赖包**

```bash
npm uninstall package
npm uninstall package --save-dev
npm uninstall package -D
npm uninstall package -g
```

**强制重新build**

```bash
npm rebuild
```

**清除缓存**

```bash
npm cache clean
```



### cnpm工具

由于网络的原因，不能很好的从https://registry.npmjs.org上下载下来一些需要的包，可以使用cnpm设置为淘宝的镜像

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm config get registry
```

也可以将npm设置成淘宝的镜像

查看npm镜像

```bash
npm config get registry
```

设置npm的镜像

```bash
npm config set registry https://registry.npm.taobao.org
```



### npx工具

npx是npm5.2之后自带的一个命令，使用它来调用项目中的某个模块的指令

例如，使用项目下的而不是全局的webpack来进行操作，使用npx会到当前目录的node_modules/.bin目录下查找对应的命令

```bash
# 在项目根目录下
./node_modules/.bin/webpack --version
# 等于
npx webpack --version
```



### npm发布

发布包

1. 注册npm账号

2. 在命令行登录 `npm login`

3. 修改package.json

   <img src="JavaScript.assets/image-20220506205356382.png" alt="image-20220506205356382" style="zoom:40%;" /> 

4. 发布到npm registry上 `npm publish`

更新仓库

1. 修改版本号
2. 重新发布 `npm publish`

删除发布的包：`npm unpublish`

让发布的包过期：`npm deprecate`



# DOM 树

<img src="JavaScript.assets/image-20211203182322669.png" alt="image-20211203182322669" style="zoom:67%;" /> 

- **文档**：一个页面就是一个文档，DOM 中使用 document 表示

- **元素**：页面中的所有**标签都是元素**，DOM 中使用 element 表示

- **节点**：网页中的**所有内容都是节点**（标签、属性、文本、注释等），DOM 中使用 node 表示

  所有的DOM节点类型都继承自Node接口

  <img src="JavaScript.assets/image-20220508004250344.png" alt="image-20220508004250344" style="zoom:50%;" /> 

- DOM 把以上内容都看做是对象



# 元素

## 获取元素内容

### 根据ID获取

`document.getElementById('id');`

- 参数id区分大小写
- 返回的是一个元素对象



### 根据标签名获取

`document.getElementsByTagName('标签名');`

- 以返回带有**指定标签名的对象的集合**
- 以伪数组的形式存储，操作里面的元素需要遍历
- 得到元素对象是动态的



### 根据类名获取

document.getElementsByClassName(‘类名’);

- HTML5 新增



### 根据选择器获取首个

`document.querySelector('选择器');`

- 根据指定选择器返回**第一个元素对象**
- 选择器需要加符号 `#nav` `.box`
- HTML5 新增



### 根据选择器获取所有

`document.querySelectorAll('选择器');`

- 根据指定选择器返回**所有元素对象**
- 选择器需要加符号 `#nav` `.box`
- HTML5 新增



### 获取body元素

`document.body`



### 获取html元素

`document.documentElement`



## 设置元素内容

### 元素文本内容

- 从起始位置到终止位置的内容，**去除 html 标签**， 同时**去除空格和换行**，可读写

  `element.innerText`

- 从起始位置到终止位置的内容，**保留 html 标签**， 同时**保留空格和换行**，可读写

  `element.innerHTML`



### 元素属性

如：`element.src` 、`element.href`

修改表单元素的属性操作，如：`element.type`、`element.value`、`element.checked`、`element.selected`、`element.disabled`



### 行内样式

`element.style`

JS 里面的样式采取驼峰命名法，比如 `fontSize`、 `backgroundColor`

修改 style 样式操作，产生的是**行内样式**



### 类名样式

`element.className`

会直接更改元素类名，**覆盖原先的类名**

因为class是个保留字，所以用className来操作元素类名属性



### 插入DOM树

文本解析成HTML插入DOM树中的指定位置

不会重新解析正在使用的元素，比直接使用innerHTML操作更快

`element.insertAdjacentHTML(position, text);`

position 参数

- beforebegin： 元素自身的前面
- afterbegin：插入元素内部的第一个子节点之前
- beforeend：插入元素内部的最后一个子节点之后
- afterend：元素自身的后面

```javascript
let html = `<div id="two">two</div>`;
div.insertAdjacentHTML('beforeend', html);
```



## 元素属性操作

### 获取属性

- 获取**内置属性值**（元素本身自带的属性）,可读写

  `element.属性`

- 获取**内置属性和自定义属性**，主要获得自定义的属性

  `element.setAttribute('属性', '值')`

  H5规定自定义属性要以`data-`开头做为属性名

  ```html
  <div data-index="1"></div>
  ```

  ```javascript
  element.getAttribute('data-index');
  ```

- 获取**自定义属性**

  `element.dataset.自定义属性`

  `element.dataset['自定义属性']`

  `dataset`是一个集合，存放了所有以`data`开头的自定义属性

  自定义属性获取时使用驼峰命名法，-（破折号）会被去掉

  H5新增，ie11才开始支持
  
  ```html
  <div data-list-name="andy"></div>
  ```
  
  ```javascript
  var listName = div.dataset.listName
  ```



### 设置属性

- 设置**内置属性**值

  `element.属性 = '值'`

- 设置**内置属性和自定义属性**，主要设置自定义的属性

  `div.setAttribute('属性', '值')`

  ```html
  <div data-index="1"></div>
  ```

  ```javascript
  element.setAttribute('data-index', 2);
  ```

  ```javascript
  // 这里写class，而不是className
  div.setAttribute('class', 'footer');
  ```

- 设置**自定义属性**

  `element.dataset.自定义属性 = '值'`

  `element.dataset['自定义属性'] = '值'`

  ```html
  <div data-date-of-birth>John</div>
  ```

  ```javascript
  element.dataset.dateOfBirth = '1960-10-03'; 
  ```



### 移除属性

`element.removeAttribute('属性');`

没有返回值，不能使用链式调用



## 元素类名操作

### 获取类名

`element.classList`

- 返回元素**所有的类名集合**

- ie10以上支持



### 添加类

`element.classList.add('类名')`

- 类名不带点
- ie10以上支持



### 移除类

`element.classList.remove('类名')`

- 类名不带点
- ie10以上支持



### 切换类

`element.classList.toggle('类名')`

**存在**指定类名就**删除**，**不存在**就**添加**

- 类名不带点
- ie10以上支持



# 节点

## 常见属性

### 节点名称

`var str = node.nodeName`

获取当前节点的节点名称的字符串

- 元素节点，返回元素名称，相当于tagName属性
- 文本节点，返回`#text`字符串

- 只读属性



### 节点类型

`var type = node.nodeType`

返回一个整数，代表这个节点的类型

- 元素节点 ，返回1
- 文本节点，返回3 （文本节点包含文字、空格、换行等）
- 只读属性



### 节点的值

`var str = node.nodeValue`

`node.nodeValue = str`

返回节点值的字符串

- 对于文档节点和**元素节点是不可用的**

  如果是元素节点，返回null

- 文本节点，返回这个文本节点的内容

- 可读写，但不能设置元素节点的值



## 获取节点

### 父节点

`parentNode = node.parentNode`

`parentElement = node.parentElement`

返回**最近的一个父节点**

- 如果指定的节点没有父节点则返回 null

- `parentNode`找的是节点，可能是元素节点，也可能是文档节点或者DocumentFragment节点

  当找到根部document时候就返回`#document`

- `parentElement`找的是元素

  当找到根部document时候就是出现值为null的报错



### 最近祖先元素

`var closestElement = element.closest('选择器');`

如果找到匹配的祖先，**返回最接近的元素**，找不到，返回null

- 选择器可以使用复合选择器，如`p:hover, .toto + q`

- IE不支持




### 子节点集合

`var ndList = parentNode.childNodes`

返回包含指定节点的**子节点的集合**，该集合为**即时更新的集合**

- 返回值里面包含了所有的子节点，包括**元素节点，文本节点**等

- 如果只想要获得里面的元素节点，需要专门处理，所以不提倡使用childNodes

  ```javascript
  var ul = document.querySelector('ul');
  for(var i = 0; i < ul.childNodes.length;i++) {
  	if (ul.childNodes[i].nodeType == 1) {
  	// ul.childNodes[i] 是元素节点
  	console.log(ul.childNodes[i]);
  	}
  }
  ```



### 子元素节点集合

`var elList = parentNode.children`

**返回所有的子元素节点**，其余节点不返回

- 只读属性
- 非标准，但是得到了各个浏览器的支持，放心使用



### 首个子节点

`var childNode = parentNode.firstChild`

返回第一个子节点，找不到则返回null

- 包含所有的节点



### 最后子节点

`var childNode = parentNode.lastChild`

返回最后一个子节点，找不到则返回null

- 包含所有的节点



### 首个子元素节点

`var element = parentNode.firstElementChild`

返回第一个子元素节点，找不到则返回null

- IE9 以上支持

`var element = parentNode.chilren[0]`

- 没有兼容问题



### 最后子元素节点

`var element = parentNode.lastElementChild`

返回最后一个子元素节点，找不到则返回null

- IE9 以上支持

`var element = parentNode.chilren[parentNode.chilren.length - 1]`

- 没有兼容问题



### 下个兄弟节点

`var nextNode = node.nextSibling`

返回下一个兄弟节点，找不到则返回null

- 包含所有的节点



### 上个兄弟节点

`var previousNode = node.previousSibling`

返回上一个兄弟节点，找不到则返回null

- 包含所有的节点



### 下个兄弟元素节点

`var nextNode = node.nextElementSibling`

返回下一个兄弟元素节点，找不到则返回null

- IE9 以上支持

返回下一个兄弟元素节点函数

```javascript
function getNextElementSibling(element) {
	var el = element;
	while (el = el.nextSibling) {
		if (el.nodeType === 1) {
			return el;
		}
	}
	return null;
}
```

- 没有兼容问题



### 上个兄弟元素节点

`var previousNode = node.previousElementSibling`

返回上一个兄弟元素节点，找不到则返回null

- IE9 以上支持

返回上一个兄弟元素节点函数

```javascript
function getPreviousElementSibling(element) {
	var el = element;
	while (el = el.previousSibling) {
		if (el.nodeType === 1) {
			return el;
		}
	}
	return null;
}
```

- 没有兼容问题



## 操作节点

### 创建元素节点

`var element = document.createElement('tagName')`

创建指定的 HTML 元素

- 创建多个元素效率稍低，但是结构更清晰



### 创建元素

`document.write('字符串')`

直接将内容写入页面的内容流，但是**文档流执行完毕，再调用这句话会导致页面全部重绘**

```javascript
document.write('<div>123</div>')
```

`element.innerHTML = '字符串'`

将内容写入某个 DOM 节点，**不会导致页面全部重绘**

- 创建多个元素效率更高（不要拼接字符串，采取数组形式拼接），结构稍微复杂



### 末尾添加节点

`node.appendChild(child)`

一个节点（通常为元素）添加到指定父节点的子节点列表末尾

- 类似于 CSS 里面的after 伪元素。 类似于数组的push



### 前面添加节点

`node.insertBefore(child, 指定元素)`

一个节点添加到父节点的指定子节点前面



### 删除节点

` var oldChild = node.removeChild(child)`

从 DOM 中删除一个子节点，返回删除的节点

























