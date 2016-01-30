Compiler flags
===============

Compiler flags are the way to tell SaneScript compiler how to handle this block of code, like `'use strict'` in JavaScript.

Flag set is inherited from outer block, and root block's flag set is inherited from default flag set.

Syntax for compiler flag is `'set <flag-name> <flag-value>'` and then that name of flag is set to a string as specified. You can omit `<flag-value>` then flag is set as `Boolean true`.

Flag name is not case-sensitive, while flag value is case-sensitive.

Custom flag names can be used for language plug-ins. By default, they're safely ignored.

List of language-default flags are below.

# ExistentialOperator

Default: `'$'`

Set [existential operator](https://github.com/SaneScript/SaneScript/blob/master/Features.md#existential-operator) symbol.

## Behavior

- none or `true`

  Restore existential operator symbol to default value `'$'`

- else

  Change existential operator symbol to specified string.

  ```js
  'set ExistentialOperator is_exist'

  obj.$        // not transformed
  obj.is_exist // transformed to `obj != null`
  ```

# AddNewInsertionTarget

Default: none

Add target name for [automatic `new` insertion](https://github.com/SaneScript/SaneScript/blob/master/Features.md#automatic-new-insertion)

Notice that automatic `new` insertion is triggered by checking identifier name, not an object in that reference.

## Behavior

- none or `true`

  Behavior not defined, ignored.

- else

  Flag value is treated as a space-separated list of identifiers.

  ```js
  'set AddNewInsertionTarget MyClass lib.OtherClass'

  let obj1 = MyClass() // transformed to `new MyClass()`
  let obj2 = lib.OtherClass() // transformed to `new lib.OtherClass()`

  let SameClass = MyClass
  let obj3 = SameClass() // not transformed
  ```

# RemoveNewInsertionTarget

Default: none

Remove target name for [automatic `new` insertion](https://github.com/SaneScript/SaneScript/blob/master/Features.md#automatic-new-insertion)

## Behavior

See above [AddNewInsertionTarget](https://github.com/SaneScript/SaneScript/blob/master/Details.md#addnewinsertiontarget)

# HiddenPropertyPrefix

Default: `'_'`

Set [hidden property](https://github.com/SaneScript/SaneScript/blob/master/Features.md#hidden-property) prefix symbol.

## Behavior

- none or `true`

  Behavior not defined, ignored.

- else

  Change hidden property prefix symbol to specified string.

  ```js
  'set HiddenPropertyPrefix $$'

  obj._prop    // not transformed
  obj.$$prop   // transformed to Symbol

  obj.$$       // not transformed
  obj.$$$$prop // transformed to '$$prop'
  ```
