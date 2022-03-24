<template>
  <BackDrop v-if="condition[0] === 'delete'" @click.self="close">
    <div class="row justify-content-center mt-5">
      <div class="col-md-4">
        <div class="card border-0 shadow-sm">
          <div class="card-body">
            <h3>Are You Sure?</h3>
            <div class="d-flex justify-content-end">
              <button class="btn-secondary btn me-2" @click="close">Close</button>
              <button class="btn-danger btn" @click="deleteId($event,condition[1])">Delete</button>
            </div>
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
    BackDrop
  },
  props: ["condition"],
  methods: {
    close() {
      this.$emit("closeModal", []);
    },
    deleteId(e, id) {
      console.log(id);
      this.$emit("closeModal", []);
    }
  }
};
</script>

<style scoped>
</style>
