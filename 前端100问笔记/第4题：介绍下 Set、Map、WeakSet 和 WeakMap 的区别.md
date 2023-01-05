# 第 4 题：介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

> <https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/6>

> <https://es6.ruanyifeng.com/#docs/set-map>

## Set

> 集合 是由一堆无序的、相关联的，且不重复的内存结构【数学中称为元素】组成的组合
> 它类似于数组，但是成员的值都是唯一的，没有重复的值。

```javascript
// 去除数组的重复成员
[...new Set(array)]

// 去除字符串里面的重复字符。
[...new Set('ababbc')].join('')
// "abc"
```

> 向 Set 加入值的时候，不会发生类型转换

Set 实例的属性:

* Set.prototype.constructor：构造函数，默认就是Set函数。
* Set.prototype.size：返回Set实例的成员总数。

操作方法

* Set.prototype.add(value)：添加某个值，返回 Set 结构本身。
* Set.prototype.delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
* Set.prototype.has(value)：返回一个布尔值，表示该值是否为Set的成员。
* Set.prototype.clear()：清除所有成员，没有返回值。

遍历方法

* Set.prototype.keys()：返回键名的遍历器
* Set.prototype.values()：返回键值的遍历器
Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法，可以直接用for...of循环遍历 Set。

```js
Set.prototype[Symbol.iterator] === Set.prototype.values
```

* Set.prototype.entries()：返回键值对的遍历器
* Set.prototype.forEach()：使用回调函数遍历每个成员

> keys方法和values方法的行为完全一致

## WeakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

* 首先，WeakSet 的成员只能是对象，而不能是其他类型的值。
* WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存

> WeakSet 适合临时存放一组对象

WeakSet 结构有以下三个方法。

* WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员。
* WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员。
* WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。

> WeakSet 没有size属性，没有办法遍历它的成员。

## Map

> ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应

任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构（详见《Iterator》一章）都可以当作Map构造函数的参数。这就是说，Set和Map都可以用来生成新的 Map。

```js
const set = new Set([
  ['foo', 1],
  ['bar', 2]
]);
const m1 = new Map(set);
m1.get('foo') // 1

const m2 = new Map([['baz', 3]]);
const m3 = new Map(m2);
m3.get('baz') // 3
```
>
> * 如果对同一个键多次赋值，后面的值将覆盖前面的值。
> * 如果读取一个未知的键，则返回undefined。
> * 只有对同一个对象的引用，Map 结构才将其视为同一个键。这一点要非常小心。Map 的键实际上是跟内存地址绑定的
>

```js
const map = new Map();

map.set(['a'], 555);
map.get(['a'])  // undefined
```

> 作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组

```js
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```

### Map 结构的实例

#### 属性

* size 属性   size属性返回 Map 结构的成员总数。

#### 操作方法

* Map.prototype.set(key, value)  set方法设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。    set方法返回的是当前的Map对象，因此可以采用链式写法。

```js
const map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```

* Map.prototype.get(key)  get方法读取key对应的键值，如果找不到key，返回undefined。
* Map.prototype.has(key)   has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
* Map.prototype.delete(key)    delete方法删除某个键，返回true。如果删除失败，返回false。
* Map.prototype.clear()    clear方法清除所有成员，没有返回值。

遍历方法   Map 的遍历顺序就是插入顺序。

* Map.prototype.keys()：返回键名的遍历器。
* Map.prototype.values()：返回键值的遍历器。
* Map.prototype.entries()：返回所有成员的遍历器。
* Map.prototype.forEach()：遍历 Map 的所有成员。

```js
const reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  // forEach方法的回调函数的this，就指向reporter。
  this.report(key, value);
}, reporter);
```

## WeakMap

WeakMap与Map的区别有两点。 这里的区别和  WeakSet与Set的区别类似

* 首先，WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。
* WeakMap的键名所指向的对象，不计入垃圾回收机制，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存

> WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。
> 弱引用：是指不能确保其引用的对象不会被垃圾回收器回收的引用

```js
const wm = new WeakMap();
let key = {};
let obj = {foo: 1};

wm.set(key, obj);
obj = null;
wm.get(key)
// Object {foo: 1}
```

> 键值obj是正常引用。所以，即使在 WeakMap 外部消除了obj的引用，WeakMap 内部的引用依然存在。

WeakMap只有四个方法可用：get()、set()、has()、delete()。
