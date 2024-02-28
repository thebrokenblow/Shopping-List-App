<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">Add Item</button>
  </div>
  <form class="add-item-form" v-if="editing" @submit.prevent="saveItem">
    <input v-model.trim="newItem" type="text" placeholder="Add an item" />
    <label>
      <input type="checkbox" v-model="newItemHighPriority" />
      High Priority
    </label>
    <button :disabled="newItem.length > maxLength" class="btn btn-primary">Save Item</button>
  </form>
  <p v-if="editing">{{ lengthEditingValue }} / {{ maxLength }}</p>
  <ul>
    <li
      v-for="item in reversedItems"
      @click="togglePurchased(item)"
      :key="item.id"
      class="static-class"
      :class="{
        strikeout: item.purchased,
        priority: item.highPriority
      }"
    >
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">Nothing to see here</p>
</template>
<script>
export default {
  data() {
    return {
      header: 'Shopping List App',
      editing: false,
      newItemHighPriority: false,
      newItem: '',
      maxLength: 200,
      items: [
        {
          id: 1,
          label: '10 party hats',
          purchased: true,
          highPriority: false
        },
        {
          id: 2,
          label: '2 board games',
          purchased: true,
          highPriority: false
        },
        {
          id: 3,
          label: '20 cups',
          purchased: false,
          highPriority: true
        }
      ]
    }
  },
  methods: {
    saveItem() {
      this.items.push({
        id: this.items.length + 1,
        label: this.newItem,
        highPriority: this.newItemHighPriority
      })

      this.newItem = ''
      this.newItemHighPriority = ''
    },
    doEdit(e) {
      this.editing = e
      this.newItem = ''
      this.newItemHighPriority = ''
    },
    togglePurchased(item) {
      item.purchased = !item.purchased
    }
  },
  computed: {
    lengthEditingValue() {
      return this.newItem.length
    },
    reversedItems() {
      return [...this.items].reverse()
    }
  }
}
</script>
