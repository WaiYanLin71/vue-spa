<template>
  <div class="container-fluid">
    <Teleport to="#modal">
      <DeleteModal :condition="toggleModal" @closeModal="closeModal" />
    </Teleport>
    <div class="container">
      <div class="row aligin-items-end justify-content-between mt-5">
        <div class="col-md-4">
          <button class="btn btn-primary">Create</button>
        </div>
        <div class="col-md-3 d-flex align-items-end">
          <input type="search" class="form-control me-2" placeholder="Search by name or email" />
          <button class="btn btn-success">Search</button>
        </div>
      </div>
      <div class="row justify-content-center aligin-items-center" v-if="datas.length > 0">
        <div class="col-12 mt-4">
          <TableHeadVue>
            <thead class="bg-dark text-white">
              <tr>
                <th>No</th>
                <th>Name</th>
                <th>Email</th>
                <th>Action</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(data,i) in datas" :key="data.id">
                <td>{{i+1}}</td>
                <td>{{data.name}}</td>
                <td>{{data.email}}</td>
                <td class="d-flex justify-content-center">
                  <button class="btn btn-warning me-2" @click="action($event,'edit',data.id)">Edit</button>
                  <button class="btn btn-danger" @click="action($event,'delete',data.id)">Delete</button>
                </td>
              </tr>
            </tbody>
          </TableHeadVue>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TableHeadVue from "./components/table/TableHead.vue";
import DeleteModal from "./components/modal/DeleteModal.vue";

export default {
  name: "App",
  components: {
    TableHeadVue,
    DeleteModal
  },
  methods: {
    action(e, id) {
      this.toggleModal = ["delete", id];
    },
    closeModal(emit) {
      this.toggleModal = emit;
    }
  },

  data() {
    return {
      datas: [],
      errors: null,
      toggleModal: []
    };
  },
  mounted() {
    fetch("http://localhost:8000/api/students")
      .then(res => res.json())
      .then(res => {
        if (!res.students) {
          throw res;
        }
        this.datas = [...res.students.data];
      })
      .catch(e => e.errors && (this.errors = e.errors));
  }
};
</script>

<style></style>
