Features
=========

Brief explanation of the SaneScript features.

Note that examples below can be a bit different from actual compiled results, due to technical/minor reasons. For such cases, see [Details.md](https://github.com/SaneScript/SaneScript/blob/master/Details.md)

# Sane equality operator

> `===`, `==`, `!==`, `!=`

Basically, === and == is swapped in SaneScript, So are !== and !=.

So 2-letter-operators don't change types, while 3-letter-ones do.

```js
// SaneScript

'a' == 'a' // true
3 != '3' // true

undefined === null // true
4 !== '4' // false
```

```js
// JavaScript

'a' === 'a' // true
3 !== '3' // true

undefined == null // true
4 != '4' // false
```

# Chainable comparison operators

> `2 < x < 7`

Multiple comparison operators, in a single expression. Quite readable.

```js
// SaneScript

2 < x < 7

3 < a <= b <= 8

if (120 <= height < 190) {
  console.log('You can ride this roller coaster')
}
```

```js
// JavaScript

(2 < x) && (x < 7)

(3 < a) && (a <= b) && (b <= 8)

if ((120 <= height) && (height < 190)) {
  console.log('You can ride this roller coaster')
}
```

# Better `switch`/`case`

`break` statement will be automatically inserted. If you want to execute same code for multiple conditions, use comma-separated expressions.

```js
// SaneScript

switch (value) {
  case 'val1':
    doSomething()
  case 'val2', 'val3':
    doSomeAnotherThing()
  default:
    doNothing()
}
```

```js
// JavaScript

switch (value) {
  case 'val1':
    doSomething()
    break
  case 'val2':
  case 'val3':
    doSomeAnotherThing()
    break
  default:
    doNothing()
    break
}
```

# Existential operator

> `a.$.b.$().c`

`.$` is an existential operator. You can read this as 'Do only if possible'.

```js
// SaneScript

a.$

a.$.b.$.c

a.$()
```

```js
// JavaScript

a != null

(a == null
  ? null
  : a.b == null
    ? null
    : a.b.c)

(typeof a !== 'function'
  ? null
  : a())
```

# Automatic `new` insertion

You can omit `new` keyword to instantiate some JavaScript's built in classes.

```js
// SaneScript

let map = Map()
```

```js
// JavaScript

let map = new Map()
```

# Syntactic getter/setter

> `map[key]`, `map[key] = value`

Computed property name is an alias of `get`/`set` method.

```js
// SaneScript

obj[key]
obj[key] = 'value'
```

```js
// JavaScript

obj.get(key)
obj.set(key, 'value')
```

# Hidden property

> `obj._hidden`

Underscore-prefixed property is an alias for Symbol(). Symbols are pooled by its name, and each module(source file) has it's own symbol pool. So symbols with same name but used in different modules aren't the same, normally.

`_iterator`, `_match`, `_species` are reserved; for example, `_iterator` is an alias for `Symbol.iterator`.  That is, they are the same symbol across different modules.

`Symbol` keyword is treated as a namespace for this module's symbol pool. You can access/modify your symbol pool using this keyword.

Symbol pool is automatically exported as keyword `_symbol`. This feature is useful if you want to share symbols across modules.

```js
// SaneScript

import {_symbol as sharedSymbols} from './symbols'

Symbol.meta = sharedSymbols.meta

let obj = {
  _prop: 'foo'
}
obj._data = 42
obj._iterator = function* () {
  yield 'foo'
  yield 'bar'
  yield 'baz'
}

export function doSomething (obj) {
  console.log(obj._prop)
  console.log(obj._meta)
  console.log(Symbol.data)
}
```

```js
// JavaScript

const __saneSymbols__ = {
  iterator: Symbol.iterator,
  match: Symbol.match,
  species: Symbol.species,
  prop: Symbol('prop'),
  data: Symbol('data'),
  meta: Symbol('meta')
}

import {_symbol as sharedSymbols} from './symbols'

__saneSymbols__.meta = sharedSymbols.meta

let obj = {
  [__saneSymbols__.prop]: 'foo'
}
obj[__saneSymbols__.data] = 42
obj[__saneSymbols__.iterator] = function* () {
  yield 'foo'
  yield 'bar'
  yield 'baz'
}

export function doSomething (obj) {
  console.log(obj[__saneSymbols__.prop])
  console.log(obj[__saneSymbols__.meta])
  console.log(__saneSymbols__.data)
}

export {__saneSymbols__ as _symbol}
```

# Macro

> `~Macro()`

Macro is a compile-time function which takes source code and replace it with modified code. In SaneScript, macros can be implemented as a compiler plug-in.

Expression prefixed by `~`(binary not operator in JavaScript) is treated as macro.

Example below describes how the default `~Map()` macro works.

```js
// SaneScript

let map = ~Map({
  foo: 'bar',
  [obj]: 42,
  myFunc (arg) {
    console.log(arg)
  },
  ...otherMap
})
```

```js
// JavaScript

let map = new Map([
  ['foo', 'bar'],
  [obj, 42],
  ['myFunc', function myFunc (arg) {
    console.log(arg)
  }],
  ...otherMap
])
```

# Miscellaneous

- `'use strict'` is inserted automatically.

- Multi-lined template strings are automatically de-indented at compile time, like [dedent](https://github.com/dmnd/dedent).

- Array literal within `await` expressions are automatically wrapped with `Promise.all()`.
