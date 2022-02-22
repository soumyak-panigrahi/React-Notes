# States and Event

## State

First things to note, once the JSX code is compiled. It will assume that the excueted code is the both
intial and final state. In other words, the code it self will be static by all means.

So, if you want to change the State, which are just possible values or structure of your data. Now you
have to tell React which value or rather data can have different states.

```
For Example:-

Let us say, we have a button. Each time we click it will toggle between light and dark theme.

First, we have to define the intial State, let it be light theme.

(light theme) ----(Button Click)---> (dark theme)
(light theme) <----(Button Click)----(dark theme)

So, when that button click event is tiggered. We have to change between 2 states.
```

Note:- There may be any number of states for each event and they may be sharing value. So, note data has state not event.
So, react gives you privilege of accessing only the previous state irrespective of the event.

### useState

It is a react Hook. which is defined in 'react' library. This will be used to define state for a given data.

First for a given data you have to give it's intial state, And then it will return a array with two value.
The first value is the variable with state value and second is a function which can be used to can change state
of the value.

```
const [data , setData] = useState(intialValue);

You can use data for referring to the value.

setData, is used to change data.
```

#### How State is Changed

So, once a state for a data is defined. React will taken account of in which components does the given state
affects. And only those Component will be re-rendered. In other words, if your using a state in a Component.
Then the whole component will be rendered.

```
function SomeComponent() {

    const [time , setTime] = useState(0);

    setIntervel(() => {
        setTime((prev) => {
            return prev + 1;
        });
    } , 1000);

    return (
        <div>
            <h1>{time}</h1>
        </div>
    );
}
```

After every second `time` is increased by 1 (as you can see in `setInterval`'s callback function), now
we are using the data as header in the JSX code. So, every time the callback function is execueted.
`SomeComponent` will be rendered with set value in `setTime` will be assigned to time, and note that
the given value is stored; not the intial State value. So, time is not stored as global varible , it is
just reassigned each time state is change.

Now, the setter function is not execueted synchrously. Rather it is set asyncrously, so that has to noted
for unusual behaviour.

#### setData

It is refer to 2nd value given by useState react Hook. As we noted this function is execueted asynchrously.

```
Usage: setData(data) or setData(fn)

fn is a callback functiona and it returns the value to be set.
```

So, which one should be used. One benefit of the argument as a callback function is that it will be take
account of asynchrous behaviour. This is done, by default the first parameter is supplied with the previous
state value. So, even though it is executed asynchrously it will always receive the previous state value only.

```
const [data , setData] = useState(intialValue);

/*Can be used if your not using the previous state*/
setData(nextStateValue);

/*This may not always work as it is async data value may not always be previous state value */
setData( expression with data );

/*Most preferred*/
setData((previousState) => {
    // use previousState rather than data
    return nextStateValue;
});
```

In other words, in `setData` always supply it with a callback function whose first argument is `previous state value`,
use it rather than the stored varible for `data` as it may not always has the previous state value, then return
`next state value` from the callback function.

## Events

More on Different Events in [JS](https://developer.mozilla.org/en-US/docs/Web/API/Event) and Synthetic Event in [React](https://reactjs.org/docs/events.html).

In React, we use `Synthtic Event`, all these attributes starts from `on` and these are handled by a callback fn with the
data suppiled by the `one who triggers the event`(like forms , button etc) and the data handled by the callback fn.

```
function SomeComponent(props) {

    function clickHandler(e) { // We Supply the Handler
        console.log(e.target);
    }

    return (
        {/* The button triggers the handler*/}
        <button onClick={clickHandler} >Click Me</button>
    );
}
```

So, the given pattern is `Important`, as shown above if we want to handle data supplied by `some other componenet` (even custom).
We supply the `handler` via `Synthetic event attribute` as a callback function, and the `Some other component` just call the
handler with the data as agrument.

### Child-to-Parent Communication

As, we have already seen `Parent Component` can easily communicate with their `child` via `props` (Property Object).

But the other way around is done by the above stated pattern via Callback Function. In other words, think of your
`Custom Child Component as a Form` because if your `Custom Child Component` is suppling data to the given (or parent)
component then it just acts like a form.

So, to Handle this situation we also use a similar pattern that of a `form`. But in `form` the `Handlers` are handled
automatically by themselves, we just have to provide the handler. In our case, we have to do both `supply the handler from parent`
and also `manage the handler in child`.

```
function SomeFormLikeComponent(props) {

    function myChangeHandler(e) {
        // Use Event Object e to form a supply object (or any other data) obj

        props.onChange(e);
    }

    return (
        <form>
            <input type="text" onChange={myChangeHandler}>
        </form>
    );
}

function SomeComponent() {

    function changeHandler(e) { // e is the data supplied as callaback function
        // Use e as you need.
    }

    return (
        <div>
            <SomeFormLikeComponent onChange={changeHandler} />
        </div>
    );
}
```

So, the pattern is used as required.

Now, suppose you like you make Component which both handles the data and supplies the data, this is called two-way binding.
This pattern is common if you want to have a consistent view over your data, such component are called Controlled Component.
This can aain be done by using props with `suppiled data` and child component make sure to use the `supplied data only`.

One use of having Controlled Component is having more customizing power from it and gain to do these the child component
should permit it. So, it you want to make the Component `Controlled` make sure that the `Children Custom Component`
handle the supplied data via props and make sure to `handle errors too`.

So, To make sure that given Component is customizable. Make sure to add to below thing

- It is take different data for different states (like className , id etc.)
- It is Handle most common events related to that componenet (like onChange , onClick etc.)
- It supports Controlled Component.
