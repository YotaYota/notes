# Angular

Single Page Application (SPA).

## Component

`@Component(...)`

- Define Views.
- Can use a Service.

Inject the store in order to be able to gain access to the global state.

### Interaction

- `@Input()`
- `@Output()`
- `ngOnChanges()`

#### `@Input()`

Either a variable

```js
@Input() aVariable: string;
...
[aVariable]="value"
```

or a setter function

```js
aVariable: string;
@Input()
set aVariable(inputVar: string) {...}
...
[aVariable]="value"
```

#### `@Output()`

#### `ngOnChanges()`

### Lifecyle Component/Directive Hook Methods

- `ngOnChanges()`
- `ngOnInit()` once
- `ngDoCheck()`
- `ngAfterContentInit()` once
- `ngAfterContentChecked()`
- `ngAfterViewInit()` once
- `ngAfterViewChecked()`
- `ngOnDestroy()` once

Angular only calls a directive/component hook method if it is defined.

## Service

`@Injectible(...)`

- use `providedIn` to declare module `name-provider.ts` that provides it

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

- `.map`
- `.switchMap`
- `.switchMapTo`
- `.combineLatest`
- ...

### Observable

`varName$`

- `.pipe(...)`
- `.subscribe(...)`

**Note**: subscribing via an `async` pipe will automatically unsubscribe. When manually subscribing,
unsubscribe has to be called to prevent memory leaks (e.g. in `ngOnDestroy()`).

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

