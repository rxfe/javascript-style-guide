## 变量
- 13.1 通常使用`const`或者`let`来声明变量, 如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。`eslint: no-undef prefer-const`
```js
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```

- 13.2 一个`const`或者`let`只声明一个变量. `eslint: one-var jscs: disallowMultipleVarDecl`
> 为什么？增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。你也能对每一个声明设置`debugger`， 而不是一次跳过所有变量.
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
> 为什么？根据eslint文档，一元递增和递减语句会自动插入分号，应用程序中使用一元操作符会导致一些静默的错误。使用 `num += 1`来替代`num++`或者`num ++`也会使递增的表达式变得更加生动明了。使用`++`和`--`可能会导致因为你无意对值进行修改而产生没有预期到的行为.
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
> 为什么？`=`的两边换行会混淆分配的值
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
