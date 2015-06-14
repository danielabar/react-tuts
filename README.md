## Steps to creating a component

[HTML](lesson01/index.html)

* Properly transform JSX
* Define component with `React.createClass`
* Use the `render` method
* Render component with `React.renderComponent`

To properly transform JSX in script tag, first line must be pragma

```
/** @jsx React.DOM */
```

`createClass` defines the component, but does not render it.
It accepts an object literal, most properties are optional, except `render` method.

Render method describes component's eventual structure in the DOM, it's written in JSX,
and needs to return a node.

Behind the scenes, JSX converts the node returned from createClass into a function.
Can only return one root node from the render function. To return more than one node, wrap it in a div.

`renderComponent` takes in a component as first parameter.
Since it uses JSX, can write the tag rather than the variable.

Second parameter is an HTML node that will act as container.

Last parameter is an optional callback function, fired when component is finished rendering,
but not commonly used.
