SaneScript
===========

JavaScript with sanity

Inspired from [this article](https://github.com/rwaldron/tc39-notes/blob/master/es6/2015-01/JSExperimentalDirections.pdf)

# Description

## What's SaneScript?

SaneScript is a programming language which has same syntax as JavaScript, especially ES2015, but slightly different semantic. It's main purpose is to compile to JavaScript.

## Another compile-to-js? Why does it even needed?

JavaScript is a good language, but some point of it is insane. Many of those bad points can be replaced by new things from ES2015, but using new syntax is a little bit harder to use and need more typing then traditional way.

They cannot be replaced due to backward compatibility. But wait, now everybody compiles to JavaScript, then why don't we compile JavaScript to JavaScript?

## How about coffee-script?

CoffeeScript is a good language. It's syntax can hide lots of JavaScript's ugly parts. But it's not a JavaScript at all, and JavaScript is rapidly changing now. For example, if you want to use async-await keywords in CoffeeScript, you should just wait until language designer and compiler maintainer decide to include that feature to CoffeeScript.

In SaneScript, it's syntax is just JavaScript. It means SaneScript is 100% compatable with JS transpilers, like babel.

And also, you can use your comfortable tools like editors and linters. No more configuration for it. Just start coding!

# Detail

## 'use madness'

'use madness' directive prevents SaneScript to modify your code.
Like 'use strict', outside of the block that contains this directive will not be affected.

## ==, ===, !=, !==

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

## switch, case

`break` statement will be automatically inserted. If you want to execute same code for multiple condition, use comma-separated expressions.

```js
//SaneScript

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
//JavaScript

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

## a.$.b.$.c

`.$` is an existential operator. You can read this operator as `Do only if possible`

```js
//SaneScript

a.$
a.$.b.$.c
a.$().b
```

```js
//JavaScript

a != null
(typeof a === 'undefined'
  ? null
  : typeof a.b === 'undefined'
    ? null
    : a.b.c)
(typeof a !== 'function'
  ? null
  : a().b)
```

## obj.\_hidden

Underscore-prefixed property is an alias for Symbol(). Symbols can be import/export to share hidden reference.

To use hidden properties, you should explicitly declare it by import statement from `'symbol'`. It's not really a importing, so different module get different symbol even they imported the same name of symbol.

`_iterator, _match, _species` is reserved. For example, `_iterator` is an alias for `Symbol.iterator`. It means, they aren't unique across modules.

```js
//SaneScript

import {_prop, _iterator} from 'symbol'
import {_external} from 'lib'

obj._prop = 'foo'
obj._iterator = function* () {
  yield 'foo'
  yield 'bar'
  yield 'baz'
}

function doSomething (obj) {
  console.log(obj._prop)
  console.log(obj._external)
}

export {_prop}
```

```js
//JavaScript

const _prop = Symbol('prop'), _iterator = Symbol.iterator
import {_external} from 'lib'

obj[_prop] = 'foo'
obj[_iterator] = function* () {
  yield 'foo'
  yield 'bar'
  yield 'baz'
}

function doSomething (obj) {
  console.log(obj[_prop])
  console.log(obj[_external])
}

export {_prop}
```

## ~{}, ~[]

`~{}` is an alias of new Map(), and `~[]` is an alias of new Set().

Like Object/Array literal, you can insert values with instantiation.

```js
//SaneScript

let map = ~{}
let set = ~[]

let map2 = ~{
  foo: 'bar',
  [obj]: value
  func (arg) {
    console.log(arg)
  }
}

let set2 = ~[
  foo,
  bar,
  baz
]
```

```js
//JavaScript

let map = new Map()
let set = new Set()

let map2 = new Map([
  ['foo', 'bar'],
  [obj, value],
  ['func', function (arg) {
    console.log(arg)
  }]
])

let set2 = new Set([
  foo,
  bar,
  baz
])
```

## obj[key], key in obj

Computed property name is an alias of `get/set/delete` method.

`in` expression is an alias of `has` method

> If you want maps, you take maps.

```js
// SaneScript

obj[key]
obj[key] = 'value'
delete obj[key]
key in obj
```

```js
// JavaScript

obj.get(key)
obj.set(key, 'value')
obj.delete(key)
obj.has(key)
```

## misc

`'use strict'` is automatically inserted.
