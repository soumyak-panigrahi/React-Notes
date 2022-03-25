# Optimization Techniques

So, first of all any Component is re-evaluated based on changes in any state. Therefore, the evaluation and Rendered is done
by two separate libraries name `react` and `react-dom`.

- `react`, as seen is mostly responsible for State Mangement and Conversion of Jsx to Js.
- `react-dom`, is interaction with the DOM, it handles rendering.

First of all, Re-evaluating means for Functional component is that they re-execuetes the function and
Re-rendering means the Dom is being Manipilated.

## Default Scenario

So, by default `react` re-evaluates each time a state changes, this includes

- The present Component is Re-evaluated.
- All the children are Re-evaluated, this means all the grand and futher down children are also re-evaluated.
- If the State is used in Dom then it is re-rendered by `react-dom`.

**Note** while Re-Rendering, the whole thing is not re-rendered but only the changes are done to match-up.

One problem with the default settings is that even if a single state changes at a particular component,
then the whole sub-tree is re-evaluated, this is toally fine if the all state are tightly connected with
all the below component but unneccessary re-evaluation will lead to slower environment.

## Optimization Scenario

To Optimizie means is to basically stop unneccesary re-evaluation to the best extend.

The below techniques are more of a space-time trade off optimization, that is we store more information as to do less execution.

### React.memo

So, as already said all children and their children will be re-evaluatd if the parent state changes.

This will be optimized futher by storing the `previous props states`, and comparing if the props changes then re-evaluate otherwise just skip as the parent can only influence the child via the `props`.

**Note**: By Default `react` doesn't store the prop value as this may end up taking a lot of unneccessary space without any real encahncements, this way doing is only reccommanded if the component is very deep (means a lot of children).

To do this, just use `React.memo`,

- This will take a component as argument
- And returns a Component whose props will be memonized

```js
import SomeComponent from "/";

const MemoizedSomeComponent = React.memo(SomeComponent);
```

If you use the `Memoized` version of the component, then the Component will be Re-evaluated if the previous
`props` changes.

### useMemo and useCallback

As we know, Object and function are Reference types means the reference is stored rather than the value itself.

Sometime, passed down data's reference changes but the value remains the same. So, even if you use `React.memo` it will
not help to optimize as simply the comparisation is done without regards to value, as `reference type`'s reference always
changes on re-evaluation will be done anyways.

As the given below Hooks, make sure the `Reference types`'s reference changes only when the value changes. This is more of
a Compiler optimization but sometime it depends on the usecase.

#### useMemo

This is used to memoize data.

This works as,

- The First argument is a function which return the `data` to be stored.
- The Second argument is a array of dependencies.

```jsx
const someData = useMemo(
  () => {
    return data;
  },
  [
    /* Dependencies Goes Here*/
  ]
);
```

#### useCallBack

This is used to memoize a callback function.

Works similar to `useMemo`

- The First Argument is the callback function
- The Dependencies array, this include all the states used inside the function.

```jsx
const someData = useMemo(callBackFn, [
  /* Dependencies Goes Here*/
]);
```
