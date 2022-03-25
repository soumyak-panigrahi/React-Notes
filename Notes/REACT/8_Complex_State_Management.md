# State Management

So, we have already discussed about [States](./4_States_and_event.md#state), but managing complex states using `useState` can give unreadable codes. So, other state management are introduced.

## useReducer

So, think of it as extension of useState but flow of it bit different, recall [setState or setData](./4_States_and_event.md#setdata) this works in two ways either call it with the `data or next state` or provide a callbackFn which returns the
`next state`, the second approach is more recommanded as it will take care of asynchrous behaviour of `previous state` by
providing the first parameter of the callbackFn by `Previous State`.

This works fine if your handling independent states or if the state is not heavily intervened. But if your state depends on
other state's `previous state`, then using by a callBackFn method will lead to less Robust code because we are handling
and supling data within `setState` react hook.

So, in `useReducer`, we are dealing with `suppling` and `handling` of states separately.

```
The Syntax to use is

[state , dispatch] = useReducer(Reducer, init_State , init_StateFn)

Follow, the naming convention for

    dispacth as dispatch[State]
    Reducer as [state]Reducer

state is the name of your state.

```

here, state works in a similar way to that of `useState`.

- `dispatch`, will take the data called `action` and supply it the `Reducer`, it will not set the next state
- `Reducer(prevState , action)` is should be supplied as a calledBackFn, the returned value is set as the next state
  - The first Parameter is supplied with the `Previous State`
  - The Second Parameter is supplied by the dispatch's `action`
- `init_State` is intial state value for the state.
- `init_StateFn` is the function which can be used to intialize state.

So, the flow of `useReducer`

```
                                            --------------------------------------
                                            |                                    |
                                            V                                    |
dispatch( action )           Reducer(previous_State, action) ---[ Returns ]--> next_State
            |                                           ^
            |                                           |
            ---------------------------------------------
```

## Context

As we have already seen the data flow between component is done by props. If from `parent to child` by `setting attributes` and
`child to parent` by `setting handlers`.

So, a component can only supply data to their children. Hence, if you want to supply some data to some grandchild, then you have
to supply it to the `grandchild's parent` who is one of the children. And then from there you can set the props and then
that grandchild will get he data.

This is fine if the depth is between 2-3, but what if you want to supply data to far way successor (let say of depth 20), then you have to forward is 20 times and the intermediate component are not even using it. So, this way of doing is not always efficient.

So, now here we will use `context`, think of context as environment in which you can create environment variable and anyone within
this environment has access to all environment variable but it can only supplied by the environment itself.

So, to create a context,

```
Use:

const ContextComponent = createContext({...context});
```

Note: context and `Context Data` will be used interchangable

So, `createContext` will create a `Context` Component with the given data as it's `context`.

And it has two member `Provider` and `Consumer`, so `Provider` will be incharge of suppling of `context`
and consumer can use these `context`. So, to create a context environment, `Provider` will be the root of
sub-tree and all the node of this sub-tree will have access to those `context` or will act like a `Consumer`.

```
() -> root
(*) -> Any Node

                        (Provider) -> will supply the context
                         / | \
                        /  |  \
                       .   .   .
                      .    .    .
                       (*Consumer) -> will use the context
```

So, whenever we use `createContext`, it will return a Component which will be `Context Component` and
it will have two member Component `Provider` and `Consumer`. So, that same `context` can have different
provider, and it depends on the usecase.

In other words, `Context` is peice of information residing for a usecase, for example StrictContext , SecureContext are both Context will be suppliy some extra peice of information as to enact the context like in Strict you will be only limited access to information and in Secure you will have to go through number of authetication etc. Another example is FreeContext and PaidContext for some application, If a user in `Free` context then some feature will be disabled whereas in `Paid` Context all feature will be enabled and also some extra information will also be provided.

So, think of context as different states of a environment. In our case the subTree with `Provider` as the root will acts as the
environment.

### Provider

In any context, a `Provider` is a Component which supply the `context` for a environment, in this the sub-tree with the
`Provider` as root.

To Use Provider, first make `ContextComponent` by using `createContext`. Then use the member `Provider`

```jsx
<ContextComponent.Provider value={context}>
  // JSX Code Here
</ContextComponent.Provider>
```

So, all children and their successor will get context of this given Provider. Now to set the `context`, use Attribute `value` of
`Provider`.

#### Creating a Provider Environment

Some points, as how to make a Context Provider a Independent unit. So, First make sure to divide your context data as
which are static and dynamic, static value can be hard coded whereas for Dynamic data make sure to make state for either
by using `useState` or `useReducer` (depending on the complexity). Other than that make sure to supply different setters as
to change state of States, setters are important don't directly supply the setter itself but some self-contained function
which will be used in specific usecase.

- Static is eaily manage, so just hard code them.
- Dynamic data, states must be made, and also different scenrio setters. You can directly supply the setter just it is best
  to make different function for each usecase as to make the context more robust.

Make to sure to use different `Context` by using `createContext`, for independent Contexts.

For Example, We are making a context for Autheticating

```jsx
import React, { useState } from "react";

const AuthContext = React.createContext({
  isAuth: false,
  authToken: {
    value: "",
    SessionID: "",
  },
});

function SomeContextProvider(props) {
  const [auth, setAuth] = useState(false);
  const [token, setToken] = useStae({
    value: "",
    SessionID: "",
  });

  function onClearAuth() {
    setAuth(false);
  }

  function onReceivingToken(obj) {
    const { token, SessionID } = obj;
    if (token.trim.length) {
      setAuth(true);
    }
    setToken({ token, SessionID });
  }

  function onClearingToken() {
    setToken({ token: "", SessionID: "" });
  }

  return (
    <AuthContext.Provider
      value={{
        isAuth: auth,
        authToken: { ...token },
        onClearAuth,
        onReceivingToken,
        onClearingToken,
      }}
    >
      {props.children}
    </AuthContext.Provider>
  );
}
```

So, in above example we are creating a context using the states `auth` and `token`. These are used in context. And
we are not directly supply `setAuth` or `setToken` rather different different setter with concerned procedure.
It is just for clear Code not always applicable.

### Consumer

So, there are two ways to access the context, First by using the member `Consumer` and other using `useContext` hook.

#### Consumer Member

To use it by this method first go where you want to consume the context. and Wrap around then with the `Consumer` of the
`Context Component`. Then call a callbackFn has the first parameter is provided with the context.

```jsx
import SomeContext from "./location";

function SomeConsumerComponent(props) {
  return (
    <SomeContext.Consumer>
      {(context) => {
        // here use context to have access
      }}
    </SomeContext.Consumer>
  );
}
```

This may lead to bloated code, it is not recommanded to use.

#### useContext

Just pass the `Context Component` as the argument in `useContext`.

```jsx
import { useContext } from "react";
import SomeContext from "...";

function SomeConsumerComponent(props) {
  const context = useContext(SomeContext); // Simple just supply the Context Component Object
}
```

Note:

- Don't call `useContext` with either `Provider` or `Consumer`, just pass the Context Component itself.
- If your calling a Consumer with no provider, the default value will be passed over.
