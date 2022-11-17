<template>
  <div class="">
    <h1 class="text-center bg-primary text-white p-3">To do list</h1>
    <div class="container">
      <div class="row">
        <div class="alert alert-danger" v-if="emptyTask">
          You must add a task
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control m-2"
            v-model="newTask"
            v-on:keyup.enter="addTask()"
          />
        </div>
        <div class="col">
          <input
            type="button"
            class="btn btn-warning"
            value="Add"
            @click="addTask()"
          />
          <input
            class="btn btn-danger m-2"
            type="button"
            value="delete"
            @click="deleteTask()"
          />
        </div>
      </div>
      <div class="row">
        <div class="col fs-3 m-2">Task</div>
        <div class="col fs-3 m-2">Done</div>
      </div>
      <div v-if="tasks.length > 0">
        <div class="row" v-for="(task, index) in filterTask" :key="index">
          <div
            class="col fs-5 m-2"
            :class="task.action === true ? 'delete' : ''"
          >
            {{ index + 1 }}.{{ task.task }}
          </div>
          <div class="col fs-5 m-2">
            <input type="checkbox" v-model="task.action" />
          </div>
        </div>
      </div>
      <div v-else-if="filterTask.length === 0">
        <div class="alert alert-warning text-center">There is no task.</div>
      </div>
      <div class="row mt-auto">
        <h5 class="bg-danger text-center text-white col">
          Hide complete tasks.
          <input type="checkbox" v-model="hideCompleted" />
        </h5>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    hideCompleted: false,
    emptyTask: false,
    newTask: "",
    tasks: [],
  }),
  computed: {
    filterTask() {
      return this.hideCompleted
        ? this.tasks.filter((v) => !v.action)
        : this.tasks;
    },
  },
  methods: {
    addTask() {
      if (this.newTask === "") {
        this.emptyTask = true;
      } else {
        this.tasks.push({
          task: this.newTask,
          action: false,
        });
        this.setLocalStorage();
        this.emptyTask = false;
        this.newTask = "";
      }
    },
    deleteTask() {
      this.tasks = this.tasks.filter((v) => !v.action);
      this.setLocalStorage();
    },
    setLocalStorage() {
      localStorage.setItem("myLocalTasks", JSON.stringify(this.tasks));
    },
  },
  mounted() {
    let data = localStorage.getItem("myLocalTasks");
    if (data != null) {
      this.tasks = JSON.parse(data);
    }
  },
};
</script>

<style>
.delete {
  text-decoration: line-through;
}
</style>
