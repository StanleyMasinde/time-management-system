<template>
  <form id="addActivity" @submit.prevent="update" action method="post">
    <div v-show="updated" class="alert alert-success" role="alert">
      <strong>Updated</strong>
    </div>
    <div class="form-group">
      <label for>Title</label>
      <input
        autofocus
        type="title"
        v-model="title"
        name="title"
        id="inputTitle"
        class="form-control"
        placeholder="Some activity"
      >
    </div>

    <div class="form-group">
      <label for>Date</label>
      <input type="date" name="date" id="inputDate" v-model="date" class="form-control">
    </div>

    <div class="form-group">
      <label for>Duration</label>
      <input
        type="text"
        name="duration"
        v-model="duration"
        id="duration"
        class="form-control"
        placeholder="several hours"
        aria-describedby="inputDurationId"
      >
      <small id="inputDurationHelp" class="text-muted">You can put this in words</small>
    </div>

    <button type="submit" class="btn btn-primary">Update</button>
    <button @click="cancel" type="button" class="btn btn-primary">Cancel</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: null,
      date: null,
      duration: null,
      updated: false
    };
  },
  methods: {
    update() {
      axios
        .put(`activity/${this.activity}`, {
          title: this.title,
          date: this.date,
          duration: this.duration
        })
        .then(res => {
          this.updated = true;
          setTimeout(() => {
            this.cancel();
          }, 2000);
          this.$emit("update");
        });
    },
    getRecord() {
      axios.get(`activity/${this.activity}`).then(res => {
        this.title = res.data.title;
        this.date = res.data.date;
        this.duration = res.data.duration;
      });
    },
    cancel() {
      this.$emit("cancel");
    }
  },
  props: ["activity"],
  watch: {
    activity() {
      this.getRecord();
    }
  },
  mounted() {
    this.getRecord();
  }
};
</script>

<style>
</style>
