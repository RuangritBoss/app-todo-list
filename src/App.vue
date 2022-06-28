<template>
  <div id="app">
    <!-- Create task form -->
    <div class="creatNewTask">
      <span>
        <input type="text" class="inputTask" v-model="newTask" />
        <button type="button" class="buttonAdd" v-on:click="onCreateNewTask">
          Create new task
        </button>
      </span>
    </div>

    <!-- Task table -->
    <div class="taskTable">
      <table>
        <tr v-for="task in tasks" v-bind:key="task.id">
          <td>
            <input type="checkbox" v-model="task.done" />
          </td>
          <td>
            <p :style="task.done && { textDecoration: 'line-through' }">
              {{ task.name }}
            </p>
          </td>
          <td>
            <button
              type="button"
              class="buttonDelete"
              v-on:click="() => onDeleteTask(task.id)"
            >
              Delete
            </button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      newTask: "",
      tasks: [],
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
      this.tasks = [
        {
          id: new Date().getTime(),
          name: this.newTask,
          done: false,
        },
        ...this.tasks,
      ];
      this.newTask = "";
    },
    onDeleteTask(id) {
      console.log(id)
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },
};
</script>

<style>
#app {
  text-align: center;
}
.creatNewTask {
  margin-bottom: 10px;
}
.creatNewTask .inputTask {
  width: 300px;
  height: 28px;
}
.creatNewTask .buttonAdd {
  height: 28px;
  margin-left: 10px;
}
.taskTable table {
  background-color: aliceblue;
  margin: 0px auto;
  width: 420px;
}
.taskTable p {
  text-align: left;
}

body {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  background: linear-gradient(315deg, #6fc3c4, #6be2a8);
}
::selection {
  color: #fff;
  background: #6fc3c4;
}
</style>
