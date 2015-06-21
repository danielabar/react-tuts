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
  - [Props](#props)
    - [Simple example](#simple-example)
    - [Default Values](#default-values)
    - [Validation](#validation)
    - [Composition](#composition)
  - [Synthetic Events](#synthetic-events)

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

## Props

[Example](lesson04/index.html)

Props are used to make components composable. When state is modified, not only is the component re-rendered,
but also all its child elements are re-rendered. State data is pushed down from top level component to child components.

State is pushed from parent to child components via _props_. They're passed as attributes in JSX.
Props are immutable. To update props, state must change. And state is only stored once at top level component.

This makes components like state machines, given a new state, they re-render with their properties.

Props can have default values. Can be validated by stating whether they're required and what type they should be.

An _owner_ is a component that sets the props of other components.

### Simple example

Just before rendering component, declare a variable and pass it in as an attribute:

```javascript
var message = 'Yo!';

var reactComponent = React.renderComponent(
  <MessageBox titleMessage={message} />
  document.getElementById('app');
);
```

Then it can be used in the component render using an expression:

```javascript
var MessageBox = React.createClass({
    // init...

    render: function() {
      return (
        <div>
          <h2>{this.props.titleMessage}</h2>
        </div>
      );
    }
});
```

In the console, can access the props associated with the component:

```javascript
reactComponent.props
// Object {titleMessage: "Yo!"}
```

### Default Values

Default values can be set for props using `getDefaultProps` function:

```javascript
var SubMessage = React.createClass({
  getDefaultProps: function() {
    return {
      message: 'Its good to see you'
    }
  },

  render: function() {
    return (
      <div>{this.props.message}</div>
    );
  }
});
```

Prop values of the child component can be set when used in the parent,
i.e. the parent component is an _owner_ of the child component.

```javascript
var MessageBox = React.createClass({

  render: function() {

    var subMessage = 'Its not good to see you';

    return (
      <div>
        <h2>Hello World</h2>

        <SubMessage message={subMessage}/>
      </div>
    );
  }
});
```

### Validation

`propTypes` property can be used to validate prop types and whether they're required.
For example, to declare that `message` must be a string:

```javascript
var SubMessage = React.createClass({
  propTypes: {
    message: React.PropTypes.string
  },
  getDefaultProps: function() {
    return {
      message: 'Its good to see you'
    }
  },
  render: function() {
    return (
      <div>{this.props.message}</div>
    );
  }
});

```

To declare that it must be a string AND required `React.PropTypes.string.isRequired`

### Composition

Declare an array of messages as part of the parent components initial state.
Then in the `render` function, create an array of child components,
each having its `message` prop set to an entry from the messages array.
Then include this array of child components as part of the parents render method.

```javascript
var MessageBox = React.createClass({

  getInitialState: function() {
    return {
      isVisible: true,
      messages: [
        'I like the world',
        'Coffee flavored ice cream is highly underrated',
        'My spoon is too big',
        'Tuesday is coming. Did you bring yoru coat?',
        'I am a banana'
      ]
    }
  },

  render: function() {

    var messages = this.state.messages.map(function(message) {
      return <SubMessage message={message} />
    });

    return (
      <div>
        <h2>Hello World</h2>

        {messages}
      </div>
    );
  }
});
```

The component can be manipulated in the console by adding a new item

```javascript
var newMessagesArray = reactComponent.state.messages.concat('New Item');
reactComponent.setState({
  messages: newMessagesArray
});
```

## Synthetic Events

[Example](lesson05/index.html)

React doesn't use native DOM events, instead it uses _synthetic events_,
which are consistent with the W3C Standard.

For example, add a button that when clicked, will add a new message to the list.
Add camel case `onClick` expression to the JSX. Note this is not the same onClick event from DOM,
this is specifically used by React.

Camel case is the standard for all React event types (keyUp, mouseUp etc.).

Note `<button className=...` rather than `<button class=...`
(because it's JSX and cannot use reserved keyword `class`)

```javascript
var MessageBox = React.createClass({

  handleAdd: function(e) {
    // console.log(e);
    console.log(e.target);
  },

  getInitialState: function() {
    return {
      messages: [
        'I like the world',
        'Coffee flavored ice cream is highly underrated',
        'My spoon is too big',
        'Tuesday is coming. Did you bring yoru coat?',
        'I am a banana'
      ]
    }
  },

  render: function() {

    var messages = this.state.messages.map(function(message) {
      return <SubMessage message={message} />
    });

    return (
      <div className="container">
        <h2>Hello World</h2>
        <button className="btn btn-primary" type="button" name="button" onClick={this.handleAdd}>Add</button>
        {messages}
      </div>
    );
  }
});
```

The click handler receives an event of type _SyntheticXXXEvent_, for example `SyntheticMouseEvent`,
not a native DOM event.

React auto-binds `this` context to the component in event handlers.

React uses _event delegation_. Even though the onClick event is declared for example,
on an individual DOM element such as a button, doesn't mean the event is _attached_ to that element.

React stores all event handlers at a top level node.
At this node, React creates mappings for this handler and knows how to dispatch the event.
This is more efficient than creating multiple event handlers on each individual dom element that requires it.
