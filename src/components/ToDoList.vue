<template>
  <div id="app">
    <v-card>
      <h1 class="mb-3">My To Do List</h1>
      <!-- Add Task -->
      <v-text-field
        v-model="task"
        class="mb-4"
        placeholder="Enter Your Task..."
        append-icon="mdi-plus"
        clearable
        outlined
        hide-details
        color="#DCCCBB"
        @click:append="add"
        @keypress.enter="add"
      />
      <!-- Operation -->
      <div
        v-if="toDoRef.length"
        class="my-3"
      >
        <v-btn
          v-show="isShowCompleted"
          @click="hideCompleted"
          outlined
        >
          <v-icon class="mr-1">mdi-eye-off</v-icon>
          Hide Completed
        </v-btn>
        <v-btn
          v-show="!isShowCompleted"
          @click="showCompleted"
          outlined
        >
          <v-icon class="mr-1">mdi-eye</v-icon>
          Show Completed
        </v-btn>
        <v-btn 
          @click="clearAll"
          outlined
        >
          Clear All
        </v-btn>
      </div>
      <!-- Task list -->
      <v-row
        no-gutters
        v-for="(item,index) in toDoList"
        :key="index"
        :style="(index == editIndex) ? 'background-color: rgba(220, 204, 187, 0.3);' : ''"
      >
        <v-col cols="1">
          <v-checkbox
            v-model="toDoList[index].check"
            color="grey"
            class="ma-0 pa-0"
            hide-details
            :ripple="false"
            @change="updateRef"
          />
        </v-col>
        <v-col>
          <v-text-field
            ref="editInput"
            v-model.trim="toDoList[index].task"
            class="ma-0 pa-0"
            :class="(toDoList[index].check && index !== editIndex) ? 'done' : ''"
            color="grey"
            :disabled="index !== editIndex"
            hide-details
            @keypress.enter="submit"
          />
        </v-col>
        <v-col cols="2" align-center>
          <EditBtn
            v-show="index === editIndex"
            :options="{submit: true, cancel: true}"
            @submit="submit(index)"
            @cancel="cancel(index)"
          />
          <EditBtn
            v-show="index !== editIndex"
            :options="{edit: true, remove: true}"
            @edit="edit(index)"
            @remove="remove(index)"
          />
        </v-col>
      </v-row>
      <p class="mt-4">You have {{ getLeftTask }} task(s) left！</p>
    </v-card>
    <Dialog ref="dialog" />
  </div>
</template>
<script>
import EditBtn from './_EditBtn.vue'
import Dialog from './_Dialog.vue'
export default {
  name: 'ToDoList',
  components: {
    EditBtn,
    Dialog
  },
  data() {
    return {
      task: '',
      toDoList: [],
      // backup
      toDoRef: [],
      checkbox: false,
      editIndex: null,
      isShowCompleted: true
    }
  },
  computed: {
    getLeftTask () {
      return this.toDoList.filter(item => !item.check).length
    }
  },
  methods: {
    add () {
      if (!this.task) return
      const task = {
        check: false,
        task: this.task
      }
      this.toDoList.push(task)
      this.toDoRef.push(JSON.parse(JSON.stringify(task)))
      this.task = ''
      this.editIndex = null
    },
    edit (index) {
      this.editIndex = index
      this.$nextTick(() => this.$refs.editInput[index].$refs.input.focus())
    },
    // agree edit
    submit (index) {
      if (!this.toDoList[this.editIndex].task) this.remove(index)
      else this.editIndex = null
    },
    // cancel edit
    cancel () {
      this.toDoList[this.editIndex].task = this.toDoRef[this.editIndex].task
      this.editIndex = null
    },
    async remove (index) {
      const valid = await this.$refs.dialog.open("Delete Task", `Do you want to delete task "${this.toDoList[index].task}" ?`)
      if (!valid) return

      this.toDoList.splice(index, 1)
      this.toDoRef.splice(index, 1)
    },
    async clearAll () {
      const valid = await this.$refs.dialog.open("Delete All Tasks", "This will delete all tasks in this list.")
      if (!valid) return
      
      this.toDoList = []
      this.toDoRef = []
    },
    updateRef () {
      this.toDoRef = JSON.parse(JSON.stringify(this.toDoList))
    },
    hideCompleted () {
      this.toDoList = this.toDoList.filter(item => !item.check)
      this.isShowCompleted = false
    },
    showCompleted () {
      this.toDoList = JSON.parse(JSON.stringify(this.toDoRef))
      this.isShowCompleted = true
    }
  },
}
</script>
<style lang="scss">
.v-text-field > .v-input__control > .v-input__slot:before,
.v-text-field > .v-input__control > .v-input__slot:after {
  border-style: none !important;
}
</style>
<style lang="scss" scoped>
#app {
  padding: 30px;
  height: 100%;
  text-align: center;
  background-color: #DCCCBB;
  .v-card {
    margin: 0 auto;
    padding: 16px 30px;
    width: 80%;
    .row {
      border-top: 1px solid grey;
      border-bottom: 1px solid grey;
      display: flex;
      align-items: center;
      padding-left: 10px;
    }
    .row + .row {
      border-top: 0px;
    }
    .row:hover {
      background-color: rgba(220, 204, 187, 0.1);
    }
    .done {
      text-decoration: line-through;
    }
  }
  .v-btn + .v-btn {
    margin-left: 10px;
  }
}
</style>
