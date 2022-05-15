<template>
  <b-input-group class="input-group col-12 d-flex">
    <b-form-input
      id="new-task"
      type="text"
      v-model.trim="task"
      @keydown.enter="addNewTask($event)"
    />
    <b-input-group-append>
      <b-button variant="success" @click="addNewTask">Add Task</b-button>
    </b-input-group-append>
  </b-input-group>
</template>

<script>
export default {
  name: 'TdNewTask',
  data() {
    return {
      task: '',
    };
  },
  methods: {
    addNewTask(event) {
      if (this.task) {
        this.$emit('addNewTask', this.task);
      }
      event.target.blur();
      this.task = '';
    },
  },
};
</script>

<style scoped></style>
