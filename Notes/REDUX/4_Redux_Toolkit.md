# Reduc Toolkit

First install

```
$ npm install @reduxjs/toolkit
```

It already includes `redux` package , so no need to install that separately.

Few problems, we faced using only `redux`

- Defining mutally exclusive states and their action indepentantly.
- Combing different `Slices` of the store
- Handling Side-effects

## Creating Slices: `createSlice`

[Official Docs](https://redux-toolkit.js.org/api/createSlice)

This function helps us to define `Slice` of state, the state which are mutally exclusive.

This takes a object with

- `name`, this is used to distinguish diffferent slices
- `intialState`, to intialize the state of the slice
- `reducers`, this the collection of different methods for each `actions type`.

### reducers

As we did in `Reducer Function`, for different type of action, we ran different block of code.

Here, defining actions of each type is enhanced by the `reducers`, it takes a object with the method as the `action type`.

In each action function

- First Argument, is the Present state.
- Second Argument, provided action.

This add a few extra things to ease of changing state by using a library called [`immer`](https://immerjs.github.io/immer/), this provides us with which

- We can directly mutate the agrument for the state, behind-the-scence a newer object will automatically will be created
- Mutaing the `state arugument variable` will directly reflect the changes in next state, no need to return a new object.

**Note**: All have to pure function, don't add any Side-effect code in there.

```jsx
import { createSlice } from "@reduxjs/toolkit";

const someSlice = createSlice({
  name: "some",
  intialState: {},
  reducers: {
    // this will create a action creator of type "increment"
    increment(state, action) {
      const { payload } = action;
      /** Here mutating the state will create a newer object with changed state, and assign to the next state
       *
       * Mutaing 'state' will change the next state, no need to return anything
       */
      state.key = value;
      state.someOtherKey = otherValue;
    },
  },
});

someSlice.reducer; // Combined Reducer
someSlice.actions; // Collection of Action Creator
```

The `reducers` object is combine and gives a property `reducer`, which is avaible in the `slice onject`.

### action creators

As, we have seen above, that defining action type can be done by defining a methods in `reducers`.

But, `redux` doesn't directly create a `action`, whereas it simply creates a `action creator`, it is simply a function which takes the `payload` as argument and returns the `action`

All key in `reducers` will have a `action creator` with the same key in `someSlice.actions` and we can use it in the dispatch function.

Using the same example as above.

```jsx
// To Dispatch `increment` action, the below won't work
dispatch({ type: "increment", payload: { ...data } });

/** To dispatch a action use. the below
 *
 * It will have same key as defined in "reducers"
 *
 * the supplied data, will be available in payload
 */
dispatch(someSlice.actions.increment(data));
```

## Combing Slice: `configureStore`

This function is used to combine different slices. This is done by combing some states and reducers. To combine use

```jsx
import { configureStore } from "@reduxjs/toolkit";

const store = configureStore({
  reducer: {
    identifier: slice.reducer,
  },
});
```

So, in `reducer` property in supplied object, we have to provide

- A Identifier for each slice
- The Identifier's value is it's `reducer`, which is

While, accessing state; we have to use the identifier to access state of particular slice.

```jsx
import { useSelector } from "react-redux";

function SomeComponent() {
  const value = useSelector((state) => state.identifier.value);
}
```

## Handling Side-Effects: `redux-thunk`

As `reducers` are pure function, all side-effects can either be placed in `useEffect` or redux provide something called `thunk`
wich is helpful, when you want to have async code in reducers.

This is done by using `action creator`, genrally they are function which return `action`. But in this case it will return a `async function` where the first argument is the dispatch function and body can have any Side-effect (or async code).

```jsx
//Usually
function action_creator(data) {
  return { type: "SOME_UNIQUE_IDENTIFIER", payload: data };
}

//Using redux-thunk, we can also return a async/sync function
function thunk_action_creator(data) {
  return async function action_handler(dispatch) {
    // Run you Side-Effects, here

    use(data); // use the payload data

    // Fetch data to change state
    dispatch(action_creator(fetched_data));
  };
}
```

So, usually by using `@reducjs/toolkit` the `action creators` are made for us. Whereas Thunk is managed by user.

Where to put thunk? As they are not action, it shouldn't go with reducers. So, they are implemented as standalone function.
Always define them below the slice, as to avoid confuse, and make sure to separate out each slice in it's own file.

```jsx
const someSlice = createSlice({});

// Thunk goes here, don't include them inside the slice
const someThunk = (payload) => async (diapatch) => {
  // you can use dispatch to send action to someSlice.
};
```

to use the thunk, use it as we did for any other `action creator`

```jsx
dispatch(someThunk(data));
```
