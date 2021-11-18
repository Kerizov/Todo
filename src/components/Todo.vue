<template>
  <div class="container">
    <div class="container__inner">
      <div>
        <input v-model="task" type="text" placeholder="Enter task">
        <button @click="submitTask" class="btn">submit</button>
      </div>
      <table>
        <thead>
        <tr>
          <th>Task</th>
          <th>Status</th>
          <th>#</th>
          <th>#</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td><p :class="{'finished': task.status === 'Не активно'}">{{ task.title }}</p></td>
          <td @click="changeStatus(index)">{{ task.status }}</td>
          <td class="deleteBtn" @click="deleteTask(index)">delete</td>
          <td @click="editTask(index)">Edit</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['Активно', 'Не активно'],
      tasks: [{
        title: 'sdfsdfsdf',
        status: 'Активна'
      }]
      // tasks: JSON.parse(localStorage.getItem('tasks'))
    }
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return

      if (this.editedTask === null) {
        this.tasks.push({
          title: this.task,
          status: 'Активно'
        });
      } else {
        this.tasks[this.editedTask].title = this.task;
        this.editedTask = null;
      }

      this.task = ''
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    editTask(index) {
      this.task = this.tasks[index].title;
      this.editedTask = index;
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 1) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  }
}
</script>


<style>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  font-family: sans-serif;
}

.container {
  margin: 0 auto;
  width: 1200px;
  padding: 15px;
  text-align: center;
}

.container__inner {
  background-color: #fff;
  margin: 0 auto;
  width: 500px;
  min-height: 400px;
  text-align: left;
  box-shadow: 0px 0px 25px rgba(136, 53, 208, 0.5);
}

.container__inner {
  padding: 10px;
}

table {
  border-collapse: collapse;

}

th, td {
  height: 25px;
  border: 1px solid black;
}

input {
  height: 25px;
  margin-bottom: 10px;
}

.btn {
  outline: none;
  background-color: #fff;
  border: 1px solid black;
  width: 110px;
  height: 25px;
}

.btn:hover, .deleteBtn:hover {
  background-color: #e5e5e5;
}

.deleteBtn {
  cursor: pointer;
}

th, input {
  width: 370px;
  text-align: left;
  padding-left: 5px;
}

th + th {
  width: 150px;
  text-align: center;
  padding-left: 0;

}

td {
  width: 300px;
  padding-left: 5px;
  word-break: break-all;
}

td + td {
  width: 150px;
  text-align: center;
  padding-left: 0;
  cursor: pointer;
}

td + td:hover {
  background-color: #e5e5e5;
}

.finished {
  text-decoration: line-through;
}

</style>
