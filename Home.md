# Syntax

###### About

> Mask is a reversed HTML with partial CSS and JavaScript syntax. But not the syntax is the key point of the MaskJS - but the performance: the way how the Parser works, how the Builder creates a Shadow DOM and then inserts into the live DOM - these all make the application start up significant faster then any other approach. Also, worth to mention, the Parser works same on node.js and a modified Builder creates the Shadow DOM-alike AST, and then renders HTML. And the 2-Phase initialization of a Component allows it to be truly isomorphic.

### Comments
Same as in JS.
```mask
// single line comment
/*
  Multiline comment
*/
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
Pretty similar, right? Any formatting possible in HTML, is also possible in Mask: new lines, spaces, etc. You can also use some syntax sugar from css to declare the `id` and `class` attributes
```mask
div#foo.bar.qux name='quux' hidden;
```
If a tag begins with `#id` or `.class`, the the tag name can be omitted. Mask Parser will guess the tagName. Default is `div`, inside `ul` the guessed tagName is `li`, and so on.
```mask
#foo.bar.qux name='quux' hidden;
```

#### Children
- Single node (without children)
	As from examples above, if a tag has no children, it should be closed with `;` (semicolon). 

- And to define children of a tag, `{}` (bracket) blocks are used - just like in JS, CSS, LESS, etc.
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
- `sugar` (Better readability) You can use `>` (like in CSS) if a node has only one child.
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
	```mask
	// note: any new lines or spaces are possible
	.dialog-backdrop > .dialog-panel {
		header  > 'Hello'
		section > img src='/baz.png';
	}
	```
