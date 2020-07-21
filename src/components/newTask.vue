<template>
  <div class="wrapper">
    <form v-on:submit.prevent id="form" class="form">
      <label for="form">
        <h3>Create new task</h3>
      </label>
      <input type="text" label="Group" :placeholder="group" v-model="newGroup">
      <input ref="input" type="text" placeholder="New Task" v-model="title">
      <input type="text" placeholder="Enter description" v-model="text">
      <button class="btn confirm" @click="create()">Create</button>
      <button class="btn confirm" @click="cancel()">Cancel</button>
    </form>
  </div>
</template>

<script>
import { EventBus } from "../eventBus";
export default {
  name: "newTask",
  data: function() {
    return {
      title: "",
      text: "",
      newGroup: ""
    };
  },
  props: ["value", "group", "tasks"],

  methods: {
    create() {
      let tasks = this.tasks;
      let task = {
        id: tasks[tasks.length - 1].id + 1,
        group: this.newGroup || this.group,
        title: this.title.trim() || "New Task",
        text: this.text,
        completed: false
      };
      EventBus.$emit("create", task);
    },

    cancel() {
      EventBus.$emit("cancel", true);
    }
  },
  computed: {
    search: {
      get() {
        return this.value;
      },
      set(val) {
        this.$emit("input", val);
      }
    }
  }
};
</script>

<style scoped>
.form {
  position: absolute;
  display: flex;
  flex-direction: column;
  width: 400px;
  padding: 20px;
  background-color: white;
  margin: auto;
  top: 30%;
  left: 50%;
  margin-left: -25%;
  filter: drop-shadow(10px 10px 10px whitesmoke);
  border: solid 1px darkgrey;
}
.form > * {
  padding: 0.3rem;
  margin: 0.1rem;
}
</style>