# Angular

Single Page Application (SPA).

## Component

`@Component(...)`

- Define Views.
- Can use a Service.

Inject the store in order to be able to gain access to the global state.

## Service

`@Injectible(...)`

## Template

- directive
- binding markup
  - event
  - property
- pipes

- `<ng-template>`
- `<ng-container>` is not rendered

## Module

`@NgModule(...)`

3 files
- `name.module.ts`
  - `imports`: things the module uses
  - `declarations`: everything in the module
  - `exports`: exported components
- `name-provider.ts`, class services should assign to
- `name-routing.ts`, declares paths

Declares compilation contex for a set of modules.

## Resolver

- In Routing module
- Connected to a path
- Run before anything is showed and gives data to the component on the path

## Guard

- Run before resolvers
- Used for access verification

## RxJS

`async`

- `.switchMap`
- `switchMapTo`
- ...

### Observable

`varName$`

- `.pipe(...)`
- `.subscribe(...)`

## NgRx store

Global state.

`store.dispatch(Action)` updates state.

### Reducer

- Create a new state (cannot change current state object)
- Pure functions
- no side effects
- sync

Move as much logic as possible here from component.

### Selector

- filters state
- used in `.pipe()`

### Effect

- Listen on `.ofType(Action)` and produces new Action.
- async

