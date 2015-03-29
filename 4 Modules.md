- A template can import/include many file types:
	- **Mask**       — import components or nodes from other `*.mask` files.
	- **Javascript** — `CommonJS` modules are used, but via the configuration a javascript module loader can be defined to support any module system.
	- **JSON**       — load data from files to use it within templates
	- **HTML**       — load html and parse it to `Mask AST`, so that it can be used as generic `mask` nodes.
	- **Style**      — load `css` styles
- A template can export components or nodes.

> :package: Use [Mask Optimizer](https://github.com/atmajs/mask-optimizer) to create single **style**, **makrup** and **script** files from many sources. 

----

##### &#9776;

- `1` [Export](#1-export)
- `2` [Import](#2-import)
	- `2.1` [Path](#21-path-handling)
	- `2.2` [Mask module](#22-mask-module)
		- `2.2.1` [Embed](#221-embed)
		- `2.2.2` [Import nodes](#222-import-nodes)
		- `2.2.3` [Import components](#223-import-components)
	- `2.3` [Javascript module](#23-javascript-module)
	- `2.4` [Data module](#24-data-module)
	- `2.5` [Style module](#25-style-module)
	- `2.6` [HTML module](#26-html-module)
	
- `3` [Loaders](#3-loaders)
	- `3.1` [Mask, Html, Json](#31-mask-html-json)
	- `3.2` [Javascript](#32-javascript)
	- `3.3` [Style](#33-style)

# `1` Export

There are no keywords and no specific syntax to export things, thats why any template can be rendered as a root template. But when a template is requested as a module, then exported are:
- Components, which are created with `define` directive
- Nodes. All generic Mask Nodes.


Basic examples

- **Export generic nodes**

	```mask
	// baz.mask
	h4 #baz > 'Baz'
	```

- **Create and export components**

	```mask
	// foo.mask
	define foo {
	    /* definition */
	}
	```

- **Create and export components and nodes**

	```mask
	// foo.mask
	define foo {
	    /* definition */
	}
	define baz {
		/* definition */
	}
	h4 #qux  > 'Lorem ~[name]'
	h4 #quux > 'Ipsum'
	```


# `2` Import

```mask
// mask examples
import from 'foo';
import foo from 'foo.mask';
import foo as baz from './foo';
import foo as xxx, baz as yyy from 'foo';
import * as Titles from 'foo.mask';
import * as Titles, foo, baz from 'foo.mask';
import h4#qux as Title from './baz';

// javascript example
import User from '/models/User.js';
```

> [**&uarr;** Up](#)

## `2.1` Path Handling

File extension defines the type of a module. When no extension is provided then `mask` extension will be automatically added. 

- **Mask**: `mask` or empty file extension
- **Javascript**: `js`, `es6`, `coffee`
- **Style**: `css`, `sass`, `less`
- **Html**: `html`
- **Data**: `json`

##### Root path

In NodeJS this is `process.cwd()`. In Browser this is the `baseURI` of the document. You can also set custom root path:

- _via Javascript_:

	```javascript
	mask.Module.cfg('base', EXPRESSION);
	mask.Module.cfg('base', 'foo/'); // e.g
	```

- _via Mask_

	```mask
	import:base (EXPRESSION);
	import:base ('foo/'); // e.g
	```



##### Absolute path

`/path` - is an absolute path and is concatenated with a root path to get the file path.

##### Relative path

`path`, `./path` - are relative paths to the parents **module** location or parents **component** location. But if the template is the root module, then the `context` is checked for `filename`/`dirname` properties, if they are not set, then the root path is used. 

Setting the current templates location via context object:
```javascript
mask.render(template, module, {
	dirname: '/foo/baz/'
})
```

> [**&uarr;** Up](#)

## `2.2` Mask module


#### `2.2.1` Embed

For simple cases you can just embed everything from a module into the requested template. Embedding means the imported template is rendered in-place.
```mask
import from 'foo';
```

#### `2.2.2` Import Nodes

- **Import all nodes as alias**

	```mask
	import * as XXX from 'foo';

	// now `XXX` can be used as normal tag name within the template
	XXX;

	//e.g: use `XXX` nodes to render each item in `foos` array in the model
	ul > each (foos) > li > XXX;
	```

- **Use css selector to get specific node for an import**

	_`filter` method is used to get the node, not the `find`, so it should be the root node in the set._

	```mask
	import h4#qux as lorem from 'foo';

	lorem;
	```

#### `2.2.3` Import Components

- By defined name

	```mask
	import foo from 'foo';

	ul > each (foos) > li > foo;
	```

- By defined name with alias

	```mask
	import foo as FOO from 'foo';

	ul > each (foos) > li > FOO;
	```

> [**&uarr; Up**](#)

## `2.3` Javascript module

Imported object is set to the current controllers scope. So it can be accessed in interpolations and in `define` extendings.

- Scope access examples

	```mask
	import * as App from '/configs/application.js';

	h4 > '~[App.name]'
	```
	```mask
	import name from '/configs/application.js';

	h4 > '~[name]'
	```

- Component definition

	```mask
	import * as Utils from '/helpers/baseutils.js';

	define foo extends Utils {
		/* definition */
	}
	```
	
> [**&uarr;** Up](#)

## `2.4` Data module

_Similar to Javascript_
```json
{
	"application": "Foo App"
}
```

```mask
import * as Config from 'config.json';
h4 > '~[Config.application]'
```

> [**&uarr;** Up](#)

## `2.5` Style module

Speaking about the UI it is always needed for a component to load stylesheets. `Import` can load them for you.

```css
#foo { background: red; }
```
```mask
import from 'foo.css';
section #foo > 'Lorem ipsum' // has then red background color
```

> [**&uarr;** Up](#)

## `2.6` HTML module

Similar to `*.mask` HTML is loaded and parsed to `Mask AST`

```html
Lorem <i>ipsum</i>
```
```mask
import * as MyTemplate from 'lorem.html';
section #foo {
	MyTemplate;
}
```

# `3` Loaders

> [**&uarr;** Up](#)

## `3.1` Mask, Html, Json

#### NodeJS
`fs` module is used to load the mask files

#### Browser
`XMLHttpRequest` is used to load the files

#### Custom loader
```javascript
mask.cfg('getFile', function (path) {
	// should return Promise or Deferred
	// and when ready resolve with the string, or reject with an `Error`.
})
```

> :grey_exclamation: `JSON.parse` is used for `json` data


> [**&uarr;** Up](#)

## `3.2` Javascript

#### NodeJS
`require` is used to load the javascript module

#### Browser 
`<script src>` tag is used to load the module. MaskJS also provides global `module` object to simulate `CommonJS` exports.

#### Custom loader
```javascript
mask.cfg('getScript', function (path) {
	// should return Promise or Deferred
	// and when ready resolve with `exported` object, or reject with an `Error`.
})
```

> [**&uarr;** Up](#)

## `3.3` Style

#### NodeJS
Not loading.

#### Browser
`<link href>` tag is used.

#### Custom load
```javascript
mask.cfg('getStyle', function(path) {
	// should return Promise or Deferred
	// and when ready just resolve it
})
```

----
:checkered_flag: If you have any questions, please [open](https://github.com/atmajs/MaskJS/issues/new) an issue or [mail](mailto:team@atmajs.com) us.