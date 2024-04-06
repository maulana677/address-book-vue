<script setup>
//import ref
import { ref } from "vue";

//import router
import { useRouter } from "vue-router";

//import api
import api from "../../api";

//init router
const router = useRouter();

//define state
const nama = ref("");
const no_telp = ref("");
const alamat = ref("");
const image = ref("");
const errors = ref([]);

//method for handle file changes
const handleFileChange = (e) => {
  //assign file to state
  image.value = e.target.files[0];
};

//method "storePost"
const storePost = async () => {
  //init formData
  let formData = new FormData();

  //assign state value to formData
  formData.append("nama", nama.value);
  formData.append("no_telp", no_telp.value);
  formData.append("alamat", alamat.value);
  formData.append("image", image.value);

  //store data with API
  await api
    .post("/api/posts", formData)
    .then(() => {
      //redirect
      router.push({ path: "/posts" });
    })
    .catch((error) => {
      //assign response error data to state "errors"
      errors.value = error.response.data;
    });
};
</script>

<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <form @submit.prevent="storePost()">
              <div class="mb-3">
                <label class="form-label fw-bold">Image</label>
                <input
                  type="file"
                  class="form-control"
                  @change="handleFileChange($event)"
                />
                <div v-if="errors.image" class="alert alert-danger mt-2">
                  <span>{{ errors.image[0] }}</span>
                </div>
              </div>
              <div class="mb-3">
                <label class="form-label fw-bold">Nama</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="nama"
                  placeholder="Title Post"
                />
                <div v-if="errors.nama" class="alert alert-danger mt-2">
                  <span>{{ errors.nama[0] }}</span>
                </div>
              </div>
              <div class="mb-3">
                <label class="form-label fw-bold">No Telepon</label>
                <input
                  type="number"
                  class="form-control"
                  v-model="no_telp"
                  placeholder="Title Post"
                />
                <div v-if="errors.no_telp" class="alert alert-danger mt-2">
                  <span>{{ errors.no_telp[0] }}</span>
                </div>
              </div>
              <div class="mb-3">
                <label class="form-label fw-bold">Alamat</label>
                <textarea
                  class="form-control"
                  v-model="alamat"
                  rows="5"
                  placeholder="Content Post"
                ></textarea>
                <div v-if="errors.alamat" class="alert alert-danger mt-2">
                  <span>{{ errors.alamat[0] }}</span>
                </div>
              </div>
              <button
                type="submit"
                class="btn btn-md btn-primary rounded-sm shadow border-0"
              >
                Save
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>