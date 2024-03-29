---
title: "Notes - SvelteJS"
date: 2021-03-10T10:31:48-08:00
categories:
- Tech
- Web
tags:
- Svelte
- Framework
draft: false
---

# SvelteJS notes

## Up and running
### Get Start
See instruction for [Svelte Template](https://github.com/sveltejs/template)

```
npx degit sveltejs/template svelte-app
cd svelte-app
npm install

// dev mode
npm run dev
// -- create your app


// production mode
npm run build
npm run start

```

* main.js
```javascript
import App from './App.svelte';

const app = new App({
	target: document.body,
});

export default app;
```

* App.svelte
```javascript
<h1>Hello, World!</h1>
```

### FAQ
#### How can I add an image to my svelte component?
* For static files(images, videos, etc) must be under `public` folder. Use the path relative to `public/index.html`. 
* For Svelte component files, they should be under `src` folder. 

For example, if you want to show image under `public/static/flag.jpg` and show button from `src/Button.svelte`:
```
// src/App.svelte
<script>
    import Button from './Button.svelte'

    let src = 'static/flag.jpg'
</script>

<Button/>
<img {src} alt="my pic">
```

## Component

### Component communication options
There are six ways to share data between Svelte components.
1. `Props` pass data from parent components to child components, and optionally back to the parent by using `bind`.
2. `Slots` pass content from parent components to child components so children can decide whether and where to render it.
3. `Events` are used to notify a parent component that something has happened in a child component, and they optionally include data in the event object that is passed to the parent.
4. `Contexts` allow ancestor components to make data available to descendant components without explicitly passing it to all the layers in between.
5. `Module context` stores data in component modules and makes it available to all instances of the component.
6. `Stores` store data outside components and can make it available to any of them.

### Prop
* A prop value can be a literal value of any type (Boolean, number, string, object, array, or function) or the value of a JavaScript expression. 
* When the value is a string, it is enclosed in single or double quotes. 
* Prop values that are non-string literals or JavaScript expressions must be surrounded by curly braces instead of quotes. 
  These can evaluate to any kind of JavaScript value, including objects, arrays, and functions.
* The braces around a prop value can optionally be surrounded with quotes  
```
<Person
    fullName="Jane Programmer"
    developer={true}
    ball={{name: 'baseball', grams: 149, new: false}}
    favoriteColors={['yellow', 'orange']}
    age={calculateAge(person)}
    onBirthday={celebrateFunc}
/>
```
### Event directive
```
on:event-name={handler}
```

## Style
### CSS specificity
See [here](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

The following list of selector types increases by specificity:
* Type selectors (e.g., h1) and pseudo-elements (e.g., ::before).
* Class selectors (e.g., .example), attributes selectors (e.g., `[type="radio"]`) and pseudo-classes (e.g., :hover).
* ID selectors (e.g., #example).
* Inline styles added to an element (e.g., `style="font-weight: bold;"`) always overwrite any styles in external stylesheets, 
and thus can be thought of as having the highest specificity.

* CSS properties specified using the `:global` modifier override those for the same CSS selector in `public/global.css`.
* The `:global` modifier can also be used to override styles in descendant components.
This relies on creating CSS rules with selectors that have higher specificity than rules in descendant components.

### Scoped Style
In Svelte, CSS inside a component's `<style>` block will be scoped only to that component.
This works by adding a class to selected elements, which is based on a hash of the component styles.

```
// greet.svelte
<h1>Hello World!</h1>

// style in component
<style>
  h1 {
    color: red;
  }
</style>
```

When compiling the app, Svelte changes our `h1` styles definition to `h1.svelte-1tky8bj`,
and then modifies every `<h1>` element in our component to `<h1 class="svelte-1tky8bj">`,
so that it picks up the styles as required.

```
// html page
<h1 class="svelte-1tky8bj">Hello World!</h1>

// builder.css
<style>
h1.svelte-1tky8bj {
    color: red;
}
</style>
```

### Global Style
If you want to apply styles to a selector globally, use the `:global(...)` modifier:
```
// style in component
<style>
  :global(h1) {
    color: red;
  }
</style>
```

### CSS library
```
<script>
	import { ButtonSet, Button } from "carbon-components-svelte";
	import Login16 from "carbon-icons-svelte/lib/Login16";
</script>

<svelte:head><link rel="stylesheet" href="https://unpkg.com/carbon-components-svelte@0.30.0/css/g10.css" /></svelte:head>

<div style="padding: 2rem;">
	<ButtonSet>
		<Button icon={Login16} kind="secondary">Cancel</Button>
		<Button kind="primary">Save</Button>
	</ButtonSet>
</div>
```

## Import
### How can I import CSS(Bootstrap) library in REPL?
```css
<style>
	@import url("https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css");
</style>
```

### Import Other Component
* Child.svelte
```
<script>
	export let answer;
</script>

<p>The answer is {answer}</p>
```
* App.svelte
```
<script>
	import Child from './Child.svelte';
</script>

<Child answer={42}/>
```

### Import JavaScript Files
* util.js
```
export const a = {"a":1,"b":2}
```

* App.svelte
```
<script>
	import {a} from './file.js'
</script>
<pre>
  {JSON.stringify(a, null, 2)}
</pre>

// output: 
// {
//   "a": 1,
//   "b": 2
// }
```

## Context
See [How and When to Use Component Context in Svelte](https://imfeld.dev/writing/svelte_context)

Note:
* `setContext` and `getContext` must be called during component initialisation;
* context is not reactive

### Context Keys
We can use anything as a context key. Prefer to use an object literal instead of string. 
Using an object literal means the keys are guaranteed not to conflict in any circumstance 
(since an object only has referential equality to itself, i.e. {} !== {} whereas "x" === "x").

### Contexts vs. stores
Contexts and stores seem similar. They differ in that stores are available to any part of an app, 
while a context is only available to a component and its descendants.

### Module Context

## Slot
Components can have child content. The content is exposed in the child component using the `<slot>` element.

### Fall back content in slot
Child.svelte
```
<div>
    <slot>default text</slot>
</div>    
```
App.svelte
```
<script>
    import Child from './Child.svelte';
</script>

<Child /> <!-- show default text -->

<Child>
    this will replace the default text in the slot position
</Child>        
```
### Named Slot
Named slot will get the content from the element with `slot="name"` prop. 
All other contents will fill in the default slots.

Child.svelte
```
<div>
  <slot>default slot</slot>
  <slot name="notes"></slot>
</div>
```

App.svelte
```
<script>
    import Child from './Child.svelte';
</script>

<Child>
    <p>Above</p>
    <p slot="notes">Notes</p>
    <p>Bottom</p>
</Child>    
```
Output
```
Above
Bottom
Notes
```

### How to get a slot list passed by the parent from a child component?
The keys of the `$$slots` object are the names of the slots passed into the child component by the parent.
```
<!-- Child.svelte -->
<div>
    <slot name="title"></slot>
    <slot name="age"></slot>
    {$$slots.title} - {$$slots.age}
</div>
```

```
<!-- App.svelte -->
<script>
	import Child from './Child.svelte';
</script>

<Child>
	<span slot="title">My Title</span>
</Child>
```

```
<!-- Output -->
My Title

true - undefined
```

### How to pass data from component to parent?
By using slot props you can pass data from component to parent.

`Child.svelte`
```
<script>
	let myVal = 4;
</script>

<div>
	<slot childProp={myVal}></slot>
</div>
```

`App.svelte`
```
<script>
	import Child from './Child.svelte';
	let parentVal = 8;
</script>

<Child let:childProp={childVal}>
	<p>{childVal + parentVal}</p>
</Child>
```

Note: 
* Use `let:childProp={childVal}` to assign the `childProp` value from `Child` component to variable `childVal` 
* `childVal` is only visible in this `Child` component

## Event
### Event Handler
* Pass the handler function name
```
<script>
	let count = 0;
	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	count: {count}
</button>
```

### Event handler with parameters
To pass parameters to event hanader, use anonymous arrow function to wrap it:
```
<script>
  let fruits = [
    {name: "Apple", id: 1},
    {name: "Banana", id:2}
  ]
  const deleteFruit = (id) => {
      console.log(id)
  }
</script>

{#each fruits as fruit (fruit.id)}
    <p>{fruit.name}</p>
    <button on:click={() => deleteFruit(fruit.id)}>Delete</button>
{/each}
```
Warning: It will execute immediately if you use `<button on:click={deleteFruit(fruit.id)}>Delete</button>`

### Event Modifier
**`self` Example:** 

* If no `self` modifier, when you click anywhere within `div` area(including the button inside it), the log will be executed;
* If add `self` modifier, the function will be executed only when you click in the `div`(red) area, but not the button inside it.
```
<script>
	let log = ()=>console.log("clicked")
</script>

<div on:click|self={log}>
	<button>Click Me</button>
</div>

<style>
	div {
		width:80px;
		height:40px;
		border: solid;
		padding: 30px;
        background-color: red;
	}
</style>
```

### Event Emit
Components can emit events using `createEventDispatcher`, or by forwarding DOM events.

**Emit event:** Using `createEventDispatcher`
Dispatch email from a component
```
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
</script>

<button on:click="{() => dispatch('notify', 'detail value')}">Fire Event</button>
```
Events dispatched from child components can be listened to in their parent.
```
<SomeComponent on:whatever={handler}/>
```
 
```
<script>
	function callbackFunction(event) {
		console.log(`Notify fired! Detail: ${event.detail}`)
	}
</script>

<Child on:notify="{callbackFunction}"/>
```

**Event forwarding:**
Unlike DOM events, component events don't bubble. 
If you want to listen to an event on some deeply nested component, the intermediate components must forward the event.

As with DOM events, if the `on:` directive is used without a value, 
the component will forward the event, meaning that a consumer of the component can listen for it.

```
// an on:message event directive without a value means 'forward all message events'.
<SomeComponent on:whatever/>
```

## Fetch & Await block
See: 
[Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) and
[Using Fetch to Consume APIs with Svelte](https://sveltesociety.dev/recipes/component-recipes/using-fetch-to-consume-apis/)

Note:

`fetch` returns a promise containing the response (a Response object). 
This is just an HTTP response, not the actual JSON. 

To extract the JSON body content from the response, we use the json() method (defined on the Body mixin, 
which is implemented by both the Request and Response objects.)

The Body mixin defines the following methods to extract a body (implemented by both Request and Response). 
These all return a promise that is eventually resolved with the actual content.
* arrayBuffer()
* blob()
* json()
* text()
* formData()

Example: 
```
<script>
let promise
const handleClick = () => {
  promise = fetch("https://mywebsite.com/v1/api/stats.json").then((response) => {
    if (!response.ok){
      throw new Error('Network response was not ok')
    }
    return response.json()
    })
}
</script>

<button on:click="{handleClick}">
  Click to load data
</button>

{#await promise}
  <p>waiting for the promise to resolve...</p>
{:then data}
  <p>the value is {JSON.stringify(data, null, 2)}</p>
{:catch error}
  <p style="color: red">something went wrong: {error.message}</p>
{/await}  
```

## Actions
Actions are essentially functions that are executed when an element is mounted.
What's inside the function is entirely up to you. It's similar to React Hooks.

## Component State
* All Javascript statements must go inside the `<script>` block
* A component can have at most one instance-level `<script>` element and one module-level `<script context="module">`
* The module context variables and functions can be shared by all instances, but instance context variables and functions are not accessible in the module context
* Module context variables are not reactive.

## Lifecycle Functions
* `onMount` function schedules a callback to run as soon as the component has been mounted to the DOM.
* `beforeUpdate` function schedules a callback to run immediately before the component is updated after any state change.
* `afterUpdate` function schedules a callback to run immediately after the component has been updated.
* `onDestroy` function schedules a callback to run immediately before the component is unmounted.

### Order
```
// The first time a component is loaded
beforeUpdate
onMount
afterUpdate

// whenever the component state changed
beforeUpdate
afterUpdate

// after unloaded the component
onDestroy
```

### onMount Example

```
<script>
  import {onMount} from 'svelte';
  let name = '';
  let nameInput;
  onMount(() => nameInput.focus());
</script>

// The bind:this directive sets a variable to a reference to a DOM element.
<input bind:this={nameInput} bind:value={name}>
```

## Deploy
### Svelte APP To Netlify
* New site from Git
* Select and authorize your Svelte project repository
* Basic build settings
```text
Branch to deploy: main
// Svelte in Action: npm install; npm run build
Build command: npm run build (or yarn build to match your package.json settings)
Publish directory: public/
```
* click Deploy site

### SvelteKit app to Netlify
* For simple website, you can set build command and publish directory the same as the above; 
  For complex website, in the root of your project, create a `netlify.toml` file:

```toml
[build]
  command = "npm run build"
  publish = "build/"
  functions = "functions/"

...(add even more settings)  
```

* Install `adapter-netlify`
```
npm i -D @sveltejs/adapter-netlify@next
```
* Edit `~/svelte.config.cjs`, change `adapter-node` to `adapter-netlify`
```
-const node = require('@sveltejs/adapter-node')
+const netlify = require('@sveltejs/adapter-netlify')

-    adapter: node(),
+    adapter: netlify(),
```

* Add the new repo to Netlify (e.g. the "New site from Git" button)
* Accept the default options


## Pitfalls
### The value of Boolean props without a value will be true!
* When the prop is omitted, its value defaults to false. 
* When the prop name is specified without a value, its value is inferred to be true.
```
// BoolProp.svelte
<script>
    export let marked = false
</script>

<p>	marked is {marked} </p>
```

```
// App.svelte
<script>
	import BoolProp from './BoolProp.svelte'
	let marked=false
</script>

<BoolProp />
<BoolProp marked={marked} />
<BoolProp marked />

// Output
// marked is false
// marked is false
// marked is true // without a value will be true even if prop is false in App.svelte
```

### Assign value to arrays
https://svelte.dev/tutorial/updating-arrays-and-objects

### Keyed each block
https://svelte.dev/tutorial/keyed-each-blocks

## FAQ
### How's the `each` block works internally?
In the [keyed each block](https://svelte.dev/tutorial/keyed-each-blocks) example, 
See the reason [here](https://stackoverflow.com/questions/62499335/further-explanation-of-sveltes-keyed-each-block)

### How to integrate Svelte app with Go app?
See [Serving a React app with GoLang using Gin](https://medium.com/@synapticsynergy/serving-a-react-app-with-golang-using-gin-c6402ee64a4b)