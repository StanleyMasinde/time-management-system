<template>
  <div class="row justify-content-center">
    <div class="col-md-8">
      <div class="card shadow">
        <div class="card-body">
          <ul v-show="activities" class="list-group">
            <div v-for="activity in activities" :key="activity.id">
              <li class="list-group-item shadow mb-1">
                <b>Title:</b>
                {{ activity.title }}
                <br>
                <b>Date</b>
                {{ activity.date }}
                <br>
                <b>Duration:</b>
                {{ activity.duration }}
                <br>
                <button
                  @click="showEditForm(activity.id)"
                  type="button"
                  class="btn btn-primary btn-sm"
                >Edit</button>
                <button
                  @click="deleteActivity(activity.id)"
                  type="button"
                  class="btn btn-danger btn-sm"
                >Delete</button>
              </li>
            </div>
          </ul>
        </div>
      </div>
    </div>
    <edit-component
      v-on:cancel="cancelEdit"
      v-on:update="getActivities"
      v-if="toEdit"
      :activity="toEdit"
      class="col-md"
    ></edit-component>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activities: null,
      toEdit: null
    };
  },
  methods: {
    getActivities() {
      axios.get(`current-user-activities`).then(res => {
        this.activities = res.data;
      });
    },
    deleteActivity(activity) {
      axios.delete(`activity/${activity}`).then(res => {
        this.getActivities();
      });
    },
    showEditForm(activity) {
      this.toEdit = activity;
    },
    cancelEdit() {
      this.toEdit = null;
    }
  },
  props: ["submitted"],
  mounted() {
    this.getActivities();
  },
  watch: {
    submitted() {
      this.getActivities();
    }
  }
};
</script>

<style>
</style>
