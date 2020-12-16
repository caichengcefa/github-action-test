**readme 文档**

vite、vue-cli create project； beta、devtool exension 、 vetur for vscode

**快速开始**

Scaffold via Vite:

npm init vite-app hello-vue3

yarn create vite-app hello-vue3

vue create hello-vue3

\# select vue 3 preset

**development 情况**

说明

官方插件在 beta 中，年底能 stabilize

Devtools Extension

a new version of the Devtools with a new UI and refactored internals to support multiple Vue versions

the beta channel may conflict with the stable version of devtools so you may need to temporarily disable the stable version for the beta channel to work properly.

vscode 开发的 vetur

**Installation**

cli、cdn、vite； use different builds: vue.global.prod.js (built as IIFES)、vue.esm-browser.js(for modern browser)、vue.cjs.js、vue.esm-bundler.js

Use the official CLI 

cdn

<script src="https://unpkg.com/vue@next"></script>

Vite

Vite (opens new window)is a web development build tool that allows for lightning fast serving of code due its native ES Module import approach.

$ yarn create vite-app <project-name> $ cd <project-name> $ yarn $ yarn dev

**Explanation of Different Builds**

**#From CDN or without a Bundler**

\#vue(.runtime).global(.prod).js:

For direct use via <script src="..."> in the browser, exposes the Vue global.

In-browser template compilation:

vue.global.js is the "full" build that includes both the compiler and the runtime so it supports compiling templates on the fly.

vue.runtime.global.js contains only the runtime and requires templates to be pre-compiled during a build step.

Global builds are not UMD (opens new window)builds. They are built as IIFEs (opens new window)and are only meant for direct use via <script src="...">.

**#vue(.runtime).esm-browser(.prod).js:**

For usage via native ES modules imports (in browser via <script type="module">.

**#For Server-Side Rendering**

\#vue.cjs(.prod).js:

**With a Bundler**

\#vue(.runtime).esm-bundler.js:

When using vue-loader, templates inside *.vue files are pre-compiled into JavaScript at build time. You don’t really need the compiler in the final bundle, and can therefore use the runtime-only build.

**Introduction**

**Getting Started**

declarative rendering、bind attributes、handle user input 、 two-way binding 、conditionals and loops、transtion system 

Declarative Rendering

bind element attributes

Handling User Input

two-way binding

Conditionals and Loops

<div id="conditional-rendering"> <span v-if="seen">Now you see me</span> </div>

This example demonstrates that we can bind data to not only text and attributes, but also the structure of the DOM

Moreover, Vue also provides a powerful transition effect system that can automatically apply transition effects when elements are inserted/updated/removed by Vue.

**Composing with Components**

application and components ，root component， components property like data，methods，computed、inject、 lifecycle hooks

Registering a component in Vue is straightforward: we create a component object as we did with App objects and we define it in parent's components option:

// Create Vue application

const app = Vue.createApp(...)

// Define a new component called todo-item

app.component('todo-item', {

template: `<li>This is a todo</li>`

})

// Mount Vue application

app.mount(...)

But this would render the same text for every todo, which is not super interesting. 

We should be able to pass data from the parent scope into child components.

app.component('todo-item', { props: ['todo'], template: `<li>{{ todo.text }}</li>` })

We can now further improve our <todo-item> component with more complex template and logic without affecting the parent app.

**Application & Component Instances**

**Application**

const app = Vue.createApp({ /* options */ })

The application instance is used to register 'globals' that can then be used by components within that application

const app = Vue.createApp({})

app.component('SearchInput', SearchInputComponent)

app.directive('focus', FocusDirective)

app.use(LocalePlugin)

Most of the methods exposed by the application instance return that same instance, allowing for chaining:

Vue.createApp({}) .component('SearchInput', SearchInputComponent) .directive('focus', FocusDirective) .use(LocalePlugin)

**The Root Component**

The options passed to createApp are used to configure the root component. 

mounted into a DOM element.

const RootComponent = { /* options */ } const app = Vue.createApp(RootComponent) const vm = app.mount('#app')

Unlike most of the application methods, mount does not return the application. Instead it returns the root component instance.

most real applications are organized into a tree of nested, reusable components

As a convention, we often use the variable vm (short for ViewModel) to refer to a component instance.

Each component will have its own component instance, vm. 

For some components, such as TodoItem, there will likely be multiple instances rendered at any one time.

All of the component instances in this application will share the same application instance.

**Component Instance Properties**

data properties

const app = Vue.createApp({ data() { return { count: 4 } } }) const vm = app.mount('#app') console.log(vm.count) // => 4

Properties defined in data are exposed via the component instance

There are various other component options that add user-defined properties to the component instance, such as methods, props, computed, inject and setup. 

Vue also exposes some built-in properties via the component instance, such as $attrs and $emit.

These properties all have a $ prefix to avoid conflicting with user-defined property names.

**Lifecycle Hooks**

Each component instance goes through a series of initialization steps when it's created - for example, it needs to set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes. 

Along the way, it also runs functions called lifecycle hooks, giving users the opportunity to add their own code at specific stages.

All lifecycle hooks are called with their this context pointing to the current active instance invoking it.

Don't use arrow functions (opens new window)on an options property or callback, such as created: () => console.log(this.a) or vm.$watch('a', newValue => this.myMethod()). Since an arrow function doesn't have a this, this will be treated as any other variable and lexically looked up through parent scopes until found, often resulting in errors such as Uncaught TypeError: Cannot read property of undefined or Uncaught TypeError: this.myMethod is not a function.

**Template Syntax**

interpolations text、raw html、attributes

javascript expressions（can’t access windows‘ properties except for math and date）

directs: arguments、dymamic arguments、modifiers、horthands

Vue.js uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying component instance's data. 

Under the hood, Vue compiles the templates into Virtual DOM render functions

Combined with the reactivity system, Vue is able to intelligently figure out the minimal number of components to re-render and apply the minimal amount of DOM manipulations when the app state changes.

If you are familiar with Virtual DOM concepts and prefer the raw power of JavaScript, you can also directly write render functions instead of templates, with optional JSX support.

**Interpolations Text**

"Mustache" syntax (double curly braces)

**Raw HTML**

The double mustaches interprets the data as plain text, not HTML. In order to output real HTML, you will need to use the v-html directive

<p>Using v-html directive: <span v-html="rawHtml"></span></p>

 interpreted as plain HTML - data bindings are ignored. Note that you cannot use v-html to compose template partials, because Vue is not a string-based templating engine

**Attributes**

Mustaches cannot be used inside HTML attributes. Instead, use a v-bind directive:

<div v-bind:id="dynamicId"></div>

In the case of boolean attribute

<button v-bind:disabled="isButtonDisabled">Button</button>

If isButtonDisabled has the value of null or undefined, the disabled attribute will not even be included in the rendered <button> element.

**Using JavaScript Expressions**

actually supports the full power of JavaScript expressions inside all data bindings:

{{ number + 1 }} {{ ok ? 'YES' : 'NO' }} {{ message.split('').reverse().join('')

JavaScript Expressions

Template expressions are sandboxed and only have access to a whitelist of globals (opens new window)such as Math and Date. 

You should not attempt to access user defined globals in template expressions

**Directives**

Directives are special attributes with the v- prefix. 

Directive attribute values are expected to be a single JavaScript expression (with the exception of v-for and v-on, which will be discussed later). 

A directive's job is to reactively apply side effects to the DOM when the value of its expression changes.

<p v-if="seen">Now you see me</p>

**Arguments**

Some directives can take an "argument", denoted by a colon after the directive name. For example, the v-bind directive is used to reactively update an HTML attribute:

<a v-bind:href="url"> ... </a>

Here href is the argument, which tells the v-bind directive to bind the element's href attribute to the value of the expression url.

Another example is the v-on directive, which listens to DOM events:

<a v-on:click="doSomething"> ... </a>

**Dynamic Arguments**

It is also possible to use a JavaScript expression in a directive argument by wrapping it with square brackets

Here attributeName will be dynamically evaluated as a JavaScript expression, and its evaluated value will be used as the final value for the argument.

Dynamic arguments are expected to evaluate to a string, with the exception of null. The special value null can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

```js
<!-- This will trigger a compiler warning. -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

We recommend replacing any complex expressions with a computed property, one of the most fundamental pieces of Vue, which we'll cover shortly.

When using in-DOM templates (templates directly written in an HTML file), you should also avoid naming keys with uppercase characters, as browsers will coerce attribute names into lowercase:

<a v-bind:[someAttr]="value"> ... </a>

**Modifiers**

Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way.

.prevent modifier tells the v-on directive to call event.preventDefault() on the triggered event:

<form v-on:submit.prevent="onSubmit">...</form>

**Shorthands**

Therefore, Vue provides special shorthands for two of the most often used directives, v-bind and v-on:

```js
<a v-bind:href="url"> ... </a>
<!-- shorthand -->
<a :href="url"> ... </a>
<!-- shorthand with dynamic argument -->
<a :[key]="url"> ... </a>

<a v-on:click="doSomething"> ... </a>
<!-- shorthand -->
<a @click="doSomething"> ... </a>
<!-- shorthand with dynamic argument -->
<a @[event]="doSomething"> ... </a>
```



They may look a bit different from normal HTML, but : and @ are valid characters for attribute names and all Vue-supported browsers can parse it correctly.
