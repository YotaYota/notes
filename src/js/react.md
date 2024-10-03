# React

## Folder Structure

- *public/index.html*
- *src.index.js*

Only files in *src/* are processed by webpack.

Only files in *public/* can be used from *public/index.html*.

Other top-level directories will not be included in the production build.

## Component Lifecycle

1. **Triggering**: Happens because of
    - it is the initial render, or
    - the component’s (or one of its ancestors’) state has been updated. Ie, `set` function has been called.
2. **Rendering**: React calls the component function. It's a recursive process in the sense that it also calls child components.
    - initial: React will call the root component.
    - re-render: React calls the function component whose state update triggered the render, and calculates the difference since previous render.
3. **Committing**: React commit changes to the DOM.
    - initial: React uses `appendChild()`
    - re-render: React applies minimal nevessary operation calculated from *Rendering* phase.

**Note**: React only changes the DOM nodes if there is a difference between renders.

After these steps, the browser will repaint the screen.

## useEffect hook

```js
useEffect(() => {
    <LOGIC>
    return <CLEANUP FUNCTION>
}, [<DEPENDENCIES>])
```

- `useEffect(() => {...})` runs on every render
- `useEffect(() => {...}, [])` runs on first render
- `useEffect(() => {...}, [x, y])` runs when `x` or `y` renders with a new value

### Example: Abort fetch request

```js
useEffect(() => {
    const controller = new AbortController()
    const signal = controller.signal

    fetch('google.com', { signal })
        .then((res) => res.json())
        .then((data) => {
            console.log(data)
        }).catch((err) => {
            if(err.name === 'AbortError') {
                console.log("cancelled request")
            } else {
                // Handle error
            }
        })

    return () => {
        controller.abort()
    }
}, [<DEPENDENCIES>])
```

### Example: Abort timer

```js
useEffect(() => {
    const interval = setTimeout(() => {
        console.log("hey ho!");
    }, 5000);

    return () => { clearTimeout(interval); }
}, [<DEPENDENCIES>]);
```

## useState hook

```js
const [something, setSomething] = useState(<INITIAL STATE>)
```

**Note**: State variable `something` is not updated until next render. Ie, the new value is not directly present in the current snapshot of the component after `setSomething()`.

## Context

Contexts are global and have no JSX.

1. `createContext`

```js
import { createContext } from 'react';

export const MyContext = createContext("initialValue");
```

2. `useContext`

```js
import { useContext } from 'react';
import { MyContext } from './MyContext';

const MyContext = useContext(MyContext);
```

3. Provide context

```jsx
import { MyContext } from './MyContext';

function MyComponent() {
    return (
        <MyContext.Provider value={...}>
            {children}
        </MyContext.Provider>
    )
}
```

## ESLint and Prettier

Prettier is for formatting rules. ESLint is for code quality rules.

- [eslint-config-airbnb](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb) is a base config for react. `npx install-peerdeps --dev eslint-config-airbnb`
- [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) turns off rules that conflict with prettier, needs to be last in extends
- [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier) turns prettier changes inte ESLint rules (not neccessary)

```sh
npx install-peerdeps --dev eslint-config-airbnb
npm install --save-dev --save-exact prettier
```

*.eslinttrc.json*
```json
{
  "extends": [
    "airbnb",
    "prettier" // eslint-config-prettier
  ]
}
```

**Note:** *eslint.config.js* is used for eslint >= v8

formatting
```
npx prettier . --write
npx eslint . --fix
```
