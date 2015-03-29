- `1` [Register](#1-register)
- `2` [Define _(Javascript)_](#2-definition)
- `3` [Render flow](#3-render-flow)

# `1` Register

```javascript
mask.registerHandler(TAG_NAME, Function | Object)
```

Binds a handler (component) to a tag.

# `2` Definition

> :exclamation: Low-level api. Use [Compo](https://github.com/atmajs/mask-compo) to create components, as it has much more features to eliminate a lot of boilerplate for you

### `Function`

Components constructor. All the methods and properties are **optional**.

- **`tagName: String`** <a name='tagname'>#</a>

	When set, then builder will wrap the childnodes into this element


- **`attr: Object`** <a name='attr'>#</a>

	Works when `tagName` is set, the element will get this attributes

- **`renderStart(model, ctx, container):Promise|Void`** <a name='tagname'>#</a>

	When defined, builder will call this function, from there a component can redefine the model object and the nodes.

	:star: **Async**

	To create an async component just return a promise.

- **`renderStartClient(model, ctx, container)`** <a name='renderStartClient'>#</a>

	Is called when the component was rendered in NodeJS, as `renderStart` is called only once during the process

- **`render(model, ctx, container)`** <a name='render'>#</a>

	Component has full responsibility for the render process. _The Builder does not render the subnodes and does nothing._

- **`renderEnd(elements, model, ctx, container)`** <a name='renderEnd'>#</a>

	When the builder is ready with render, this function is called and created elements are passed to it.

- **`renderEndServer(elements, model, ctx, container)`** <a name='renderEndServer'>#</a>

	Is called when the component was rendered in NodeJS, as normally the `renderEnd` is called once and always on client-side.

- **your properties and methods**

	Any other stuff you may want to use in the component

### `Object`

The constructor is then created with this Object as a `prototype`. _For the possible methods and properties see the `Function` option_

# `3` Render flow

When `render` function is not overriden

- Browser only

	- `renderStart`
	- `renderEnd`

- NodeJS only
	
	- `renderStart`

- Isomorphic

	- Server
		- `renderStart`
		- `renderEndServer`
	- Browser
		- `renderStartClient`
		- `renderEnd`


----
:checkered_flag: