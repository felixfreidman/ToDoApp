<template>
  <div class="container-mt-2">
    <b-form>
      <b-form-group label="Title" label-for="subject">
        <b-form-input
          id="subject"
          v-model="form.subject"
          type="text"
          placehoder="Please enter a plan's title"
          required
          autocomplete="off"
        ></b-form-input>
      </b-form-group>

      <b-form-group label="Description" label-for="description">
        <b-form-textarea
          id="description"
          v-model="form.description"
          type="text"
          placehoder="Please enter a description for your plan"
          required
          autocomplete="off"
        ></b-form-textarea>
        <b-button type="submit" variant="outline-primary" @click="saveTask">Add</b-button>
      </b-form-group>
    </b-form>
  </div>
</template>

<script>
import ToastMixin from '@/mixins/toastMixin.js'
import '../styles/form.css'

export default {
  name: "Form",

  mixins: [ToastMixin],
  data() {
    return {
      form: {
        subject: "",
        description: ""
      },
      methodSave: "new"
    }
  },

  created(){
    if(this.$route.params.index === 0 || this.$route.params.index !== undefined) {
      this.methodSave = "update"
      let tasks = JSON.parse(localStorage.getItem("tasks"))
      this.form = tasks[this.$route.params.index]
    }
  },

  methods: {
    saveTask() {
      // Saving to Local Storage and Toasting
      if(this.methodSave === "update") {
        let tasks = JSON.parse(localStorage.getItem("tasks"))
        tasks[this.$route.params.index] = this.form
        localStorage.setItem("tasks", JSON.stringify(tasks))
        this.showToast("success", "Success", "Your Note Have Been Saved")
        this.$route.push({name: "list"})
        return
      }


      let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : []
      tasks.push(this.form) 
      localStorage.setItem("tasks", JSON.stringify(tasks)) 
      this.showToast("success", "Success", "Your Note Have Been Saved")
      this.$router.push({name: "list"})
  },
}
}
</script>