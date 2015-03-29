Library supports `HTML` and `Mask` syntaxes. Both are parsed to the `Mask AST`. And you are free to choose which one to use. You can even mix them within one template. 


#### Why do we support two languages?


`HTML` and `Mask` are similar but at the same time opposite to each other.
`HTML` is a **text-based** markup, and `Mask` is **element-based** markup. We would encourage you to use `Mask` for the application structure. And `HTML` is convenient when you write texts for the application. 


- `1` **&rsaquo;** :link: [Html](1.1-Markup-Html)
- `2` **&rsaquo;** :link: [Mask](1.2-Markup-Mask)
- `3` [Interpolations](#3-interpolations)
	- `3.1` [Accessor](#31-property-accessor)
	- `3.2` [Expression](#32-expression)
	- `3.3` [Helper](#33-helper)
	
- `4` [Compound statements](#4-compound-statements)
- `5` [Template merging](#5-template-merging)


# `3` Interpolations
This is a complex topic to explain. Template rendering involves **Data Models** and **Controllers**.
Interpolation quotes: (_start_ `~[`, _end_ `]`). Interpolations are used in TextNodes and inside attribute values.

### `3.1` Property accessor

```mask
~[ACCESSOR]
```

- Resolvers
	- Lookup: `MODEL → CONTROLLERS SCOPE → PARENT CONTROLLERS SCOPE → ...`

		```mask
		'~[PATH]'
		```
	- Lookup: `CURRENT NODES ATTRIBUTES`

		```mask
		'~[$a.PATH]'
		```
	- Lookup: `CONTROLLER → PARENT CONTROLLER → ...`

		```mask
		'~[$c.PATH]'
		```
	- Sometimes it is needed to refer current model value. Use single `.` dot for this.

		```mask
		var letters = ['A', 'B', 'C'];
		each (letters) {
			// `each` statement creates new model scope
			// so here, in the block, model is a string item (A, B, C)
			'~[.]'
		}
		```

```mask
h4 name='~[foo.name]' > 'Lorem ipsum: ~[foo.bar.qux]'
```

### `3.2` Expression

```mask
~[:EXPRESSION]
````

Internal engine is used to parse and evaluate expressions. No `eval` is used - so is much faster and safer. Expressions should start with `:` _`(colon)`_

```mask
h4 > '~[: user.name == null ? "uknown" : user.name]'
h4 > '~[: user.name || "uknown" ]'
h4 > 'Fn call example: ~[: foo(bar, "quux", 1) ]'
```

### `3.3` Helper

Custom helpers can preprocess expressions or strings

```mask
'~[HELPER_NAME: DATA]'
```

# `4` Compound statements

_Same as in Javascript_
```mask
if (EXPRESSION) {
	// nodes
}
```

:orange_book: [Read more...**&crarr;**](2-Compound-statements)


# `5` Template merging
Powerful feature to combine 2 or more templates. Based on named placeholders, which start with `@` symbol. This is like "Templates for a Template". Merging occures before render, and relates **only** to templates. Data models, controllers **are not** participate in this Phase. This feature is useful, when you want to encapsulate the template and not to repeat yourself all the time. Examples below show very basic merging template `B` into template `A`. 

- Merge nodes

	```mask
	// A
	.container > @body;
	// B
	@body {
		'Lorem nodes here'
	}
	// A+B
	.container {
		'Lorem nodes here'
	}
	```
- Merge attribute

	```mask
	// A
	@icon > i.fa.fa-@[attr.name] data-id='@attr.id';
	// B
	@icon name='plus' id='foo';
	// A+B
	i.fa.fa-plus data-id='foo';
	```
- Wrapp merged template. Use the `placeholder` **keyword** to insert current template.

	```mask
	// A
	@header > h4 > @placeholder;
	@content > article > @placeholder;
	
	// B
	@content { 'Lorem nodes here' }
	// A + B
	article  { 'Lorem nodes here' }
	```

	As template `B` does not contain `@header` section, `h4` is not in the resulted template.

- `@if`: conditional merging. Values for a condition are taken from the template `B`

	```mask
	// A	
	@if (condition) {
		'Lorem nodes here'
	}
	```
- `@each`: repeat same placeholders from template `B`.

	```mask
	// A
	@each (panel) {
		.panel name='@attr.name' > .panel-container {
			h4  > @header;
			div > @body;
		}
	}
	// B
	@panel name = foo {
		@header > 'Foo panel'
		@body > 'Foo content'
	}
	@panel name = baz {
		@header > 'Baz panel'
		@body > 'Baz content'
	}

	// A + B
	.panel name='foo' > .panel-container {
		h4  > 'Foo panel'
		div > 'Foo content'
	}
	.panel name='baz' > .panel-container {
		h4  > 'Baz panel'
		div > 'Baz content'
	}
	```

### Small real world example.

Are you tired to repeat yourself all the time with twitters bootstrap buttons?
```mask
button.btn.btn-success x-tap='fooSignal' {
	span.glyphicon.glyphicon-plus;
	'Foo Text'
}
```
Create the component.
```mask
define actionBtn {
	button.btn.btn-@[attr.type || 'success'] x-tap='@attr.signal' {
		span.glyphicon.glyphicon-@[attr.icon];
		@placeholder;
	}	
}
```
And use it everywhere in the project
```mask
actionBtn 
	type   = warn 
	signal = editFooBar 
	icon   = pencil >  'Edit the FooBar'
```
First of all, it is more readable then the raw bootstrap example. And afterwards, we can change the implementation of the `actionBtn` any time, and add some features, like spinner indicator on click, etc.

----
:checkered_flag: