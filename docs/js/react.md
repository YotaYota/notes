# React

## useEffect hook

```js
useEffect(() => {
    <LOGIC>
    return <CLEANUP FUNCTION>
}, [<DEPENDENCIES>])
```

## useState hook

```js
const [value, setValue] = useState(<INITIAL STATE>)
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

**Note:** *eslint.config.js* us used for eslint >=v8

formatting
```
npx prettier . --write
npx eslint . --fix
```
