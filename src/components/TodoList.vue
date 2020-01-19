<template>
  <ul>
    <li v-for="item in items" :key="item.time">
      {{ item.text | capitalize }}
      <button @click="itemDone(item.time)">Done</button>
    </li>
  </ul>
</template>

<script>
export default {
  name: "TodoList",
  props: {
    items: {
      type: Array
    }
  },
  methods: {
    itemDone (id) {
      this.$emit('onItemDone', id)
    }
  }
};
</script>

<style scoped></style>
