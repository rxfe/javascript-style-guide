## 解构
- 5.1 使用解构存取和使用多属性对象。`eslint: prefer-destructuring jscs: requireObjectDestructuring`
> 为什么？因为解构能减少临时引用属性。
```javascript
// bad
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;

  return `${firstName} ${lastName}`;
}

// good
function getFullName(user) {
  const { firstName, lastName } = user;
  return `${firstName} ${lastName}`;
}

// best
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}
```

- 5.2 对数组使用解构赋值. ` eslint: prefer-destructuring jscs: requireArrayDestructuring`
```javascript
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

- 5.3 需要回传多个值时，使用对象解构，而不是数组解构。
> 为什么？增加属性或者改变排序不会改变调用时的位置。
```javascript
// bad
function processInput(input) {
  // then a miracle occurs
  return [left, right, top, bottom];
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);

// good
function processInput(input) {
  // then a miracle occurs
  return { left, right, top, bottom };
}

// the caller selects only the data they need
const { left, top } = processInput(input);
```

## 字符串
- 6.1 字符串使用单引号。 `eslint: quotes jscs: validateQuoteMarks`
```javascript
// bad
const name = "Capt. Janeway";

// bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```

- 6.2 字符串即使超过100个字符，也不应该换行
> 为什么？把字符串打碎会让可查找性下降，并且难以协作(painful to work with)
```javascript
// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// bad
const errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';

// good
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
```

- 6.3 程序化生成字符串时，使用模板字符串代替字符串连接。` eslint: prefer-template template-curly-spacing jscs: requireTemplateStrings`
> 为什么？模板字符串具有更高的可读性，简洁的语法和字符插入的功能
```javascript
// bad
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// bad
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// bad
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// good
function sayHi(name) {
  return `How are you, ${name}?`;
}
```

- 6.4 不要使用`eval`, 这种用法很容易被攻击，有安全风险. ` eslint: no-eval`

- 6.5 不要使用无用的转义
> 为什么?反斜线会造成很差的可读性问题，所以仅仅在需要的时候使用
```javascript
// bad
const foo = '\'this\' \i\s \"quoted\"';

// good
const foo = '\'this\' is "quoted"';
const foo = `my name is '${name}'`;
```

## 函数
- 7.1 使用命名函数表达式`(named function expressions)`替代函数声明
> 为什么？函数声明会被提升，这意味着很容易在定义函数之前进行使用。这种方式会导致可读性和可维护性变差。如果你发现一个函数太复杂以至于会影响到文件其他部分的理解，那么你应该将函数放到另一个模块中。别忘了给它一个明确的名字
> stackoverflow上的讨论：[named function expressions](https://stackoverflow.com/questions/15336347/why-use-named-function-expressions)
```javascript
// bad
function foo() {
  // ...
}

// bad
const foo = function () {
  // ...
};

// good
// lexical name distinguished from the variable-referenced invocation(s)
const short = function longUniqueMoreDescriptiveLexicalFoo() {
  // ...
};
```
- 7.2 将立即调用函数式表达式用括号包裹起来。` eslint: wrap-iife jscs: requireParenthesesAroundIIFE`
> 为什么？立即调用的函数表达式是一个独立的单元,利用括号包裹，并利用括号去执行它，这样表达式看起来会清晰。但是现在模块被广泛使用，所以通常是不要去使用`IIFE`
```javascript
// immediately-invoked function expression (IIFE)
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```

- 7.3 永远不要在一个非函数代码块`（if、while 等）`中声明一个函数，把那个函数赋给一个变量。浏览器允许你这么做，但它们的解析表现不一致。`eslint: no-loop-func`

- 7.4 **注意:** ECMA-262 把 block 定义为一组语句。函数声明不是语句。
```javascript
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
let test;
if (currentUser) {
  test = () => {
    console.log('Yup.');
  };
}
```

- 7.5 永远不要把参数命名为 arguments。这将取代原来函数作用域内的 arguments 对象。
```javascript
// bad
function foo(name, options, arguments) {
  // ...
}

// good
function foo(name, options, args) {
  // ...
}
```

- 7.6 不要使用 arguments。可以选择 rest 语法 ... 替代。`eslint: prefer-rest-params`
> 为什么？使用 `...` 能明确你要传入的参数。另外 `rest`参数是一个真正的数组，而 `arguments` 是一个类数组。
```javascript
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```

- 7.7 直接给函数的参数指定默认值，不要使用一个变化的函数参数。
```javascript
// really bad
function handleThings(opts) {
  // No! We shouldn’t mutate function arguments.
  // Double bad: if opts is falsy it'll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {};
  // ...
}

// still bad
function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// good
function handleThings(opts = {}) {
  // ...
}
```

- 7.8 直接给函数参数赋值时需要避免副作用。
>  为什么？因为这样的写法让人感到很困惑。
```javascript
var b = 1;
// bad
function count(a = b++) {
  console.log(a);
}
count();  // 1
count();  // 2
count(3); // 3
count();  // 3
```

- 7.9 将拥有默认值的参数放到参数列表的后面
```javascript
// bad
function handleThings(opts = {}, name) {
  // ...
}

// good
function handleThings(name, opts = {}) {
  // ...
}
```

- 7.10 不要利用`Function`来构造一个新的函数.`eslint: no-new-func`
> 为什么?这种方式和利用`eval()`类似，会有安全风险
```javascript
// bad
var add = new Function('a', 'b', 'return a + b');

// still bad
var subtract = Function('a', 'b', 'return a - b');
```

- 7.11 在函数签名两边需要一个空格.`eslint: space-before-function-paren space-before-blocks`
> 为什么？保持声明方式的一致性，这样你就不需要再删除或者增加一个名字的时候去增加或者删除一个空格。
```javascript
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const y = function a() {};
```

- 7.12 不要去改变传入参数里的属性值。`eslint: no-param-reassign`
> 为什么？操作传入的参数（该参数是一个对象），在原始函数调用者中可能会产生不想要的副作用。
```javascript
// bad
function f1(obj) {
  obj.key = 1;
}

// good
function f2(obj) {
  const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
}
```

- 7.13 不要对传入的参数重新赋值。`eslint: no-param-reassign`
> 为什么？修改参数会导致不可预期的行为，特别是使用`arguments`对象的时候。也会导致优化的问题，特别是在V8引擎中。
```javascript
// bad
function f1(a) {
  a = 1;
  // ...
}

function f2(a) {
  if (!a) { a = 1; }
  // ...
}

// good
function f3(a) {
  const b = a || 1;
  // ...
}

function f4(a = 1) {
  // ...
}
```

- 7.14 对于调用可变参数的函数，优先使用`...`操作。`eslint: prefer-spread`
> 为什么？这样比较简洁，你不需要去绑定`context`,你也不需要使用`apply`去组合成使用`new`来构造的函数
```javascript
// bad
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);

// good
const x = [1, 2, 3, 4, 5];
console.log(...x);

// bad
new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

// good
new Date(...[2016, 8, 5]);
```

- 7.15 拥有多个多行签名或者实参调用的函数，每一个签名或者每一个实参都应该独立一行，并且最后一个签名或者参数需要加逗号。`eslint: function-paren-newline`
```javascript
// bad
function foo(bar,
             baz,
             quux) {
  // ...
}

// good
function foo(
  bar,
  baz,
  quux,
) {
  // ...
}

// bad
console.log(foo,
  bar,
  baz);

// good
console.log(
  foo,
  bar,
  baz,
);
```

## 箭头函数
- 8.1 当你必须使用一个匿名函数(或者仅仅是一行的回调函数)时，使用箭头函数的符号.`eslint: prefer-arrow-callback, arrow-spacing jscs: requireArrowFunctions`
> 为什么？因为箭头函数创造了新的一个 `this` 执行环境通常情况下都能满足你的需求，而且这样的写法更为简洁。

> 为什么不？如果你有一个相当复杂的函数，你或许可以把逻辑部分转移到一个函数声明上。
```javascript
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

- 8.2 如果一个函数只有一行表达式，并且没有副作用，那就把花括号、圆括号和 return 都省略掉。如果不是，那就不要省略。`eslint: arrow-parens, arrow-body-style jscs: disallowParenthesesAroundArrowParam, requireShorthandArrowFunctions`
> 为什么？语法糖。在链式调用中可读性很高。
```javascript
// bad
[1, 2, 3].map(number => {
  const nextNumber = number + 1;
  `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// good
[1, 2, 3].map((number) => {
  const nextNumber = number + 1;
  return `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map((number, index) => ({
  [index]: number,
}));

// No implicit return with side effects
function foo(callback) {
  const val = callback();
  if (val === true) {
    // Do something if callback returns true
  }
}

let bool = false;

// bad
foo(() => bool = true);

// good
foo(() => {
  bool = true;
});
```

- 8.3 如果表达式跨了多行，那么使用括号包裹起来，这样能提高可读性
> 为什么？这样能更好的展示函数的开头和结尾
```javascript
// bad
['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod,
  )
);

// good
['get', 'post', 'put'].map(httpMethod => (
  Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod,
  )
));
```

- 8.4 如果函数只有一个参数，并且这个参数没有加括号，那么，函数体不需要大括号。`eslint: arrow-parens jscs: disallowParenthesesAroundArrowParam`
> 为什么？视觉上更清晰，没有杂乱的部分
```javascript
// bad
[1, 2, 3].map((x) => x * x);

// good
[1, 2, 3].map(x => x * x);

// good
[1, 2, 3].map(number => (
  `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
));

// bad
[1, 2, 3].map(x => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

- 8.5 避免让箭头函数的语法`=>`与比较操作`<= or >=`造成疑惑. `eslint: no-confusing-arrow`
```javascript
// bad
const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

// bad
const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

// good
const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height > 256 ? largeSize : smallSize;
};
```
