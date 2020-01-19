<template>
  <div id="app">
    <TodoList :items="sortedItemDes" @onItemDone="done" />
    <InputForm @onSave="save" />
  </div>
</template>

<script>
import TodoList from "@/components/TodoList";
import InputForm from "@/components/InputForm";
import { mapState, mapMutations } from "vuex";

export default {
  name: "app",
  components: {
    TodoList,
    InputForm
  },
  filters: {
    capitalize(value) {
      return value.toUpperCase();
    }
  },
  mounted() {
    this.initItem(JSON.parse(localStorage["todoItems"]));
    // this.items = JSON.parse(localStorage["todoItems"]);
  },
  computed: {
    ...mapState(["items"]),
    sortedItemDes() {
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      return this.items
        .sort((a, b) => {
          return b.time - a.time;
        })
        .filter(element => {
          return element.completed === false;
        });
    }
  },
  methods: {
    ...mapMutations[("addItem", "initItem")],
    done(id) {
      this.items = this.items.map(element => {
        if (element.time === id) {
          element.completed = true;
        }
        return element;
      });
      this.initItem(JSON.parse(localStorage["todoItems"]));
    },
    save(text) {
      this.addItem({
        text: text,
        time: Date.now(),
        completed: false
      });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
