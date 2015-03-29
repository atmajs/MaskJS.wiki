##### &#9776;

- `1` [`if`](#1-if)
- `2` [`for..in`](#2-for-in)
- `3` [`for..of`](#3-for-of)
- `4` [`each`](#4-each)
- `5` [`switch`](#5-switch)
- `6` [`repeat`](#6-repeat)
- `7` [`visible`](#7-visible)
- `8` [Extensibility](#8-extensibility)
- `9` [Binded statements](#9-bindings)

## `1` `if`

Controls render flow. If the condition is `true` all subnodes are rendered, otherwise goes to next `else`. 

```mask
if (CONDITION) {
	/*nodes*/
}
else if (CONDITION) {

}
else {

}
```

## `2` `for..in`

Loops over the objects keys and values; renders on every iteration subnodes with current key, value.

> :exclamation: Statment uses controllers scope to store references to current `key` or `value` variables. That means, that the current `Model` is also passed to subnodes

```mask
for (KEY_NAME in OBJECT) {
	/*nodes*/
}
for ((KEY_NAME, VALUE_NAME) in OBJECT) {
	/*nodes*/
}
```
> ```mask
for (keyProp in user) {
	div > 'Property of the user: ~[keyProp]'
}
for ((name, value) in user) {
	if (value != null) {
		'Prop: ~[name]; Stringified value: ~[: value.toString()]'
	}
}
```

## `3` `for..of`

Loops over the array values. _Similar to `for..in`_

> :exclamation: Statment stores variables in the `controllers` scope. So parents model is accessible.

```mask
for (ITEM_NAME of ARRAY) {
	/*nodes*/
}
for ((ITEM_NAME, INDEX_NAME) of ARRAY) {
	/*nodes*/
}
```

## `4` `each`

Loops over the array

> :exclamation: On each iteration new `models` scope is created for the array

Though parents scope is not accessible, but this approach is good for incapsulation. We suggest to use this statement over `for..of`, unless you really need the access to the parents Model

```mask
each (ARRAY) {
	/*nodes*/
}
```
> ```mask
each (users) {
	'Username: ~[name]'
}
```

## `5` `switch`

Controls render flow

```mask
switch (EXPRESSION) {
	case (expression1) {
		/*nodes*/
	}
	case (expression2) {
		/*nodes*/	
	}
	case (expressionN) {
		/*nodes*/	
	}
	default {
		/*nodes*/
	}
}
```

## `6` `repeat`

Render nodes N times. For each iteration there is a current `index` in controllers scope.
```mask
repeat (START_INDEX..END_INDEX) {
	/*nodes*/
}
```
> ```mask
repeat (3..5) > log(index);
// In console:
// 3
// 4
// 5
```

## `7` `visible`

Controlls visibility state of the subnodes. If condition is `false` the statement still renders all the subnodes, but applies `display:none` to the elements.
```mask
visible (CONDITION) {
	/*nodes*/
}
```
> ```mask
visible(name.indexOf('foo') !== 1) {
	h4 > '~[name]'
}
```

# `8` [Extensibility](#extensibility)

> :warning: It is a low level work with the Mask's Parser and Builder. You would rare need to add here something.

All the statements are based on MaskJS Extensibility. You can easily add new behaviour control, etc.
```javascript
mask.registerStatement(name, function(node, model, ctx, container, ctr){
	// perform smth. with a node and all the subnodes
});
```

You can even register a custom parser, in case you want to provide some other syntax to parse a node:
```javascript
mask.registerParser(name, function(string, index, length, parentNode) {
	// perform parsing
	// set the index to the postion, from which
	//    Mask should continue to parse the template
	var node = MyNewNode();
	return [ node,  index ];
});
```

# `9` Bindings

:orange_book: [Read more...**&crarr;**](https://github.com/atmajs/mask-binding)

All the above statements can be bound to the model. On the model change UI will be accordingly refreshed. Just add `+`_(plus)_ sign before

```mask
+if (isPanelVisible) {
	div > 'Hello'
}

+switch (state) {
	case ('foo') > i > 'Foo'
	case ('bar') > b > 'Bar'
}
// etc.
```

----
:checkered_flag: If you have any questions, please [open](https://github.com/atmajs/MaskJS/issues/new) an issue or [mail](mailto:team@atmajs.com) us.

