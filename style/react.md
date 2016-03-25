## React

### Syntax

We use [JSX syntax](https://facebook.github.io/react/docs/jsx-in-depth.html) in React components files.

#### Only export a single react class.

Every JSX file should export a single React class, and nothing else.

Note that the file can still define multiple classes, it just can't export
more than one.

#### React classes should be named

Use a named variable or `displayName` property for React to properly store name
of the class -- this helps when debugging components.

Preferred syntax:

```js
const MyComponent = React.createClass({
  render: function(){ /* ... */ }
});

module.exports = MyComponent;
```

Also OK:

```js
module.exports = React.createClass({
  displayName: 'MyComponent',

  render: function(){ /* ... */ }
});
```

No (React "guesses" the name of the class as `exports`):

```js
module.exports = React.createClass({
  render: function(){ /* ... */ }
});
```

### Using other libraries with React

#### Minimize use of jQuery.

Never use jQuery for DOM manipulation. React should handle all DOM rendering.

Try to avoid using jQuery plugins. When necessary, wrap the jQuery plugin with a React component so you only have to touch the jQuery once.
