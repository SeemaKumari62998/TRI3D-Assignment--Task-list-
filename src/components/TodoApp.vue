<template>
  <div class="container" style="max-width: 100%">
    <h2 class="text-center mt-5">TRI3D ASSIGNMENT - (Task-List)</h2>

    <div class="d-flex mt-5">
      <input
        v-model="task"
        type="text"
        placeholder="Enter the task..."
        class="form-con"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">ADD</button>
    </div>
    <div class="d-flex mt-5">
      <select v-model="status" class="form-con">
        <option value="" disabled>Select status</option>
        <option v-for="status in availableStatuses" :key="status">
          {{ status }}
        </option>
      </select>
      <button @click="submitStatus" class="btn btn-warning rounded-0">
        Add Status
      </button>
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
        <tr v-for="(task, index) in filteredTasks" :key="index">
          <td>
            <input
              v-if="editedTask === index"
              v-model="task.name"
              @blur="saveEditedTask"
              @keyup.enter="saveEditedTask"
            />
            <span v-else>{{ task.name }}</span>
          </td>

          <td style="width: 120px">
            <select v-model="task.status" @change="updateStatus(index)">
              <option
                v-for="status in availableStatuses"
                :key="status"
                :value="status"
              >
                {{ status }}
              </option>
            </select>
          </td>

          <td>
            <input
              v-if="editedTask === index"
              v-model="task.deadline"
              @blur="saveEditedTask"
              @keyup.enter="saveEditedTask"
            />
            <span v-else>{{ task.deadline }}</span>
          </td>
          <td>
            <div
              class="text-center"
              @click="editTask(index)"
              style="cursor: pointer"
            >
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div
              class="text-center"
              @click="deleteTask(index)"
              style="cursor: pointer"
            >
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
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      status: "",
      editedStatus: null,
      editedTask: null,
      availableStatuses: ["todo", "in-progress", "completed"],
      tasks: [
        {
          name: "submit assignment",
          status: "completed",
          deadline: "2023/10/12",
        },
        {
          name: "waiting for TRI3D's reply",
          status: "in-progress",
          deadline: "2023/10/20",
        },
      ],
      selectedDate: null,
      activeDateIndex: -1,
      selectedStatus: "all",
    };
  },
  computed: {
    filteredTasks() {
      if (this.selectedStatus === "all") {
        return this.tasks;
      } else {
        return this.tasks.filter((task) => task.status === this.selectedStatus);
      }
    },
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "todo",
          deadline: "2023/12/21",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = "";
    },

    submitStatus() {
      if (this.status.length === 0) return;

      if (this.editedStatus === null) {
        this.tasks.push({
          name: this.task,
          status: this.status,
          deadline: "2023/12/21",
        });
      } else {
        this.tasks[this.editedStatus].name = this.task;
        this.tasks[this.editedStatus].status = this.status;
        this.editedStatus = null;
      }
      this.task = "";
      this.status = "";
    },

    deleteStatus(index) {
      this.availableStatuses.splice(index, 1);
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    saveEditedTask() {
      this.editedTask = null;
    },

    updateStatus() {
      this.editedStatus = null;
    },

    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<style scoped>
.pointer {
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
.btn {
  width: 30%;
}
.table tbody tr {
  height: 60px;
}
.line-through {
  text-decoration: line-through;
}
</style>
