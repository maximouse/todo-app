<template>
  <div class="wrapper">
    <form id="form" class="form" v-on:submit.prevent>
      <label for="form">
        <h3>Create new Group</h3>
      </label>
      <p class="err" v-if="message">{{message}}</p>

      <input type="text" label="Group" placeholder="New Group" v-model="newGroup">
      <button class="btn confirm" @click="create()">Create</button>
      <button class="btn confirm" @click="cancel()">Cancel</button>
    </form>
  </div>
</template>

<script>
import { EventBus } from "../eventBus";
export default {
  name: "newGroup",
  data: function() {
    return {
      newGroup: ""
    };
  },
  props: ["value", "groups", "message"],

  methods: {
    create() {
      let group = this.newGroup || "New Group";
      EventBus.$emit("create", group);
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
.err {
  color: red;
  font-weight: bold;
}
</style>