## 代码块
- 16.1 使用大括号包裹所有的多行代码块。 `eslint: nonblock-statement-body-position`
```js
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function foo() { return false; }

// good
function bar() {
  return false;
}
```

- 16.2 如果通过 if 和 else 使用多行代码块，把 else 放在 if 代码块关闭括号的同一行。`eslint: brace-style jscs: disallowNewlineBeforeBlockStatements`
```js
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```

- 16.3 如果在`if`的代码块里执行一个`return`的语句，那么第二个`else`是不需要的。如果一个包含有`return`的`if`语句接下来有一个`else if`语句，而且这个语句里又有一个`return`语句,那么这个语句应该被分成多个`if`语句来写。`eslint: no-else-return`
```js
// bad
function foo() {
  if (x) {
    return x;
  } else {
    return y;
  }
}

// bad
function cats() {
  if (x) {
    return x;
  } else if (y) {
    return y;
  }
}

// bad
function dogs() {
  if (x) {
    return x;
  } else {
    if (y) {
      return y;
    }
  }
}

// good
function foo() {
  if (x) {
    return x;
  }

  return y;
}

// good
function cats() {
  if (x) {
    return x;
  }

  if (y) {
    return y;
  }
}

// good
function dogs(x) {
  if (x) {
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}
```
