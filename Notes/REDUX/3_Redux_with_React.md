# Redux with React

So, at Redux's Core, it is just a state mangement library. So, to proper to mix-in with React, some of the funanalities and binding are introduced with are in [react-redux](https://react-redux.js.org/)

## Creating a Store

So, the store in created in similar way as we did in [Declaring Store](./2_Redux.md#creating-a-store) and reducers also is the same as it is not redux specific it is just a JS function.

```js
import { createStore } from "redux";

const store = createStore(Reducers);
```

Here, extracting state and using dispatch is different.

## Declaring Provider Scope

In `react-redux`, a Component name `Provider` is declared. It basically determines the `tree` to which the store is available.
Here, `tree` is the the components tree wrapped within it.

Here, you have to provide the `store` prop with the store to which you ike to store.

```jsx
import { Provider } from "react-redux";
import { store } from "/"; // store is basically the create object as shown above

function storeProvider(props) {
  return <Provider store={store}>{props.children}</Provider>;
}
```

So, to whichever you wrap it, the entire component tree will have access to states.

If you want Redux as Global Stae Management, as it made for that use it in `index.js` file as it is the top most level.

## Extracting State And Dipatch function

### using `useSelector` Hook

As, Hook can only be used in functional components. It can't be used in Class-based Components.

It is simply takes a callback function, where the first argument as the state, you can return a piece or the whole state as per convenience.

```jsx
import { useSelector } from "react-redux";

const partialState = useSelector((state) => {
  return state.piece;
});
```

So, in whichever you use this Hook, automatically `Subcription` will provided. No need to Subscribe like we did in [subscribe](2_Redux.md#using-the-store-state-an-subscriber).

### using `useDispatch` Hook

```jsx
import { useDispatch } from "react-redux";

const dispatch = useDispatch();
```

You can then simply call `dispatch` with appropriate actions.

### using `connect` function

This is the only way use state in Class-based Components. It can also be used in functional.
So, it is little complicated,

- Arguments
  - First, `mapStatesToProps`
  - Second, `mapDispatchToProps`
- Returns, a function
  - The function takes Component as Argument
  - Returns the transformed Componet

#### `mapStateToProps`

This the First Argument in `connect` function.

It is callback function

- The first argument is the `store` state
- The second argument is props to component, whichever supllied later.

```jsx
function MapStateTOProps(state, props) {
  return {
    key: transform(state, prop),
    storeState: state, // Or just supply the whole state
  };
}
```

All the above set key will be available as props in the supplied components.

#### `mapDispatchToProps`

This the Second argument in `connect` function.

It is similar to `mapStateToProps`

- The first argument is the Dispatch function of the store
- The Second argument is props

```jsx
function MapDispatchToProps(dispacth, props) {
  return {
    method :() => dispaatch(action)
    dispatch, // Or just supply the Dispatch itself
  };
}

// To access
this.props[key];
```

So, for both above, gives us a change to modularize our code.

Then, once you write `mapStateToProps` and `mapDispatchToProps`

```jsx
import { connect } from "react-redux";

const transformComponent = connect(mapStateToProps, mapDispatchToProps);

// Use transformedComponent in other components To have access to redux store states
const transformedComponent = transformComponent(Component);
```

## Some Lacking Features with `redux`

As, we already seen, [Redux Flow](./2_Redux.md#redux-data-flow) and how can we use it with react.

Some of lacking feature with using redux is

- It doesn't provide a way to combine different store to form a bigger store.
  - This is useful as e can define mutally independent state and then combine them to form a global state.
  - This ease up the process of using different store in bigger application.
- It doesn't provide tools and guidelines to define action.
- Reducers becomes quite complex, if there are different action for mutally excluisve states.
- Handling async (or Side Effects) operation

Therefore, redux introduced a super-set library for redux called `@reduxjs/toolkit`, which includes `redux` and other libraries to ease up the process of

- Combining different slices.
- Ease of defining and using actions.
- Handling and using Side-effects.
