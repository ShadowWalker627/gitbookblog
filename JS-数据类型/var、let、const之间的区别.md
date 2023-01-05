# var、let、const之间的区别

## var

- 用var声明的变量既是全局变量，也是顶层变量
- 使用var声明的变量存在变量提升的情况
- 使用var，我们能够对一个变量进行多次声明，后面声明的变量会覆盖前面的变量声明
- 在函数中使用使用var声明变量时候，该变量是局部的；而如果在函数内不使用var，该变量是全局的

```JS
  var a = 20
  function change(){
      var a = 30
  }
  change()
  console.log(a) // 20 
```

```js
  var a = 20
  function change(){
    a = 30
  }
  change()
  console.log(a) // 30 
```

## let

- 所声明的变量，只在let命令所在的代码块内有效
- 不存在变量提升，使用let声明变量前，该变量都不可用，也就是“暂时性死区”
- let不允许在相同作用域中重复声明

## const

- const声明一个只读的常量，一旦声明，常量的值就不能改变
- const一旦声明变量，就必须立即初始化

```js
const a;
// SyntaxError: Missing initializer in const declaration
```

- 用var或let声明过变量，再用const声明同样会报错
- const实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动
  - 简单类型的数据，值就保存在变量指向的那个内存地址，因此等同于常量
  - 对于复杂类型的数据，变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的，并不能确保改变量的结构不变

```js
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```
