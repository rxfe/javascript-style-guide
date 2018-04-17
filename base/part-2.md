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
