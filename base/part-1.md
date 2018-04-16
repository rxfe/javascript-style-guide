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
  `Symbols`不能完全的被模拟，所以在不能原生支持这个属性的环境中不要去使用它

  - 1.2 复杂类型：通过引用的方式来存取复杂类型
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
  - 2.1 使用`const`来定义你的引用；避免使用`var`。 eslint：` prefer-const, no-const-assign`
  > 为什么？这样可以保证你不能对引用重新赋值，否则可能会导致bug的发生，并且代码也会难以理解
  ```javascript
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
  ```

  - 2.2 如果你需要对引用重新赋值，那么请使用`let`来代替`var`。 eslint:` no-var jscs: disallowVar`
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
  - 3.3 使用对象方法的简写方式
