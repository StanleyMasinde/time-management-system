<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card shadow">
          <div class="card-body">
            <div v-show="failures" class="alert alert-danger" role="alert">
              <li v-for="error in failures">{{ error[0] }}</li>
            </div>

            <div v-show="submitted" class="alert alert-success" role="alert">
              <strong>The activity was recorded</strong>
            </div>
            <form id="addActivity" @submit.prevent="addActivity" action method="post">
              <div class="form-group">
                <label for>Title</label>
                <input
                  type="title"
                  autocomplete="off"
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
                autocomplete="off"
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

              <button type="submit" class="btn btn-primary">Add activity</button>
            </form>
          </div>
        </div>
      </div>
    </div>

    <show-activities class="pt-2" :submitted="submitted"></show-activities>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: null,
      date: null,
      duration: null,
      submitted: false,
      failed: false,
      failures: null
    };
  },
  methods: {
    addActivity() {
      axios
        .post(`activity`, {
          title: this.title,
          date: this.date,
          duration: this.duration
        })
        .then(res => {
          this.title = null;
          this.date = null;
          this.duration = null;
          this.failures = null;
          this.submitted = true;
          window.setTimeout(() => {
            this.submitted = false;
          }, 3000);
        })
        .catch(err => {
          this.failed = true;
          this.failures = err.response.data.errors;
        });
    }
  }
};
</script>

<style>
</style>
