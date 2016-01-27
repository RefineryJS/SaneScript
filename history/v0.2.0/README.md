SaneScript
===========

JavaScript as it should be

Inspired from [this article](https://github.com/rwaldron/tc39-notes/blob/master/es6/2015-01/JSExperimentalDirections.pdf)

SaneScript v0.2.0

# Description

## What's SaneScript?

SaneScript is a programming language which has same syntax as JavaScript, especially ES2015 and newer, but slightly different semantic. It's main purpose is to compile to JavaScript.

## Another compile-to-js? Why does it even needed?

JavaScript is a good language, but some point of it is insane. Many of those bad points can be replaced by new things from ES2015 or newer, but using new syntax is less readable and need more typing then traditional way.

They cannot be replaced due to backward compatibility. But wait, now everybody compiles to JavaScript, then why don't we compile JavaScript to JavaScript?

## How about CoffeeScript?

CoffeeScript is a good language. It's syntax can hide lots of JavaScript's ugly parts. But it's not a JavaScript at all, and JavaScript is rapidly changing now. For example, if you want to use async-await keywords in CoffeeScript, you should just wait until language designer and compiler maintainer decide to include that feature to CoffeeScript.

In SaneScript, it's syntax is just JavaScript. It means we don't need any configuration to adapt JavaScript's future standard.

And there must be some people who don't like CoffeeScript's syntax, such as python-like block and parenthesis-less function call, but also hate JavaScript's old-and-wrong choices. If you're such a person, SaneScript is for you!

And also, you can use your comfortable tools like editors and linters. No more configuration for it. Just start coding!

# Features

## Using plain-old JavaScript in SaneScript

> `'use javascript'`, `'use sanescript'`

'use javascript' directive prevents SaneScript to modify your code.

Like 'use strict', outside of the block that contains this directive will not be affected.

'use sanescript' directive prevents preventing modifying, so use it if you want to write SaneScript code in your JavaScript-ified block.

```js
console.log("It's SaneScript")

{
  'use javascript'

  console.log("It's JavaScript")

  if (42) {
    'use sanescript'

    console.log("It's also SaneScript")
  }

  console.log("It's JavaScript again")
}
```

## Sane equality operator

> `===`, `==`, `!==`, `!=`

Basically, === and == is swapped in SaneScript. Same as !== and !=

So 2-letter-operators do not type coercion, while 3-letter-ones do.

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

## Chainable comparison operator

> `2 < x < 7`

How can you check some value is in the range of 2~7? Of course, there's more readable way.

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

## Better `switch`/`case`

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

## Existential operator

> `a.$.b.$().c`

`.$` is an existential operator. You can read this operator as `Do only if possible`

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

## Automatic `new` insertion

You can omit `new` keyword to instantiate some JavaScript's built in classes.

```js
// SaneScript

let map = Map()
```

```js
// JavaScript

let map = new Map()
```

## Syntactic getter/setter

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

## Hidden property

> `obj._hidden`

Underscore-prefixed property is an alias for Symbol(). Symbols are pooled by it's name, and each module(source file) has it's own symbol pool. So symbols with same name but used in different modules not be same normally.

`_iterator`, `_match`, `_species` are reserved. For example, `_iterator` is an alias for `Symbol.iterator`. It means, they are same symbol across modules.

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

```

## Macro

> `~Macro()`

Macro is a compile-time function which takes source code and replace it with modified code. In SaneScript, macro can be implemented as a compiler plug-in.

Expression prefixed by `~`(binary not operator in JavaScript) is treated as macro.

Example below describes how `~Map()` macro works.

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

## Miscellaneous

- `'use strict'` is automatically inserted.

- Multi-lined template strings automatically de-indented at compile time using [dedent](https://github.com/dmnd/dedent)

# Detail

Feature descriptions above are simply brief concept of what those features are, but actual transformation from SaneScript to JavaScript may differ due to some technical/minor reasons. This section describe those deep inside of SaneScript specification.

## Existential operator

Values that used more then once will be cached.

```js
// SaneScript

a.$.b

state.a.$.b.$().$.c.d
```

```js
// JavaScript

let __saneValue__

(
  __saneValue__ = a,
  __saneValue__ == null
    ? null
    : __saneValue__.b
)

(
  __saneValue__ = state.a,
  __saneValue__ == null
    ? null
    : (
      __saneValue__ = __saneValue__.b,
      typeof __saneValue__ !== 'function'
        ? null
        : (
          __saneValue__ = __saneValue__(),
          __saneValue__ == null
            ? null
            : __saneValue__.c.d
        )
    )
)
```

## Automatic `new` insertion

List of supported keywords are below.

- Map
- Set
- WeakMap
- WeakSet
- Date
- Promise
- Proxy
- `/([A-Z].*)?Error/`
- ArrayBuffer
- DataView
- `/(Int|Uint|Float)(8|16|32|64)(Clamped)?Array/`
- Intl.Collator
- Intl.DateTimeFormat
- Intl.NumberFormat

## Syntactic getter/setter

To support immutable data structure like Immutable.js, setter transform is little bit more complicate then described above. This makes possible to handle immutable data as normal mutable data.

And also, values that used more then once will be cached.

```js
// SaneScript

map[foo] = bar

map['answer'] = state.otherMap['truth'] = 42
```

```js
// JavaScript

let __saneCollection__, __saneValue__

(
  __saneCollection__ = map,
  __saneValue__ = bar,
  map = __saneCollection__.set(foo, __saneValue__) || __saneCollection__,
  __saneValue__
)

(
  __saneCollection__ = map,
  __saneValue__ = (
    __saneCollection__ = state.otherMap,
    __saneValue__ = 42,
    state.otherMap = __saneCollection__.set('truth', __saneValue__) || __saneCollection__,
    __saneValue__
  ),
  map = __saneCollection__.set('answer', __saneValue__) || __saneCollection__,
  __saneValue__
)
```

## Macro

To see the full specifications of macros below, see (Macro.md)[https://github.com/hyeonupark/sanescript/blob/master/Macro.md]

List of default macros are below

- ~Map()
