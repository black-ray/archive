## 数组方法

### 数组分割 chunk

> ```javascript
> /**
> *	array: 需要处理的数组
> * size: 每个数组区块的长度
> * 返回一个包含拆分区块的新数组
> **/
> _.chunk(array, [size=1])
> ```
>
> - 将数组 `array` 拆分成多个 `size` 长度的区块，并将这些区块组成一个新数组（**二维数组**）
>
> - 如果 `array` 无法被分割成全部等长的区块，最后剩余的元素将组成一个区块

```javascript
_.chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]

_.chunk(['a', 'b', 'c', 'd'], 3);
// => [['a', 'b', 'c'], ['d']]

_.chunk(['a', 'b', 'c', 'd'], 5);
// => ['a', 'b', 'c', 'd']
```



### 过滤假值 compact

> ```javascript
> /**
> *	array: 需要处理的数组
> * 返回一个过滤掉假值后的新数组
> **/
> _.compact(array)
> ```
>
> - 去除原数组中的 `false`、 `null`、`0`、`""`、`undefined`、`NaN`、`0n`
> - 返回一个新数组，包含原数组中所有的非假值元素

```javascript
_.compact([0, 1, false, 2, '', 3]);
// => [1, 2, 3]
```



### 反转数组 reverse

> ```typescript
> /**
> * @param array 要修改的数组
> * @returns 反转后的数组
> */
> function reverse<T>(array: T[]):T[]
> ```
>
> - 反转数组，会改变原数组

```javascript
var array = [1, 2, 3];
_.reverse(array);
// array => [3, 2, 1]
```



### 数据拼接 concat

> ```javascript
> /**
> *	array: 需要处理的数组
> * values: 连接的值
> * 返回连接后的新数组
> **/
> _.concat(array, [values])
> ```
>
> - 将 `array` 与任何**数组或值**连接在一起
> - 返回拼接后的新数组，**不会修改原数组**
> - 拼接值 `value` 为剩余参数

```javascript
var array = [1];
var other = _.concat(array, 2, [3], [[4]]);
console.log(other);
// => [1, 2, 3, [4]]
console.log(array);
// => [1]
```



### 去除头/尾部 n 个元素 drop

#### drop 头部去除

```javascript
// array: 需要处理的数组
// n: 要去除的元素个数
// 返回去除元素后的新数组
_.drop(array, [n=1])
```

- 去除 `array` 前面的 `n` 个元素，`n` 默认值为 `1`，**返回去除后的新数组**，**不会改变原数组**
- 等同于 `tail(array)`，去除数组中的第一个元素，返回截取后的数组
- 等同于 `const [, ...tail] = array`

```javascript
const array = [1, 2, 3];
_.drop(array, 1);
// array => [1, 2, 3]
// => [2, 3]
_.drop([1, 2, 3], 2);
// => [3]
_.drop([1, 2, 3], 5);
// => []
_.drop([1, 2, 3], 0);
// => [1, 2, 3]
```



#### dropRight 尾部去除

```javascript
// array: 需要处理的数组
// n: 要去除的元素个数
// 返回去除元素后的新数组
_.dropRight(array, [n=1])
```

- 去除 `array` 尾部的 `n` 个元素，`n` 默认值为 `1`，**返回去除后的新数组**，**不会改变原数组**
- 等同于 `initial(array)`，去除数组中的最后一个元素，返回截取后的数组



#### dropWhile 头部条件去除

```javascript
// array: 需要处理的数组
// predicate: 匹配条件
// 返回去除元素后的新数组
_.dropWhile(array, [predicate=_.identity])
```

- 从头部开始去除 `array` 中满足 `predicate` 条件的元素，**返回去除后的新数组**，**不会改变原数组**
- 从头部开始依次匹配，**匹配函数不满足便会停止执行**，**如果头部元素不匹配便会停止执行**

```javascript
const users = [
  { 'user': 'barney',  'active': false },
  { 'user': 'fred',    'active': false },
  { 'user': 'pebbles', 'active': true }
];
_.dropWhile(users, item => !item.active);
// => [{user: "pebbles", active: true}]
_.dropWhile(users, item => item.active);
// => [{user: "barney", active: false}, {user: "fred", active: false}, {user: "pebbles", active: true}]
_.dropWhile(users, item => item.user !== 'fred');
// => [{user: "fred", active: false}, {user: "pebbles", active: true}]
```



#### dropRightWhile 尾部条件去除

```javascript
// array: 需要处理的数组
// predicate: 匹配条件
// 返回去除元素后的新数组
_.dropRightWhile(array, [predicate=_.identity])
```

- 与 `dropWhile` 相同，但是是从尾部开始匹配



### 获取头/尾部 n 个元素 take

> ### take 头部获取
>
> ```typescript
> /**
> * @param array 需要处理的数组
> * @param n 要提取的元素个数
> * @returns 从起始元素开始n个元素的新数组
> */
> function take<T>(array: T[], n?: number): T[]
> ```
>
> - 从 `array` 数组的起始元素开始提取 `n` 个元素，**返回提取的新数组**，**不会改变原数组**
>
> ```javascript
> _.take([1, 2, 3]);
> // => [1]
> _.take([1, 2, 3], 2);
> // => [1, 2]
> _.take([1, 2, 3], 5);
> // => [1, 2, 3]
> _.take([1, 2, 3], 0);
> // => []
> ```
>
> 
>
> ### takeRight 尾部获取
>
> ```typescript
> /**
> * @param array 需要处理的数组
> * @param n 要提取的元素个数
> * @returns 从结尾元素开始n个元素的新数组
> */
> function takeRight<T>(array: T[], n?: number): T[];
> ```
>
> - 从 `array` 数组的最后一个元素开始提取 `n` 个元素，**返回提取的新数组**，**不会改变原数组**
>
> 
>
> ### takeWhile 头部条件获取
>
> ```typescript
> /**
> * @param array 需要处理的数组
> * @param predicate 匹配条件
> * @returns 从起始元素开始n个元素的新数组
> */
> function takeWhile<T>(array: T[], predicate?: (value: T, index: number, collection: T[]) => unknown): T[]
> ```
>
> - 从头部开始提取 `array` 中满足 `predicate` 条件的元素，**返回提取后的新数组**，**不会改变原数组**
>   从头部开始依次匹配，**匹配函数不满足便会停止执行**，**如果头部元素不匹配便会停止执行**
>
> ```javascript
> const users = [
>   { 'user': 'barney',  'active': false },
>   { 'user': 'fred',    'active': false },
>   { 'user': 'pebbles', 'active': true },
>   { 'user': 'kitten', 'active': false }
> ];
> _.takeWhile(users, item => !item.active);
> // => [{user: "barney", active: false}, {user: "fred", active: false}]
> _.takeWhile(users, item => item.active);
> // => []
> _.takeWhile(users, item => item.user !== 'fred');
> // => [{user: "barney", active: false}]
> ```
>
> 
>
> ### takeRightWhile 尾部条件获取
>
> ```typescript
> /**
> * @param array 需要处理的数组
> * @param predicate 匹配条件
> * @returns 从结尾元素开始n个元素的新数组
> */
> function takeRightWhile<T>(array: T[], predicate?: (value: T, index: number, collection: T[]) => unknown): T[];
> ```
>
> - 与 `takeWhile` 相同，但是是从尾部开始匹配



### 求数组差集 difference

- `difference(A, B)` 相当于求 A - B，即 **A 和 B 的差集，即属于 A 但是不属于 B 的元素**

> ### difference 求差集
>
> ```typescript
> /**
> * @param arrays 待检查的数组
> * @param values 排除值数组
> * @returns 返回排除的值后的新数组
> */
> function difference<T>(array: T[], ...values: T[]): T[];
> ```
>
> - `difference` **为数组 `array` 排除掉给定数组 `value` 中的值**，`value` 中的每个值不包含在其他给定的 `array` 数组中，**返回一个新数组**
>
> ```javascript
> _.difference([3, 2, 1], [4, 2])
> // 等价于
> [3, 2, 1].filter(item => ![4, 2].includes(item))
> ```
>
> ```javascript
> _.difference([3, 2, 1], [4, 2]);
> // => [3, 1]
> _.difference([1, 2, 3, 4, 5], [1, 3], [2, 4]);
> // => [5]
> ```
>
> 
>
> ### differenceBy 按属性求差集（每个元素处理后求差集）
>
> ```typescript
> /**
> * @param array 待检查的数组
> * @param values 排除值数组
> * @param iteratee 调用每个元素的迭代器
> * @returns 排除的值后的新数组
> */
> function differenceBy<T1, T2>(array: T1[], values: T2[], iteratee: (value: T1 | T2) => unknown): T1[];
> ```
>
> - `differenceBy` 接受一个迭代器参数，对 `array` 和 `values` 数组中的**每个元素先调用迭加器函数进行处理，然后对比处理后的值**
>
> ```javascript
> const array = [{ property: 1, propertyName: 'name' }, { property: 2, propertyName: 'age' }];
> const values = [{ property: 1, propertyName: 'phone' }];
> _.differenceBy(array, values, item => item.property);
> // => [{property: 2, propertyName: "age"}]
> _.differenceBy([3.1, 2.2, 1.3], [4.4, 2.5], Math.floor);
> // => [3.1, 1.3]
> ```
>
> 
>
> ### differenceWith 按条件求差集
>
> ```typescript
> /**
> * @param array 待检查的数组
> * @param values 排除值数组
> * @param comparator 比较每个元素的比较器
> * @returns 排除的值后的新数组
> */
> function differenceWith<T1, T2>(array: T1[], values: T2[], comparator: (a: T1, b: T2) => boolean): T1[];
> ```
>
> - `differenceWith` 接受一个**返回值为布尔值的比较器来自定义比较规则**，在比较器的规则下将 `values` 中的值与 `array` 中的值对比，比较器返回 `true` 表明 `array` 中该值需要被剔除
>
> ```javascript
> const array = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];
> const values = [{ 'x': 1, 'y': 2 }];
> _.differenceWith(array, values, _.isEqual);
> // => [{x: 2, y: 1}]
> ```



### 求数组交集 intersection

- `intersection(A, B)` 相当于求 A ∩ B，即 **A 和 B 的交集，即既属于 A 又属于 B 的元素**

> ### intersection 求交集
>
> ```typescript
> /**
> * @param arrays 待检查的数组
> * @returns 数组交集元素的新数组
> */
> function intersection<T>(...arrays: T[]): T[]
> ```
>
> - 查找给定的**多个数组中都包含的元素**，返回包含交集元素的新数组
>
> ```javascript
> _.intersection([2, 1], [4, 2], [1, 2]);
> // => [2]
> ```
>
> 
>
> ### intersectionBy 按属性求交集（每个元素处理后求交集）
>
> ```typescript
> /**
> * @param array 待检查的数组
> * @param values 被相交数组
> * @param iteratee 调用每个元素的迭代器
> * @returns 数组交集元素的新数组
> */
> function intersectionBy<T1, T2>(array: T1[], values: T2[], iteratee: (value: T1 | T2) => unknown): T1[];
> ```
>
> - 接受迭代器参数，对 `array` 和 `values` 数组中的**每个元素先调用迭代器函数进行处理，然后对比处理后的值**
>
> ```javascript
> _.intersectionBy([{ x: 1, y: 1 }], [{ x: 2, y: 1 }, { x: 1, y: 2 }], item => item.x);
> // => [{x: 1, y: 1}]
> _.intersectionBy([2.1, 1.2], [4.3, 2.4], Math.floor);
> // => [2.1]
> ```
>
> 
>
> ### intersectionWith 按条件求交集
>
> ```typescript
> /**
> * @param array 待检查的数组
> * @param values 被相交数组
> * @param comparator 比较每个元素的比较器
> * @returns 数组交集元素的新数组
> */
> function intersectionWith<T1, T2>(array: T1[], values: T2[], comparator: (a: T1, b: T2) => boolean): T1[]
> ```
>
> - 接受一个**返回值为布尔值的比较器来自定义比较规则**，在比较器的规则下将 `values` 中的值与 `array` 中的值对比，比较器返回 `true` 表明 `array` 中该值为交集对象
>
> ```javascript
> var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];
> var others = [{ 'x': 1, 'y': 1 }, { 'x': 1, 'y': 2 }];
> _.intersectionWith(objects, others, _.isEqual);
> // => [{ 'x': 1, 'y': 2 }]
> ```



### 求数组并集 union

- `union(A, B)` 相当于求 A + B，即 **A 和 B 的并集，即 A 与 B 的元素集合**

> ### union 求并集
>
> ```typescript
> /**
> * @param arrays 待检查的数组
> * @returns 数组并集元素的新数组
> */
> function union<T>(...arrays: T[]): T[];
> ```
>
> - **多个数组元素的并集**，按顺序返回，返回数组的元素是唯一的
> - 数组元素是引用数据类型时，使用 `unionWith` 和 `isEqual`
>
> ```javascript
> _.union([2], [1, 2]);
> // => [2, 1]
> ```
>
> ```javascript
> // 引用类型
> _.union([{ 'x': 1 }], [{ 'x': 2 }, { 'x': 1 }]);
> // => [{x: 1}, {x: 2}, {x: 1}]
> ```
>
> 
>
> ### unionBy 按属性求并集（每个元素处理后求并集）
>
> ```typescript
> /**
> * @param arrays1 待检查的数组
> * @param arrays2 待检查的数组
> * @param iteratee 调用每个元素的迭代器
> * @returns 数组并集元素的新数组
> */
> function unionBy<T>(arrays1: T[], arrays2: T[], iteratee?: (value: T) => unknown): T[];
> ```
>
> - 接受迭代器参数，对 `array` 和 `values` 数组中的**每个元素先调用迭代器函数进行处理，然后对比处理后的值**
>
> ```javascript
> _.unionBy([{ x: 1 }], [{ x: 2 }, { x: 1 }], item => item.x);
> // => [{ 'x': 1 }, { 'x': 2 }]
> _.unionBy([2.1], [1.2, 2.3], Math.floor);
> // => [2.1, 1.2]
> ```
>
> 
>
> ### unionWith 按条件求并集
>
> ```typescript
> /**
> * @param array 待检查的数组
> * @param values 待检查的数组
> * @param comparator 比较每个元素的比较器
> * @returns 数组并集元素的新数组
> */
> function unionWith<T>(arrays: T[], arrays2: T[], comparator?: (a: T, b: T) => boolean): T[];
> ```
>
> - 接受一个**返回值为布尔值的比较器来自定义比较规则**，在比较器的规则下将 `values` 中的值与 `array` 中的值对比，比较器返回 `true` 表明 `array` 中该值为并集对象
>
> ```javascript
> var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];
> var others = [{ 'x': 1, 'y': 1 }, { 'x': 1, 'y': 2 }];
> _.unionWith(objects, others, _.isEqual);
> // => [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }, { 'x': 1, 'y': 1 }]
> ```



### 求数组对称差集 xor

- `xor(A, B)` 相当于求 A △ B，即 **A 和 B 的交集的取反，即保留 A 和 B 不同的元素**

> ### xor 求对称差集
>
> ```typescript
> /**
> * @param arrays 待检查的数组
> * @returns 数组对称差集元素的新数组
> */
> function xor<T>(...arrays: T[]): T[];
> ```
>
> - 多个数组**不相交元素**，返回值的顺序取决于数组的出现顺序
>
> ```javascript
> _.xor([2, 1], [2, 3]);
> // => [1, 3]
> ```
>
> 
>
> ### xorBy 按属性求对称差集（每个元素处理后求对称差集）
>
> ```typescript
> /**
> * @param arrays1 待检查的数组
> * @param arrays2 待检查的数组
> * @param iteratee 调用每个元素的迭代器
> * @returns 数组对称差集元素的新数组
> */
> function xorBy<T>(arrays: T[], arrays2: T[], iteratee?: (value: T) => unknown): T[];
> ```
>
> - 接受迭代器参数，对 `array` 和 `values` 数组中的**每个元素先调用迭代器函数进行处理，然后对比处理后的值**
>
> ```javascript
> _.xorBy([2.1, 1.2], [2.3, 3.4], Math.floor);
> // => [1.2, 3.4]
> _.xorBy([{ x: 1 }], [{ x: 2 }, { x: 1 }], item => item.x);
> // => [{ 'x': 2 }]
> ```
>
> 
>
> ### xorWith 按条件求对称差集
>
> ```typescript
> /**
> * @param array 待检查的数组
> * @param values 待检查的数组
> * @param comparator 比较每个元素的比较器
> * @returns 数组对称差集元素的新数组
> */
> function xorWith<T>(arrays: T[], arrays2: T[], comparator?: (a: T, b: T) => boolean): T[];
> ```
>
> - 接受一个**返回值为布尔值的比较器来自定义比较规则**，在比较器的规则下将 `values` 中的值与 `array` 中的值对比，比较器返回 `true` 表明 `array` 中该值为对称差集对象
>
> ```javascript
> var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];
> var others = [{ 'x': 1, 'y': 1 }, { 'x': 1, 'y': 2 }];
> _.xorWith(objects, others, _.isEqual);
> // => [{ 'x': 2, 'y': 1 }, { 'x': 1, 'y': 1 }]
> ```



### 移除指定元素 pull

#### pull / pullAll 移除指定元素

```typescript
/**
* @param array 要修改的数组
* @param values 要删除的值
* @returns 删除后的数组，会改变原数组
*/
function pull<T>(array: T[], ...values: T[]): T[]

/**
* @param array 要修改的数组
* @param values 要删除值的数组
* @returns 删除后的数组，会改变原数组
*/
function pullAll<T>(array: T[], values: T[]): T[]
```

- 移除数组 `array` 中**所有和给定值相等的元素**，会改变原数组
- 相比于 `difference` 方法，`pull` 会改变元数组
- `pullAll` 的 `values` 参数为数组，`pull` 为元素

```javascript
const array = [1, 2, 3, 1, 2, 3];
_.pull(array, 2, 3);
// array => [1, 1]
```

```javascript
const array = [1, 2, 3, 1, 2, 3];
_.pullAll(array, [2, 3]);
// array => [1, 1]
```



#### pullAllBy 按属性指定移除元素

```typescript
/**
* @param array 要修改的数组
* @param values 要删除值的数组
* @param iteratee 调用每个元素的迭代器
* @returns 删除后的数组，会改变原数组
*/
function pullAllBy<T>(array: T[], values?: T[], iteratee?: (value: T) => unknown): T[]
```

- 接受迭代器参数，对 `array` 和 `values` 数组中的**每个元素先调用迭代器函数进行处理，然后对比处理后的值**

```javascript
const array = [{ 'x': 1 }, { 'x': 2 }, { 'x': 3 }, { 'x': 1 }];
_.pullAllBy(array, [{ 'x': 1 }, { 'x': 3 }], item => item.x);
// array => [{ 'x': 2 }]
```



#### pullAllWith 按条件指定移除元素

```typescript
/**
* @param array 要修改的数组
* @param values 要删除值的数组
* @param comparator 比较每个元素的比较器
* @returns 删除后的数组，会改变原数组
*/
function pullAllWith<T>(array: T[], values?: T[], comparator?: (a: T, b: T) => boolean): T[]
```

- 接受一个**返回值为布尔值的比较器来自定义比较规则**，在比较器的规则下将 `values` 中的值与 `array` 中的值对比，比较器返回 `true` 表明 `array` 中该值要被剔除

```javascript
const array = [{ 'x': 1, 'y': 2 }, { 'x': 3, 'y': 4 }, { 'x': 5, 'y': 6 }];
_.pullAllWith(array, [{ 'x': 3, 'y': 4 }], _.isEqual);
// array => [{ 'x': 1, 'y': 2 }, { 'x': 5, 'y': 6 }]
```



#### pullAt 按索引移除元素

```typescript
/**
* @param array 要修改的数组
* @param indexes 要移除元素的索引
* @returns 移除元素组成的新数组
*/
pullAt<T>(array: T[], ...indexes: number[]): T[];
```

- 根据索引 `indexes`，移除 `array` 中对应的元素，并返回被移除元素的数组
- **会改变原数组**

```javascript
var array = [5, 10, 15, 20];
var evens = _.pullAt(array, 1, 3);
// array => [5, 15]
// evens => [10, 20]
```



### 数组去重 uniq

#### uniq 去重

```typescript
/**
* @param array 要检查的数组
* @returns 去重后的新数组
*/
function uniq<T>(array: T[]): T[];
```

- 创建去重后的新数组，只有第一次出现的元素才会被保留
- 数组元素是引用数据类型时，使用 `uniqWith` 和 `isEqual`

```javascript
_.uniq([2, 1, 2]);
// => [2, 1]
```

```javascript
_.uniq([{ 'x': 1 }, { 'x': 2 }, { 'x': 1 }]);
// => [{ 'x': 1 }, { 'x': 2 }, { 'x': 1 }]
```



#### uniqBy 按属性去重

```typescript
/**
* @param array 待检查的数组
* @param iteratee 调用每个元素的迭代器
* @returns 去重后的新数组
*/
function uniqBy<T>(array: T[], iteratee: (value: T) => unknown): T[];
```

- 接受迭代器参数，对 `array` 数组中的**每个元素先调用迭代器函数进行处理，然后对比处理后的值**
- 可以指定回调函数处理每个元素处理后去重

```javascript
_.uniqBy([2.1, 1.2, 2.3], Math.floor);
// => [2.1, 1.2]
_.uniqBy([{ x: 1 }, { x: 2 }, { x: 1 }], item => item.x);
// => [{ 'x': 1 }, { 'x': 2 }]
```



#### uniqWith 按条件去重

```typescript
/**
* @param array 待检查的数组
* @param comparator 比较每个元素的比较器
* @returns 去重后的新数组
*/
function uniqWith<T>(array: T[], comparator?: (a: T, b: T) => boolean): T[]
```

- 接受一个**返回值为布尔值的比较器来自定义比较规则**，比较器返回 `true` 表明 `array` 中该值要被去重

```javascript
_.uniqWith([{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }, { 'x': 1, 'y': 2 }], _.isEqual);
// => [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }]
```



### 数组元素分组 zip

#### zip 元素分组

```typescript
/**
* @param arrays 要处理的数组
* @returns 分组元素的新数组
*/
function zip<T>(...arrays: T[]): T[][];
```

- 创建一个分组元素的数组，多个**给定数组中相同索引的元素**打包**到分组数组的相同索引中**

```javascript
_.zip(['fred', 'barney'], [30, 40], [true, false]);
// => [['fred', 30, true], ['barney', 40, false]]
```



#### unzip 元素重组

```javascript
/**
* @param arrays 要处理的分组元素数组
* @returns 重组元素的新数组
*/
function unzip<T>(array: T[][]): T[][];
```

- 将分组元素返回到打包前的结构

```javascript
_.unzip([['fred', 30, true], ['barney', 40, false]]);
// => [['fred', 'barney'], [30, 40], [true, false]]
```



#### zipWith 按条件分组

```typescript
/**
* @param arrays 要处理的数组
* @param iteratee 调用每个元素的迭代器
* @returns 重组元素的新数组
*/
function zipWith<T, TResult>(arrays: T[], iteratee: (value1: T) => TResult): TResult[];
```

- 
