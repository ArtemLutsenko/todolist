<template>
  <b-container class="col-lg-7 col-md-10 col-sm-12">
    <h1 class="text-center mt-5">Todo app</h1>
    <td-new-task @addNewTask="addNewTask"></td-new-task>
    <hr class="mt-3 mb-2" />
    <td-task-list :listOfTasks="taskList"></td-task-list>
    <hr class="mt-2 mb-3" />
    <td-import-export :taskList="taskList" @export="exportTaskList" />
    <b-modal id="bvModal" @ok="deleteItem" title="Delete item">
      Are you sure you want to delete this task?
    </b-modal>
  </b-container>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
import TdNewTask from '@/components/NewTask.vue';
import TdTaskList from '@/components/TaskList.vue';
import TdImportExport from '@/components/ImportExport.vue';

export default {
  name: 'App',
  components: {
    TdImportExport,
    TdTaskList,
    TdNewTask,
  },
  data() {
    return {
      taskList: [
        {
          text: 'task',
          completed: false,
          id: uuidv4(),
        },
      ],
      deletedItemId: null,
    };
  },
  provide() {
    return {
      deleteItem: (id) => this.deleteTaskItem(id),
      editItem: (id, newValue) => this.editTaskItem(id, newValue),
      toggleCompletionStatus: (id) => this.toggleCompletionStatus(id),
    };
  },
  methods: {
    addNewTask(task) {
      const taskObj = {
        text: task,
        completed: false,
        id: uuidv4(),
      };
      this.taskList = [taskObj, ...this.taskList];
    },
    deleteTaskItem(id) {
      this.$bvModal.show('bvModal');
      this.deletedItemId = id;
    },
    deleteItem() {
      const newTaskList = this.taskList.filter(
        (task) => task.id !== this.deletedItemId,
      );
      this.taskList = [...newTaskList];
      this.deletedItemId = null;
    },
    editTaskItem(id, newValue) {
      const editedTaskList = this.taskList.map((task) => {
        if (task.id === id) {
          // eslint-disable-next-line no-param-reassign
          task.text = newValue;
        }
        return task;
      });
      this.taskList = [...editedTaskList];
    },
    toggleCompletionStatus(id) {
      const editedTaskList = this.taskList.map((task) => {
        if (task.id === id) {
          // eslint-disable-next-line no-param-reassign
          task.completed = !task.completed;
        }
        return task;
      });
      this.taskList = [...editedTaskList];
    },
    exportTaskList(newList) {
      this.taskList = [...newList];
    },
  },
};
</script>

<style></style>
