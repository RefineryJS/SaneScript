Compiler flags
===============

Compiler flags are the ways to tell the SaneScript compiler and it's plug-ins how to handle a specific block of code. Think of `'use strict'` in JavaScript.

Syntax for compiler flag is `'set <flag-name> <flag-value>'` which sets the flag with the name `<flag-name>`to string `<flag-value>`. If `<flag-value>` is omitted, flag `<flag-name>` is set to empty string. `<flag-name>` is not case-sensitive, while `<flag-value>` is. `<flag-name>` can't hold whitespace character.

Effect of compiler flag will be automatically removed at the end of the block where it is located.

Custom flag names can be used by compiler plug-ins. `<flag-name>`s that nobody uses are safely ignored.

List of default flags are below.

# ExistentialOperator

Default: `'$'`

Set [existential operator](https://github.com/RefinedJS/SaneScript/blob/master/Features.md#existential-operator) symbol.

```js
'set ExistentialOperator is_exist'

obj.$        // not transformed
obj.is_exist // transformed to `obj != null`
```

- none or `''`

  Inherits existential operator symbol from outer block.

- else

  Change existential operator symbol to specified string.

# HiddenPropertyPrefix

Default: `'_'`

Set [hidden property](https://github.com/RefinedJS/SaneScript/blob/master/Features.md#hidden-property) prefix symbol.

```js
'set HiddenPropertyPrefix $$'

obj._prop    // not transformed
obj.$$prop   // transformed to Symbol

obj.$$       // not transformed
obj.$$$$prop // transformed to '$$prop'
obj.__prop   // not transformed
```

- none or `''`

  No effect.

- else

  Change hidden property prefix symbol to specified string.

# ExportedSymbolKeyword

Default: `'_symbol'`

Set export keyword of module's symbol pool.

This flag must be in module's root block. Ones that placed in inner block will throw error.

- none

  No effect.

- `''`

  Prevent exporting module's symbol pool.

- else

  Set export keyword of this module's symbol pool to specified string.

# InsertNew

Default: none

Add target name for [automatic `new` insertion](https://github.com/RefinedJS/SaneScript/blob/master/Features.md#automatic-new-insertion)

Notice that automatic `new` insertion is triggered by checking identifier name, not an object in that reference.

```js
'set InsertNew MyClass lib.OtherClass'

let obj1 = MyClass()        // transformed to `new MyClass()`
let obj2 = lib.OtherClass() // transformed to `new lib.OtherClass()`

let SameClass = MyClass
let obj3 = SameClass()      // not transformed
```

- none or `''`

  No effect.

- else

  Flag value is treated as a space-separated list of identifiers, and those identifiers are added to the list of targets.

# IgnoreInsertNew

Default: none

Remove target name for [automatic `new` insertion](https://github.com/RefinedJS/SaneScript/blob/master/Features.md#automatic-new-insertion)

See [above](https://github.com/RefinedJS/SaneScript/blob/master/Details.md#insertnew) for behavior.
