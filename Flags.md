Compiler flags
===============

Compiler flags are the way to tell SaneScript compiler how to handle this block of code, like `'use strict'` in JavaScript.

Flag set is inherited from outer block, and root block's flag set is inherited from default flag set.

Syntax for compiler flag is `'set <flag-name> <flag-value>'` and then that name of flag is set to a string as specified. You can omit `<flag-value>` then flag is setted as `Boolean true`.

Flag name is not case-sensitive, while flag value is case-sensitive.

Custom flag names can be used for language plug-ins. By default, they're safely ignored.

List of language-default flags are below. If behavior is not specified, `true` means activate and other then `true` means deactivate.

Recommended pattern to use switch-like flag is, `'set FLAG_NAME'` to turn it on, and `'set FLAG_NAME off'` to turn it off.

# SaneScript

Default: none

Switch for SaneScript compiler. Additionally, you can specify the version of the SaneScript specification.

## Behavior

- `true`

  Transpile this block as a latest available version of the SaneScript specification.

- `'off'`

  Do not transpile this block.

- else

  Transpile this block as a specified version of the SaneScript specification.

# ExistentialOperator

Default: `'$'`

Set [existential operator](https://github.com/SaneScript/SaneScript/blob/master/Features.md#existential-operator) symbol.

## Behavior

- String

  Change existential operator symbol to specified string.

  ```js
  'set ExistentialOperator is_exist'

  obj.$        // not transformed
  obj.is_exist // transformed to `obj != null`
  ```

- else

  Restore existential operator symbol to default value `'$'`

# AddNewInsertionTarget

Default: none

Add target name for [automatic `new` insertion](https://github.com/SaneScript/SaneScript/blob/master/Features.md#automatic-new-insertion)

Notice that automatic `new` insertion is triggered by checking identifier name, not an object in that reference.

## Behavior

- String

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

See above [AddNewInsertionTarget](target name for [automatic `new` insertion](https://github.com/SaneScript/SaneScript/blob/master/Details.md#addnewinsertiontarget))

# HiddenPropertyPrefix

Default: `'_'`

Set [hidden property](https://github.com/SaneScript/SaneScript/blob/master/Features.md#hidden-property) prefix symbol.

## Behavior

- String

  Change hidden property prefix symbol to specified string.

  ```js
  'set HiddenPropertyPrefix $$'

  obj._prop    // not transformed
  obj.$$prop   // transformed to Symbol

  obj.$$       // not transformed
  obj.$$$$prop // transformed to '$$prop'
  ```

# DisableEqualityOperator

Default: none

Switch for [sane equality operator](https://github.com/SaneScript/SaneScript/blob/master/Features.md#sane-equality-operator)

# DisableComparisonOperator

Default: none

Switch for [chainable comparison operator](https://github.com/SaneScript/SaneScript/blob/master/Features.md#chainable-comparison-operator)

# DisableSwitchCase

Default: none

Switch for [better `switch`/`case`](https://github.com/SaneScript/SaneScript/blob/master/Features.md#better-switchcase)

# DisableExistentialOperator

Default: none

Switch for [existential operator](https://github.com/SaneScript/SaneScript/blob/master/Features.md#existential-operator)

# DisableNewInsertion

Default: none

Switch for [automatic `new` insertion](https://github.com/SaneScript/SaneScript/blob/master/Features.md#automatic-new-insertion)

# DisableSyntacticGetterSetter

Default: none

Switch for [syntactic getter/setter](https://github.com/SaneScript/SaneScript/blob/master/Features.md#syntactic-gettersetter)

# DisableHiddenProperty

Default: none

Switch for [hidden property](https://github.com/SaneScript/SaneScript/blob/master/Features.md#hidden-property)

# DisableMacro

Default: none

Switch for [macro](https://github.com/SaneScript/SaneScript/blob/master/Features.md#macro)

# DisableDedentTemplateString

Default: none

Switch for [de-indent multi-lined template strings](https://github.com/SaneScript/SaneScript/blob/master/Features.md#miscellaneous)

# DisableWrapAwaitArrayLiteral

Default: none

Switch for [wrap array literal within `await` expression with `Promise.all`](https://github.com/SaneScript/SaneScript/blob/master/Features.md#miscellaneous)
