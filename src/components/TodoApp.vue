<template>
  <div class="container" style="max-width: 100%">
    <h2 class="text-center mt-5">TRI3D ASSIGNMENT - (Task-List) </h2>

    <div class="d-flex mt-5">
      <input v-model="task" type="text" placeholder="Enter the task..." class="form-con">
      <button @click="submitTask" class="btn btn-warning rounded-0">ADD</button>

    </div>

    <!-- table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">DeadLine</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
           <td>
            <input v-if="editedTask === index" v-model="task.name" @blur="saveEditedTask" @keyup.enter="saveEditedTask" />
            <span v-else>{{ task.name }}</span>
          </td>
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer"
              :class="{
                'text-danger': task.status === 'todo',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'completed',
              }"
            >
              {{ firstCharUpper(task.status) }}

            </span>
          </td>
          <td>
            <input v-if="editedTask === index" v-model="task.deadline" @blur="saveEditedTask" @keyup.enter="saveEditedTask" />
            <span v-else>{{ task.deadline }}</span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)" style="cursor:pointer">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)" style="cursor:pointer">
              <span class="fa fa-trash"></span>
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
    msg: String,
  },
  data() {
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['todo', 'in-progress', 'completed'],
      tasks: [
        {
          name: 'submit assignment',
          status: 'completed',
          deadline: "2023/10/12",
        },
        {
          name: "waiting for TRI3D's reply",
          status: 'in-progress',
          deadline: "2023/10/20",
        },
      ],
      selectedDate: null,
      activeDateIndex: -1,
    };
  },
    methods: {
    submitTask(){
      if(this.task.length ===0) return;
      if(this.editedTask===null){
      this.tasks.push({
        name:this.task,
        status:'to-do',
         deadline: "2023/12/21",

      });
      }else{
        this.tasks[this.editedTask].name=this.task;
        this.editedTask=null;
      }
      this.task='';

    },

    deleteTask(index){
     this.tasks.splice(index, 1);
    },

    editTask(index){
      this.task= this.tasks[index].name;
     
      this.editedTask=index;
    },

    saveEditedTask() {
      this.editedTask = null;
    },

    changeStatus(index){
     let newIndex= this.availableStatuses.indexOf(this.tasks[index].status);
     if(++newIndex>2) newIndex=0;
     this.tasks[index].status=this.availableStatuses[newIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    
    

  },


};
</script>



<style scoped>
.pointer{
  cursor: pointer;
}

.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    max-width: 30%;
    background-color: #def1db;

}

 .table {
    background-color: #2da840;
     width: 30%;

  }
  .form-con {
    width: 700px;
  }
   textarea.form-con {
    height: 100%;
   }
  .btn{
    width: 30%;
  }
   .table tbody tr {
    height: 60px; 
   }
.line-through {
  text-decoration: line-through;
}
</style>