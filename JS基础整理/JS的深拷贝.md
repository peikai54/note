## JS 的深拷贝

### 浅拷贝与深拷贝

浅拷贝是创建一个新对象，把原对象所有的属性值直接精准拷贝过去。如果属性是一个基本类型，则拷贝了它的值，如果属性是复杂类型，拷贝的则是指向堆内存的一个引用地址。因此浅拷贝下，内部对象的值的修改会污染到原对象。

```javascript
function clone(obj) {
  if (Array.isArray(obj)) {
    let result = [...obj];
    return result;
  }

  if (typeof obj === "object") {
    let result = {};
    for (let key in obj) {
      result[key] = obj[key];
    }
    return result;
  }
}

const A = {
  test: {
    value: 1,
  },
};
const B = clone(A);
console.log(A.test.value); // 1
B.test.value = 2;
console.log(A.test.value); // 2 源数据被污染
```

### 深拷贝

深拷贝是将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象。

深拷贝的实现也很简单，拷贝对象时只要检测到属性值是个复杂类型，进行递归就可以进行深拷贝。

```javascript
function DeepClone(obj) {
  function clone(obj) {
    let result = {};
    for (let key in obj) {
      if (typeof obj[key] === "function" || typeof obj[key] === "object") {
        result[key] = clone(obj[key]);
      } else {
        result[key] = obj[key];
      }
    }
    return result;
  }

  return clone(obj);
}
```

除了这个办法，**还有一种深拷贝实现是利用 JSON 的 API 将对象转化成字符串再转化成 JSON 对象，但实际开发里这种办法要慎用**。因为 JSON.stringfy()只能处理纯 JSON 对象，不能处理 js 里的日期对象、字符串对象和函数，容易给自己挖坑。

### 循环引用及解决办法

假设存在两个对象 A 和 B，如果 A 的属性值 b 指向 B，B 的 属性值 a 指向 A，就会出现循环引用。当使用递归进行深拷贝时，会进入死循环导致栈溢出。

```javascript
const A = { test: "测试" };

const B = {
  a: A,
};

DeepClone(A); // RangeError: Maximum call stack size exceeded
```

处理循环引用时，可以使用一个 Map 来存储已经被拷贝过的对象。递归调用深拷贝前，先到 map 进行查询，若该对象已经被引用过，则直接返回 map 缓存的结果而不是接续递归解开引用，就能处理深拷贝的问题。同时，也可以直接抛出错误，提醒开发者不能使用深拷贝。

```javascript
function DeepClone(obj) {
  const map = new Map();

  function clone(obj) {
    let result = {};
    for (let key in obj) {
      if (map.get(key)) {
        result[key] = map.get(key);
        continue;
      }
      map.set(key, obj[key]);
      if (typeof obj[key] === "function" || typeof obj[key] === "object") {
        result[key] = clone(obj[key]);
      } else {
        result[key] = obj[key];
      }
    }
    return result;
  }

  return clone(obj);
}

const A = { test: "测试" };

const B = {
  a: A,
};
```
