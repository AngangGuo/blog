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

## FAQ
### How's the `each` block works internally?
In the [keyed each block](https://svelte.dev/tutorial/keyed-each-blocks) example, 
See the reason [here](https://stackoverflow.com/questions/62499335/further-explanation-of-sveltes-keyed-each-block)
