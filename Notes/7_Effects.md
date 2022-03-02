# Effects

[Flow of Change of State](./4_States_and_event.md#flow-of-change-of-state), is discussed. So, read it before proceeding.

As, we know React is only concerened with the UI as of yet, this includes

- The DOM, which is done via Virtual DOM
- Managing States in a Components, which is done via a Hook `useState`
- Rendering and Rerendering the Component based on states and Events.
- Manipulating both DOM and the Virtual DOM also based on states and Events, which is done via Portals and Refs.

But in a Website, there are many things which are not directly concerned with the UI itself, ofcourse they somehow
influence them. But these processes or rather procedure are itself independent of UI, these are called Effect or
Side-Effects. Most common effects are Http Request, Validating Data, API calls, Caching , Cookies etc.

In other word, The procedure which takes outside of our UI are effects. Also, some effects many as well be of the UI
itself, so distinguishing them is immaterial and not always important.

In this section, we use [Web Storage APIs](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API#web_storage_concepts_and_usage), so it is better to know about it. A short note is given below about it.

#### Web Storage

As we in already see in daily life, whenever we open some website (especially commercial), they immediately ask for consent about
storing [cookies](https://developer.mozilla.org/en-US/docs/Glossary/Cookie), Web Storage API is the other way of storing data in your system when you visit a website.

You can see about the Storage in Appliciation Tab in Devloper tool.

There are two Web Storage API, `sessionStorage` and `localStorage` both defined in Window Objects. They both have
common opertion (given below) but usage is different

##### Operation for storing and retriving

- `setItem(key , value)` will set, a value with given key. If the key doesn't exist then new pair will be made.
- `getItem(key)` will get value with given key. If key doesn't exist `null` will be returned.
- `removeItem(key)` will remove the pair with the given key.
- `clear()` will remove all key-value pairs

`localStorage` will persist the value even after closing the window but in `sessionStorage` the data will persist
until the window is open (means reloading will not affect the storage).

## useEffect

The `useEffect` usage is as follows

```
useEffect(callBackFn , [...Dependencies])

Whenever any of Dependencies state changes, the callbackFn is execueted.

callBackFn can also return a function, called cleanUp function, which will be execueted before
callbackFn is called on state change.

Note: that cleanUp function is only called after intial State changes.
```

### useEfect in Component LifeCycle

As we have been seeing, for any component react does defines a cycle. That when it is rendered, rerenderd and when removed.

React defined Lifecycle

- Mount, when a component is rendered
- Updated, when a component is rerendered due to change in state.
- UnMount, when a component is removed. Like deleting a list or condition rendering.

So, how the flow of useEffect is execueted is important, it is always execueted only after the component (in which it is present)
is rendered and how useEffect is execueted based on above lifecycle

```
(Component Rendered or Mounted)
        |
        V
    (Intial State)
        |
        V                                                   (Parent Component Rerendered)
(useEffect callbackFn execueted)                                           |
        |                                                                  |
        |                                                                  V
        |                          --------------------------------------------------------
        |                          |                                                      |
        |                          |                                                      |
        V                          V                                                      |
(State Changed) -->  {   (Component Rerendered)          }                                |
    ^                {            |                      }                                |
    |                {            V                      }                                |
    |                {  [If Dependecnies State Changes]  }                                | Component Remove (or UnMounted)
    |                {            |                      } -------                        |
    |                {            V                      }       |                        |
    |                {     (useEffect's CleanUp)         }       |                        |
    |                {            |                      }       | State Changes          |
    |                {            V                      }       |                        |
    |                { (useEffect callBackFn execueted)  }       |                        v
    |                                                            |                     (useEffect's CleanUp)
    --------------------------------------------------------------

```

So, the Flow of only useEffect is

```
* callBackFn is the callabackFn defined in useEffect and CleanUp is the function returned by the callBackFn

  [Mounted]                   [Redendered]                   [UnMounted]

(callBackFn)    ===>    (CleanUp) --> (callBanckFn)  ===>     (CleanUp)
                            ^              |
                            |              |
                            ----------------
```

Note: Some different scenario based on dependencies

- If dependencies is supplied as `[]`
  - the `useEffect's callBackFn` is called once when the `component is mounted`.
  - the `useEffect's cleanUp function` is called once the `component is unmounted`
- If is dependencies is supplied as `undefined`
  - Similar to above, But the `useEffect` will be called each time it rerenders.
- For others, the above stated flow.

### Usage of useEffect

Let us consider the code,

```
import { useState , useEffect} from "react";

function SomeComp(props) {

    const {validate} = props;

    const [userName , setUserName] = useState() ,
          [password , setPassword] = useState()
          [accessPermission , setAccessPermission] = useState(false);

    /* This Below code will create a infinite loop

        if(validate(userName , password))
            setAccessPermission(true);
    */

    /* Here you use useEffect Hooks */

    useEffect(() => {
        if(validate(userName , password))
            setAccessPermission(true);
    } , [userName , password]);

    return (
        // JSX Code;
    )
}

```

In above we are using useEffect to avoid Infinite loop, the use case may vary but usage is as stated above.

When to use useEffects, a key rule is consider using it, if your app logic is somehow goes outside the realm of
just UI rendering.

- Validatig or Authecating Data
- Fetch API's Request
- Send `http Requests`
- Set some previously lost state by using some persist data
  - This may happen, suppose you login into a page and then reload, then all states will be lost. So, you can get these by using some persisting data via cookies or caching.
- Post Processing of some Component.

It can act as Post Processing Function as it is always execueted once the component is rendered or also by for separating
logic based on combination of different states. For example, in above eample we can validate the permission access in `event handler`
after we retrive the data, but this first of all keep the code `dry` as we have validate in each for `userName` and `password`
handler. And mostly Event Handler are only concerned for one state change (not always the case but recommanded), so that
collection of states can be handled by using `useEffect` as shown above.

### CleanUp function

As above noted it is called before the `useEffect callBackFn` after Intial state. It can be used in many ways, one
example is given below using the above example.

Suppose, we don't want to validate for each change in date rather only if the user stops typing between a time lag
(let say 1 second). read more about [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)

```
function SomeComp() {
    useEffect(() => {
        const ID = setTimeout(() => {
            if(validate(userName , password))
            setAccessPermission(true);
        } , 1000);
        return function cleanUp() {
            clearTimeout(ID);
        }
    }, [userName , password]);
}
```

So, the code work as, on each onChange event userName and Password is changed. So, each time useEffect will be called
now we are setting a time out of `1 sec`. Now, suppose the user changes the states within this `time lag`, then useEffect
will be triggered, but before the `callbackFn` is called, the `CleanUp` is triggered and it clear the timeout by using
`clearTimeout`, so validating is untriggered. but now when the `callbackfn` is called it will set a new time out. And the
above cycle continous, until the user gives a `time lag` of ` 1 sec`.
