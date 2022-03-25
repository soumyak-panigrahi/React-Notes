# Class-based Components and Error Boundary

So, till now we have been dealing with `functional Components`, so in genral functional components are much leaner
and efficient compared to class-based ones. So, it will be mostly it is prefered to use `functional Components`.

The only use case for `Class-based Components` is for `Error Boundary`, which is discussed later.

## Class-based Components

### Declaration

First of all, every Class-based Components are Descendant of `React.Component` Class. So, to make it work
we have to override some of the function, the important one is `render`, this the function which returns the JSX code.

```jsx
import React from "react";

class ClassBasedComponent extends React.Component {
  render() {
    // Write your JSX Code here
    return JSXCode;
  }
}
```

### Accessing props

use the `props` property to access it.

```jsx
import React from "react";

class ClassBasedComponent extends React.Component {
  render() {
    /* access it with props object*/
    this.props;
    return JSXCode;
  }
}
```

**Note**, while passing callback function for event handling and others. Make sure to `bind` the `this` with current object.

### Definig State

First of all, in `constructor` make sure to call the super before writing any code.

The intial state are declared inside the `constructor`. We have to use state we have to overwrite a predefined object `state`.
And to set use the function `setState`.

- `state` is the object which holds all the states.
- `setState` takes a object and merges it with the state
  - It can also take a callback Fn, like we did in `useState` with first argument the `Previous State`.
  - the only difference between this.setState and `useState`'s `setState` is that the first one merges the object and the other overwrites it.

```jsx
import React from "react";

class ClassBasedComponent extends React.Component {
  constructor() {
    super();
    this.state = {
      state1: value1,
    };
  }

  foo() {
    /* Note this will merge not overwrite, so state1 will still reside */
    this.setState({
      state2: value2,
    });
  }

  render() {
    // Write your JSX Code here
    return JSXCode;
  }
}
```

### Defening Context

So, in classed based without using `Context.Consumer`, you can only use one context.

To do so,

- Assign a static variable named `contextType` with that Context component.
- use `context` property to use the Context.

```jsx
import React from "react";
import SomeContext from "/";
class ClassBasedComponent extends React.Component {
  static contextType = SomeContext;

  foo() {
    /* To use access */
    this.context.key;
  }
  render() {
    // Write your JSX Code here
    return JSXCode;
  }
}
```

### Effects and Component Life Cycle

Like we used `useEffect` for handling sideeffects, to deal Side-effects in class-based.

We have to override some methods.

- `componentDidMount`, this will run once the component is rendered first time or Mounted.
  - This is equivalent to `useEffects( , [])`
- `componentDidUpdate`, this will run after the the component is re-evaluated. In this
  - The First argument will hold the `previous props` value
  - The Second argument will hold the `previous state` value
- `componentWillUnmount`, this will called before the component is removed or Unmounted.
  - Equivalent to the cleanUp with no dependencies.

## Error Boundaries

More about it on [Official Docs](https://reactjs.org/docs/error-boundaries.html#introducing-error-boundaries)

This `functional Components`, there is no way to handle a Error thrown from a Component as they are not really
used as function in the JSX Code, so using `try ... catch` will not work.

For Error Handling it can only to done by using `Class-Based Components`. This should only be used for unexpected
and unforseen Error, dont use it to change the Control Flow.

To make a Component to act like Error Boundary, override the method `componentDidCatch`, so if any component within
this sub-tree throws an Error then it will be handled here.

```jsx
import React from "react";

class ErrorBoundary extends React.Component {

 componentDidMount(err) {
     // Handle the Error here
 }
  render() {
    /* Return Error Jsx Code*/
    if(/*Error Took Place*/)
        return ErrorJSXCode;

    /*if no Error, simply forward the Childrens*/
    return this.props.children
  }
}

/* Simply wrap the component to create the Bounday*/

<ErrorBoundary>
    {JSXCODE}
</ErrorBoundary>

```
