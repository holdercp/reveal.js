# Vue Intro

[Completed Code](./code-complete.md)
[Vue 2 Docs](https://v2.vuejs.org/v2/guide/)
[References](links.md)

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

[Data Docs](https://v2.vuejs.org/v2/guide/instance.html#Data-and-Methods)

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

Ok, so we have a list of items that is reactive but we need to be able to add them dynamically instead of hardcoding them. Let's wire up the input to add an item to our list.

1. Reset `items` to an empty array

How are we going to keep track of the value inside the input? We need to bind our data to the value of the input. In React we call this a controlled component.

1. Create an `itemName` property in `data` and initialize it to an empty string.
2. Bind `itemName` to the input value.

> Show how value is bound but the model is not updated when the input changes

3. Use `v-model`
   - This automatically sets up two-way binding with inputs
   - It seems magical but it's just sugar around around setting up an emitter on the input and a value binding.
   - We won't cover custom events today

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button">Add to List</button>
    </form>

    <h2>Shopping List</h2>

    <ul>
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
};
</script>
```

## Event Handling

Now we have our input bound both ways to our `itemName` property. But how do we add the value to our `items` array when someone clicks our button?

1. Add a methods property to our component and log out so we know it's working.
2. Listen for the click event on the button and call our method.
   - `v-on` takes an event type as a argument and will call a method name or evaluate an expression.
   - It will be passed the `event` that is emitted.
   - shorthand is `@`
3. Create a new array for `this.items` with the new `itemName` and clear the input.

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" v-on:click="addItem">Add to List</button>
    </form>

    <h2>Shopping List</h2>

    <ul>
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  methods: {
    addItem() {
      this.items = [...this.items, this.itemName];
      this.itemName = "";
    },
  },
};
</script>
```

## Exercise - Clear list

You now have the tools to be able to add one more piece of functionality: clearing the list.

[Event Handling Reference](https://v2.vuejs.org/v2/guide/events.html)

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" @click="addItem">Add to List</button>
    </form>

    <h2>Shopping List</h2>

    <ul>
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
    <button @click="clearList">Clear List</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  methods: {
    addItem() {
      this.items = [...this.items, this.itemName];
      this.itemName = "";
    },
    clearList() {
      this.items = [];
    },
  },
};
</script>
```

## Conditional Rendering

So now we can add items and clear them, but this component is not very professional. When there are no items in the list things just look broken. Let's add an empty state message.

1. `v-if` and `v-else` is how we do conditional rendering in Vue. In React we use ternaries or short-circuits.
   - `v-if` takes an expression that will be evaluated and checked for truthiness
   - `v-else` must be next to an element with `v-if`
2. Group the clear list button in a `<template>`

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" @click="addItem">Add to List</button>
    </form>

    <h2>Shopping List</h2>

    <template v-if="items.length > 0">
      <ul>
        <li v-for="item in items" :key="item">{{ item }}</li>
      </ul>
      <button @click="clearList">Clear List</button>
    </template>
    <p v-else>Your list is empty. Try adding an item above.</p>
  </div>
</template>
```

## Props

Let's add a feature that allows users to name the list when they use this component. We can pass data into a component using props. This is the same concept as React.

1. Add a props property on our model
2. Add a `listName` prop and give it a type of String and a value of empty String
3. Render it in the template
4. In `App.vue` pass a prop

## Exercise - Conditional Rendering

Use conditional rendering to format the list title in a friendly way.

[Conditional Rendering Reference](https://v2.vuejs.org/v2/guide/conditional.html)

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" @click="addItem">Add to List</button>
    </form>

    <h2>
      Shopping List <template v-if="listName"> for {{ listName }}</template>
    </h2>

    <template v-if="items.length > 0">
      <ul>
        <li v-for="item in items" :key="item">{{ item }}</li>
      </ul>
      <button @click="clearList">Clear List</button>
    </template>
    <p v-else>Your list is empty. Try adding an item above.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  props: {
    listName: {
      type: String,
      default: "",
    },
  },
  methods: {
    addItem() {
      this.items = [...this.items, this.itemName];
      this.itemName = "";
    },
    clearList() {
      this.items = [];
    },
  },
};
</script>
```

## Computed Properties

There's a another way to handle this in Vue: Computed properties.

These are properties that are derived from other data like `props` or `data` and are evaluated any time a dependency changes.

1. Add `computed` property object
2. Create a `formattedTitle` property
3. Render it in the template

```js
<template>
  <div>
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" @click="addItem">Add to List</button>
    </form>

    <h2>
      {{ formattedTitle }}
    </h2>

    <template v-if="items.length > 0">
      <ul>
        <li v-for="item in items" :key="item">{{ item }}</li>
      </ul>
      <button @click="clearList">Clear List</button>
    </template>
    <p v-else>Your list is empty. Try adding an item above.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  props: {
    listName: {
      type: String,
      default: "",
    },
  },
  methods: {
    addItem() {
      this.items = [...this.items, this.itemName];
      this.itemName = "";
    },
    clearList() {
      this.items = [];
    },
  },
  computed: {
    formattedTitle() {
      const baseTitle = "Shopping List";
      return this.listName ? `${baseTitle} for ${this.listName}` : baseTitle;
    },
  },
};
</script>
```

## Class bindings

Just like classNames.

```js
<template>
  <div :class="{ themed: theme }">
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" @click="addItem">Add to List</button>
    </form>

    <h2>
      {{ formattedTitle }}
    </h2>

    <template v-if="items.length > 0">
      <ul>
        <li v-for="item in items" :key="item">{{ item }}</li>
      </ul>
      <button @click="clearList">Clear List</button>
    </template>
    <p v-else>Your list is empty. Try adding an item above.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  props: {
    listName: {
      type: String,
      default: "",
    },
    theme: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    addItem() {
      this.items = [...this.items, this.itemName];
      this.itemName = "";
    },
    clearList() {
      this.items = [];
    },
  },
  computed: {
    formattedTitle() {
      const baseTitle = "Shopping List";
      return this.listName ? `${baseTitle} for ${this.listName}` : baseTitle;
    },
  },
};
</script>
```

## Dynamic components

Vue has a built in way to handle dynamic components. `<component>` and `:is`. All we need to do is bind the `is` to the value we want.

[Dynamic components docs](https://v2.vuejs.org/v2/guide/components-dynamic-async.html)

1. Change `div` to `component`
2. Create a new `element` prop that accepts a string; demo validator
3. Bind to prop

```js
<template>
  <component :is="element" :class="{ themed: theme }">
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" @click="addItem">Add to List</button>
    </form>

    <h2>
      {{ formattedTitle }}
    </h2>

    <template v-if="items.length > 0">
      <ul>
        <li v-for="item in items" :key="item">{{ item }}</li>
      </ul>
      <button @click="clearList">Clear List</button>
    </template>
    <p v-else>Your list is empty. Try adding an item above.</p>
  </component>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  props: {
    listName: {
      type: String,
      default: "",
    },
    theme: {
      type: Boolean,
      default: false,
    },
    element: {
      type: String,
      default: "div",
      validator: function (value) {
        return ["div", "section"].includes(value);
      },
    },
  },
  methods: {
    addItem() {
      this.items = [...this.items, this.itemName];
      this.itemName = "";
    },
    clearList() {
      this.items = [];
    },
  },
  computed: {
    formattedTitle() {
      const baseTitle = "Shopping List";
      return this.listName ? `${baseTitle} for ${this.listName}` : baseTitle;
    },
  },
};
</script>
```

## Slots

Lastly, let's cover Slots. This component will accept a footer slot. Let's display the item count.

[Slot docs](https://v2.vuejs.org/v2/guide/components-slots.html)

- Use the `<slot>` tag
- Use a `template` and a `v-slot`

### Scoped Slots

```js
<template>
  <ShoppingList>
    <template v-slot:itemCount="slotProps">
      <small>{{ slotProps.itemCount }} items</small>
    </template>
  </ShoppingList>
</template>

<template>
  <component :is="element" :class="{ themed: theme }">
    <form>
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" @click="addItem">Add to List</button>
    </form>

    <h2>
      {{ formattedTitle }}
    </h2>

    <template v-if="items.length > 0">
      <ul>
        <li v-for="item in items" :key="item">{{ item }}</li>
      </ul>
      <button @click="clearList">Clear List</button>
    </template>
    <p v-else>Your list is empty. Try adding an item above.</p>

    <div>
      <slot :itemCount="items.length" name="itemCount"></slot>
    </div>
  </component>
</template>
```
