## ES5 与 ES6 的作用域

个人工作年限较短，所以工作开发中其实不太接触到 es5 作用域相关的一些奇奇怪怪的特性...现在大部分开发使用的应该都是 es6 和 typescript 了，不过关于 es5 的知识在平时开发和问题排查上还是能用上的。但从个人感觉上看，es5 的作用域似乎确实有一点反直觉。。。

### 块级作用域与声明提升

es5 的 var 声明变量时是不存在块级作用域的。js 中函数及变量的声明会被提升到最顶部，先声明为 undefined(初始化和赋值操作不会被提升)。因此 es5 变量其实可以在声明之前就使用，只不过值是 undefined。另一个点是，js 不存在块级作用域，也就是说 if 判断内部和 for 循环内部用 var 声明的变量，其实是声明到全局了。
这就造就了下面这块奇怪的代码和奇怪的结果。

```javascript
console.log("声明之前变量a的值: ", a); // 声明之前变量a的值:  undefined

if (true) {
  var a = 10;
}

console.log("外部调用的变量a的值: ", a); // 外部调用的变量a的值: 10

// => 声明之前变量a的值:  undefined
// => 外部调用的变量a的值: 10
```

同样的函数声明也有以上效果。

但在 es6 中，引入了 let 与 const 进行变量声明。这时 let 和 const 的作用域运行规则就比较符合直觉。在变量声明之前使用它会抛出错误，在块级作用域外调用它也会抛出错误。

```javascript
console.log("声明之前变量a的值: ", a);

if (true) {
  let a = 10;
}

// ReferenceError: a is not defined
```

```javascript
if (true) {
  let a = 10;
}

console.log("外部调用的变量a的值: ", a);

// ReferenceError: a is not defined
```

### 函数作用域及闭包

es5 不存在块级作用域，但存在函数作用域。这也是早期避免变量污染的手段之一。与函数作用域随之而来的便是闭包。
闭包概念引用 MDN 定义 ：闭包（closure）是一个函数以及其捆绑的周边环境状态（lexical environment，词法环境）的引用的组合。换而言之，闭包让开发者可以从内部函数访问外部函数的作用域。在 JavaScript 中，闭包会随着函数的创建而被同时创建。
