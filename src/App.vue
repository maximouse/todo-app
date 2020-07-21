<template>
  <div id="app">
    <div class="panel">
      <div class="filters">
        <filters v-model="filter"></filters>
        <search v-model="search"></search>
      </div>
      <createGroup
        v-if="showGroupForm"
        :groups="groups"
        :message="errMsg"
        @create="task"
        v-on:cancel="this.showModal = false"
      ></createGroup>

      <button class="create" @click="showGroupFormF">Create Group</button>
    </div>

    <group
      @show-task-form-f="showTaskFormF"
      @remove-task="removeTask"
      @remove-group="removeGroup"
      @change-group="changeGroup"
      :tasks="filteredTasks"
      :groups="groups"
    ></group>

    <transition name="fade">
      <newTask
        v-if="showTaskForm"
        :group="group"
        :tasks="tasks"
        @create="task"
        v-on:cancel="this.showModal = false"
        ref="focusTitle"
      ></newTask>
    </transition>
    <transition name="fade">
      <confirm
        v-if="showModal"
        :message="confirmMsg"
        v-on:confirm="true"
        v-on:cancel="this.showModal = false"
      ></confirm>
    </transition>
    <transition name="fade">
      <changeGroup
        v-if="showModalChange"
        v-model="groupSelector"
        :value="group"
        :groups="groups"
        :message="confirmMsg"
        v-on:change="true"
        v-on:cancel="false"
      ></changeGroup>
    </transition>
  </div>
</template>

<script>
import group from "./components/group.vue";
import newTask from "./components/newTask";
import filters from "./components/filters";
import search from "./components/search";
import confirm from "./components/confirm";
import changeGroup from "./components/changeGroup";
import createGroup from "./components/createGroup";
import { EventBus } from "./eventBus";
export default {
  name: "App",
  components: {
    group,
    newTask,
    filters,
    search,
    confirm,
    changeGroup,
    createGroup
  },
  data: function() {
    return {
      data: {},
      tasks: [
        {
          id: 0,
          group: "food",
          title: "Buy milk",
          text: "Please do it",
          completed: false
        },
        {
          id: 1,
          group: "studying",
          title: "Complete math test",
          text: "Until tomorrow",
          completed: false
        },
        {
          id: 2,
          group: "food",
          title: "Make dinner",
          text: "im hungry",
          completed: true
        }
      ],
      groups: ["food", "studying"],
      showTaskForm: false,
      showModal: false,
      showModalChange: false,
      showGroupForm: false,
      filter: "all",
      search: "",
      errMsg: "",
      group: "",
      confirmMsg: "Sure?",
      groupSelector: "",
      task: {}
    };
  },

  methods: {
    addTask(task) {
      if (!this.groups.includes(task.group)) {
        this.groups.push(task.group);
      }
      this.tasks.push(task);
      this.showTaskForm = false;
    },
    showTaskFormF(group) {
      this.showTaskForm = true;
      this.group = group;
      let groups = this.groups;
      let tasks = this.tasks;
      let addTask = this.addTask;
      EventBus.$on("create", function(task) {
        addTask(task);
        EventBus.$off();
      });
      EventBus.$on("cancel", () => (this.showTaskForm = false));
    },
    showGroupFormF() {
      this.showGroupForm = true;
      let createGroup = this.createGroup;
      EventBus.$on("create", function(group) {
        createGroup(group);
      });
      EventBus.$on("cancel", () => (this.showGroupForm = false));
    },
    createGroup(group) {
      if (!this.groups.includes(group)) {
        this.groups.push(group);
        this.showGroupForm = false;
        this.errMsg = "";
        EventBus.$off();
      } else {
        this.errMsg = "Group already exists!";
        EventBus.$off();
        //  this.showGroupForm = false
        this.showGroupFormF();
      }
    },
    removeTask(id, title) {
      this.confirmMsg = "Are you sure you want to delete card: ?";
      this.showModal = true;
      EventBus.$on("confirm", () => {
        this.tasks = this.tasks.filter(t => t.id !== id);
        this.showModal = false;
        EventBus.$off();
      });
      EventBus.$on("cancel", () => (this.showModal = false));
    },
    changeGroup(id, group) {
      this.confirmMsg =
        "Chose new group for card: " + this.tasks[id].title + "?";
      this.groupSelector = this.tasks[id].group;
      this.showModalChange = true;

      EventBus.$on("change", () => {
        this.tasks
          .filter(t => t.id === id)
          .map(t => (t.group = this.groupSelector));
        console.log(this.tasks);
        this.showModalChange = false;
        EventBus.$off();
      });
      EventBus.$on("cancel", () => (this.showModalChange = false));
    },
    removeGroup(group) {
      this.confirmMsg = "Are you sure you want to delete group: " + group + "?";
      this.showModal = true;
      EventBus.$on("confirm", () => {
        this.tasks = this.tasks.filter(t => t.group !== group);
        this.groups = this.groups.filter(g => g !== group);
        this.showModal = false;
        EventBus.$off();
      });
      EventBus.$on("cancel", () => (this.showModal = false));
    },

    filterByValue(array, string) {
      return array.filter(o =>
        Object.keys(o).some(k =>
          o.title.toLowerCase().includes(string.toLowerCase())
        )
      );
    }
  },
  computed: {
    filteredTasks() {
      if (this.search) {
        return this.filterByValue(this.tasks, this.search);
      }
      if (this.filter === "all") {
        return this.tasks;
      } else if (this.filter === "completed") {
        return this.tasks.filter(t => t.completed);
      } else if (this.filter === "uncompleted") {
        return this.tasks.filter(t => !t.completed);
      }
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  border-radius: 2px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 900px;
  min-width: 400px;
  color: #2c3e50;
  margin: 20px 60px 200px 60px;
  display: flex;
  flex-direction: column;
}
.filters {
  width: 100%;
  display: flex;
  justify-content: left;
  border-bottom: 1px solid grey;
  margin-bottom: 5px;
}
.filters > * {
  margin: 10px;
}
.create {
  color: white;
  width: 200px;
  height: 33px;
  font-weight: bold;
  font-size: 1.8rem;
  float: right;
  border: none;
  /* border-radius: 10px; */
  background-color: green;
  cursor: pointer;
  transition: width 0.3s, left 0.3s;
}
.create:hover {
  width: 220px;
}
.err {
  color: red;
  font-weight: bold;
}
button {
  outline: none;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
