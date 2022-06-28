<template>
  <div class="wrapper">
    <div class="task-input">
      <img class="image" src="./assets/MorDeeLogo40.png" alt="icon" />
      <input type="text" placeholder="Add a new task" v-model="newTask"/>
      <button type="button" class="buttonAdd" v-on:click="onCreateNewTask">
        ADD
      </button>
    </div>

    <ul class="task-box overflow">
      <li class="task" v-for="task in tasks" v-bind:key="task.id">
        <label>
          <p class="completed">
            {{ task.name }}
          </p>
        </label>
        <div class="settings">
          <i
            v-on:click="() => onDeleteTask(task.id)"
            class="ri delete-bin-fill"
          ><Icon icon="ri:delete-bin-fill" /></i>
        </div>
      </li>
      <li v-for="task in tasks" v-bind:key="task.id" class="task"></li>
    </ul>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue';
import axios from "axios";
export default {
  components: {
		Icon,
	},
  name: "App",
  data() {
    return {
      newTask: "",
      tasks: [],
      logo: "",
    };
  },
  // Fetches posts when the component is created.
  created() {
    axios.get(`http://jsonplaceholder.typicode.com/posts`).then((response) => {
      // JSON responses are automatically parsed.
      response.data.forEach((element) => {
        this.tasks.push({ id: element.id, name: element.title });
      });
    });
  },
  methods: {
    onCreateNewTask() {
      if (this.newTask != "") {
        this.tasks = [
          {
            id: new Date().getTime(),
            name: this.newTask,
            done: false,
          },
          ...this.tasks,
        ];
        this.newTask = "";
      }
    },
    onDeleteTask(id) {
      console.log(id);
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },
};
</script>

<style>
@import "./assets/style.css";
@import url(https://unicons.iconscout.com/release/v4.0.0/css/line.css);
</style>
