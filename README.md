<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Getting Started with React.js](#getting-started-with-reactjs)
  - [What is React?](#what-is-react)
    - [Virtual DOM](#virtual-dom)
    - [Components](#components)
    - [JSX](#jsx)
  - [Hello World Component](#hello-world-component)
    - [Properly transform JSX](#properly-transform-jsx)
    - [Define component with `React.createClass`](#define-component-with-reactcreateclass)
    - [Use the `render` method](#use-the-render-method)
    - [Render component with `React.renderComponent`](#render-component-with-reactrendercomponent)
  - [JSX vs. React DOM](#jsx-vs-react-dom)
  - [Managing State](#managing-state)
  - [{this.state.titleMessage}](#thisstatetitlemessage)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Getting Started with React.js

> Learning React.js with Tuts Plus [course](http://code.tutsplus.com/courses/getting-started-with-reactjs)

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

[Example](lesson01/index.html)

### Properly transform JSX

To properly transform JSX in script tag, first line must be pragma

```
/** @jsx React.DOM */
```

JSX is an independent technology that was made with React in mind.
The pragma links JSX and React together. It tells JSX to use the React.DOM namespace.

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

## JSX vs. React DOM

[Example](lesson02/index.html)

JSX allows for writing HTML like nodes in JavaScript (because React has no templating language).

An alternative to JSX is to use the React.DOM namespace.

JSX is syntactic sugar on top of React.DOM.

## Managing State

[Example](lesson03/index.html)

When state is changed, it causes React components to re-render. State is usually data that changes over time.
For example, to toggle show/hide of a message box, the value of the toggle would be part of state.

State can be set in the class using `getInitialState` function, which returns an Object literal. State values can be used in the render function, using curly brace notation.

```javascript
var MessageBox = React.createClass({
  getInitialState: function() {
    return {
      isVisible: true,
      titleMessage: 'Hello, World'
    }
  },
  render: function() {
    var inlineStyles = {
      display: this.state.isVisible ? 'block' : 'none'
    };

    return (
      <div className="container jumbotron" style={inlineStyles}>
        <h2>{this.state.titleMessage}</h2>
      </div>
    );
  }
});

var reactComponent = React.renderComponent(
  <MessageBox />,
  document.getElementById('app')
);
```

State can be modified by calling `setState` function of reactComponent, and DOM will automatically be updated. For example, to make the message box appear hidden:

```javascript
reactComponent.setState({
  isVisible: false
});
```

_Reconciliation_ is the process by which React updates the DOM with each new render pass.
When a new state change happens, React makes a new Virtual DOM tree, diffs it from the previous one,
then only applies those diff's to the DOM.
