Macro
=====

This document is a specification of default macros of SaneScript.

## ~Map()

This macro convert object literal to ES2015 Map.

### Features

1. Transform object literal's key-value pairs to Map's key-value pairs.
1. Computed key's inner expression become map's key directly, without stringify.
1. Spread property transfered directly, to make it easy to creating a map from existing map.

### Example

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
