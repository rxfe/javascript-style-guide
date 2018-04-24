## 类和构造器
- 9.1 总是使用 `class`。避免直接操作 `prototype` 。
> 为什么? 因为 class 语法更为简洁更易读。
```javascript
// bad
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};

// good
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```

- 9.2 使用 `extends` 继承。
> 为什么？因为 `extends` 是一个内建的原型继承方法并且不会破坏 `instanceof`。
```javascript
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
  return this.queue[0];
};

// good
class PeekableQueue extends Queue {
  peek() {
    return this.queue[0];
  }
}
```

- 9.3 方法可以返回 this 来帮助链式调用。
```javascript
// bad
Jedi.prototype.jump = function () {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function (height) {
  this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }
}

const luke = new Jedi();

luke.jump()
  .setHeight(20);
```

- 9.4 可以写一个自定义的 toString() 方法，但要确保它能正常运行并且不会引起副作用
```js
class Jedi {
  constructor(options = {}) {
    this.name = options.name || 'no name';
  }

  getName() {
    return this.name;
  }

  toString() {
    return `Jedi - ${this.getName()}`;
  }
}
```

- 9.5 在一个类中，如果没有显式的编写`constructor`，那么会有一个默认的`constructor`, 所以不需要写一个空的`constructor`或者去代理父类的`constructor`
```js
// bad
class Jedi {
  constructor() {}

  getName() {
    return this.name;
  }
}

// bad
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
  }
}

// good
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
    this.name = 'Rey';
  }
}
```

- 9.6 避免重复声明类成员.`eslint: no-dupe-class-members`
> 为什么？重复声明的类成员时，仅仅是最后一次的声明的成员会起效，前面声明的会被覆盖。- 重复声明类成员很可能是出现了一个bug
```js
// bad
class Foo {
  bar() { return 1; }
  bar() { return 2; }
}

// good
class Foo {
  bar() { return 1; }
}

// good
class Foo {
  bar() { return 2; }
}
```

## 模块
- 10.1 总是使用模组 (import/export) 而不是其他非标准模块系统。你可以编译为你喜欢的模块系统。
> 为什么？模块就是未来，让我们开始迈向未来吧。
```js
// bad
const AirbnbStyleGuide = require('./AirbnbStyleGuide');
module.exports = AirbnbStyleGuide.es6;

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide';
export default AirbnbStyleGuide.es6;

// best
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

- 10.2 不要使用通配符 import。
> 为什么？这样能确保你只有一个默认 `export`。
```js
// bad
import * as AirbnbStyleGuide from './AirbnbStyleGuide';

// good
import AirbnbStyleGuide from './AirbnbStyleGuide';
```

- 10.3 不要从 `import` 中直接 `export`。
> 为什么？虽然一行代码简洁明了，但让 `import` 和 `export` 各司其职让事情能保持一致。
```js
// bad
// filename es6.js
export { es6 as default } from './AirbnbStyleGuide';

// good
// filename es6.js
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

- 10.4 只在同一个地方导入具有相同路径的模块。`eslint: no-duplicate-imports`
> 为什么？在不同的地方导入具有相同路径的模块会导致代码很难维护
```js
// bad
import foo from 'foo';
// … some other imports … //
import { named1, named2 } from 'foo';

// good
import foo, { named1, named2 } from 'foo';

// good
import foo, {
  named1,
  named2,
} from 'foo';
```

- 10.5 不要导出可变的绑定。`eslint: import/no-mutable-exports`
> 为什么？通常情况下是要避免导出一个可变的绑定，除非在特殊情况下,比如某些技术实现上需要这么做
```js
// bad
let foo = 3;
export { foo };

// good
const foo = 3;
export { foo };
```

- 10.6 在只有一个`export`的模块中，优先使用`default export`。` eslint: import/prefer-default-export`
> 为什么？鼓励更多的文件只导出一件东西，这样可读性和可维护性会更好
```js
// bad
export function foo() {}

// good
export default function foo() {}
```

- 10.7 把所有的`import`放到非`import`语句之上.`eslint: import/first`
> 为什么？因为`import`语句会被提升,所以把他们放到顶部以防止出现意外的行为.
```js
// bad
import foo from 'foo';
foo.init();

import bar from 'bar';

// good
import foo from 'foo';
import bar from 'bar';

foo.init();
```

- 10.8 当导出多个内容是，每个内容项都应该独立一行，就像数组和对象一样。
```js
// bad
import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

// good
import {
  longNameA,
  longNameB,
  longNameC,
  longNameD,
  longNameE,
} from 'path';
```

- 10.9 在模块导入声明中，不得使用`webpack loader`的语法。`eslint: import/no-webpack-loader-syntax`
> 在`import`中使用webpack的语法会使代码和模块加载器产生耦合，所以更好的方式是在`webpack.config.js`中使用`loader`的语法
```js
// bad
import fooSass from 'css!sass!foo.scss';
import barCss from 'style!css!bar.css';

// good
import fooSass from 'foo.scss';
import barCss from 'bar.css';
```

## 迭代器和生成器
- 11.1 不要使用迭代器。更合适的方式是使用`javascript`的高阶函数来代替使用`for-in`、`for-of`的循环.
> 为什么？这加强了我们不变的规则。处理纯函数的回调值更易读，这比它带来的副作用更重要。

> 使用`map() / every() / filter() / find() / findIndex() / reduce() / some() / ... `来对数组进行迭代. 使用`Object.keys() / Object.values() / Object.entries()`来产生数组，然后再对`object`进行迭代.
```js
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {
  sum += num;
}
sum === 15;

// good
let sum = 0;
numbers.forEach((num) => {
  sum += num;
});
sum === 15;

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;

// bad
const increasedByOne = [];
for (let i = 0; i < numbers.length; i++) {
  increasedByOne.push(numbers[i] + 1);
}

// good
const increasedByOne = [];
numbers.forEach((num) => {
  increasedByOne.push(num + 1);
});

// best (keeping it functional)
const increasedByOne = numbers.map(num => num + 1);
```

- 11.2 现在还不要使用 generators。
> 为什么？因为它们现在还没法很好地编译到 ES5。

- 11.3 如果你一定要使用`generators`，或者你要忽略我们上面的建议，那么在函数签名的地方应该正确的加上空格.`eslint: generator-star-spacing`
> 为什么？`function`和`*`是相同概念的组成部分 - `*`是用来描述`function`的， `function*`是独立的构造，和`function`不是同一个概念
```js
// bad
function * foo() {
  // ...
}

// bad
const bar = function * () {
  // ...
};

// bad
const baz = function *() {
  // ...
};

// bad
const quux = function*() {
  // ...
};

// bad
function*foo() {
  // ...
}

// bad
function *foo() {
  // ...
}

// very bad
function
*
foo() {
  // ...
}

// very bad
const wat = function
*
() {
  // ...
};

// good
function* foo() {
  // ...
}

// good
const foo = function* () {
  // ...
};
```

## 属性
- 12.1 使用`.`来访问对象的属性。`eslint: dot-notation jscs: requireDotNotation`
```js
const luke = {
  jedi: true,
  age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```

- 12.2 当通过变量访问属性时使用中括号 []。
```js
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```

- 12.3 当计算数值的乘方时，使用乘方操作符`**`.`eslint: no-restricted-properties.`
```js
// bad
const binary = Math.pow(2, 10);

// good
const binary = 2 ** 10;
```
