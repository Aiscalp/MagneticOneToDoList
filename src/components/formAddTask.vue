<template>
  <b-form>
    <b-form-group label="Note:" label-for="subject" class="text">
      <b-form-input id="subject" v-model="formAddTask.subject" type="text" placeholder="Note name" required
        autocomplete="off"></b-form-input>
    </b-form-group>

    <b-form-group label="Todo:" label-for="description" class="text">
      <b-form-textarea id="description" v-model="formAddTask.description" type="text" placeholder="Task" required
        autocomplete="off"></b-form-textarea>
    </b-form-group>

    <b-button type="submit" variant="outline-secondary" @click="saveTask">
      Save Note
    </b-button>
    <b-button type="submit" style="margin-left: 10px;" variant="outline-secondary" @click="cancelTask">
      Cancel
    </b-button>
    <b-button v-if="this.$route.params.index === 0 ||
      this.$route.params.index !== undefined" type="submit" style="margin-left: 10px;" variant="outline-danger"
      @click="removeTask">
      Remove
    </b-button>
</b-form>
</template>

<script>
import ToastMixin from "@/mixins/toastMixin.js";

export default {
  name: "formAddTask",

  mixins: [ToastMixin],

  data() {
    return {
      formAddTask: {
        subject: "",
        description: "",
      },
      methodSave: "new",
    };
  },

  created() {
    if (
      this.$route.params.index === 0 ||
      this.$route.params.index !== undefined
    ) {
      this.methodSave = "update";
      let tasks = JSON.parse(localStorage.getItem("tasks"));
      this.formAddTask = tasks[this.$route.params.index];
    }
  },

  methods: {
    saveTask() {
      if (this.methodSave === "update") {
        let tasks = JSON.parse(localStorage.getItem("tasks"));
        tasks[this.$route.params.index] = this.formAddTask;
        localStorage.setItem("tasks", JSON.stringify(tasks));
        this.showToast("success", "Success!", "Note has been changed");
        this.$router.push({ name: "home" });
        return;
      }

      let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [];
      tasks.push(this.formAddTask);
      localStorage.setItem("tasks", JSON.stringify(tasks));
      this.showToast("success", "Success!", "Note has been added");
      this.$router.push({ name: "home" });
    },

    removeTask() {
      let tasks = JSON.parse(localStorage.getItem("tasks"));
      tasks[this.$route.params.index] = this.formAddTask;
      localStorage.removeItem("tasks", JSON.stringify(tasks));
      this.showToast("success", "Success!", "Note has been removed");
      this.$router.push({ name: "home" });
      return;
    },

    cancelTask() {
      this.$router.push({ name: "home" });
    },
  },
};
</script>

<style>
.container {
  height: calc(100vh - 64px);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  padding-top: 30px;
}

.text {
  color: rgb(219, 209, 209);
  font-weight: 600;
}

#description,
#subject {
  background-color: #342432;
  border-radius: 7px;
  color: rgb(255, 255, 255);
  box-shadow: -1px 1px 11px 1px rgba(0, 0, 0, 0.36);
  -webkit-box-shadow: -1px 1px 11px 1px rgba(0, 0, 0, 0.36);
  border: none;
}
</style>
