## 类型
  - 1.1 基本类型： 直接存取基本类型。
    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`

  ```javascript
  const foo = 1;
  let bar = foo;

  bar = 9;

  console.log(foo, bar); // => 1, 9
  ```
  `Symbols`不能完全的被模拟，所以在不能原生支持这个属性的环境中不要去使用它

  - 1.2 复杂类型：通过引用的方式来存取复杂类型
    - `object`
    - `array`
    - `function`

  ```javascript
  const foo = [1, 2];
  const bar = foo;

  bar[0] = 9;

  console.log(foo[0], bar[0]); // => 9, 9
  ```
## 引用
  - 2.1 使用`const`来定义你的引用；避免使用`var`。 eslint：` prefer-const, no-const-assign`
  > 为什么？这样可以保证你不能对引用重新赋值，否则可能会导致bug的发生，并且代码也会难以理解
  ```javascript
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
  ```

  - 2.2 如果你需要对引用重新赋值，那么请使用`let`来代替`var`。 eslint:` no-var jscs: disallowVar`
  > 为什么?`let`是块级作用域,而`var`是函数作用域
  ```javascript
  // bad
  var count = 1;
  if (true) {
    count += 1;
  }

  // good, use the let.
  let count = 1;
  if (true) {
    count += 1;
  }
  ```
  - 2.3 注意： `let`和`const`都是块级作用域
  ```javascript
  // const and let only exist in the blocks they are defined in.
  {
    let a = 1;
    const b = 1;
  }
  console.log(a); // ReferenceError
  console.log(b); // ReferenceError
  ```

## 对象
  - 3.1 使用字面量的语法来创建对象. eslint: `no-new-object`
  ```javascript
  // bad
  const item = new Object();

  // good
  const item = {};
  ```

  - 3.2 当属性名是动态生成的时候，使用可被计算生成的属性名
  > 为什么？这样你能在一个地方定义所有的属性
  ```javascript
  function getKey(k) {
    return `a key named ${k}`;
  }

  // bad
  const obj = {
    id: 5,
    name: 'San Francisco',
  };
  obj[getKey('enabled')] = true;

  // good
  const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true,
  };
  ```
  - 3.3 使用对象方法的简写方式. `eslint: object-shorthand jscs: requireEnhancedObjectLiterals`
  ```javascript
  // bad
  const atom = {
    value: 1,

    addValue: function (value) {
      return atom.value + value;
    },
  };

  // good
  const atom = {
    value: 1,

    addValue(value) {
      return atom.value + value;
    },
  };
  ```

  - 3.4 使用属性值的简写方式. ` eslint: object-shorthand jscs: requireEnhancedObjectLiterals`
  > 为什么？写起来更简短，并且更容易描述
  ```javascript
  const lukeSkywalker = 'Luke Skywalker';

  // bad
  const obj = {
    lukeSkywalker: lukeSkywalker,
  };

  // good
  const obj = {
    lukeSkywalker,
  };
  ```


- 3.5 把简写的属性归为一组放到对象声明的开头
> 为什么？这样更容易分清哪些属性利用了简写方式
```javascript
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
};

// good
const obj = {
  lukeSkywalker,
  anakinSkywalker,
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
};
```

- 3.6 对属性使用引号是不建议的.`eslint: quote-props jscs: disallowQuotedKeysInObjects`
> 我们觉得不使用引号是更容易阅读的。 对于代码高亮也有一定的改善，对于一些JS引擎也更友好
```javascript
// bad
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// good
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```

- 3.7 不要直接调用`Object.prototype`上的方法，比如`hasOwnProperty`、`propertyIsEnumerable` 、`isPrototypeOf`等
> 为什么？这些方法可能会被对象里的属性覆盖，考虑这些情况 `{ hasOwnProperty: false}`, 或者这个对象可能是一个`null`对象（`Object.create(null)`）
```javascript
// bad
console.log(object.hasOwnProperty(key));

// good
console.log(Object.prototype.hasOwnProperty.call(object, key));

// best
const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
/* or */
import has from 'has'; // https://www.npmjs.com/package/has
// ...
console.log(has.call(object, key));
```

- 3.8 使用`...`操作符来代替`Object.assign`来对对象进行`shallow-copy`。使用`rest`操作来获得一个删除了某些确定属性的对象
```javascript
// very bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```

## 数组
- 4.1 使用字面值创建数组。`eslint: no-array-constructor`
```javascript
// bad
const items = new Array();

// good
const items = [];
```

- 4.2 向数组添加元素时使用 Arrary#push 替代直接赋值。
```javascript
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```
- 4.3 使用拓展运算符 ... 复制数组
```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```
- 4.4 把一个类数组转换成数组时，优先使用`...`操作，而不是`Array.from`
```javascript
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```

- 4.5 当对类数组进行`map`操作时，请使用`Array.from`而不是`...`操作，因为这样可以避免创建中间数组
```javascript
// bad
const baz = [...foo].map(bar);

// good
const baz = Array.from(foo, bar);
```

- 4.6 在数组方法的回调中，需要有`return`的值. 但是如果在函数体重仅仅只有一行表达式，并且这个表达式没有副作用，那么可以省略`return`。 `eslint: array-callback-return`
```javascript
// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map(x => x + 1);

// bad - no returned value means `acc` becomes undefined after the first iteration
[[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
  const flatten = acc.concat(item);
  acc[index] = flatten;
});

// good
[[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
  const flatten = acc.concat(item);
  acc[index] = flatten;
  return flatten;
});

// bad
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  } else {
    return false;
  }
});

// good
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  }

  return false;
});
```

- 4.7 当数组有多行时，在中括号开始和结束的地方需要进行换行
```javascript
// bad
const arr = [
  [0, 1], [2, 3], [4, 5],
];

const objectInArray = [{
  id: 1,
}, {
  id: 2,
}];

const numberInArray = [
  1, 2,
];

// good
const arr = [[0, 1], [2, 3], [4, 5]];

const objectInArray = [
  {
    id: 1,
  },
  {
    id: 2,
  },
];

const numberInArray = [
  1,
  2,
];

```
