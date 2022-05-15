<template>
  <b-list-group-item>
    <b-input-group
      size="lg"
      class="d-flex justify-content-between align-content-center"
    >
      <b-form-checkbox
        class="listitem-checkbox"
        align-self="center"
        id="checkbox-1"
        size="lg"
        @change="toggleTaskCompletion"
        :checked="taskItem.completed"
      >
      </b-form-checkbox>
      <b-form-input
        v-if="editMode"
        type="text"
        ref="editInput"
        class="listitem-input"
        v-model="taskItemText"
        @keydown.esc="cancelEdition"
        @keydown.enter="editTaskItem"
        @focusout="editTaskItem"
        size="sm"
      />
      <b-col
        class="listitem-text"
        v-else
        :class="{ 'listitem-text_done': taskItem.completed }"
      >
        {{ taskItem.text }}
      </b-col>
      <b-button-group>
        <b-button
          v-show="editMode"
          variant="info"
          @click="editTaskItem(taskItem.id, taskItemText)"
        >
          <b-icon icon="save" scale="1"></b-icon>
          <span class="btn-text">Save</span>
        </b-button>
        <b-button variant="danger" v-show="editMode" @click="cancelEdition">
          <b-icon icon="x-circle" scale="1" variant="white"></b-icon>
          <span class="btn-text">Cancel</span>
        </b-button>
        <b-button variant="warning" v-show="!editMode" @click="startEdit">
          <b-icon icon="pen" scale="1" alt="edit"></b-icon>
          <span class="btn-text">Edit</span>
        </b-button>
        <b-button
          variant="danger"
          v-show="!editMode"
          @click="deleteItem(taskItem.id)"
        >
          <b-icon icon="x-circle" scale="1" variant="white"></b-icon>
          <span class="btn-text">Delete</span>
        </b-button>
      </b-button-group>
    </b-input-group>
  </b-list-group-item>
</template>

<script>
export default {
  name: 'TdTaskItem',
  props: {
    taskItem: {
      type: Object,
      require: true,
    },
  },
  data() {
    return {
      editMode: false,
      taskItemText: this.taskItem.text,
    };
  },
  inject: ['deleteItem', 'editItem', 'toggleCompletionStatus'],
  methods: {
    startEdit() {
      this.editMode = true;
      this.$nextTick(() => this.$refs.editInput.focus());
    },
    editTaskItem() {
      this.editItem(this.taskItem.id, this.taskItemText);
      this.editMode = false;
    },
    cancelEdition() {
      this.editMode = false;
      this.taskItemText = this.taskItem.text;
    },
    deleteTaskItem() {
      this.$emit('deleteTaskItem', this.taskItem.id);
    },
    toggleTaskCompletion() {
      this.toggleCompletionStatus(this.taskItem.id);
    },
  },
};
</script>

<style scoped>
.listitem-checkbox {
  padding-left: 0 !important;
  display: flex;
  align-items: center;
}

.listitem-input {
  padding-top: 0 !important;
  padding-bottom: 0 !important;
  margin-left: 15px !important;
}

.listitem-text {
  margin-left: 15px !important;
  margin-right: 10px !important;
  display: flex;
  align-items: center;
  width: 100%;
  overflow: hidden;
}

.listitem-text_done {
  text-decoration: line-through;
}
.btn-text {
  padding-left: 10px;
}

@media (max-width: 768px) {
  .btn-text {
    display: none;
  }
}

/*.btn-text {*/
/*  @media (min-width: 768px) {*/
/*    display: none*/
/*  }*/
/*}*/
</style>
