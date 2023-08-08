<template>
  <ul class="list">
    <todo-item
      v-for="item in filteredList"
      :key="item.id"
      :item="item"
      @delete="deleteItem"
    />
  </ul>
</template>

<script>
import TodoItem from './TodoItem.vue'

export default {
  components: {
    TodoItem
  },
  props: {
    filteredList: Array
  },
  methods: {
    deleteItem (itemId) {
      this.$emit('delete', itemId)
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/base.scss';

.list {
  list-style: none;
}
</style>
