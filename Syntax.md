###### About

> Mask is 'reversed' HTML with little CSS and JavaScript syntax. But not the syntax is the key point of the MaskJS - but the performance: the way how the Parser works, how the Builder creates a Shadow DOM and then inserts into the live DOM - these all make the application start up significant faster then any other approach. Also, worth to mention, the Parser works same on Node.js and a modified Builder creates the Shadow DOM-alike AST, and then renders HTML. And the 2-Phase initialization of a Component allows it to be truly isomorphic.

### Comments
```mask
// single line comment
/*
  Multiline comment
*/
```

### Literals
```mask
'Foo'
"Foo"
"""
  Multiline with any '"' inside
"""
'''
  Multiline with any '"' inside
'''
```


### Tags / Components
#### Declaration
```html
<!-- html -->
<div id='foo' class='bar qux' name='quux' hidden></div>
```
```mask
// mask
div id='foo' class='bar qux' name='quux' hidden;
```
Pretty similar, right? Any formatting possible in HTML, is also possible in Mask: new lines, spaces, etc. 

- `sugar` For the `id` and `class` attributes can be used CSS syntax: `#id`, `.class`

	```mask
	div#foo.bar.qux name='quux' hidden;
	```

- `sugar` If a tag begins with `#id` or `.class`, the the tag name can be omitted. Mask Parser will guess the tagName. Default is `div`, inside `ul` the guessed tagName is `li`, and so on.

	```mask
	#foo.bar.qux name='quux' hidden;
	```

#### Attributes
Like in HTML: 
- `'` and `"` are used for values,
- When attributes value contains no whitespace, the quotes are not required.
- Attributes value is also not required, like in HTML.
- Attributes value can contain new lines (_if is a literal_)
```mask
section
	name=foo 
	style = '
		border: 1px solid green;
		background: url("foo.png") no-repeat;
	';
```

#### Expression
A node can have an expression enclosed in Parentheses `()`, and the syntax inside depends on the particular node type
```
log (foo.name);
```


#### Childnodes
- **No childnodes**. As from examples above, if a tag has no children, it should be closed with `;` _`(semicolon)`_. 

- **Childnodes**. To define children of a tag, `{}` _`(brackets)`_ blocks are used, _just like in JS, CSS, LESS, etc_.
	```html
	<section>
		<input type='text' placeholder='Lorem ipsum' />
		<input 
			type = submit 
			value = 'Do some work' />
	</section>
	```
	```mask
	section {
		input type='text' placeholder='Lorem ipsum';
		input
			type = submit
			value = 'Do some work' ;
	}
	```
- `sugar` _`(Better readability)`_ You can use `>` (like in CSS) if a node has only one child.

	```html
	<div class='dialog-backdrop'>
		<div class='dialog-panel'>
			<header> 
				Hello 
			</header>
			<section> 
				<img src='/baz.png' />
			</section>
		</div>
	</div>
	```
	```mask
	// blocks
	.dialog-backdrop {
		.dialog-panel {
			header  { 
				'Hello' 
			}
			section { 
				img src='/baz.png'; 
			}
		}
	}
	```
	```mask
	.dialog-backdrop > .dialog-panel {
		header  > 'Hello'
		section > img src='/baz.png';
	}
	```

- **Custom**. There are node handlers, which implement some custom syntax parsing, like `style`, `script`, `function` etc.
	```mask
	style scoped {
		/* css syntax: HtmlStyleElement is rendered */
		background: red;
		color: white;
	}
	function onRenderStart () {
	    console.log('Start rendering');
	}
	```


### Interpolations
This is a complex topic to explain. Template rendering involves Data Models and Controllers. Here we just show the interpolation syntax in string literals
Interpolation quotes: (_start_ `~[`, _end_ `]`)

- **Simple**. Property accessors
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
				// `each` statement creates new model scope, so here the model is string
				'~[.]'
			}
			```


	```mask
	h4 name='~[foo.name]' > 'Lorem ipsum: ~[foo.bar.qux]'
	```

- **Expressions**. Internal engine is used to parse and evaluate expressions. No `eval` is used - so is much faster and safer. Expressions should start with `:` _`(colon)`_

	```mask
	'~[: EXPRESSION]'
	```
	```mask
	h4 > '~[: user.name == null ? "uknown" : user.name]'
	h4 > '~[: user.name || "uknown" ]'
	h4 > 'Fn call example: ~[: foo(bar, "quux", 1) ]'
	```
- **Helpers**. Custom helpers can preprocess expressions

	```mask
	'~[HELPER_NAME: EXPRESSION]'
	```


### Template Merging
Powerful feature to combine 2 or more templates. Based on named placeholders, which start with `@` symbol. This is like "Templates for a Template". Merging occures before render, and relates **only** to templates. Data models, controllers **are not** participate in this Phase. This feature is useful, when you want to encapsulate the template and not to repeat yourself all the time. Examples below show very basic merging template `B` into template `A`. 

- Merge nodes

	```mask
	// A
	.container > @body;
	// B
	@body {
		/* nodes */
	}
	// A+B
	.container {
		/* nodes */
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
	@content { /* nodes */ }
	// A + B
	article  { /* nodes */ }
	```

	From the example, as template `B` does not contain `@header` section `h4` is not in the resulted template;

- `@if`: conditional merging. Values for a condition are taken from the template `B`

	```mask
	// A	
	@if (condition) {
		/* nodes */
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

Small real world example. Tired to repeat yourself all the time with twitters bootstrap buttons?
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
First of all, it is more readable then raw bootstrap example. And afterwards, we can change the implementation of the `actionBtn` any time, and add some features, like spinner indicator on click, etc.