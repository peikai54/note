## ES6 的面向对象实现

javascript 在 ES6 中引入了一个语法糖 class，让 javascript 的开发和使用上更接近常规的面向对象语言。虽然 class 语法糖还是基于原型链实现的，但实际开发时 class 比起 es5 的原型链写法要直观很多。

### ES6 面向对象——class 的封装

#### ES6 的 class 封装的基本语法

```javascript
class Dog {
  age;
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  sayName() {
    console.log(this.name);
  }
}

const dog1 = new Dog("dog1");

dog1.sayName();
```

写法上 class 语法糖比 es5 的构造函数直观了很多。常规的构造函数 constructor 会在新建对象时调用，sayName 则是挂载在 Dog 类上的方法。定义属性时既可以在 constructor 构造函数中定义，也可以在 class 顶层直接定义。而 class 的实现原理也很简单:

```javascript
class Dog {
  constructor(name, age) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}

console.log(typeof Dog); // function
console.log(Dog.prototype.sayName); // [Function: sayName]
```

从以上代码中可以看到，class 本身也是一个构造函数，挂载在上面的方法 sayName 其实也是挂载在 Dog 构造函数的 prototype 上的。所以 class 这个语法糖的底层实现上跟 ES5 的原型链写法一致的。但 class 跟普通构造函数的一个主要区别是 class 必须使用 new 进行调用。

#### 静态属性与静态方法

静态属性与静态方法指直接定义在类上，通过类访问、调用而不被类创造的对象继承的方法。（静态属性和静态方法可以被类继承）

```javascript
class Dog {
  static type = "dog";

  static test() {
    console.log("static test");
  }

  constructor() {}
}

const dog = new Dog("dog1");

Dog.test(); // static test

console.log(Dog.type); // dog

dog.test(); // 报错 TypeError: dog.test is not a function
```

使用实例对象 dog 调用 Dog 类上的静态方法时会报错。

### ES6 面向对象的继承

ES6 语法糖下，javascript 面向对象的继承也非常直观，借助 extend 字段便可直接继承父类的属性和方法。（终于告别 ES5 那别扭的写法了）

```javascript
class Animal {
  constructor() {}
  type = "animal";

  born() {
    console.log("i am born");
  }
}

class Dog extends Animal {
  constructor() {
    super();
  }
}

const dog = new Dog();
dog.born(); // i am born
console.log(dog.type); // animal
```

### ES6 面向对象的多态

ES6 想要实现对父类方法的重写也是很简单的事情，在子类直接重新定义该方法就行

```javascript
class Animal {
  constructor() {}
  type = "animal";

  born() {
    console.log("i am born");
  }
}

class Dog extends Animal {
  born() {
    console.log("a dog born");
  }

  constructor() {
    super();
  }
}

const dog = new Dog();
dog.born(); // a dog born
```

虽然对前端来说，class 不算非常重要的资源
