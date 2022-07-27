# Component Code - Completed

## ShoppingList

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

    <div>
      <slot :itemCount="items.length" name="itemCount"></slot>
    </div>
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

<style scoped>
.themed {
  color: tomato;
}
</style>
```

## App

```js
<template>
  <ShoppingList>
    <template v-slot:itemCount="slotProps">
      <small>{{ slotProps.itemCount }} items</small>
    </template>
  </ShoppingList>
</template>

<script>
import ShoppingList from "./components/ShoppingList";
export default {
  name: "App",
  components: {
    ShoppingList,
  },
};
</script>
```
