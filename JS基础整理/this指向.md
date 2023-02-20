## JS 中的 this 指向问题

通常 this 的指向规则会被总结成四种情况：函数默认调用，隐式绑定(通过其他对象调用)，显式绑定(call,bind,apply 等函数)，new 绑定。

但展开这四点之前，我个人觉得更重要的一点是要理解**函数的 this 指向与它的声明无关，只有函数被调用时，才能获取它的 this 指向。**

### 默认绑定

默认绑定规则，指函数被直接调用时，this 会指向它所处的全局环境。在浏览器运行时，默认绑定规则下的 this 指向了 windows，在 node 环境下则指向了空对象。

### 隐式绑定

通过对象调用函数时，this 会直接指向最后调用它的对象。

默认绑定规则某种程度上也可以看作隐式绑定在 windows 全局环境下的表现。因为函数直接调用可看作由 windows 对象调用，全局变量的定义也可看作把变量挂载到 windows 上。因此在浏览器环境下会出现这样的效果

```javascript
var a = 10;
console.log(window.a); // 10
function getThis() {
  console.log(this === window); // true
  console.log(this.a); // 10
}
getThis();
```

因为 getThis()和 window.getThis()在浏览器环境下其实是同样的效果。
隐式绑定强调了 this 指向是在调用时决定而非声明时决定。如以下代码:

```javascript
function getThis() {
  console.log(this.name);
}

var a = {
  name: "a对象",
  getThis,
};

var b = {
  name: "b对象",
  getThis,
  c: {
    name: "c对象",
    getThis,
  },
};

a.getThis(); // a对象
b.getThis(); // b对象
b.c.getThis(); // c对象
```

函数最后由哪个对象调用，this 就指向哪里。

隐式绑定另一个比较特别的场景是常见的 dom 元素事件绑定。例如给一个按钮绑定点击事件,调用时会发现 this 指向这个按钮元素。原因还是在于函数的 this 指向是被调用时才确定的，而点击事件的内部调用方式就是通过按钮的 dom 元素进行调用，只不过浏览器隐藏了调用代码。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <button onclick="console.log(this);">getThis</button>
  </body>
</html>
```

因为隐式绑定的存在，react 在使用 class 组件绑定 dom 事件时往往需要注意以下 this 指向问题。

### 显式绑定

JS 提供了几个函数，可以显示改变函数被调用时的 this 指向。

#### call 函数

call 函数由函数对象调用(万物皆对象)，可接受多个参数，第一个参数为 this 需要绑定的对象，后面的参数作为调用者的入参执行该函数。如以下代码

```javascript
function getThis() {
  console.log(this.name);
}

var a = {
  name: "a对象",
};

// 将 this 绑定给 a 并执行getThis()
getThis.call(a); // => "a对象"
```

#### apply 函数

apply 函数与 call 函数基本一样，唯一区别是原函数的入参会作为一个数组传进来。

#### bind 函数

bind 函数与 call 函数也相似，只不过 bind 返回的是包含了初始参数和修改了 this 内部指向的一个拷贝，而不是该函数被调用以后得到的结果。

```javascript
function getThis() {
  console.log(this.name);
}

var a = {
  name: "a对象",
};

// 将 this 绑定给 a 并执行getThis()
var bingThis = getThis.bind(a);

bingThis(); // => a对象
```
