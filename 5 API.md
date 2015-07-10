## Objects
<dl>
<dt><a href="#mask">mask</a> : <code>object</code></dt>
<dd></dd>
</dl>
## Typedefs
<dl>
<dt><a href="#AttributeHandler">AttributeHandler</a> : <code>function</code></dt>
<dd><p>Is called when the builder matches the node by attribute name</p>
</dd>
<dt><a href="#StatementHandler">StatementHandler</a> : <code>function</code></dt>
<dd><p>Is called when the builder matches the node by tagName</p>
</dd>
<dt><a href="#IMaskNode">IMaskNode</a> : <code>class</code></dt>
<dd></dd>
<dt><a href="#Configuration">Configuration</a> : <code>object</code></dt>
<dd><p>Configuration Options</p>
</dd>
<dt><a href="#IUtilHandler">IUtilHandler</a> : <code>object</code></dt>
<dd><p>Is called when the builder matches the interpolation.
Define <code>process</code> function OR group of <code>node*</code>,<code>attr*</code> functions.
The seperation <code>*RenderStart/*</code> is needed for Nodejs rendering - the first part is called on nodejs side,
the other one is called on the client.</p>
</dd>
<dt><a href="#UtilHandler">UtilHandler</a> : <code>function</code></dt>
<dd><p>Is called when the builder matches the interpolation</p>
</dd>
</dl>
<a name="mask"></a>
## mask : <code>object</code>
**Kind**: global namespace  

* [mask](#mask) : <code>object</code>
  * [.TreeWalker](#mask.TreeWalker)
    * [.walk(root, visitor)](#mask.TreeWalker.walk)
    * [.walkAsync(root, visitor, done)](#mask.TreeWalker.walkAsync)
  * [.Dom](#mask.Dom) : <code>object</code>
    * [.MaskNode](#mask.Dom.MaskNode) : <code>class</code>
    * [.TextNode](#mask.Dom.TextNode) : <code>class</code>
  * [.render(template, [model], [ctx], [container], [controller])](#mask.render) ⇒ <code>IAppendChild</code> &#124; <code>Node</code> &#124; <code>DocumentFragment</code>
  * [.renderAsync()](#mask.renderAsync) ⇒ <code>Promise</code>
  * [.parse(template)](#mask.parse) ⇒ <code>MaskNode</code>
  * [.parseHtml(template)](#mask.parseHtml) ⇒ <code>MaskNode</code>
  * [.build(node, model, ctx, container, controller, children)](#mask.build) ⇒ <code>IAppendChild</code>
  * [.stringify(node, [opts])](#mask.stringify) ⇒ <code>string</code>
  * [.run()](#mask.run) ⇒ <code>object</code>
  * [.merge(a, b, [owner], [opts])](#mask.merge) ⇒ <code>MaskNode</code>
  * [.getHandler(name, [component])](#mask.getHandler) ⇒ <code>IComponent</code>
  * [.getHandlers([component])](#mask.getHandlers) ⇒ <code>object</code>
  * [.registerHandler(name, component, component, component)](#mask.registerHandler) ⇒ <code>void</code>
  * [.registerFromTemplate(template, [component], [path])](#mask.registerFromTemplate) ⇒ <code>Promise</code>
  * [.registerScoped(scopedComponent, name, component)](#mask.registerScoped) ⇒ <code>void</code>
  * [.define()](#mask.define) ⇒ <code>void</code> &#124; <code>Promise</code>
  * [.registerAttrHandler(name, [mode], handler)](#mask.registerAttrHandler) ⇒ <code>void</code>
  * [.getAttrHandler(name)](#mask.getAttrHandler) ⇒ <code>[AttributeHandler](#AttributeHandler)</code>
  * [.registerStatement(name, StatementHandler})](#mask.registerStatement)
  * [.getStatement(name)](#mask.getStatement) ⇒ <code>[StatementHandler](#StatementHandler)</code>
  * [.config()](#mask.config)
  * _Mask Util_
    * [._](#mask._)
    * [.getUtil(name, handler)](#mask.getUtil)
    * [.registerUtil(name)](#mask.registerUtil)

<a name="mask.TreeWalker"></a>
### mask.TreeWalker
TreeWalker

**Kind**: static property of <code>[mask](#mask)</code>  

* [.TreeWalker](#mask.TreeWalker)
  * [.walk(root, visitor)](#mask.TreeWalker.walk)
  * [.walkAsync(root, visitor, done)](#mask.TreeWalker.walkAsync)

<a name="mask.TreeWalker.walk"></a>
#### TreeWalker.walk(root, visitor)
Visit each mask node

**Kind**: static method of <code>[TreeWalker](#mask.TreeWalker)</code>  

| Param | Type |
| --- | --- |
| root | <code>MaskNode</code> | 
| visitor | <code>TreeWalker~SyncVisitior</code> | 

<a name="mask.TreeWalker.walkAsync"></a>
#### TreeWalker.walkAsync(root, visitor, done)
Asynchronous visit each mask node

**Kind**: static method of <code>[TreeWalker](#mask.TreeWalker)</code>  

| Param | Type |
| --- | --- |
| root | <code>MaskNode</code> | 
| visitor | <code>TreeWalker~AsyncVisitior</code> | 
| done | <code>function</code> | 

<a name="mask.Dom"></a>
### mask.Dom : <code>object</code>
Dom

**Kind**: static property of <code>[mask](#mask)</code>  

* [.Dom](#mask.Dom) : <code>object</code>
  * [.MaskNode](#mask.Dom.MaskNode) : <code>class</code>
  * [.TextNode](#mask.Dom.TextNode) : <code>class</code>

<a name="mask.Dom.MaskNode"></a>
#### Dom.MaskNode : <code>class</code>
**Kind**: static property of <code>[Dom](#mask.Dom)</code>  
**Properties**

| Name | Type | Default |
| --- | --- | --- |
| type | <code>type</code> | <code>1</code> | 
| attr | <code>object</code> |  | 
| tagName | <code>string</code> |  | 
| nodes | <code>[Array.&lt;IMaskNode&gt;](#IMaskNode)</code> |  | 
| parent | <code>[IMaskNode](#IMaskNode)</code> |  | 
| expression | <code>string</code> |  | 
| appendChild | <code>function</code> |  | 

<a name="mask.Dom.TextNode"></a>
#### Dom.TextNode : <code>class</code>
**Kind**: static property of <code>[Dom](#mask.Dom)</code>  
**Properties**

| Name | Type | Default |
| --- | --- | --- |
| type | <code>type</code> | <code>2</code> | 
| content | <code>string</code> &#124; <code>function</code> |  | 
| parent | <code>[IMaskNode](#IMaskNode)</code> |  | 

<a name="mask.render"></a>
### mask.render(template, [model], [ctx], [container], [controller]) ⇒ <code>IAppendChild</code> &#124; <code>Node</code> &#124; <code>DocumentFragment</code>
Render the mask template to document fragment or single html node

**Kind**: static method of <code>[mask](#mask)</code>  
**Returns**: <code>IAppendChild</code> &#124; <code>Node</code> &#124; <code>DocumentFragment</code> - container  

| Param | Type | Description |
| --- | --- | --- |
| template | <code>string</code> &#124; <code>MaskDom</code> | Mask string template or Mask Ast to render from. |
| [model] | <code>\*</code> | Model Object. |
| [ctx] | <code>Object</code> | Context can store any additional information, that custom handler may need |
| [container] | <code>IAppendChild</code> | Container Html Node where template is rendered into |
| [controller] | <code>Object</code> | Component that should own this template |

<a name="mask.renderAsync"></a>
### mask.renderAsync() ⇒ <code>Promise</code>
Same to `mask.render` but returns the promise, which is resolved when all async componentsare resolved, or is in resolved state, when all components are synchronous.For the parameters doc @see [render](#mask.render)

**Kind**: static method of <code>[mask](#mask)</code>  
**Returns**: <code>Promise</code> - Alwats fullfills with `IAppendChild|Node|DocumentFragment`  
<a name="mask.parse"></a>
### mask.parse(template) ⇒ <code>MaskNode</code>
Parse **Mask** template to the AST tree

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Description |
| --- | --- | --- |
| template | <code>string</code> | Mask Template |

<a name="mask.parseHtml"></a>
### mask.parseHtml(template) ⇒ <code>MaskNode</code>
Parse **Html** template to the AST tree

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Description |
| --- | --- | --- |
| template | <code>string</code> | Html Template |

<a name="mask.build"></a>
### mask.build(node, model, ctx, container, controller, children) ⇒ <code>IAppendChild</code>
**Kind**: static method of <code>[mask](#mask)</code>  
**Returns**: <code>IAppendChild</code> - container  

| Param | Type | Description |
| --- | --- | --- |
| node | <code>MaskNode</code> |  |
| model | <code>\*</code> |  |
| ctx | <code>object</code> |  |
| container | <code>IAppendChild</code> |  |
| controller | <code>object</code> |  |
| children | <code>Array</code> | @out |

<a name="mask.stringify"></a>
### mask.stringify(node, [opts]) ⇒ <code>string</code>
Serialize Mask AST to the Mask string (@analog to `JSON.stringify`)

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| node | <code>MaskNode</code> |  | MaskNode |
| [opts] | <code>object</code> &#124; <code>number</code> |  | Indent count option or an object with options |
| [opts.indent] | <code>number</code> | <code>0</code> | Indent count, `0` for minimization |
| [opts.minify] | <code>bool</code> | <code>true</code> |  |
| [opts.minimizeAttributes] | <code>bool</code> | <code>true</code> | Remove quotes when possible |

<a name="mask.run"></a>
### mask.run() ⇒ <code>object</code>
Find all `<script type="text/mask" data-run='true'>` blocks in the pageand render each block into the parents container.The function is automatically renders the blocks`<script type="text/mask" data-run='auto'>` on `DOMContentLoaded` event

**Kind**: static method of <code>[mask](#mask)</code>  
**Returns**: <code>object</code> - Root component  
<a name="mask.merge"></a>
### mask.merge(a, b, [owner], [opts]) ⇒ <code>MaskNode</code>
Join two Mask templates or DOM trees

**Kind**: static method of <code>[mask](#mask)</code>  
**Returns**: <code>MaskNode</code> - New joined Mask DOM tree  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| a | <code>string</code> &#124; <code>MaskNode</code> |  | first template |
| b | <code>string</code> &#124; <code>MaskNode</code> |  | second template |
| [owner] | <code>MaskNode</code> &#124; <code>Component</code> |  |  |
| [opts] | <code>object</code> |  |  |
| [opts.extending] | <code>bool</code> | <code>false</code> | Clean the merged tree from all unused placeholders |

<a name="mask.getHandler"></a>
### mask.getHandler(name, [component]) ⇒ <code>IComponent</code>
Get Components constructor from the global repository or the scope

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>string</code> |  |
| [component] | <code>object</code> | pass a component to look in its scope |

<a name="mask.getHandlers"></a>
### mask.getHandlers([component]) ⇒ <code>object</code>
Get all components constructors from the global repository and/or the scope

**Kind**: static method of <code>[mask](#mask)</code>  
**Returns**: <code>object</code> - All components in an object `{name: Ctor}`  

| Param | Type | Description |
| --- | --- | --- |
| [component] | <code>object</code> | pass a component to look also in its scope |

<a name="mask.registerHandler"></a>
### mask.registerHandler(name, component, component, component) ⇒ <code>void</code>
Register a component

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>string</code> |  |
| component | <code>object</code> &#124; <code>IComponent</code> |  |
| component | <code>object</code> | Component static definition |
| component | <code>IComponent</code> | Components constructor |

<a name="mask.registerFromTemplate"></a>
### mask.registerFromTemplate(template, [component], [path]) ⇒ <code>Promise</code>
Register components from a template

**Kind**: static method of <code>[mask](#mask)</code>  
**Returns**: <code>Promise</code> - - Fullfills when all submodules are resolved and components are registerd  

| Param | Type | Description |
| --- | --- | --- |
| template | <code>string</code> | Mask template |
| [component] | <code>object</code> &#124; <code>IComponent</code> | Register in the components scope |
| [path] | <code>string</code> | Optionally define the path for the template |

<a name="mask.registerScoped"></a>
### mask.registerScoped(scopedComponent, name, component) ⇒ <code>void</code>
Register a component

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Description |
| --- | --- | --- |
| scopedComponent | <code>object</code> &#124; <code>IComponent</code> | Use components scope |
| name | <code>string</code> | Name of the component |
| component | <code>object</code> &#124; <code>IComponent</code> | Components definition |

<a name="mask.define"></a>
### mask.define() ⇒ <code>void</code> &#124; <code>Promise</code>
Universal component definition, which covers all the cases: simple, scoped, template- 1. (template)- 2. (scopedCompoName, template)- 3. (scopedCtr, template)- 4. (name, Ctor)- 5. (scopedCtr, name, Ctor)- 6. (scopedCompoName, name, Ctor)

**Kind**: static method of <code>[mask](#mask)</code>  
<a name="mask.registerAttrHandler"></a>
### mask.registerAttrHandler(name, [mode], handler) ⇒ <code>void</code>
Register an attribute handler. Any changes can be made to:- maskNode's template- current element value- controller- modelNote: Attribute wont be set to an element.

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>string</code> | Attribute name to handle |
| [mode] | <code>string</code> | Render mode `client|server|both` |
| handler | <code>[AttributeHandler](#AttributeHandler)</code> |  |

<a name="mask.getAttrHandler"></a>
### mask.getAttrHandler(name) ⇒ <code>[AttributeHandler](#AttributeHandler)</code>
Get attribute  handler

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type |
| --- | --- |
| name | <code>string</code> | 

<a name="mask.registerStatement"></a>
### mask.registerStatement(name, StatementHandler})
Register a statement handler

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>string</code> | Tag name to handle |
| StatementHandler} |  | handler |

<a name="mask.getStatement"></a>
### mask.getStatement(name) ⇒ <code>[StatementHandler](#StatementHandler)</code>
Get statement handler

**Kind**: static method of <code>[mask](#mask)</code>  

| Param | Type |
| --- | --- |
| name | <code>string</code> | 

<a name="mask.config"></a>
### mask.config()
Get or Set configuration settings- 1 `(name)`- 2 `(name, value)`- 3 `(object)`

**Kind**: static method of <code>[mask](#mask)</code>  
**See**: @{link MaskOptions} for all options  
<a name="mask._"></a>
### mask._
Utils Repository

**Kind**: static property of <code>[mask](#mask)</code>  
**Category**: Mask Util  

| Param | Type |
| --- | --- |
| name | <code>string</code> | 
| handler | <code>[IUtilHandler](#IUtilHandler)</code> &#124; <code>[UtilHandler](#UtilHandler)</code> | 

<a name="mask.getUtil"></a>
### mask.getUtil(name, handler)
Register Util Handler. Template Example: `'~[myUtil: value]'`

**Kind**: static method of <code>[mask](#mask)</code>  
**Category**: Mask Util  

| Param | Type |
| --- | --- |
| name | <code>string</code> | 
| handler | <code>mask._.IUtilHandler</code> &#124; <code>mask._.FUtilHandler</code> | 

<a name="mask.registerUtil"></a>
### mask.registerUtil(name)
Get the Util Handler

**Kind**: static method of <code>[mask](#mask)</code>  
**Category**: Mask Util  

| Param | Type |
| --- | --- |
| name | <code>string</code> | 

<a name="AttributeHandler"></a>
## AttributeHandler : <code>function</code>
Is called when the builder matches the node by attribute name

**Kind**: global typedef  

| Param | Type |
| --- | --- |
| node | <code>MaskNode</code> | 
| attrValue | <code>string</code> | 
| model | <code>object</code> | 
| ctx | <code>object</code> | 
| element | <code>DomNode</code> | 
| parentComponent | <code>object</code> | 

<a name="StatementHandler"></a>
## StatementHandler : <code>function</code>
Is called when the builder matches the node by tagName

**Kind**: global typedef  

| Param | Type | Description |
| --- | --- | --- |
| node | <code>MaskNode</code> |  |
| model | <code>object</code> |  |
| ctx | <code>object</code> |  |
| container | <code>DomNode</code> |  |
| parentComponent | <code>object</code> |  |
| children | <code>Array</code> | `out` Fill the array with rendered elements |

<a name="IMaskNode"></a>
## IMaskNode : <code>class</code>
**Kind**: global typedef  
**Properties**

| Name | Type |
| --- | --- |
| type | <code>number</code> | 

<a name="Configuration"></a>
## Configuration : <code>object</code>
Configuration Options

**Kind**: global typedef  

* [Configuration](#Configuration) : <code>object</code>
  * [.preprocessor](#Configuration.preprocessor) : <code>object</code>
  * [.style](#Configuration.style) ⇒ <code>string</code>
  * [.script](#Configuration.script) ⇒ <code>string</code>
  * [.base](#Configuration.base)
  * [.getFile](#Configuration.getFile) ⇒ <code>Promise</code>
  * [.getScript](#Configuration.getScript) ⇒ <code>Promise</code>
  * [.buildStyle](#Configuration.buildStyle) ⇒ <code>Promise</code>
  * [.buildScript](#Configuration.buildScript) ⇒ <code>Promise</code>

<a name="Configuration.preprocessor"></a>
### Configuration.preprocessor : <code>object</code>
Style and Script preprocessors

**Kind**: static property of <code>[Configuration](#Configuration)</code>  
<a name="Configuration.style"></a>
### Configuration.style ⇒ <code>string</code>
Transform style before using in `style` tag

**Kind**: static property of <code>[Configuration](#Configuration)</code>  

| Param | Type |
| --- | --- |
| style | <code>string</code> | 

<a name="Configuration.script"></a>
### Configuration.script ⇒ <code>string</code>
Transform script before using in `function,script,event,slot` tags

**Kind**: static property of <code>[Configuration](#Configuration)</code>  

| Param | Type |
| --- | --- |
| source | <code>string</code> | 

<a name="Configuration.base"></a>
### Configuration.base
Base path for modules

**Kind**: static property of <code>[Configuration](#Configuration)</code>  
**Default**: <code>null</code>  
<a name="Configuration.getFile"></a>
### Configuration.getFile ⇒ <code>Promise</code>
Define custom function for getting files content by path

**Kind**: static property of <code>[Configuration](#Configuration)</code>  

| Param | Type |
| --- | --- |
| path | <code>string</code> | 

<a name="Configuration.getScript"></a>
### Configuration.getScript ⇒ <code>Promise</code>
Define custom function for getting script

**Kind**: static property of <code>[Configuration](#Configuration)</code>  
**Returns**: <code>Promise</code> - Fulfill with exports  

| Param | Type |
| --- | --- |
| path | <code>string</code> | 

<a name="Configuration.buildStyle"></a>
### Configuration.buildStyle ⇒ <code>Promise</code>
Define custom function to build/combine styles

**Kind**: static property of <code>[Configuration](#Configuration)</code>  
**Returns**: <code>Promise</code> - Fulfill with {string} content  

| Param | Type |
| --- | --- |
| path | <code>string</code> | 
| options | <code>object</code> | 

<a name="Configuration.buildScript"></a>
### Configuration.buildScript ⇒ <code>Promise</code>
Define custom function to build/combine scripts

**Kind**: static property of <code>[Configuration](#Configuration)</code>  
**Returns**: <code>Promise</code> - Fulfill with {string} content  

| Param | Type |
| --- | --- |
| path | <code>string</code> | 
| options | <code>object</code> | 

<a name="IUtilHandler"></a>
## *IUtilHandler : <code>object</code>*
Is called when the builder matches the interpolation.Define `process` function OR group of `node*`,`attr*` functions.The seperation `*RenderStart/*` is needed for Nodejs rendering - the first part is called on nodejs side,the other one is called on the client.

**Kind**: global abstract typedef  
**Category**: Mask Util  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| arguments | <code>bool</code> | <code>false</code> | should parse interpolation string to arguments, otherwise raw string is passed |
| process | <code>[UtilHandler](#UtilHandler)</code> |  |  |
| nodeRenderStart | <code>function</code> |  | `expr, model, ctx, element, controller, attrName` |
| node | <code>function</code> |  | `expr, model, ctx, element, controller` |
| attr | <code>function</code> |  | `expr, model, ctx, element, controller, attrName` |
| attrRenderStart | <code>function</code> |  | `expr, model, ctx, element, controller, attrName` |

<a name="UtilHandler"></a>
## *UtilHandler : <code>function</code>*
Is called when the builder matches the interpolation

**Kind**: global abstract typedef  
**Category**: Mask Util  

| Param | Type | Description |
| --- | --- | --- |
| value | <code>string</code> | string after the utility name |
| model | <code>object</code> |  |
| type | <code>&quot;attr&quot;</code> &#124; <code>&quot;node&quot;</code> | Current location: text node or attribute |
| element | <code>HTMLNode</code> |  |
| name | <code>string</code> | If the interpolation is in attribute, then this will contain attributes name |

