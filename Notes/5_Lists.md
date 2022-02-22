# List

So, in JSX Code, react doesn't give us the benifit of using of Conditional Statement and looping.

But the above can be done by another way, precisely by means of `{ Js Expressions }`

More on [Using Js Expression in JSX Code](./2_Component.md#using-javascript-expression-in-jsx-code)

## Rendering a List

### Iterable Object

Iterable Object are Objects where the value are stored in a particular (or Some well Defined) order.
So if they are in a particular order, then it can be looped by means of iterator, which will tell the next
object from the present object.

Some of Iterable Objects are Array , Set , Map.

So, simply for any Iterable Object, you can defined it iterator. Whose job is to give the next element from
the present element.

Note: The Behaviour may not always as of `for..of` loop

More on [Iteration Potocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol)

### Using { } to Evaluate a List

As we have already seen `{ }` (Curly Braces) , can be used to dynamically render (or evaluate) the JS Expression.

Not only JS Expression, but it can also take a iterable Object (Most Commonly Array). When you pass a Array as
an argument then each member will be evaluted in the order i.e. 0 , 1 , .... , N.

```
function SomeComponet(props) {

    const { data } = props; // We don't know the length of data (so not hard coding)

    return (
        <div>
            { data } // Simply substitites the data one by one and printed.
        </div>
    )

}

```

As we have already know JSX Code can also be stored as Variable and used as Expression.

But we don't always directly evaluate List of JSX Code, but we use a List of Data to set Attributes
of a Component and then evaluate.

[map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map#syntax) method in a array is very helpful in regards to evaluate a list.

Suppose we have to wrap each member of data by a `h1`. It can be done by

```
return (
    <div>
        {data.map((datum) => <h1>{datum}</h1>)}
    </div>
);

```

### List Key

To differenciate between different member of a List, we should always set the `key` attribute. This
helps react to check it that element of the List is Changed , Added or Removed.

First things to note is different element in the same List should have different key, use some library
like [uuid](https://www.npmjs.com/package/uuid) to set the key.

And different List can have value can Repeated.
