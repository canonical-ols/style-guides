## React

### Syntax

We use [JSX syntax](https://facebook.github.io/react/docs/jsx-in-depth.html) in React components files.

#### Only export a single react class.

Every JSX file should export a single React class, and nothing else.

Note that the file can still define multiple classes, it just can't export
more than one.

#### React classes should be named

Use a named const for React to properly store name
of the class -- this helps when debugging components.

Yes:

```js
const MyComponent = React.createClass({
  render: function(){ /* ... */ }
});

module.exports = MyComponent;
```

No (React "guesses" the name of the class as `exports`):

```js
module.exports = React.createClass({
  render: function(){ /* ... */ }
});
```

#### Props validation

Always validate props of components using [`propTypes` property](https://facebook.github.io/react/docs/reusable-components.html#prop-validation).

Yes:
```js
const MyComponent = React.createClass({
  propTypes: {
    children: React.PropTypes.element.isRequired,
    className: React.PropTypes.string,
    onClick: React.PropTypes.function
  },
  
  render: function() {
    return <div className={this.prop.className} onClick={this.props.onClick}>
      {this.props.children}
    </div>
  }
});
```

No:

```js
const MyComponent = React.createClass({
  render: function() {
    return <div className={this.prop.className} onClick={this.props.onClick}>
      {this.props.children}
    </div>
  }
});
```

### Using other libraries with React

#### Minimize use of jQuery.

Never use jQuery for DOM manipulation. React should handle all DOM rendering.

Try to avoid using jQuery plugins. When necessary, wrap the jQuery plugin with a React component so you only have to touch the jQuery once.
