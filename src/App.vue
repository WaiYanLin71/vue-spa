<template>
  <div class="container-fluid">
    <Teleport to="#modal">
      <DeleteModal
        :condition="showModal"
        @closeModal="closeModal"
        @destroy="destroy"
      />
    </Teleport>
    <Teleport to="#modal">
      <CreateUpdate
        :condition="showModal"
        @closeModal="closeModal"
        @changeData="changeData"
      />
    </Teleport>
    <div class="container">
      <div class="row align-items-end justify-content-between mt-5">
        <div class="col-md-4">
          <button
            class="btn btn-primary"
            @click="createModal($event, { id: null, type: 'create' })"
          >
            Create
          </button>
        </div>
        <div class="col-md-3 d-flex align-items-end">
          <input
            type="search"
            class="form-control me-2"
            placeholder="Search by name or email"
            v-model="searchData"
          />
          <button class="btn btn-success" @click="search">Search</button>
        </div>
      </div>
      <div
        class="row justify-content-center algin-items-center"
        v-if="datas.length > 0"
      >
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
              <tr v-for="(data, i) in datas" :key="data.id">
                <td>{{ i + 1 }}</td>
                <td>{{ data.name }}</td>
                <td>{{ data.email }}</td>
                <td class="d-flex justify-content-center">
                  <button
                    class="btn btn-warning me-2"
                    @click="
                      createModal($event, { id: data.id, type: 'edit', data })
                    "
                  >
                    Edit
                  </button>
                  <button
                    class="btn btn-danger"
                    @click="createModal($event, { id: data.id, type: 'delete' })"
                  >
                    Delete
                  </button>
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
import CreateUpdate from "./components/modal/CreateUpdate.vue";

export default {
  name: "App",
  components: {
    TableHeadVue,
    DeleteModal,
    CreateUpdate,
  },
  methods: {
    createModal(e, data) {
      this.showModal = data;
    },
    closeModal(emit) {
      this.showModal = emit;
    },
    changeData(emit) {
      this.datas = emit.data;
      this.showModal = {};
    },
    destroy(emit) {
      this.datas = emit.data;
      this.showModal = {};
    },
    index(query=null) {
      let url = "http://localhost:8001/api/students";
      query && (url = url + `?search=${query}`)
      fetch(url)
        .then((res) => res.json())
        .then((res) => {
          console.log(res);
          if (!res.students) {
            throw res;
          }
          this.datas = [...res.students.data];
        })
        .catch((e) => e.errors && (this.errors = e.errors));
    },
    search() {
      this.index(this.searchData)
    }
  },

  data() {
    return {
      datas: [],
      errors: null,
      showModal: {},
      searchData: '',
    };
  },
  mounted() {
    this.index()
  },
};
</script>

<style></style>
