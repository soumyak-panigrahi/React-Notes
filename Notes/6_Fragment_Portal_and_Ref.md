# Fragment , Portal and References

## Fragment

As we know, for any Component there can only be one `root` element (the tag will encloses all the children composition),
So, the most generic solution is to enclose all JSX tags with a `div` (or any container). And this is to be done for
most case, but unnecessary addition of `div`s may lead to a collection of nesting with no result. So, to avoid accumulation
of `div`, we a container which just passes the childen JSX tags rather than encapsulate them. more on [official docs](https://reactjs.org/docs/fragments.html)

### Using List of Sibling Elements

So, first thing to note is we also use list to pass sibling JSX Code, this is then is rendered as a [List](./5_Lists.md#list)

```
function SomeComponent(props) {
    return [<div>Child-1</div> , <div>Child-2</div>];
}

function SomeOtherComponent(props) {
    return (
        <div>
            <h1>This is a Heading</h1>
            <SomeComponent /> {/* This will render the supplied list as did using { ... } */}
        </div>
    )
}
```

So, we have already noted you have to supply key for each child for the list to be rendered efficiently.

### Using chlidren props

When every to enclose, any JSX code between any Component, those are automatically passes down by using the
`key value: children`, more [HERE](./3_Props_and_Wrapper_Component.md#wrapper-components)

So, simply put it just a React created List of sibling JSX code, like we did above but here React take
care of every thing about the list.

so, the other way around would be to pass down the react made `list` rather than a custom one like we did
above.

```
function Wrapper(props) {
  return props.children;
}

function SomeComponent() {
  return (
    <Wrapper>
      <h1>This is a Heading</h1>
      <p>Some Paragraph</p>
    </Wrapper>
  );
}
```

The above code will simply return the list of sibling JSX Elements, like we did above.

### Using Fragment Component

So, using just the above logic, react provide us with a bulit-in Component name `Fragment` declared in `react` library.
So, it also just pass down the sibling element but without going through intermediate steps like that of making a
Wrapper Component and then just returning the children.

```
import { Fragment } from "react";

function SomeComponent(props) {
  return (
    <Fragment>
      <h1>This is a Heading</h1>
      <p>A Paragraph</p>
    </Fragment>
  );
}
```

If you redering a Fragment in a List you can also pass a `key` prop.

## Portal

So, this is simply a feature to render the jSX code outside the Virtual DOM Tree, i.e. rendering directly into
the DOM.

```
import { Fragment } from "react";
import ReactDOM from "react-dom";

function SomeComponet(props) {

    const dom = document.getElementById("container");

    return (
        <Fragment>
            {ReactDOM.createPortal(<OtherElement /> , dom)}
        </Fragment>
    );
}
```

So, you provide the jSX code you want to render as first argument and then the DOM Container Object, in which you like
it to be rendered in.

[Use case are found here](https://reactjs.org/docs/portals.html#usage)

## References or Ref

So, React uses something called Virtual DOM, which are not technically DOM but will be converted into DOM by using
the jSX code we have been using until now. So, react recommand to not mix `Virtual DOM` and `DOM` but something
for convenient sake, we like to drectly manipulate the DOM rather than going through the `Virtual DOM` to exert it.

So, react provide us with a Hook called `useRef`, which will give a `ref` object which stores the `DOM` of the element
it points to.

Usage:

```
import { useRef } from "react"

function SomeComponet(props) {

    const buttonRef = useRef();

    function ForUsingTheRef(...arg) {
        // You can now manipulate the DOM of pointed reference directly. By using the 'current'
        // property which store the DOM of element
        buttonRef.current.property = value;
    }

    return (
            .
            .
            .
        <button {...attr} ref={buttonref}> Some Button </button>

    );
}

```

After making the reference or ref, set the ref attribute of the HTML Tag to which you the reference to be stored. And use
`current` property to access the DOM of it.

[Use Case Found Here](https://reactjs.org/docs/refs-and-the-dom.html#when-to-use-refs)

## Forwarding ref
