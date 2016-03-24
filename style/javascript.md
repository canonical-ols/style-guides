## JavaScript Style Guide

---

* [Syntax](#syntax)
  * [Naming](#naming)
  * [Naming private methods and properties](#naming-private-methods-and-properties)
  * [Indentation](#indentation)
  * [Braces](#braces)

* [Libraries](#libraries)
  * [jQuery](#jquery)
  * [React](#react)

* [ES6/7 rules](#es67-rules)

* [Module pattern](#module-pattern)

This guide is adapted from the Khan Academy style guide.

----------
### Syntax

#### Naming

```js
ClassNamesLikeThis
methodNamesLikeThis
variableNamesLikeThis
parameterNamesLikeThis
propertyNamesLikeThis
SYMBOLIC_CONSTANTS_LIKE_THIS // TODO (stephen-stewart) refine for const
```

When naming variables and properties referring to jQuery element
objects, prefix the name with `$`:

```js
function doSomethingFancy(selector) {
  var $elements = $(selector);
  ...
}
```

#### Naming private methods and properties

Private methods and properties (in files, classes, and namespaces)
should be named with a leading underscore.

```js
function _PrivateClass() {
  // should not be instantiated outside of this file
}

function PublicClass(param) {
  this.publicMember = param;
  this._privateMember = new _PrivateClass();
}

var x = new _PrivateClass();  // OK - we’re in the same file.
var y = new PublicClass();    // OK
var z = y._privateMember;     // NOT OK!
```

#### File names

```
file-names-like-this.js
template-names-like-this.handlebars
```

#### Indentation

Use 2-space indenting for all code. Do not use tabs.

#### Braces

Braces should always be used on blocks.

`if/else/for/while/try` should always have braces and always go on
multiple lines, with the opening brace on the same line.

No:

```js
if (true)
  blah();
```

Yes:

```js
if (true) {
  blah();
}
```

`else/else if/catch` should go on the same line as the brace:

```js
if (blah) {
  baz();
} else {
  baz2();
}
```

#### `require()` lines.

Separate first party and third party `require()` lines, and sort
`require()` lines.

Imports should be sorted lexicographically (as per unix `sort`).


No:
```js
const _ = require("underscore");
const $ = require("jquery");
const APIActionResults = require("../shared-package/api-action-results.js");
const Cookies = require("../shared-package/cookies.js");
const cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
const HappySurvey = require("../missions-package/happy-survey.jsx");
const DashboardActions = require('./datastores/dashboard-actions.js');
const React = require("react");
const UserMission = require("../missions-package/user-mission.js");
```

Yes:
```js
const $ = require("jquery");
const React = require("react");
const _ = require("underscore");

const APIActionResults = require("../shared-package/api-action-results.js");
const Cookies = require("../shared-package/cookies.js");
const DashboardActions = require('./datastores/dashboard-actions.js');
const HappySurvey = require("../missions-package/happy-survey.jsx");
const UserMission = require("../missions-package/user-mission.js");
const cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
```

------------------------------
### Comments and Documentation

#### Inline comments

Inline comments should be the `//` kind not `/* */` kind.



----------
### Libraries

#### jQuery

jQuery should be used for incidental javascript that can not be represented by a react component.

#### React

React should be used for non-trivial UI.

See [React Style Guide](react.md) for further details.


---------------
### ES6/7 rules

We use Babel to compile our code from ES2015, so using new features from ES6/7 is encouraged.

Especially:

| Construct | Use...                                | ...instead of |
| --------- | ------------------------------------- | ---------------------- |
| backticks | `` `http://${host}/${path}` `` | `"http://" + host + "/" + path` |
| destructuring | `var { x, y } = a;` | `var x = a.x; var y = a.y;` |
| fat arrow | `foo(() => { ... })` | `foo(function() { ... }.bind(this))` |
| let/const | `let a = 1; const b = "4EVAH"; a++;` | `var a = 1; var b = "4EVAH"; a++;` |
| includes | `array.includes(item)` | `array.indexOf(item) !== -1` |
| for/of | `for (const [key, value] of Object.entries(obj)) { ... }` | `_.each(obj, function(value, key) { ... })` |
| spread | `{ ...a, ...b, c: d }` | `_.extend({}, a, b, { c: d })` |
| rest params | `function(bar, ...args) { foo(...args); }` | `function(bar) { var args = Array.prototype.slice.call(arguments, 1); foo.apply(null, args); }` |

#### Use `=>` instead of `bind(this)`

Arrow functions are easier to read (and with Babel, more efficient)
than calling `bind` manually.

#### Use rest params instead of `arguments`

The magic `arguments` variable has some odd quirks. It's simpler to
use rest params like `(...args) => foo(args)`.

#### Use backticks for string interpolation

`+` is not forbidden, but backticks are encouraged!

#### Use `let` and `const` for new files; do not use `var`

`let` is superior to `var`, so prefer it for new code.

#### Avoid using underscore/lodash if not necessary

We use ES6/7 which includes many of the features of Underscore.js (or Lodash.js)! Using Underscore should be avoided in favor of these native language features.

What follows is a method-by-method set of equivalents for what Underscore provides and what you could be using in ES6/7 instead:

Method | Use...                                | ...instead of
--------- | ------------------------------------- | ----------------------
bind | `fn.bind(someObj, args)` | `_.bind(fn, someObj, args)`
bind | `(a, b) => { ... }` | `_.bind(function(a, b) { ... }, this)`
bindAll | `obj.method = obj.method.bind(someObj);` | `_.bindAll(someObj, "method")`
defer | `setTimeout(fn, 0);` | `_.defer(fn);`
delay | `setTimeout(fn, 2000);` | `_.delay(fn, 2000);`
each (array) | `array.forEach((val, i) => {})` | `_.each(array, (val, i) => {})`
each (array) | `for (const val of array) {}` | `_.each(array, fn)`
each (object) | `for (const [key, val] of Object.entries(obj)) {}` | `_.each(obj, fn)`
extend (new) | `{...options, prop: 1}` | `_.extend({}, options, {prop: 1})`
extend (assign) | `Object.assign(json, this.model.toJSON())` | `_.extend(json, this.model.toJSON())`
filter | `array.filter(checkFn)` | `_.filter(array, checkFn)`
has (array) | `array.includes(value)` | `_.has(array, value)`
has (object) | `obj.hasOwnProperty(value)` | `_.has(obj, value)`
isArray | `Array.isArray(someObj)` | `_.isArray(someObj)`
isFunction | `typeof fn === "function"` | `_.isFunction(fn)`
isString | `typeof obj === "string"` | `_.isString(obj)`
keys | `Object.keys(obj)` | `_.keys(obj)`
last | `someArray[someArray.length - 1]` | `_.last(someArray)`
map | `array.map(mapFn)` | `_.map(array, mapFn)`
max | `Math.max(...array)` | `_.max(array)`
object | <pre>Object.entries(obj).reduce(<br>(result, [key, val]) => {<br>&nbsp;&nbsp;&nbsp;&nbsp;result[key] = value;<br>&nbsp;&nbsp;&nbsp;&nbsp;return result;<br>})</pre> | <pre>\_.object(\_.map(obj, (val, key) => {<br>&nbsp;&nbsp;&nbsp;&nbsp;return [key, value];<br>})</pre>
omit (array) | `array.filter(prop => !props.includes(prop))` | `_.omit(array, props)`
omit (object) | <pre>Object.keys(obj).reduce((result, prop) => {<br>&nbsp;&nbsp;&nbsp;&nbsp;if (!props.includes(prop)) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result[prop] = attrs[prop];<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>}, {})</pre> | `_.omit(obj, props)`
once | `$(...).one("click", ...)` | `$(...).on("click", _.once(...))`
once | <pre>{<br>&nbsp;&nbsp;&nbsp;&nbsp;method: () => {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if (this._initDone) { return; }<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this._initDone = true;<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>}</pre>| `{ method: _.once(() => { ... }) }`</pre>
once | <pre>var getResult = () => {<br>&nbsp;&nbsp;&nbsp;&nbsp;let val = $.when(...).then(...);<br>&nbsp;&nbsp;&nbsp;&nbsp;getResult = () => val;<br>&nbsp;&nbsp;&nbsp;&nbsp;return val;<br>};</pre> | <pre>var getResult = _.once(() => {<br>&nbsp;&nbsp;&nbsp;&nbsp;return $.when(...).then(...);<br>});</pre>
sortBy | `result = result.sort((a, b) => a.prop - b.prop)` | `_.sortBy(result, "prop")`
values | `Object.values(obj)` | `_.values(obj)`

--------------------
### Module pattern

We use Browserify to package our code, so even small scripts should be node-style modules.

#### Export constructor function

Modules should export a single constructor function that can be then required
and instantiated in a script to run it or in tests.


For example for `MyModule.js` as follows:
```js
const MyModule = function(){};
module.exports = MyModule;
```

usage would be:

```js
const MyModule = require('./MyModule');
new MyModule();
```


#### Bundles

Example bundle file structure:

```
my-module-bundle
├── entry.js
├── MyModule.js
└── tests
    └── t_my_module.js
```

`MyModule.js`:

```js
const MyModule = function(){};
module.exports = MyModule;
```

If a module can be instantiated without any parameters `entry.js` can look like:

```js
const MyModule = require('./MyModule');
new MyModule();
```

and then in a template it would just need to be included in `<script>` tag:
```html
<script src="{% static_url 'dist/js/my-module-bundle.js' %}"></script>
```

In case code requires some parameters to run `entry.js` should export module instance
that needs to be required and called in a script block in a template:

```js
const MyModule = require('./MyModule');
module.exports = new MyModule();
```

```html
<script src="{% static_url 'dist/js/my-module-bundle.js' %}"></script>
<script>
  const myModule = require('my-module-bundle');
  myModule.init({
    value: "{% some data from the template %}"
  });
</script>
```
