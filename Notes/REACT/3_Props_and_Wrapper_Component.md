# Properties Object and Wrapper Component

In this, evaluting Js Expression is used so please refer [HERE](./2_Component.md#using-javascript-expression-in-jsx-code)

## Properties Object (or Props)

As we know in any html tags, it has two thing, first it's attributes and the content wrapped within it.

Let us first deal with the attributes, this is the way to provide Components with both
data and metadata.

```
We want 3 data to be filled in the JSX code

function SomeComponent() {
    return (
        <div>
            <h1>{name}</h1>
            <p>{About}</p>
            <footer>{Contact}</footer>
        </div>
    );
}
```

If it was a normal function will be simply provide these via arguments. But as Component are Html Tags in
JSX code it is not possible to provide these data via arguments but as they are html tags so it will make
more sense to set the data as attribute. But how would the given component will use these data.

Here we use the Properties Object or Props. It is similar to event object in event handling which provides
both data and metadata. And they are provided as a callback function with first parameter with the properties
object (just like that of callback function in event handling).

In this Properties Object, the attribute name is the key and the value given is the value for that key. So, either
we destructure the object or directly call using . operator.

```
function SomeComponent(props) {
    const { Name , About , Contact} = props;
    return (
        <div>
            <h1>{Name}</h1>
            <p>{About}</p>
            <footer>{Contact}</footer>
        </div>
    );
}

function AnotherComponent() {
    return (
        <SomeComponent Name="Mr. Somebody" About="Unemployed" Contact=""></SomeComponent>
    );
}
```

On a side note, you can also send JSX Code via props as JSX code can be stored in a variable and then simply evalute using
{ }.

### Adding classes to a Component

As we do it for a normal HTML tag

```
<div class="box"></div>
```

In JSX code it can also be done by using `className` attribute for any normal HTML tags

```
const JSXCode = (
    <div className="box">
        //Code Here
    </div>
)
```

## Wrapper Components

Basically Wrapper Component is Composition of Components. Composition is basically a 'has-a' relationship.

As for normal HTML tags we can wrap content with the tags. eg.

```
<h1>Some Wrapped text</h1>
```

So, we can also wrap Some JSX code within our Components. And these JSX code can be refer in a Properties Object
memeber called `children`.

Note: children will return a array of sibling JSX codes.

```
function WrapperComponent(props) {
    const {children} = props; // children member
    return (
        <div>
            <h1>This is a Wrapper Component</h1>
            {children} {//JSX Code in Substibuted}
        </div>
    )
}
```

Note:

- None of attribute will be set directly, all the attributes has to be set manually. (!Important)
- If you add `className` for the wrapper component, you have to set the `className` of JSX code manually. None of the atrribute will be inherited.
- If your Component (let say X) is not wrapping any content (Composition) then write it as `<X attributes="..." />`, attributes are used as normal. But the closing tag are ommited and instead replaced by a `/` as shown above. This is done for Code enhancement.

## JSX

Till now we have seen how to write JSX, now time to deal with how react make them.

After basic preprocessing, JSX codes are basically objects. Now, React provides us basic operation
to make JSX code.

### React.createElement

This method can be used create a preprocessed JSX object.

```
const root = Component ?? "Html Tag Name as String";

const attribute = {
    key : value,
    ...
};

const arr = [JSX1 , JSX2];

const JSXObject = React.createElement.(root, attributes , ...arr);
```

For example.

```
const someJSXCode = (
    <div className="box">
        <h1>This is a Heading</h1>
        <SomeComponent />
    </div>
);

//The above is equivalent To

const root = "div";
const attributes = {
    className : "box",
};

const child1 = React.createElement("h1", {} , "This ia a Heading">),
    child2 = React.createElement(SomeComponent , {});

const arr = [child1 , child2];

const someJSXCode = React.createElement(root , attributes, ...arr);
```

It is the other way around, if you have deatils for the JSX Object. Then you can make it by `Reat.createElement` method.
