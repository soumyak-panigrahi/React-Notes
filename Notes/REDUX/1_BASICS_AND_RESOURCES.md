# Redux

At it's core it is a State Mangement Library. We have been using react provided state management either using
hooks like `useState , useReducer` or Class's `state object`. These are only proved to be helpful for small
state which extends to local.

To handle complex state mangement, we have to use library like `REDUX`. First, they provide a global scoped statemangement.
So, some point to note

- If your managing Local State, then better to use `useState` or `useReducers`.
- If your State extends to a lot of Component or their is a need for global state, then it is better to use `Redux`.

## Redux vs Context

So, we can also use [Context](../REACT/8_Complex_State_Management.md#context) to manage global state as we have seen.

So, Context drawbacks are

- It is only designed to manage a single Global State or Context. So, for each Context a differtent `context component` is needed.
  - So, this many lead to a lot of different Context, and invariably it leads to unclean code
- And it is not optimized to perform a highly volatile state. or in other words if the change frequency is high it will lead to a slow environment.

So, few thing Redux provide is

- A Global State Mangement System
- A Environment to Collectively Control or Process different pieces of Global State
- A highly optimized environment for volatile state

Redux is not really needed,

- If you don't need a environment to collectivly control different pieces of Global State
  - means a few Global Context is only needed.
- If you state is not highly volatile

[Some point as to when to use Redux](https://redux.js.org/faq/general#when-should-i-use-redux)

So, Redux is more Library needed, when your website has a lot of routes (means to very big website) like that of amazon.
Therefore, using it in small - medium application is very healthy.

## Setup

First of all, redux is not react-only state management library. So, it has to used in two step one genral Global State Managemnt and the binding it with react.

The Official Docs for [Redux](https://redux.js.org/), this is the genral library. To intall use

```
$ npm install redux
```

The Official React Binding is [react-redux](https://react-redux.js.org/), this is where redux is integrated with react. To install use

```
$ npm install react-redux
```

To use Redux in react, we have to use both.
