---
title: "Notes   SvelteJS"
date: 2021-03-10T10:31:48-08:00
draft: true
---

SvelteJS notes

## Component
### Event directive
```javascript
on:event-name={handler}
```

## Context
See [How and When to Use Component Context in Svelte](https://imfeld.dev/writing/svelte_context)

## Slot
You can compose components by using `slot`.

### Fall back content in slot
Child.svelte
```sqlite
<div>
    <slot>default text</slot>
</div>    
```
App.svelte
```sqlite
<script>
    import Child from './Child.svelte';
</script>

<Child /> <!-- show default text -->

<Child>
    this will replace the default text in the slot position
</Child>        
```

### How to get a slot list passed by the parent from a child component?
The keys of the `$$slots` object are the names of the slots passed into the child component by the parent.
```sqlite
<!-- Child.svelte -->
<div>
    <slot name="title"></slot>
    <slot name="age"></slot>
    {$$slots.title} - {$$slots.age}
</div>
```

```sqlite
<!-- App.svelte -->
<script>
	import Child from './Child.svelte';
</script>

<Child>
	<span slot="title">My Title</span>
</Child>
```

```sqlite
<!-- Output -->
My Title

true - undefined
```

### How to pass data from component to parent?
By using slot props you can pass data from component to parent.

`Child.svelte`
```sqlite
<script>
	let myVal = 4;
</script>

<div>
	<slot childProp={myVal}></slot>
</div>
```

`App.svelte`
```sqlite
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

### Event Emit
Components can emit events using `createEventDispatcher`, or by forwarding DOM events.

**Emit event:** Using `createEventDispatcher`
Dispatch email from a component
```javascript
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
</script>

<button on:click="{() => dispatch('notify', 'detail value')}">Fire Event</button>
```
Events dispatched from child components can be listened to in their parent.
```javascript
<SomeComponent on:whatever={handler}/>
```
 
```javascript
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

```javascript
// an on:message event directive without a value means 'forward all message events'.
<SomeComponent on:whatever/>
```

## Actions
Actions are essentially functions that are executed when an element is mounted.
What's inside the function is entirely up to you. It's similar to React Hooks.


## FAQ
### How's the `each` block works internally?
In the [keyed each block](https://svelte.dev/tutorial/keyed-each-blocks) example, 
See the reason [here](https://stackoverflow.com/questions/62499335/further-explanation-of-sveltes-keyed-each-block)

### How to integrate Svelte app with Go app?
See [Serving a React app with GoLang using Gin](https://medium.com/@synapticsynergy/serving-a-react-app-with-golang-using-gin-c6402ee64a4b)