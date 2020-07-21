<template>
  <div class="groups">
    <ul v-for="group of groups" :key="group">
      <div class="header">
        <h2>{{group}}</h2>
        <button class="remove" @click="$emit('remove-group', group)">&times;</button>
      </div>
      <div class="card" v-for="task in tasks" :key="task.id">
        <task
          @remove-task="removeTask"
          @change-group="changeGroup"
          :task="task"
          v-if="group == task.group"
        ></task>
        <p v-else-if="!task.group">
          <span>
            <br>
            <h3>No tasks</h3>
          </span>
        </p>
      </div>
      <button class="create" @click="$emit('show-task-form-f', group)">+</button>
    </ul>
  </div>
</template>

<script>
import task from "./task";
export default {
  name: "Group",
  components: {
    task
  },
  props: {
    tasks: Array,
    groups: Array,
    completed: Boolean
  },

  methods: {
    changeGroup(id) {
      this.$emit("change-group", id);
    },
    removeTask(id) {
      this.$emit("remove-task", id);
    },
    removeGroup(group) {
      this.$emit("remove-group", group);
    },
    showTaskFormF(group) {
      this.$emit("show-task-form-f", group);
    }
  }
};
</script>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  padding: 20px;
  margin: 20px;
  height: 100%;

  /* background-color: lightblue; */

  /* grid-template-columns: 33% 33% 33%; */
  /* filter: drop-shadow(1px 1px 1px grey); */
  border-top: 1px solid grey;
  /* border-bottom: 1px solid grey; */
}
.header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.card {
  padding: 2px;
  margin: 2px;
  width: 300px;
  display: grid;
  grid-template-columns: 33% 33% 33%;
  grid-template-rows: 100%;
}
.remove {
  color: white;
  width: 20px;
  height: 20px;
  font-weight: bold;
  border: none;
  /* border-radius: 50%; */
  background-color: crimson;
  cursor: pointer;
  transition: transform 0.3s;
}
.remove:hover {
  transform: rotate(90deg);
}
.create {
  color: white;
  width: 50px;
  height: 30px;
  font-weight: bold;
  font-size: 1.8rem;
  position: relative;
  float: right;
  border: none;
  /* border-radius: 10px; */
  background-color: green;
  cursor: pointer;
  transition: width 0.3s, left 0.3s;
}
.create:hover {
  width: 100px;
}
</style>
