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
const image = ref("");
const nama = ref("");
const no_telp = ref("");
const alamat = ref("");
const errors = ref([]);

//method for handle file changes
const handleFileChange = (e) => {
  //assign file to state
  image.value = e.target.files[0];
};

//method "storePost"
const storeContact = async () => {
  //init formData
  let formData = new FormData();

  //assign state value to formData
  formData.append("image", image.value);
  formData.append("nama", nama.value);
  formData.append("no_telp", no_telp.value);
  formData.append("alamat", alamat.value);

  //store data with API
  await api
    .post("/api/contact", formData)
    .then(() => {
      //redirect
      router.push({ path: "/contacts" });
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
            <h1>Tambah Data</h1>
            <hr />
            <form @submit.prevent="storeContact()">
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
                  placeholder="Nama"
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
                  placeholder="No Telepon"
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
                  placeholder="Alamat"
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