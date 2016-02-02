Details
========

[Features.md](https://github.com/SaneScript/SaneScript/blob/master/Features.md) is simply a brief concept of the features, but actual compilation from SaneScript to JavaScript may differ due to some technical/minor reasons. This document describes those in-depth SaneScript specification.

# Existential operator

Flags: [ExistentialOperator](https://github.com/SaneScript/SaneScript/blob/master/Flags.md#existentialoperator)

Values that are used more than once will be cached.

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

# Automatic `new` insertion

Flags: [InsertNew](https://github.com/SaneScript/SaneScript/blob/master/Flags.md#insertnew), [IgnoreInsertNew](https://github.com/SaneScript/SaneScript/blob/master/Flags.md#ignoreinsertnew)

List of the default target keywords are below.

- Map
- Set
- WeakMap
- WeakSet
- Date
- Promise
- Proxy
- Error
- ArrayBuffer
- DataView
- (Int|Uint|Float)(8|16|32|64)(Clamped)?Array
- Intl.Collator
- Intl.DateTimeFormat
- Intl.NumberFormat

You can add to, or remove from the keyword list; See [Flags.md, InsertNew and IgnoreInsertNew](https://github.com/SaneScript/SaneScript/blob/master/Flags.md#insertnew).

# Syntactic getter/setter

To support immutable data structures, like Immutable.js, setter transformation is little bit more complicated than described in [Features.md](https://github.com/SaneScript/SaneScript/blob/master/Features.md). This makes it possible to handle immutable data as normal mutable data.

Values used more than once will be cached.

```js
// SaneScript

map[foo] = bar

map['answer'] = state.otherMap['truth'] = 42

getMap()['key'] = 'value' // getMap() is not a LValue
```

```js
// JavaScript

let __saneCollection__, __saneValue__

(
  __saneValue__ = bar,
  __saneCollection__ = map,
  map = __saneCollection__.set(foo, __saneValue__) || __saneCollection__,
  __saneValue__
)

(
  __saneValue__ = (
    __saneValue__ = 42,
    __saneCollection__ = state.otherMap,
    state.otherMap = __saneCollection__.set('truth', __saneValue__) || __saneCollection__,
    __saneValue__
  ),
  __saneCollection__ = map,
  map = __saneCollection__.set('answer', __saneValue__) || __saneCollection__,
  __saneValue__
)

(
  __saneValue__ = 'value',
  getMap().set('key', __saneValue__),
  __saneValue__
)
```

# Hidden property

Flags: [HiddenPropertyPrefix](https://github.com/SaneScript/SaneScript/blob/master/Flags.md#hiddenpropertyprefix), [ExportedSymbolKeyword](https://github.com/SaneScript/SaneScript/blob/master/Flags.md#exportedsymbolkeyword)

Single underscore, with nothing behind, will not be replaced.

Double-underscore-prefixed names will be replaced by single-underscore-prefixed names.

```js
// SaneScript

obj._ = 'foo'
obj.__prop = 'bar'
```

```js
// JavaScript

obj._ = 'foo'
obj._prop = 'bar'
```
