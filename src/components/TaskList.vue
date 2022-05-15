<template>
  <b-list-group v-if="listOfTasks.length">
    <td-task-item v-for="task in listOfTasks" :key="task.id" :taskItem="task">
    </td-task-item>
  </b-list-group>
</template>

<script>
import TdTaskItem from '@/components/TaskItem.vue';

export default {
  name: 'TdTaskList',
  components: { TdTaskItem },
  props: {
    listOfTasks: {
      type: [],
      require: true,
    },
  },
};
</script>

<style scoped></style>
