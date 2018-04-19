## 逗号
- 20.1 行首逗号：不需要。`eslint: comma-style`
```js
// bad
const story = [
    once
  , upon
  , aTime
];

// good
const story = [
  once,
  upon,
  aTime,
];

// bad
const hero = {
    firstName: 'Ada'
  , lastName: 'Lovelace'
  , birthYear: 1815
  , superPower: 'computers'
};

// good
const hero = {
  firstName: 'Ada',
  lastName: 'Lovelace',
  birthYear: 1815,
  superPower: 'computers',
};
```
- 20.2 增加结尾的逗号: 需要。`eslint: comma-dangle`
> 为什么? 这会让 git diffs 更干净。另外，像 babel 这样的转译器会移除结尾多余的逗号，也就是说你不必担心老旧浏览器的尾逗号问题。
```js
// bad - git diff without trailing comma
const hero = {
     firstName: 'Florence',
-    lastName: 'Nightingale'
+    lastName: 'Nightingale',
+    inventorOf: ['coxcomb chart', 'modern nursing']
};

// good - git diff with trailing comma
const hero = {
     firstName: 'Florence',
     lastName: 'Nightingale',
+    inventorOf: ['coxcomb chart', 'modern nursing'],
};
```
```js
// bad
const hero = {
  firstName: 'Dana',
  lastName: 'Scully'
};

const heroes = [
  'Batman',
  'Superman'
];

// good
const hero = {
  firstName: 'Dana',
  lastName: 'Scully',
};

const heroes = [
  'Batman',
  'Superman',
];

// bad
function createHero(
  firstName,
  lastName,
  inventorOf
) {
  // does nothing
}

// good
function createHero(
  firstName,
  lastName,
  inventorOf,
) {
  // does nothing
}

// good (note that a comma must not appear after a "rest" element)
function createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
) {
  // does nothing
}

// bad
createHero(
  firstName,
  lastName,
  inventorOf
);

// good
createHero(
  firstName,
  lastName,
  inventorOf,
);

// good (note that a comma must not appear after a "rest" element)
createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
);
```

## 分号
- 21.1 推荐不用分号.

## 类型转换
- 22.1 在语句开始时执行类型转换。

- 22.2 字符串. `eslint: no-new-wrappers`
```js
// => this.reviewScore = 9;

// bad
const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

// bad
const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

// bad
const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

// good
const totalScore = String(this.reviewScore);
```

- 22.3 对数字使用 parseInt 转换，并带上类型转换的基数。`eslint: radix no-new-wrappers`
```js
const inputValue = '4';

// bad
const val = new Number(inputValue);

// bad
const val = +inputValue;

// bad
const val = inputValue >> 0;

// bad
const val = parseInt(inputValue);

// good
const val = Number(inputValue);

// good
const val = parseInt(inputValue, 10);
```

 - 22.4 如果因为某些原因 parseInt 成为你所做的事的瓶颈而需要使用位操作解决性能问题时，留个注释说清楚原因和你的目的。
 ```js
// good
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
const val = inputValue >> 0;
 ```

- 22.5  注: 小心使用位操作运算符。数字会被当成 64 位值，但是位操作运算符总是返回 32 位的整数（参考）。位操作处理大于 32 位的整数值时还会导致意料之外的行为。关于这个问题的讨论。最大的 32 位整数是 2,147,483,647：
```js
2147483647 >> 0; // => 2147483647
2147483648 >> 0; // => -2147483648
2147483649 >> 0; // => -2147483647
```

- 22.6 布尔：`eslint: no-new-wrappers`
```js
const age = 0;

// bad
const hasAge = new Boolean(age);

// good
const hasAge = Boolean(age);

// best
const hasAge = !!age;
```

## 命名规则
- 23.1 避免单字母命名。命名应具备描述性。`eslint: id-length`
```js
// bad
function q() {
  // ...
}

// good
function query() {
  // ...
}
```

- 23.2 使用驼峰式命名对象、函数和实例。`eslint: camelcase`
```js
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

- 23.3 使用帕斯卡式命名构造函数或类. `eslint: new-cap`
```js
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```

- 23.4 不要使用下划线 _ 结尾或开头来命名属性和方法。`eslint: no-underscore-dangle`
> 为什么? javascript里没有私有属性或者私有函数的概念，虽然下划线的通常约定是私有的，但是事实上，在js中却是完全公有的。在这种惯例会使开发者错误的认为一个变化不会造成破坏，或者相关的测试是不需要的。
```js
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// good
this.firstName = 'Panda';

// good, in environments where WeakMaps are available
// see https://kangax.github.io/compat-table/es6/#test-WeakMap
const firstNames = new WeakMap();
firstNames.set(this, 'Panda');
```

- 23.5 别保存 this 的引用。使用箭头函数或 Function#bind。
```js
// bad
function foo() {
  const self = this;
  return function () {
    console.log(self);
  };
}

// bad
function foo() {
  const that = this;
  return function () {
    console.log(that);
  };
}

// good
function foo() {
  return () => {
    console.log(this);
  };
}
```

- 23.6 如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。
```js
// file 1 contents
class CheckBox {
  // ...
}
export default CheckBox;

// file 2 contents
export default function fortyTwo() { return 42; }

// file 3 contents
export default function insideDirectory() {}

// in some other file
// bad
import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

// bad
import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
import forty_two from './forty_two'; // snake_case import/filename, camelCase export
import inside_directory from './inside_directory'; // snake_case import, camelCase export
import index from './inside_directory/index'; // requiring the index file explicitly
import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

// good
import CheckBox from './CheckBox'; // PascalCase export/import/filename
import fortyTwo from './fortyTwo'; // camelCase export/import/filename
import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
// ^ supports both insideDirectory.js and insideDirectory/index.js
```

- 23.7 当你导出默认的函数时使用驼峰式命名。你的文件名必须和函数名完全保持一致。
```js
function makeStyleGuide() {
  // ...
}

export default makeStyleGuide;
```

- 23.8 当你导出单例、函数库、空对象时使用帕斯卡式命名。
```js
const AirbnbStyleGuide = {
  es6: {
  }
};

export default AirbnbStyleGuide;
```

- 23.9 首字母缩略词应该全部大写或者全部小写.
> 为什么?名字是用来给人阅读的，而不是用来给计算机算法看的
```js
// bad
import SmsContainer from './containers/SmsContainer';

// bad
const HttpRequests = [
  // ...
];

// good
import SMSContainer from './containers/SMSContainer';

// good
const HTTPRequests = [
  // ...
];

// also good
const httpRequests = [
  // ...
];

// best
import TextMessageContainer from './containers/TextMessageContainer';

// best
const requests = [
  // ...
];
```

- 23.10
