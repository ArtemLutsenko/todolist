<template>
  <b-button-group>
    <b-button @click="download" variant="warning">Import</b-button>
    <b-button variant="info" @click="openFile">Export</b-button>
    <label class="text-reader" style="visibility: hidden">
      <input type="file" @change="loadTextFromFile" ref="fileReader" />
    </label>
  </b-button-group>
</template>

<script>
/* eslint-disable comma-dangle */
export default {
  name: 'TdImportExport',
  props: {
    taskList: {
      require: true,
    },
  },
  methods: {
    openFile() {
      this.$refs.fileReader.click();
    },
    download() {
      const jsonText = JSON.stringify(this.taskList);
      const element = document.createElement('a');
      element.setAttribute(
        'href',
        `data:text/plain;charset=utf-8,${encodeURIComponent(jsonText)}`,
      );
      element.setAttribute('download', 'TodoList');

      element.style.display = 'none';
      document.body.appendChild(element);

      element.click();

      document.body.removeChild(element);
    },
    loadTextFromFile(ev) {
      const file = ev.target.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        const result = JSON.parse(e.target.result);
        this.$emit('export', result);
      };
      reader.readAsText(file);
    },
  },
};
</script>

<style scoped></style>
