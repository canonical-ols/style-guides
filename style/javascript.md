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
var _ = require("underscore");
var $ = require("jquery");
var APIActionResults = require("../shared-package/api-action-results.js");
var Cookies = require("../shared-package/cookies.js");
var cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
var HappySurvey = require("../missions-package/happy-survey.jsx");
var DashboardActions = require('./datastores/dashboard-actions.js');
var React = require("react");
var UserMission = require("../missions-package/user-mission.js");
```

Yes:
```js
var $ = require("jquery");
var React = require("react");
var _ = require("underscore");

var APIActionResults = require("../shared-package/api-action-results.js");
var Cookies = require("../shared-package/cookies.js");
var DashboardActions = require('./datastores/dashboard-actions.js');
var HappySurvey = require("../missions-package/happy-survey.jsx");
var UserMission = require("../missions-package/user-mission.js");
var cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
```

#### Comments and Documentation

### Inline comments

Inline comments should be the `//` kind not `/* */` kind.



----------
### Libraries

#### jQuery

jQuery should be used for incidental javascript that can not be represented by a react component.

##### Naming

jQuery objects should be prefixed with `$`.

```js
var $selection = $('.myClass');

```

#### React

React should be used for non-trivial UI.

See [React Style Guide](react.md) for further details.
