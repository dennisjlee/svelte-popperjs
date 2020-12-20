![svelte-popperjs](https://user-images.githubusercontent.com/42545742/102705803-6297b780-42c6-11eb-9540-9641c764d2d3.png)

# svelte-popperjs
[![npm version](http://img.shields.io/npm/v/svelte-popperjs.svg)](https://www.npmjs.com/package/svelte-popperjs)
[![npm downloads](https://img.shields.io/npm/dm/svelte-popperjs.svg)](https://www.npmjs.com/package/svelte-popperjs)
![license](https://img.shields.io/npm/l/svelte-popperjs)
![build](https://img.shields.io/github/workflow/status/bryanmylee/svelte-popperjs/publish)
![size](https://img.shields.io/bundlephobia/min/svelte-popperjs)

Popper for Svelte with actions, no wrapper components or component bindings required!

Other Popper libraries for Svelte (including the official `@popperjs/svelte` library) use a wrapper component that takes the required DOM elements as props. Not only does this require multiple `bind:this`, you also have to pollute your `script` tag with multiple DOM references.

We can do better with Svelte [actions](https://svelte.dev/tutorial/actions)!

## Installation

```bash
$ npm i -D svelte-popperjs
```

Since Svelte automatically bundles all required dependencies, you only need to install this package as a dev dependency with the `-D` flag.

## Usage

`createPopperActions` returns a pair of actions to be used on the [reference and popper elements](https://popper.js.org/docs/v2/constructors/#usage).

The content action takes an [options object](https://popper.js.org/docs/v2/constructors/#options) for configuring the popper instance.

### Example

A Svelte version of the standard [tutorial](https://popper.js.org/docs/v2/tutorial/).

```svelte
<script>
  import { createPopperActions } from 'svelte-popperjs';
  const [popperRef, popperContent] = createPopperActions();
  const popperOptions = {
    modifiers: [
      { name: 'offset', options: { offset: [0, 8] } }
    ],
  };

  let showTooltip = false;
</script>

<button
  use:popperRef
  on:mouseenter={() => showTooltip = true}
  on:mouseleave={() => showTooltip = false}
>
  My button
</button>
{#if showTooltip}
  <div id="tooltip" use:popperContent={popperOptions}>
    My tooltip
    <div id="arrow" data-popper-arrow />
  </div>
{/if}
```

## API

### Accessing the Popper instance

If access is needed to the raw [Popper instance](https://popper.js.org/docs/v2/constructors/#instance) created by the actions, you can reference the third element returned by `createPopperActions`. The third element is a function that will return the current Popper instance used by the actions.

Using the raw Popper instance to [manually recompute the popper's position](https://popper.js.org/docs/v2/lifecycle/#manual-update).

```svelte
<script>
  import { createPopperActions } from 'svelte-popperjs';
  const [popperRef, popperContent, getInstance] = createPopperActions();
  
  async function refreshTooltip() {
    const newState = await getInstance().update();
  }
</script>
```
