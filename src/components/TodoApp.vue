<template>
  <div class="todo-app-container">
    
    <!-- Heading -->
    <h2 style="font-family:Verdana, Geneva, Tahoma, sans-serif;" class="todo-app-title">DO YOUR TO-DO</h2>

    <!-- Task counts and priorities -->
    <h4 style="font-family:Verdana, Geneva, Tahoma, sans-serif; text-align: center;">Total Tasks: {{ tasks.length }} || In Progress: {{ countTasksByStatus('in-progress') }} || To-Do: {{ countTasksByStatus('to-do') }} || Finished: {{ countTasksByStatus('finished') }} || Priorities: {{ countPriorities() }}</h4>  
    

    <!-- Input -->
    <div class="input-container">
      <input
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="task-input"
      />
      <button class="submit-button" @click="submitTask">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
    <table class="task-table">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" style="width: 80px">Prioritize</th>
          <th scope="col" style="width: 50px">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
          <span :class="{ 'completed-task': task.status === 'finished', 'in-progress-task': task.status === 'in-progress', 'bold-task': task.prioritize }">
            {{ task.name }}
          </span>
          </td>
          <td>
            <span
              class="task-status pointer"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td>
            <input type="checkbox" v-model="task.prioritize" class="priority-checkbox">
          </td>
          <td class="text-center">
            <div>
              <span style="color:brown" class="fa fa-trash pointer" @click="deleteTask(index)">Remove</span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Clear all tasks button -->
    <div class="text-center">
      <button class="clear-button" @click="clearAllTasks">Clear All Tasks</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Steal bananas from the supermarket.",
          status: "to-do",
          prioritize: false,
        },
        {
          name: "Eat 1 kg chocolate in 1 hour.",
          status: "in-progress",
          prioritize: false,
        },
        {
          name: "Create YouTube video.",
          status: "finished",
          prioritize: false,
        },
      ],
    };
  },
  methods: {
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        this.tasks.push({
          name: this.task,
          status: "to-do",
          prioritize: false,
        });
      }
      this.task = "";
    },
    clearAllTasks() {
      this.tasks = [];
    },
    countTasksByStatus(status) {
      return this.tasks.filter(task => task.status === status).length;
    },
    countPriorities() {
      return this.tasks.filter(task => task.prioritize).length;
    },
  },
};
</script>


<style scoped>
.todo-app-container {
  margin: 0 auto;
  max-width: 1500px; 
  height: 580px;
  background-color: #d3ce8d;
  padding: 10px;
  align-items: center;
}


.todo-app-title {
  text-align: center;
  margin-top: 50px;
  font-size: 30px;
  font-weight: bold;
  padding: 8px;
}

.input-container {
  display: flex;
  margin-top: 50px;
  width: 92%;
  border-radius: 12px;
  margin-left: 40px
}

.task-input {
  flex: 1;
  border-radius: 12px;

}

.submit-button {
  margin-top: 2px;
  padding: 8px 16px;
  background-color: #fffb00;
  color: #030303;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

.task-table {
  background-color: beige;
  margin-top: 50px;
  border-collapse: collapse;
  font-family:Verdana, Geneva, Tahoma, sans-serif;
  width: 92%; 
  border-radius: 4px;
  margin-left: 40px
}

.task-table th,
.task-table td {
  border: 1px solid #ddd;
  padding: 8px;
}

.task-table th {
  background-color: #f2f2f2;
}

.task-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

.completed-task {
  text-decoration: line-through;
}

.in-progress-task{
  font-style: italic;
}

.bold-task {
  font-weight: bold;
}

.task-status {
  cursor: pointer;
}

.task-status.text-danger {
  color: red;
}

.task-status.text-success {
  color: green;
}

.task-status.text-warning {
  color: orange;
}

.pointer {
  cursor: pointer;
}

.text-center {
  text-align: center;
}

.clear-button {
  margin-top: 20px;
  padding: 8px 16px;
  background-color: #f44336;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>