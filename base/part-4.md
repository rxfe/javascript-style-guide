## 变量
- 13.1 通常使用`const`或者`let`来声明变量, 如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。`eslint: no-undef prefer-const`
```js
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```

- 13.2 一个`const`或者`let`只声明一个变量. `eslint: one-var jscs: disallowMultipleVarDecl`
> 为什么？增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。你也能对每一个声明设置`debugger`， 而不是一次跳过所有变量.
```js
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

- 13.3 将所有的 const 和 let 分组
> 为什么？当你需要把已赋值变量赋值给未赋值变量时非常有用。
```js
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

- 13.4 在你需要的地方给变量赋值，但请把它们放在一个合理的位置。
> 为什么？let 和 const 是块级作用域而不是函数作用域。
```js
// bad - unnecessary function call
function checkName(hasName) {
  const name = getName();

  if (hasName === 'test') {
    return false;
  }

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}

// good
function checkName(hasName) {
  if (hasName === 'test') {
    return false;
  }

  const name = getName();

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}
```

- 13.5 不要链式的声明变量. `eslint: no-multi-assign`
> 为什么？链式的声明变量会隐式的创建全局变量
```js
// bad
(function example() {
  // JavaScript interprets this as
  // let a = ( b = ( c = 1 ) );
  // The let keyword only applies to variable a; variables b and c become
  // global variables.
  let a = b = c = 1;
}());

console.log(a); // throws ReferenceError
console.log(b); // 1
console.log(c); // 1

// good
(function example() {
  let a = 1;
  let b = a;
  let c = a;
}());

console.log(a); // throws ReferenceError
console.log(b); // throws ReferenceError
console.log(c); // throws ReferenceError

// the same applies for `const`
```

- 13.6  避免使用一元操作符`++`和`--`. `eslint no-plusplus`
> 为什么？根据eslint文档，一元递增和递减语句会自动插入分号，应用程序中使用一元操作符会导致一些静默的错误。使用 `num += 1`来替代`num++`或者`num ++`也会使递增的表达式变得更加生动明了。使用`++`和`--`可能会导致因为你无意对值进行修改而产生没有预期到的行为.
```js
// bad

const array = [1, 2, 3];
let num = 1;
num++;
--num;

let sum = 0;
let truthyCount = 0;
for (let i = 0; i < array.length; i++) {
  let value = array[i];
  sum += value;
  if (value) {
    truthyCount++;
  }
}

// good

const array = [1, 2, 3];
let num = 1;
num += 1;
num -= 1;

const sum = array.reduce((a, b) => a + b, 0);
const truthyCount = array.filter(Boolean).length;
```

- 13.7 在`=`两边避免换行， 如果你的声明语句长度超过了`max-len`，那么把你的值放到括号里. `eslint operator-linebreak`
> 为什么？`=`的两边换行会混淆分配的值
```js
// bad
const foo =
  superLongLongLongLongLongLongLongLongFunctionName();

// bad
const foo
  = 'superLongLongLongLongLongLongLongLongString';

// good
const foo = (
  superLongLongLongLongLongLongLongLongFunctionName()
);

// good
const foo = 'superLongLongLongLongLongLongLongLongString';
```

## Hoisting
- 14.1 var 声明会被提升至该作用域的顶部，但它们赋值不会提升。let 和 const 被赋予了一种称为「暂时性死区（Temporal Dead Zones, TDZ）」的概念。这对于了解为什么 type of 不再安全相当重要。
```js
// we know this wouldn’t work (assuming there
// is no notDefined global variable)
function example() {
  console.log(notDefined); // => throws a ReferenceError
}

// creating a variable declaration after you
// reference the variable will work due to
// variable hoisting. Note: the assignment
// value of `true` is not hoisted.
function example() {
  console.log(declaredButNotAssigned); // => undefined
  var declaredButNotAssigned = true;
}

// the interpreter is hoisting the variable
// declaration to the top of the scope,
// which means our example could be rewritten as:
function example() {
  let declaredButNotAssigned;
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;
}

// using const and let
function example() {
  console.log(declaredButNotAssigned); // => throws a ReferenceError
  console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
  const declaredButNotAssigned = true;
}
```

- 14.2 匿名函数表达式的变量名会被提升，但函数内容并不会。
```js
function example() {
  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function() {
    console.log('anonymous function expression');
  };
}
```

- 14.3 命名的函数表达式的变量名会被提升，但函数名和函数内容并不会。
```js
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {
    console.log('Flying');
  };
}

// the same is true when the function name
// is the same as the variable name.
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  var named = function named() {
    console.log('named');
  };
}
```

- 14.4 函数声明的名称和函数体都会被提升。
```js
function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
  }
}
```

## 比较运算符和符号
- 15.1 优先使用 === 和 !== 而不是 == 和 !=. `eslint: eqeqeq`
- 15.2 条件表达式例如 if 语句通过抽象方法 ToBoolean 强制计算它们的表达式并且总是遵守下面的规则：
  - 对象 被计算为 true
  - Undefined 被计算为 false
  - Null 被计算为 false
  - 布尔值 被计算为 布尔的值
  - 数字 如果是 +0、-0、或 NaN 被计算为 false, 否则为 true
  - 字符串 如果是空字符串 '' 被计算为 false，否则为 true
```js
if ([0] && []) {
  // true
  // an array (even an empty one) is an object, objects will evaluate to true
}
```
- 15.3 除非需要比较具体的字符串或者数字，否则使用`booleans`的简写
```js
// bad
if (isValid === true) {
  // ...
}

// good
if (isValid) {
  // ...
}

// bad
if (name) {
  // ...
}

// good
if (name !== '') {
  // ...
}

// bad
if (collection.length) {
  // ...
}

// good
if (collection.length > 0) {
  // ...
}
```

15.4 在`case`和`default`从句中如果包含了词法声明(e.g. `let`, `const`, `function`, `class`), 那么用大括号包成一个块. ` eslint: no-case-declarations`
> 为什么？词法声明在整个`switch`块里都是可见的，但是它的初始化发生在`case`执行的时候的数据分配。这种方式在多个`case`从句试图定义同一个东西的时候会出现问题。
```js
// bad
switch (foo) {
  case 1:
    let x = 1;
    break;
  case 2:
    const y = 2;
    break;
  case 3:
    function f() {
      // ...
    }
    break;
  default:
    class C {}
}

// good
switch (foo) {
  case 1: {
    let x = 1;
    break;
  }
  case 2: {
    const y = 2;
    break;
  }
  case 3: {
    function f() {
      // ...
    }
    break;
  }
  case 4:
    bar();
    break;
  default: {
    class C {}
  }
}
```

- 15.5 双目运算不应该有层叠，通常应该是放到单独一行。`eslint: no-nested-ternary`
```js
// bad
const foo = maybe1 > maybe2
  ? "bar"
  : value1 > value2 ? "baz" : null;

// split into 2 separated ternary expressions
const maybeNull = value1 > value2 ? 'baz' : null;

// better
const foo = maybe1 > maybe2
  ? 'bar'
  : maybeNull;

// best
const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```

- 15.6 避免不必要的双目运算。`eslint: no-unneeded-ternary`
```js
// bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;

// good
const foo = a || b;
const bar = !!c;
const baz = !c;
```

- 15.7 当各种操作符混合在一起的时候，利用括号将他们组成一组。除非这些操作符是标准的算术运算操作符。`eslint: no-mixed-operators`
> 为什么？这样可以改善可读性，并且使开发者的意图更加的清晰。
```js
// bad
const foo = a && b < 0 || c > 0 || d + 1 === 0;

// bad
const bar = a ** b - 5 % d;

// bad
// one may be confused into thinking (a || b) && c
if (a || b && c) {
  return d;
}

// good
const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

// good
const bar = (a ** b) - (5 % d);

// good
if (a || (b && c)) {
  return d;
}

// good
const bar = a + b / c * d;
```
