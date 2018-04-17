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
