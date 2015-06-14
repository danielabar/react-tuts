## What is React?

A view library, not a full framework, just the "V" in MVC.

### Virtual DOM

React contains a JavaScript implementation of the DOM called _Virtual DOM_.
Completely separate from the browser's implementation.

Every time state is changed, React creates a virtual dom tree.
It diff's from the current one and re-renders only the differences to the real DOM.

### Components

React represents views as _Components_ rather than _Templates_.

React components are written in JavaScript, for example:

```javascript
var MyComponent = React.createClass({
  render: function() {
    return React.DOM.div(null, 'Hello!');
  }
});
```

React provides a set of functions that represent HTML nodes.
These nodes can be used to build custom components.

### JSX

One drawback is the loss of visual representation of view
(unlike templates which look more similar to html).
This is why React also uses _JSX_, the syntax React uses to represent the view.

## Hello World Component

[HTML](lesson01/index.html)

### Properly transform JSX

To properly transform JSX in script tag, first line must be pragma

```
/** @jsx React.DOM */
```

###  Define component with `React.createClass`

`createClass` defines the component, but does not render it.
It accepts an object literal, most properties are optional, except `render` method.

### Use the `render` method

Render method describes component's eventual structure in the DOM, it's written in JSX,
and needs to return a node.

Behind the scenes, JSX converts the node returned from createClass into a function.
Can only return one root node from the render function. To return more than one node, wrap it in a div.

### Render component with `React.renderComponent`

`renderComponent` takes in a component as first parameter.
Since it uses JSX, can write the tag rather than the variable.

Second parameter is an HTML node that will act as container.

Last parameter is an optional callback function, fired when component is finished rendering,
but not commonly used.
