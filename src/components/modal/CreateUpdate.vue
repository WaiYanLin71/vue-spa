<template>
  <BackDrop
    v-if="condition.type === 'create' || condition.type === 'edit'"
    @click.self="closeModal"
  >
    <div class="col-4 mt-5">
      <div class="card border-0">
        <div class="card-body">
          <h3>Create Student</h3>
          <form>
            <div class="form-group mt-3">
              <label for="name">Name</label>
              <input type="text" class="form-control" v-model="name" />
              <span class="mt-1 text-danger">{{ errors.name }}</span>
            </div>
            <div class="form-group mt-3">
              <label for="file">Email</label>
              <input type="text" class="form-control" v-model="email" />
              <span class="mt-1 text-danger">{{ errors.email }}</span>
            </div>
            <div class="form-group mt-3">
              <label for="file">File</label>
              <input type="file" class="form-control" @change="storeImage" />
              <span class="mt-1 text-danger">{{ errors.image }}</span>
            </div>
            <div class="form-group mt-3">
              <div
                class="btn btn-primary text-capitalize"
                type="submit"
                @click.prevent="sendData"
              >
                {{ condition.type }}
              </div>
            </div>
          </form>
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
    return {
      name: '' ,
      email: '' ,
      image: null,
      errors: {
        name: "",
        email: "",
      },
    };
  },
  updated() {

  },
  watch:{
      condition(current) {
          const {data=null,type} =current;  
          if(type ==='edit') {
              this.name =data.name;
              this.email = data.email
          }
      }
  },
  methods: {
    closeModal() {
      this.$emit("closeModal", {});
    },
    sendData() {
      let formData = new FormData();
      const name = this.name;
      const email = this.email;
      const image = this.image;

      formData.append("name", name);
      formData.append("email", email);
      formData.append("image", image);

      let url = "http://localhost:8001/api/students";

      if (this.condition.type === "edit") {
        url = url + `/${this.condition.id}`;
      }
      console.log(url);
      fetch(url, {
        method: "POST",
        headers: {
          "Accept": "application/json",
          "X-Requested-With": "XMLHttpRequest",
        },
        body: formData,
      })
        .then((res) => res.json())
        .then((res) => {
          if (!res.students) {
            throw res;
          }
          this.$emit("changeData", res.students);
          this.name = "";
          this.email = "";
        })
        .catch((e) => {
        console.log(e)
          Object.keys(e.errors).map((error) => {
            this.errors[error] = e.errors[error][0];
          });
        });
    },
    storeImage(e) {
      this.image = e.target.files[0];
    },
  },

};
</script>

<style>
</style>