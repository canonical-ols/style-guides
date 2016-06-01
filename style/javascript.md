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
* [Django](#django)

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

All file names should be written in `snake_case` (lower case, separated with underscore).

Yes:

```
my_module_bundle
├── entry.js
├── my_module.js
└── tests
    └── test_my_module.js
```


No:
```
my-module-bundle
├── entry.js
├── MyModule.js
└── tests
    └── testmymodule.js
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
const APIActionResults = require("../shared_package/api_action_results.js");
const Cookies = require("../shared_package/cookies.js");
const cookieStoreRenderer = require("../shared_package/cookie_store.handlebars");
const HappySurvey = require("../missions_package/happy_survey.jsx");
const DashboardActions = require('./datastores/dashboard_actions.js');
const React = require("react");
const UserMission = require("../missions_package/user_mission.js");
```

Yes:
```js
const $ = require("jquery");
const React = require("react");
const _ = require("underscore");

const APIActionResults = require("../shared_package/api_action_results.js");
const Cookies = require("../shared_package/cookies.js");
const DashboardActions = require('./datastores/dashboard_actions.js');
const HappySurvey = require("../missions_package/happy_survey.jsx");
const UserMission = require("../missions_package/user_mission.js");
const cookieStoreRenderer = require("../shared_package/cookie_store.handlebars");
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

We use Babel to compile our code from ES2015, so using new features from ES6/7 is encouraged (although be aware of [exceptions](#exceptions)).

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


#### Exceptions

Favour ES5 in django templated scripts, as many ES6 features are not supported in browsers.

For example, the `let` keyword is [not supported](http://caniuse.com/#search=let) in Safari 9.

--------------------
### Module pattern

We use Browserify to package our code, so even small scripts should be node-style modules.

#### Export constructor function

Modules should export a single constructor function that can be then required
and instantiated in a script to run it or in tests.


For example for `my_module.js` as follows:
```js
const MyModule = function(){};
module.exports = MyModule;
```

usage would be:

```js
const MyModule = require('./my_module');
new MyModule();
```


#### Bundles

Example bundle file structure:

```
my_module_bundle
├── entry.js
├── my_module.js
└── tests
    └── test_my_module.js
```

`my_module.js`:

```js
const MyModule = function(){};
module.exports = MyModule;
```

If a module can be instantiated without any parameters `entry.js` can look like:

```js
const MyModule = require('./my_module');
new MyModule();
```

and then in a template it would just need to be included in `<script>` tag:
```html
<script src="{% static_url 'dist/js/my_module_bundle.js' %}"></script>
```

In case code requires some parameters to run `entry.js` should export module instance
that needs to be required and called in a script block in a template:

```js
const MyModule = require('./my_module');
module.exports = new MyModule();
```

```html
<script src="{% static_url 'dist/js/my_module_bundle.js' %}"></script>
<script>
  const myModule = require('my_module_bundle');
  myModule.init({
    value: "{% some data from the template %}"
  });
</script>
```

--------------------
### Django

#### Vendor scripts in Django templates

Vendor scripts should be included in global script blocks to prevent raven error logging (may not be applicable to all projects):

```html
{% block js_global %}
<script src="{% static_url 'dist/vendor/lodash.min.js' %}"></script>
{% endblock %}
```

#### Page scripts in Django templates

Page scripts should be included in js_page block:

```html
{% block js_page %}
<script src="{% static_url 'dist/js/page-bundle.js' %}"></script>
{% endblock %}
</script>
```

#### Inline scripts in templates

Inline scripts in templates shouldn't be used if unnecessary. If possible, even short scripts should be written in separate JS file compiled by Babel and bundled.

When a page script requires some data from the template (for example translation strings) it needs to be written inline in `<script>` tag rather then loaded from the file:

```html
{% block js_page %}
<script>
(function(){
  var templateUrl = '{% url 'dp-some-route' %}';
  var templateMsg = '{% trans "Translated string"|escapejs %}';
  var example = require('example-bundle');

  example.doSomething(templateUrl, templateMsg);
})();
</script>
{% endblock %}
```

##### Don't use ES6/7 in inline scripts

Inline scripts in templates are not compiles with Babel, which means that support for ES6 features depends on the browsers.
Most notably features that are not available in inline scripts are:

* [Arrow functions](http://caniuse.com/#feat=arrow-functions), use regular `function` instead of `=>`
* [let](http://caniuse.com/#feat=let), use `var` instead of `let`
* [const](http://caniuse.com/#feat=const), `const` even if supported as a keyword often doesn't really work as constant, so you may also use `var` to avoid confusion
* [Template string literals](https://kangax.github.io/compat-table/es6/#test-template_literals), use string concatenation instead of `` `template ${strings}` ``

When using ES6/7 features in inline scripts always take into account your project's
browser support and consult [Can I Use](http://caniuse.com) and [ES6 compatibility tables](https://kangax.github.io/compat-table/es6/)
to verify if given feature is supported in targeted browsers.
