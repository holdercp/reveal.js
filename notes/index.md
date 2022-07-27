# Vue Intro

[Completed Code](./code-complete.md)

## Setup

1. Open the CodeSandbox link to your forked starter
2. Let's go ahead and stub out the basic template for our component
   - We have a `form` with a `label`, `input`, and `button`
   - We have an `h2`
   - An empty list

> Pull up completed component on screen

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" />
      <button type="button">Add to List</button>
    </form>

    <h2>Shopping List</h2>

    <ul></ul>
  </div>
</template>
```

## Data

You can think of the `data` property as the component's state. It doesn't have to be updated though.

1. Create a `data` method on the component
   - This needs to be a function because otherwise _all_ instances of the component would share `data`
   - Whatever is on this object when the Vue instance is created will be set up in the reactivity system
2. Create an `items` property that will hold our items in an array.
3. Put some place holder items in there for now

```js
<script>
export default {
  data() {
    return {
      items: ['apple', 'pear', 'kiwi']
    }
  }
};
</script>
```

### Binding

Now we need to "bind" this data to the template.

The simplest way to bind data to the template is through text interpolation with mustaches.

> Show example

We can also bind data to attributes using the `v-bind` directive.

> Show example

- Valid JS expressions can be used within data bindings and directive values. However the directive we are about to talk about does not use a JS expression.
- Shorthand for `v-bind` is `:`

What happens if we try to do this with `items`? It just prints out the array

## List Rendering

[Directive docs](https://v2.vuejs.org/v2/api/#Directives)

We need to use another directive to render out lists. We do this with `.map` in React.

1. Use the `v-for` directive
   - Has an index argument
   - Can be used on objects
     - Has a second key argument
   - Similar to React, we should bind a `key` attributes

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" />
      <button type="button">Add to List</button>
    </form>

    <h2>Shopping List</h2>

    <ul>
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>
```

## v-model

Ok, so we have a list of items that is reactive but we need to be able to add them dynamically instead of hardcoding them.
