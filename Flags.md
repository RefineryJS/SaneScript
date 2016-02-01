Compiler flags
===============

Compiler flags are the ways to tell the SaneScript compiler how to handle a specific block of code. Think of `'use strict'` in JavaScript.

Flag set is inherited from outer block, and root block's flag set is inherited from default flag set.

Syntax for compiler flag is `'set <flag-name> <flag-value>'` which sets the flage with the name `<flag-name>`to string `<flag-value>`. If `<flag-value>` is omitted, flag `<flag-name>` is set to Boolean `true`.

`<flag-name>` is not case-sensitive, while `<flag-value>` is.

Custom flag names can be used by language plug-ins. By default, unspecified `<flag-name>`s are safely ignored.

List of default flags are below.

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

  The expression is ignored.

- else

  Flag value is treated as a space-separated list of identifiers, and those identifiers are added to the list of targets.

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

  The expression is ignored.

- else

  Change hidden property prefix symbol to specified string.

  ```js
  'set HiddenPropertyPrefix $$'

  obj._prop    // not transformed
  obj.$$prop   // transformed to Symbol

  obj.$$       // not transformed
  obj.$$$$prop // transformed to '$$prop'
  ```
