<template>
  <div class="container">
    <h2 class="text-center mt-5">Vue ToDo App</h2>

    <!-- Input what you do -->
    <div class="d-flex mt-5">
      <input v-model="task" type="text" class="form-control" placeholder="Enter Task ?">
      <button class="btn btn-success rounded-0" @click="submitTask">SUBMIT</button>
    </div>

    <!-- Table data -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th>
            <span :class="{'finished': task.status === 'finished'}">
              {{task.name}}
            </span>
          </th>
          <td style="width: 120px">
            <span class="pointer" @click="changeStatus(index)" 
            :class="{
              'btn btn-sm btn-primary': task.status === 'to-do', 
              'btn btn-sm btn-warning': task.status === 'in-progress', 
              'btn btn-sm btn-danger': task.status === 'finished'}">
              {{firstCharUpper(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen pointer"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  // Dfault data app
  data(){
    return{
      task: '', 
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],

      tasks: [
        {
          name: 'Sarapan biar gak spaneng.',
          status: 'to-do'
        },
        {
          name: 'Ngombe biar gak seret.',
          status: 'in-progress'
        }
      ]
    }
  },

  // to-do methods
  methods: {
    submitTask(){
      if (this.task.length === 0)return;

      if (this.editedTask === null ){
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task='';
    },

    deleteTask(index){
      this.tasks.splice(index, 1)
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }

  }
}
</script>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Mitr&display=swap');

  *, html {
    font-family: 'Mitr', sans-serif;
  }


  .pointer {
    cursor: pointer;
  }

  .finished {
    text-decoration: line-through;
  }

  .fa-pen {
    color: rgb(61, 161, 106);
  }

  .fa-trash {
    color: red;
  }
</style>
