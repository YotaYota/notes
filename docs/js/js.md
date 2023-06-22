# Javascript

## Static `import` declaration

Used to import read-only *live bindings* from another module. The imported
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

