## Redux

- Store `configureStore({...})`, `store.getState()`
- Selector `f: (state) --> any value`
    - `createSelector` (memoized)
- Action `createAction()`, `{ type: domain/eventName, payload: data }`
    - Action Creator, a factory function that returns an action
- Dispatch `store.dispatch(action)`
- Thunk let's you dispatch functions instead of an action, that can dispatch actions over time.
    - `createAsyncThunk` `f: (ActionTypeString, (f: () --> Promise)) --> ?`
- Reducer `f: (state, action) --> state`
    - `createReducer((state, action) => { ... return newState} )`, immutable, synchronous
- Slice `createSlice()`, `combineSlices()`

### Reducer, Slice, Hook, Action

Each slice gets its own reducer that handles that part of the data

_cartSlice.ts_
```js
import { createSlice } from "@reduxjs/toolkit"

type CheckoutState = "LOADING" | "READY" | "ERROR";

export interface CartState {
    items: { [id: string]: number };
    checkoutState: CheckoutState;
}
const initialState: CartState = {
    items: {},
    checkoutState: "READY",
}
const cartSlice = createSlice({
    name: "cart",
    initialState,
    reducers: {},
    // extraReducers allows slice to listen for actions that were defined elsewhere in the app
    extraReducers: function(builder) {
        builder.addCase(
            "cart/checkout/pending", // this reducers will listen for this action's dispatch (even though it's not defined here)
            (state, action) => {
            state.checkoutState = "LOADING";
        })
        builder.addCase(
            "cart/checkout/fulfilled",
            (state, action) => {
            state.checkoutState = "READY";
        })
    }
});

// Redux Toolkit creates a reducer from createSlice
export cartSlice.reducer
```

_productsSlice.ts_
```js
import {
    createSlice,   // To create Slice
    PalyoadAction, // For Action
} from "@reduxjs/toolkit"
export interface ProductsState {
    products: { [id: string]: Product }
}
const initialState = { products: {} }

const productsSlice = createSlice({
    name: "products",
    initialState,
    reducers: {
        // Redux  Toolkit automatically generates Action Creators for each reducer method
        receivedProducts(state, action: PayloadAction<Product[]>) { # create reducer method
            const products = action.payload;
            products.forEach(product => {
                state.products[product.id] = product:
            })
        },
    },
});

export const { receivedProducts } = productsSlice.actions; # Export Action for reducer method
export productsSlice.reducer
```

_store.ts_
```js
import { configureStore } from "@reduxjs/toolkit";
import cartReducer from "../features/cart/cartSlice";
import productsReducer from "../features/products/productsSlice";

export const store = configureStore({
    reducer: {
        cart: cartReducer,
        products: productsReducer,
    }
})

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### Typed Hooks

_hooks.ts_
```js
import { TypedUseSelectorHook, useSelector, useDispatch } = from "react-redux";
import type { RootState, AppDispatch } from "./store"

// Export useAppDispatch and useAppSelector typed
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

_Products.tsx_
```js
import {
    useAppSelector, // Select value from store
    useAppDispatch, // Dispatch Actions
} from "../../app/hooks";
import { receivedProducts } from "./productsSlice";
...
const products = useAppSelector((state) => state.products.products);
dispatch(receivedProducts(products));
```

### Selector

Is a function that takes the Redux state as parameter and returns any value.
The `useAppSelector` functions allows for passing a Selector as parameter.

`createSelector` will remember the value of its final selector, as long as the first one hasn't changed.

_cartSlice.ts_
```js
import {
    createSelector, # Create a memoizied Selector
} from "@reduxjs/toolkit"
import type { RootState } from "../../app/store"
...
// Create a Selector
export function getNumItems(state: RootState) {
    let numItems = 0;
    for (let id in state.cart.items) {
        numItems += state.cart.items[id]
    }
    return numItems;
}

export const getMemoizedNumItems = createSelector(
    (state: RootState) => state.cart.items,
    // this second selector is only called if the previous' value has changed
    (items) => { 
        let numItems = 0;
        # ...
        return numItems;
    }
)

// Combine state from different slices
export const getTotalPrice = createSelector(
    (state: RootState) => state.cart.items,
    (state: RootState) => state.products.products,
    (items, products) => {
        // ...
        return 0;
    }
```

_cartLink.ts_
```js
import { useAppSelector } from "../../app/hooks";
import { getNumItems } from ".cartSlice";

const numItems = useAppSelector(genNumItems);
```

### Middleware

Provides a third party extension point between dispatching and reaching the reducer.

registered in `createStore`

```
const store = createStore(
    myApp,
    applyMiddleware(logger, crashReporter)
)
```

A middleware returns the result of the next dispatch

```
const logger = store => logger => action => {
    let result = next(action)
    // ...
    return result
}
```

#### Thunk

**Thunk** is a middleware that lets you dispatch a function instead of an action (gets `dispatch` and `getState` as arguments). These Thunk-functions can then dispatch multiple Actions over time.

```js
const thunk = store => next => action =>
  typeof action === 'function'
    ? action(store.dispatch, store.getState)
    : next(action)
```

```js
export function checkout() {
    return function checkoutThunk(dispatch: AppDispatch, _state) {
        dispatch({ type: "cart/checkout/pending" });
        setTimeout(function() {
            dispatch({ type: "cart/checkout/fulfilled" });
        }, 500)
    }
}

// using createAsyncThunk
import { createAsyncThunk } from "@reduxjs/toolkit"

export const checkoutCart = createAsyncThunk(
    "cart/checkout",                // Action type
    async (items: CartItems) => {   // Should return a promise
        const response = await checkout(items);
        return response;
    }
)
```

use by `dispatch(checkout())`.

`createAsyncThunk` creates cases (usable in `extraReducer`) for state `fulfilled`, `pending` and `rejected` (states of a Promise). Reducer cases need to be created for these.


