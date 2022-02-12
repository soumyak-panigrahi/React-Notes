# Components

In React JSX is used which is a hybrid of HTML and JS. 

React uses something called Component which are building block for a bigger application. Simply Components are
JS Object within which there may be pure JS , pure HTML or JSX code prsent but React will compile as per need.

There are two ways to make a component more on [HERE](https://reactjs.org/docs/components-and-props.html#function-and-class-components)

## Function Components

Here Function Component is examined, class components will be examined later.

First of all, each Component should be presented in it's own file and the filename should be the name of the component.

### Defining a Component

For Naming Convention, the component name should be presented in Pascal's Case, where there is no character between
words and each word first letter is capital. eg. i_am_going_home is written using undersore the same in pascal case
is IAmGoingHome.

Pascal's Case is used as the Components are used as HTML tag in JSX code. So, as to differentiate between normal
HTML tag and JSX rendering Component tags.

It always return a JSX code and always returned JSX code should only have one root element.

``````
function ThisIsAComponent { // Pascal Case NamingConvention
    // You can Use Normal JS Code 

    return ( // JSX Code
        <div> // Only one root element
            // Code Here
        </div>
    );
}

function ThisIsAnotherComponent {
    const someJSXCode = ( // You can also store any JSX code as Variable
        <div>
            //Code Here
        </div>
    );
    return someJSXCode;
}
``````

Simply put JSX code are converted into Js Objects, so that why they can be stored in variables;


### Using Components in JSX code

Once you defined a Component, you can use it like a HTML tag.

```````
function HeadingAndParagraph {
    return (
        <div>
            <h1>This is a Heading</h1>
            <p>This is a Paragraph</p>
        </div>
    );
}

// This Above Component can be used in other JSX as

function ManyHeadingAndParagraph {
    // Here we used the Component just like any other HTML Tags
    return (
        <HeadingAndParagraph></HeadingAndParagraph>
        <HeadingAndParagraph></HeadingAndParagraph>
        <HeadingAndParagraph></HeadingAndParagraph>
    );
}
```````

Once you define a Component it can be used just like a HTML tag.

### Using JavaScript Expression in JSX Code

As already defined JSX is mixture of HTML and Js Codes. React can easily identify Pure Js Code.

`````
function SomeComponent {
    // You can Use normal Js code anywhere except within JSX code

    const arr = [1,2,3,4,5]; // Totally fine

    const SomeJSXCode = ( // Will Give a ERROR (Not always error will be given, sometime it will be simply rendered as text)
        <div>
            <ComponentA></ComponentA>

            const obj = { // This is will not create a object with JSX Code 
                name : "Me",
                getname: () => this.name,
            }

            <ComponentB></ComponentB>
        </div>
    );

    return someJSXCode;
}
`````

Simply put you cannot use JS code within JSX code. But you can embed Js Expression, which will be evaluated and value is
substitued instead.

`Syntax: { expression } , will evaluted the expression and the value is substituted`

````
function SomeComponent() {

    const heading = "This is a heading";

    const someJSXCode = (
        <p>This is a Paragraph</p>
    );

    return (
        <div>
            <h1>{heading}</h1> // {heading} is replaced by This is a heading
            {someJSXCode} // this is replaced by <p>This is a Paragraph</p>
        </div>
    )
}
````

So, you can `{ }` to evalute any Js expression.

**Note** that object cannot be evaluted as expression. As we already pointed out you can store JSX code as variable because they 
are converted into object but they can evaluted as expression because in regards to JS, they are expression.

Also Note that `{ }` can evaluted some object like JSX code and Iterable objects but not all.

#### Commenting on JSX Code

As JSX code is neither JS or HTML, commenting is not available. But you can comment using
the above. So, where ever you want to comment write as you write in JS and they simply wrap it 
around `{ }`.

```
{/* This is also comment in JSX */}
```

## Modularizing Component

As pointed out each Component should be placed in it's own file and the filename the component name.

> Code in ModularComponent.js

````
function ModularComponent() {
    const someJSXCode = (
        {//Your code Here}
    );
    return someJSXCode;
}

export default ModularComponent;
````

Note more on [export](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export#syntax)

If you have more than one Component, then you have to export all of them to be used in other files.

To use it in another file

````
import ModularComponet from './filePath/ModularPath'
````
**Note** `./` will start the path from where the present file is located and `../` will start from the parent directory.

## Styling with CSS 

As we do all the work with JSX, so adding CSS is little different from that of a adding it to HTML.

It is often recommended that you should style each component separated with their own file attacheed.

First you have to import the css file.

``
Use: import "/filePath/fileName.css" , always add the extension
``

then you can do what you usually do as in a html file. 

For other CSS Framework, refer to their official docs. Doing as we did for normal Html file may not
work always.