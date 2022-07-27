# Component Code - Completed

```js
<template>
  <div>
    <form v-on:submit.prevent="addItem">
      <label for="itemName">Item Name</label>
      <input id="itemName" type="text" v-model="itemName" />
      <button type="button" v-on:click="addItem">Add to List</button>
    </form>

    <h2 v-bind:class="headingClasses">
      Shopping List<template v-if="listName"> for {{ listName }}</template>
    </h2>

    <template v-if="items.length > 0">
      <ul>
        <li v-for="item in items" :key="item">{{ item }}</li>
      </ul>
      <button type="button" v-on:click="clearList">Clear list</button>
    </template>
    <p v-else>Your list is empty. Try adding one above.</p>

    <div>
      <slot v-bind:itemCount="itemCount" name="itemCount"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "ShoppingList",
  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  props: {
    listName: String,
    initialItems: {
      type: Array,
      default() {
        return [];
      },
    },
    themeHeading: {
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
    headingClasses() {
      return {
        themed: this.themeHeading,
      };
    },
    itemCount() {
      return this.items.length !== 1
        ? `${this.items.length} items`
        : `${this.items.length} item`;
    },
  },
};
</script>

<style lang="scss" scoped>
h2.themed {
  color: tomato;
}
</style>
```
