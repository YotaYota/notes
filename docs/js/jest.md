# Jest

- `jest.fn()` - creates function mock
- `jest.spyOn()` - replace existing method of any class or imported module.
  **Note**: still calls the original function unless other implementation is provided
- `jest.mock()` - overrides whole existing module and provides its replacement

## `jest.spyOne()`

**Note**: `.mockReturnValue()` etc are shorthand notation for
`.mockImplementation(...)`, so they also replace the implementation.


## `jest.mock()` and `jest.doMock()`

```
import { A } from 'B';
jest.mock('B'); // hoisted
const mockA = A as jest.MockedFunction<typeof A>;
```

**Note**: `jest.mock()` is hoisted because module needs to be mocked _before_ it is imported.
This means that it is **not** possible to use any variables in the mock.

For this reason there is `jest.doMock()` which is executed at place. But import statements are
also hoisted which means they need to be changed into a non-hoisted version.

```js
import {CONSTANT_VALUE} from './constants';
jest.doMock('./myModuke', 8) => ({
        doSomething: jest.fn(() => CONSTANT_VALUE),
    })
);
// Class containing doSomething function
const {Class} = require('./Class');
)
```

## Clear

- `jest.clearAllMocks()` - clear all mock usage data
- `jest.resetAllMocks()` - clear all mock usage data and also resets implementation with new `jest.fn()`
- `jest.restoreAllMocks()` - restore mocks back to original implementation (**Note**: only applies for `jest.spyOn`!)
