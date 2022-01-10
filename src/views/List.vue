<template>
  <div class="container-mt-2">
    <div v-for="(task, index) in tasks" :key="index">
      <b-card :title="task.subject" class="mb-2">
        <b-card-text>{{ task.description }}</b-card-text>
        <b-button variant="outline-secondary" class="mr-2" @click="edit(index)">Edit</b-button>
        <b-button variant="outline-danger" class="mr-2" @click="remove(task, index)">Delete</b-button>
      </b-card>
    </div>

    <b-modal ref="modalRemove" hide-footer title="Deleting the plan...">
      <div class="d-block text-center">
        <label
          class="modal-confirm"
        >Are you sure you want to delete Plan {{ taskSelected.subject }}</label>
      </div>
      <div class="mt-3 d-flex justify-content-end">
        <b-button variant="outline-secondary" class="mr-2" @click="hideModal(index)">Cancel</b-button>
        <b-button
          variant="outline-danger"
          class="mr-2"
          @click="confirmRemoveTask(task, index)"
        >Delete</b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
import '../styles/list.css'
export default {
  name: 'List',

  data() {
    return {
      tasks: [],
      taskSelected: []
    }
  },

  created() {
    this.tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : []
  },

  methods: {
    edit(index) {
      this.$router.push({ name: "form", params: { index } })
    },

    remove(task, index) {
      this.taskSelected = task;
      this.taskSelected.index = index;
      this.$refs.modalRemove.show()
    },

    hideModal() {
      this.$refs.modalRemove.hide()
    },

    confirmRemoveTask() {
      this.tasks.splice(this.taskSelected.index, 1)
      localStorage.setItem("tasks", JSON.stringify(this.tasks))
      this.hideModal()
    }
  }
}
</script>