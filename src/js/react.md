# React

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

## useState hook

```js
const [something, setSomething] = useState(<INITIAL STATE>)
```

**Note**: State variable `something` is not updated until next render. Ie, the new value is not directly present in the current snapshot of the component after `setSomething()`.

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
