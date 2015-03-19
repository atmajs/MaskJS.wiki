# Syntax

###### About

> HTML is a text-based markup, this means, that anything you write is a text accept the tags, and Mask is otherwise a component-based markup - anything you write is a tag, accept the literals, which are text nodes. And that's all: only one ideological difference - reversed HTML. But not the syntax is the key point of the MaskJS - but the performance: the way how the Parser works, how the Builder creates a Shadow DOM and then inserts into the live DOM - these all makes the application start up significant faster then any other approach. Also, worth to mention, the Parser works same on node.js and a modified Builder creates the Shadow DOM-alike AST, and then renders HTML. And the 2-Phase initialization of a Component allows it to be truly isomorphic.

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
Pretty similar, right? But you can also use some syntax sugar from css to declare the `id` and `class` attributes
```mask
div#foo.bar.qux name='quux' hidden;
```
If a tag begins with `#id` or `.class` declaration the tag name can be omitted. Mask parser guess the tagName. Default is `div`, inside `ul` the guessed tagName is `li`, and so on.
```mask
#foo.bar.qux name = 'quux' hidden;
```

#### Children
As from examples above, if a tag has no children, it should be closed with `;` (semicolon). And to define children of a tag, `{}` (bracket) blocks are used - just like in JS, CSS, LESS, etc.
```html
<section>
  <input type='text' placeholder='Lorem ipsum' />
  <input 
      type = submit 
      value = 'Send me' />
</section>
```
```mask
section {
   input type='text' placeholder='Lorem ipsum';
   input
      type = submit
      value = 'Send me' ;
}
```