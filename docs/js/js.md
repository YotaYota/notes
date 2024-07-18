# Javascript

## Operators

- `&&` **Logical AND**. returns the value of the *first falsey operand* or the value of the last operand (if all are truthy).
    - `true && "hello"` returns `hello`
    - `0 && "hello"` returns `0`
- `||` **Logical OR**. returns the value of the *first truthy operand* or the value of the last operand (if all are falsey).
    - `42 || "hello"` returns `42`
    - `0 || "hello"` returns `hello`
    - `0 || false || "" ` returns `""`

## Static `import` declaration

Used to import read-only _live bindings_ from another module. The imported
module will be evaluated at load time.

In order to use the `import` declaration in a source file, the file must be
interpreted by the runtime as a module.

If the same module is imported into multiple other modules, its code is executed
only once, upon the first import. Then its exports are given to all further
importers.

`import` declarations can only be present in modules, and only at the top-level.

There are 4 form of `import` decalarations

- Named import `import { export1, export2 } from "module-name";`
- Default import `import defaultImport from "module-name";`
- Namespace import `import * as name from "module-name";`
- Side effect import `import "module-name";`

## Promises

`Promise.all()` returns a promise that waits for all of the promises in
the array to resolve and then resolves to an array of the values that these
promises produced (in the same order as the original array).
If any promise is rejected, the result of Promise.all is itself rejected.

`Promise.allSettled()` is same as above, but does not reject if any
promises do.

`Promise.race()` returns the first promise that settles (fulfilled or rejected).

`Promise.any()` returns the first promise that settles (only fulfilled).
