# 一定要学会的 JavaScript 新特性

## 1. 使用"Object.hasOwn"替代“in”操作符

• in -  如果指定的属性位于对象或其原型链中，“in”运算符将返回true。

```js
const Person = function (age) {
this.age = age
}
Person.prototype.name = 'fatfish'

const p1 = new Person(24)
console.log('age' in p1) // true
console.log('name' in p1) // true  注意这里
```

• obj.hasOwnProperty - 返回一个布尔值，表示对象自身属性中是否具有对应的值（原型链上的属性不会读取）。

```js
const Person = function (age) {
this.age = age
}
Person.prototype.name = 'fatfish'

const p1 = new Person(24)
console.log(p1.hasOwnProperty('age')) // true
console.log(p1.hasOwnProperty('name')) // fasle  注意这里

// obj.hasOwnProperty已经可以过滤掉原型链上的属性，但在某些情况下，它还是不安全。
Object.create(null).hasOwnProperty('name')   // Uncaught TypeError: Object.create(...).hasOwnProperty is not a function
```

• Object.hasOwn

```js
let object = { age: 24 }
Object.hasOwn(object, 'age') // true
let object2 = Object.create({ age: 24 })
Object.hasOwn(object2, 'age') // false  
let object3 = Object.create(null)
Object.hasOwn(object3, 'age') // false
```

## 2. 使用"#"声明私有属性

用_表示私有属性，但它并不靠谱，还是会被外部修改。

```js
class Person {
  constructor (name) {
    this._money = 1
    this.name = name
  }
  get money () {
    return this._money
  }
  set money (money) {
    this._money = money
  }
  showMoney () {
    console.log(this._money)
  }
}
const p1 = new Person('fatfish')
console.log(p1.money) // 1
console.log(p1._money) // 1
p1._money = 2 // 依旧可以从外部修改_money属性，所以这种做法并不安全
console.log(p1.money) // 2
console.log(p1._money) // 2
```

使用“#”实现真正私有属性

```js
class Person {
  #money=1
  constructor (name) {
    this.name = name
  }
  get money () {
    return this.#money
  }
  set money (money) {
    this.#money = money
  }
  showMoney () {
    console.log(this.#money)
  }
}
const p1 = new Person('fatfish')
console.log(p1.money) // 1
// p1.#money = 2 // 没法从外部直接修改
p1.money = 2
console.log(p1.money) // 2
console.log(p1.#money) // Uncaught SyntaxError: Private field '#money' must be declared in an enclosing class
```
