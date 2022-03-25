<template>
  <BackDrop v-if="condition.type === 'delete'" @click.self="close">
    <div class="col-md-4 mt-5">
        <div class="card border-0 shadow-sm">
          <div class="card-body">
            <h3>Are You Sure?</h3>
            <div class="d-flex justify-content-end">
              <button class="btn-secondary btn me-2" @click="close">
                Close
              </button>
              <button
                class="btn-danger btn"
                @click="deleteId($event, condition.id)"
              >
                Delete
              </button>
            </div>
          </div>
        </div>
      </div>
  </BackDrop>
</template>

<script>
import BackDrop from "./BackDrop.vue";
export default {
  components: {
    BackDrop,
  },
  props: ["condition"],
  data() {
    return {};
  },
  methods: {
    close() {
      this.$emit("closeModal", {});
    },
    deleteId(e, id) {
      fetch(`http://localhost:8001/api/students/${id}`, {
        method: "DELETE",
        headers: {
          Accept: "application/json",
          "X-Requested-With": "XMLHttpRequest",
        },
      })
        .then((res) => res.json())
        .then((res) => {
          if (!res.students) {
            throw res;
          }
          this.$emit("destroy", res.students);
        });
    },
  },
};
</script>

<style scoped>
</style>
