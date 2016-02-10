Features
=========

Brief description of the SaneScript features.

Note that examples below can be a bit different from actual compiled results, due to technical/minor reasons. For such cases, see [Details.md](https://github.com/RefinedJS/SaneScript/blob/master/Details.md)

# Sane equality operator

> `===`, `==`, `!==`, `!=`

Basically, === and == is swapped in SaneScript, so are !== and !=.

So 2-letter-operators don't coerce types, while 3-letter-ones do.

```js
// SaneScript

'a' == 'a'         // true
3 != '3'           // true

undefined === null // true
4 !== '4'          // false
```

```js
// JavaScript

'a' === 'a'        // true
3 !== '3'          // true

undefined == null  // true
4 != '4'           // false
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

# Existential operator <sup><sup>[detail](https://github.com/RefinedJS/SaneScript/blob/master/Details.md#existential-operator)</sup></sup>

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

# Automatic `new` insertion <sup><sup>[detail](https://github.com/RefinedJS/SaneScript/blob/master/Details.md#automatic-new-insertion)</sup></sup>

> No more `new`

You can omit `new` keyword to instantiate some JavaScript's built in classes.

```js
// SaneScript

let map = Map()
```

```js
// JavaScript

let map = new Map()
```

# Syntactic getter/setter <sup><sup>[detail](https://github.com/RefinedJS/SaneScript/blob/master/Details.md#syntactic-gettersetter)</sup></sup>

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

# Hidden property <sup><sup>[detail](https://github.com/RefinedJS/SaneScript/blob/master/Details.md#hidden-property)</sup></sup>

> `obj._hidden`

Underscore-prefixed property is an alias for Symbol(). Symbols are pooled by its name, and each module(source file) has it's own symbol pool. So symbols with same name but used in different modules aren't the same, normally.

`_iterator`, `_match`, `_species` are reserved; For example, `_iterator` is an alias for `Symbol.iterator`.  That is, they are the same symbol across different modules.

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

class MyClass {
  _method (arg) {
    console.log('method -', arg)
  }
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
  meta: Symbol('meta'),
  method: Symbol('method')
}

// !!IMPORTANT!!
const Symbol = __saneSymbols__

import {_symbol as sharedSymbols} from './symbols'

Symbol.meta = sharedSymbols.meta

let obj = {
  [Symbol.prop]: 'foo'
}
obj[Symbol.data] = 42
obj[Symbol.iterator] = function* () {
  yield 'foo'
  yield 'bar'
  yield 'baz'
}

class MyClass {
  [Symbol.method]
}

export function doSomething (obj) {
  console.log(obj[Symbol.prop])
  console.log(obj[Symbol.meta])
  console.log(Symbol.data)
}

export {Symbol as _symbol}
```

# Macro

> `~Macro()`

Macro is a compile-time function which takes source code(not runtime value) and replace it with modified code. In SaneScript, macros can be implemented as a compiler plug-in.

Raw function(not a property of another object) call prefixed by `~`(binary not operator in JavaScript) is treated as macro. `~` operator other than macro is not allowed in SaneScript.

To use macro, you should declare it with special form of the import statement. Imports from `~` prefixed name are treated as macro import, works only in compile time.

Macros will never exist in runtime, so any code that attempt to do so should not be compiled. For example, Assigning macro to variable, assigning or declaring variable that has same name as declared macro, calling macro as a normal function, or calling runtime value as a macro, will generate compile error.

Example below describes how to use `~MapLiteral()` macro in `sanescript-plugin-literals` package.

```js
// SaneScript

import {MapLiteral as oMap} from '~literals'

let map = ~oMap({
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

// No import '~literals'

let map = new Map([
  ['foo', 'bar'],
  [obj, 42],
  ['myFunc', function myFunc (arg) {
    console.log(arg)
  }],
  ...otherMap
])
```

# [Compiler flag](https://github.com/RefinedJS/SaneScript/blob/master/Flags.md)

# Miscellaneous

- `'use strict'` is inserted automatically.

- Multi-lined template strings are automatically de-indented at compile time, like [dedent](https://github.com/dmnd/dedent).

- Array literals within `await` expression are automatically wrapped with `Promise.all()`.
